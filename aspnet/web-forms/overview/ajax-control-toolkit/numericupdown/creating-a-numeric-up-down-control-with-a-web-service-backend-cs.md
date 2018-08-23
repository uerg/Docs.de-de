---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Erstellen eine numerische hoch-/Herunterskalieren-Steuerelement mit einer Web-Dienst-Back-End (c#) | Microsoft-Dokumentation
author: wenz
description: Anstatt einen Benutzer aus, geben Sie einen Wert in ein Kontrollkästchen, kann ein numerischen UpAndDown-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden) als weitere c Beweisen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: ffefed61e259994990315d17a545ef74074092a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834418"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="61f2d-103">Erstellen einen numerischen UpAndDown-Steuerelement mit einer Web-Dienst-Back-End (c#)</span><span class="sxs-lookup"><span data-stu-id="61f2d-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>
====================
<span data-ttu-id="61f2d-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="61f2d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="61f2d-105">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="61f2d-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="61f2d-106">Geben Sie einen Wert in das Kontrollkästchen für einen Benutzer, anstatt ein numerischer hoch-/Herunterskalieren-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden) kann sich als mehr vertraut.</span><span class="sxs-lookup"><span data-stu-id="61f2d-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="61f2d-107">Standardmäßig werden das NumericUpDown-Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst, erweist sich als flexibler.</span><span class="sxs-lookup"><span data-stu-id="61f2d-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="61f2d-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="61f2d-108">Overview</span></span>

<span data-ttu-id="61f2d-109">Geben Sie einen Wert in das Kontrollkästchen für einen Benutzer, anstatt ein numerischer hoch-/Herunterskalieren-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden) kann sich als mehr vertraut.</span><span class="sxs-lookup"><span data-stu-id="61f2d-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="61f2d-110">In der Standardeinstellung die `NumericUpDown` Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst, erweist sich als flexibler.</span><span class="sxs-lookup"><span data-stu-id="61f2d-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="61f2d-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="61f2d-111">Steps</span></span>

<span data-ttu-id="61f2d-112">Das ASP.NET AJAX Control Toolkit enthält die `NumericUpDown` Extender automatisch zwei Schaltflächen zu einem Textfeld hinzugefügt: eine zum Erhöhen des Werts, eine zum Verringern der es.</span><span class="sxs-lookup"><span data-stu-id="61f2d-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="61f2d-113">Das Steuerelement unterstützt jedoch auch ein Aufruf des Webdiensts (oder eine Seite-Methodenaufruf).</span><span class="sxs-lookup"><span data-stu-id="61f2d-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="61f2d-114">Wenn das hoch- oder Herunterskalieren Schaltfläche geklickt wird, die JavaScript-Code eine Verbindung mit dem Webserver her und führt es eine Methode.</span><span class="sxs-lookup"><span data-stu-id="61f2d-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="61f2d-115">Die Signatur der Methode ist den folgenden Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="61f2d-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="61f2d-116">Die `current` Argument ist der aktuelle Wert in das Textfeld; die `tag` -Attribut ist zusätzlicher Kontext-Daten, die als eine Eigenschaft festgelegt werden können die `NumericUpDown` Extender (ist jedoch nicht erforderlich).</span><span class="sxs-lookup"><span data-stu-id="61f2d-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="61f2d-117">In diesem Beispiel die numerische hoch-/Herunterskalieren Steuerelement sollte nur zulassen Werte, die Potenzen von 2: 1, 2, 4, 8, 16, 32, 64 und So weiter.</span><span class="sxs-lookup"><span data-stu-id="61f2d-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="61f2d-118">Aus diesem Grund muss die Methode ausgeführt wird, wenn der Benutzer den Wert erhöhen möchte double den alten Wert; die andere Methode muss durch zwei Teilen.</span><span class="sxs-lookup"><span data-stu-id="61f2d-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="61f2d-119">Hier ist also die vollständige Webdienst:</span><span class="sxs-lookup"><span data-stu-id="61f2d-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="61f2d-120">Abschließend erstellen Sie eine neue ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="61f2d-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="61f2d-121">Wie üblich, müssen Sie eine `ScriptManager` -Steuerelement, ein `TextBox` Steuerelement und ein `NumericUpDownExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="61f2d-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="61f2d-122">In letzterem Fall müssen Sie die Web-Service-Informationen bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="61f2d-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="61f2d-123">`ServiceDownMethod` Name des der aus web-Methode oder die Seite Methode</span><span class="sxs-lookup"><span data-stu-id="61f2d-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="61f2d-124">`ServiceDownPath` Pfad auf den Webdienst mit der nach-unten-Service-Methode Lassen Sie bei Verwendung eine Seitenmethode</span><span class="sxs-lookup"><span data-stu-id="61f2d-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="61f2d-125">`ServiceUpMethod` Name des oben-Webmethode oder die Seite Methode</span><span class="sxs-lookup"><span data-stu-id="61f2d-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="61f2d-126">`ServiceUpPath` Pfad auf den Webdienst mit der Sie Dienstmethode Lassen Sie bei Verwendung eine Seitenmethode</span><span class="sxs-lookup"><span data-stu-id="61f2d-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="61f2d-127">Hier ist das vollständige Markup für die Seite:</span><span class="sxs-lookup"><span data-stu-id="61f2d-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="61f2d-128">Wenn Sie auf die Seite ausführen, beachten Sie, wie der Wert in das Textfeld immer verdoppelt sich, beim Klicken Sie auf die Schaltfläche "oben", und wird beim Klicken auf die untere Schaltfläche halbiert.</span><span class="sxs-lookup"><span data-stu-id="61f2d-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="61f2d-129">[![Nur Zahlen, die eine Potenz von 2 angezeigt werden.](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61f2d-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span></span>

<span data-ttu-id="61f2d-130">Nur Zahlen, die eine Potenz von 2 angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="61f2d-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="61f2d-131">Nächste</span><span class="sxs-lookup"><span data-stu-id="61f2d-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
