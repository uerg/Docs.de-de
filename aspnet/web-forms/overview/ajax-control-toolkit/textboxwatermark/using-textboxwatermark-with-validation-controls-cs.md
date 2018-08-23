---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Verwenden von TextBoxWatermark mit Validierungssteuerelementen (c#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, es ich...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 35dd340ffce7b83303a44a0d6532ce73be5350f0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836221"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="b0d7d-104">Verwenden von TextBoxWatermark mit Validierungssteuerelementen (c#)</span><span class="sxs-lookup"><span data-stu-id="b0d7d-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="b0d7d-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b0d7d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b0d7d-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0d7d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="b0d7d-107">Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="b0d7d-108">Wenn ein Benutzer in das Feld klickt, wird es geleert.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="b0d7d-109">Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="b0d7d-110">Dies möglicherweise in Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite, aber diese Probleme umgehen können.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="b0d7d-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b0d7d-111">Overview</span></span>

<span data-ttu-id="b0d7d-112">Die `TextBoxWatermark` Steuerelement im AJAX Control Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="b0d7d-113">Wenn ein Benutzer in das Feld klickt, wird es geleert.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="b0d7d-114">Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="b0d7d-115">Dies möglicherweise in Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite, aber diese Probleme umgehen können.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="b0d7d-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="b0d7d-116">Steps</span></span>

<span data-ttu-id="b0d7d-117">Die grundlegende Einrichtung des Beispiels ist Folgendes: eine `TextBox` Steuerelement wird mit einem Wasserzeichen versehen mit einer `TextBoxWatermarkExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="b0d7d-118">Eine Schaltfläche löst einen Postback und später zum Auslösen der Validierungssteuerelemente auf der Seite verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="b0d7d-119">Darüber hinaus eine `ScriptManager` Steuerelement ist erforderlich, um ASP.NET AJAX zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="b0d7d-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="b0d7d-120">Fügen Sie jetzt eine `RequiredFieldValidator` -Steuerelement, das überprüft, ob Text in das Feld vorhanden ist, wenn das Formular übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="b0d7d-121">Die `InitialValue` Eigenschaft des Validierungssteuerelements muss festgelegt werden, auf den gleichen Wert, der verwendet wird die `TextBoxWatermarkExtender` Kontrolle: Wenn das Formular gesendet wird, ist der Wert des eine unverändert Textbox des Grenzwerts in diesem:</span><span class="sxs-lookup"><span data-stu-id="b0d7d-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="b0d7d-122">Allerdings ist ein Problem bei diesem Ansatz: Wenn der Client über JavaScript deaktiviert, das Textfeld ist nicht bereits ausgefüllt ist mit der Wasserzeichentext aus diesem Grund die `RequiredFieldValidator` eine Fehlermeldung wird nicht ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="b0d7d-123">Aus diesem Grund eine zweite `RequiredFieldValidator` Steuerelement ist erforderlich, die überprüft, ob ein leeres Textfeld (das Auslassen der `InitialValue` Attribut).</span><span class="sxs-lookup"><span data-stu-id="b0d7d-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="b0d7d-124">Da beide Validierungssteuerelemente verwenden `Display` = `"Dynamic"`, der Endbenutzer kann nicht über die visuelle Darstellung der zwei Validatoren ausgelöst wurde. unterscheiden; stattdessen offenbar gab es nur eine davon.</span><span class="sxs-lookup"><span data-stu-id="b0d7d-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="b0d7d-125">Abschließend fügen Sie serverseitigen Code, um den Text im Feld ausgeben, wenn kein Validierungssteuerelement eine Fehlermeldung ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="b0d7d-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="b0d7d-126">[![Das Validierungssteuerelement meldet, dass es in das Feld kein Text ist](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0d7d-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="b0d7d-127">Das Validierungssteuerelement meldet, dass kein Text vorhanden, in das Feld ist ([klicken Sie, um das Bild in voller Größe anzeigen](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b0d7d-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0d7d-128">[Zurück](using-textboxwatermark-in-a-formview-cs.md)
> [Weiter](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b0d7d-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
