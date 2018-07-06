---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Verwenden das Schieberegler-Steuerelement mit automatischem Postback (c#) | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, stellen die Schieberegler Autopost...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cb3c041f2a8a499d27cbcbc2f8975eedcac12e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834150"
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="0b437-104">Verwenden das Schieberegler-Steuerelement mit automatischem Postback (c#)</span><span class="sxs-lookup"><span data-stu-id="0b437-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="0b437-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0b437-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0b437-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0b437-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="0b437-107">Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können.</span><span class="sxs-lookup"><span data-stu-id="0b437-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="0b437-108">Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.</span><span class="sxs-lookup"><span data-stu-id="0b437-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="0b437-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="0b437-109">Overview</span></span>

<span data-ttu-id="0b437-110">Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können.</span><span class="sxs-lookup"><span data-stu-id="0b437-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="0b437-111">Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.</span><span class="sxs-lookup"><span data-stu-id="0b437-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="0b437-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="0b437-112">Steps</span></span>

<span data-ttu-id="0b437-113">Um den Schieberegler automatisch postback auf eine Änderung vornehmen, benötigen beide Felder das Attribut `AutoPostBack="true"`: das Textfeld, das den Schieberegler selbst werden soll, und das Textfeld, das den Schieberegler auf die Position enthält.</span><span class="sxs-lookup"><span data-stu-id="0b437-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="0b437-114">Hier ist das erforderliche Markup für diese:</span><span class="sxs-lookup"><span data-stu-id="0b437-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="0b437-115">Die `SliderExtender` Steuerelement von ASP.NET AJAX Control Toolkit weist die beiden Textfelder die Schieberegler-Funktionalität:</span><span class="sxs-lookup"><span data-stu-id="0b437-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="0b437-116">Zusätzliche Label-Element wird später verwendet werden, um die Benutzer eines Postbacks zu informieren:</span><span class="sxs-lookup"><span data-stu-id="0b437-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="0b437-117">Zum Schluss die `ScriptManager` Steuerelement von ASP.NET AJAX lädt die erforderliche JavaScript für das Steuerelement-Toolkit funktioniert:</span><span class="sxs-lookup"><span data-stu-id="0b437-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="0b437-118">Nachdem Sie der Schieberegler wieder veröffentlicht; Dieses Ereignis kann abgefangen und eine Aktion ausgeführt werden, auf der Serverseite:</span><span class="sxs-lookup"><span data-stu-id="0b437-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="0b437-119">[![Verschieben des Schiebereglers wird einen Postback ausgelöst.](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0b437-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="0b437-120">Verschieben des Schiebereglers einen Postback auslöst ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0b437-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="0b437-121">[![Das Datum dieser Änderung wird anschließend in die Bezeichnung geschrieben.](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0b437-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="0b437-122">Anschließend wird das Datum der Änderung in die Bezeichnung geschrieben ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0b437-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0b437-123">Nächste</span><span class="sxs-lookup"><span data-stu-id="0b437-123">Next</span></span>](databinding-the-slider-control-cs.md)
