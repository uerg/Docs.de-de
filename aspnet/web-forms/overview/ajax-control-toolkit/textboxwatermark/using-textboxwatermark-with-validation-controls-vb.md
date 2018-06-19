---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Validierungssteuerelemente (VB) TextBoxWatermark mit | Microsoft Docs
author: wenz
description: TextBoxWatermark-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld es ich...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879230"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="565f9-104">Verwenden von TextBoxWatermark mit Validierungssteuerelemente (VB)</span><span class="sxs-lookup"><span data-stu-id="565f9-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="565f9-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="565f9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="565f9-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="565f9-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="565f9-107">TextBoxWatermark-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="565f9-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="565f9-108">Klickt ein Benutzer in das Feld, wird es geleert.</span><span class="sxs-lookup"><span data-stu-id="565f9-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="565f9-109">Wenn der Benutzer das Feld verlässt, ohne Text eingeben, wird das vorab ausgefüllte Text erneut.</span><span class="sxs-lookup"><span data-stu-id="565f9-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="565f9-110">Dies kann einen Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite verursachen, aber diese Probleme umgehen können.</span><span class="sxs-lookup"><span data-stu-id="565f9-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="565f9-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="565f9-111">Overview</span></span>

<span data-ttu-id="565f9-112">Die `TextBoxWatermark` Steuerelement im AJAX-Steuerelement-Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="565f9-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="565f9-113">Klickt ein Benutzer in das Feld, wird es geleert.</span><span class="sxs-lookup"><span data-stu-id="565f9-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="565f9-114">Wenn der Benutzer das Feld verlässt, ohne Text eingeben, wird das vorab ausgefüllte Text erneut.</span><span class="sxs-lookup"><span data-stu-id="565f9-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="565f9-115">Dies kann einen Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite verursachen, aber diese Probleme umgehen können.</span><span class="sxs-lookup"><span data-stu-id="565f9-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="565f9-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="565f9-116">Steps</span></span>

<span data-ttu-id="565f9-117">Einfache Einrichtung des Beispiels lautet wie folgt: ein `TextBox` Steuerelement ist mit einem Wasserzeichen versehen mithilfe einer `TextBoxWatermarkExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="565f9-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="565f9-118">Eine Schaltfläche löst einen Postback und später zum Auslösen der Validierungssteuerelemente auf der Seite verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="565f9-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="565f9-119">Darüber hinaus eine `ScriptManager` -Steuerelement ist erforderlich, um ASP.NET AJAX zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="565f9-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="565f9-120">Fügen Sie jetzt eine `RequiredFieldValidator` Steuerelement, das überprüft, ob Text im Feld vorhanden ist, wenn das Formular gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="565f9-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="565f9-121">Die `InitialValue` das Validierungssteuerelement Eigenschaft muss festgelegt werden, auf den gleichen Wert, der verwendet wird die `TextBoxWatermarkExtender` Steuerelement: beim Senden des Formulars wird der Wert von einer unveränderten Textfeld Wert für die Wasserzeichen darin:</span><span class="sxs-lookup"><span data-stu-id="565f9-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="565f9-122">Allerdings ist ein Problem bei diesem Ansatz: Wenn der Client, JavaScript deaktiviert, das Textfeld "ist nicht ausgefüllt mit Wasserzeichentexts, daher die `RequiredFieldValidator` eine Fehlermeldung wird nicht ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="565f9-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="565f9-123">Aus diesem Grund eine zweite `RequiredFieldValidator` Control ist erforderlich, die überprüft, ob leere Textfelder (Auslassen der `InitialValue` Attribut).</span><span class="sxs-lookup"><span data-stu-id="565f9-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="565f9-124">Da beide Validierungssteuerelemente verwenden `Display` = `"Dynamic"`, der Endbenutzer kann nicht über die visuelle Darstellung, die die zwei Validierungssteuerelemente ausgelöst wurde unterscheiden; stattdessen scheint gab es nur eine Kopie.</span><span class="sxs-lookup"><span data-stu-id="565f9-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="565f9-125">Fügen Sie schließlich serverseitigen Code zum den Text im Feld ausgegeben, wenn kein Validierungssteuerelement eine Fehlermeldung ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="565f9-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="565f9-126">[![Die Bestätigung meldet, dass es in das Feld kein Text ist](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="565f9-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="565f9-127">Die Bestätigung meldet, dass es in das Feld kein Text ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="565f9-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="565f9-128">Vorherige</span><span class="sxs-lookup"><span data-stu-id="565f9-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
