---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Erstellen eine numerische nach oben/unten Steuerelement mit einer Web-Service-Backend (VB) | Microsoft Docs
author: wenz
description: "Anstatt einen Benutzer aus, geben Sie einen Wert in ein Kontrollkästchen konnte eine numerische-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) nach oben/unten als weitere c nachweisen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="48299-103">Erstellen einen numerischen Steuerelements nach oben/unten mit einem Web-Service-Backend (VB)</span><span class="sxs-lookup"><span data-stu-id="48299-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="48299-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="48299-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="48299-105">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="48299-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="48299-106">Anstatt einen Benutzer einen Wert in ein Kontrollkästchen geben numerische nach oben/unten-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) kann sich als mehr vertraut.</span><span class="sxs-lookup"><span data-stu-id="48299-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="48299-107">Wird standardmäßig das NumericUpDown-Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst wird bewiesen, dass mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="48299-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="48299-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="48299-108">Overview</span></span>

<span data-ttu-id="48299-109">Anstatt einen Benutzer einen Wert in ein Kontrollkästchen geben numerische nach oben/unten-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) kann sich als mehr vertraut.</span><span class="sxs-lookup"><span data-stu-id="48299-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="48299-110">Wird standardmäßig die `NumericUpDown` Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst wird bewiesen, dass mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="48299-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="48299-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="48299-111">Steps</span></span>

<span data-ttu-id="48299-112">Das ASP.NET AJAX-Steuerelement-Toolkit enthält die `NumericUpDown` Extender automatisch zwei Schaltflächen zu einem Textfeld hinzugefügt: eine zum Erhöhen des Objektwerts, eine zum Verringern der es.</span><span class="sxs-lookup"><span data-stu-id="48299-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="48299-113">Das Steuerelement unterstützt jedoch auch ein Aufruf des Webdiensts (oder Methodenaufruf für die Seite ").</span><span class="sxs-lookup"><span data-stu-id="48299-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="48299-114">Wenn die oben oder unten Schaltfläche geklickt wird, die JavaScript-Code eine Verbindung mit dem Webserver her und führt eine Methode vorhanden.</span><span class="sxs-lookup"><span data-stu-id="48299-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="48299-115">Die Signatur der Methode ist die folgende:</span><span class="sxs-lookup"><span data-stu-id="48299-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="48299-116">Der `current` Argument ist der aktuelle Wert im Textfeld; der `tag` Attribut ist zusätzliche Kontextdaten, die als Eigenschaft festgelegt werden, können die `NumericUpDown` Extender (ist jedoch nicht erforderlich).</span><span class="sxs-lookup"><span data-stu-id="48299-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="48299-117">Für dieses Beispiel soll der numerischen Wert nach oben/unten Steuerelement Werte, die Potenzen von 2 sind nur zulassen: 1, 2, 4, 8, 16, 32, 64 und So weiter.</span><span class="sxs-lookup"><span data-stu-id="48299-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="48299-118">Aus diesem Grund muss die Methode ausgeführt wird, wenn der Benutzer zum Erhöhen des möchte double den alten Wert; die andere Methode muss durch zwei Teilen.</span><span class="sxs-lookup"><span data-stu-id="48299-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="48299-119">Hier ist also der vollständige Webdienst:</span><span class="sxs-lookup"><span data-stu-id="48299-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="48299-120">Abschließend erstellen Sie eine neue ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="48299-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="48299-121">Wie üblich, Sie müssen eine `ScriptManager` -Steuerelement, ein `TextBox` Steuerelement und ein `NumericUpDownExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="48299-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="48299-122">Für die letztgenannte Aufgabe müssen Sie die Webdienstinformationen angeben:</span><span class="sxs-lookup"><span data-stu-id="48299-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="48299-123">`ServiceDownMethod`Name des der aus-Webmethode oder Seite Methode</span><span class="sxs-lookup"><span data-stu-id="48299-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="48299-124">`ServiceDownPath`der Pfad zu den Webdienst mit der nach-unten Dienstmethode; Lassen Sie bei Verwendung von einer Seitenmethode</span><span class="sxs-lookup"><span data-stu-id="48299-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="48299-125">`ServiceUpMethod`Name der auf--Webmethode oder Seite Methode</span><span class="sxs-lookup"><span data-stu-id="48299-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="48299-126">`ServiceUpPath`der Pfad zu den Webdienst mit dem Stand Dienstmethode; Lassen Sie bei Verwendung von einer Seitenmethode</span><span class="sxs-lookup"><span data-stu-id="48299-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="48299-127">Hier ist das vollständige Markup für die Seite an:</span><span class="sxs-lookup"><span data-stu-id="48299-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="48299-128">Wenn Sie auf der Seite "ausführen, beachten Sie, wie der Wert in das Textfeld immer verdoppelt wird, wenn Sie klicken Sie auf die Schaltfläche" oben "und werden beim Klicken auf die Schaltfläche mit den niedrigeren halbiert.</span><span class="sxs-lookup"><span data-stu-id="48299-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="48299-129">[![Nur Ziffern, die eine Potenz von 2 angezeigt werden.](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="48299-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="48299-130">Nur Ziffern, die eine Potenz von 2 angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="48299-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="48299-131">Zurück</span><span class="sxs-lookup"><span data-stu-id="48299-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
