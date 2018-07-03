---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Verwenden von automatischem Postback mit CascadingDropDown (VB) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 274189184b82734e89a30c9450079d7e07971f3c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378160"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="b52ad-103">Verwenden von automatischem Postback mit CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="b52ad-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="b52ad-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b52ad-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b52ad-105">[Code herunterladen](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b52ad-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="b52ad-106">Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="b52ad-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b52ad-107">Jedoch beim Einsatz des CascadingDropDown-Steuerelements, ASP. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da ein (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden.</span><span class="sxs-lookup"><span data-stu-id="b52ad-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="b52ad-108">Dieser Effekt kann vermieden werden, und Javascriptcode.</span><span class="sxs-lookup"><span data-stu-id="b52ad-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="b52ad-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b52ad-109">Overview</span></span>

<span data-ttu-id="b52ad-110">Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="b52ad-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b52ad-111">(Z. B. eine Liste enthält eine Liste der US-Staaten und die folgenden Liste wird dann mit der größten Städte in diesem Zustand gefüllt.) Jedoch beim Einsatz des CascadingDropDown-Steuerelements, ASP. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da ein (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden.</span><span class="sxs-lookup"><span data-stu-id="b52ad-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="b52ad-112">Dieser Effekt kann vermieden werden, und Javascriptcode.</span><span class="sxs-lookup"><span data-stu-id="b52ad-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="b52ad-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="b52ad-113">Steps</span></span>

<span data-ttu-id="b52ad-114">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der &lt; `form` &gt; Element):</span><span class="sxs-lookup"><span data-stu-id="b52ad-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="b52ad-115">Anschließend ist ein DropDownList-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="b52ad-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="b52ad-116">Für diese Liste wird ein CascadingDropDown-Extender Bereitstellen von Informationen von Web-Dienst-URL und -Methode hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="b52ad-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="b52ad-117">Der CascadingDropDown-Extender wird zwischenrückruf dann ein Webdienst mit der folgenden Methodensignatur:</span><span class="sxs-lookup"><span data-stu-id="b52ad-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="b52ad-118">Die Methode gibt ein Array vom Typ CascadingDropDown-Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="b52ad-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="b52ad-119">Der Konstruktor des Typs erwartet, dass zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut).</span><span class="sxs-lookup"><span data-stu-id="b52ad-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="b52ad-120">Beim Laden der Seite im Browser füllt die Dropdown-Liste mit drei Anbietern zusammen, das zweite Argument wird standardmäßig ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="b52ad-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="b52ad-121">ASP.NET darüber hinaus definiert die `__doPostBack()` JavaScript-Methode.</span><span class="sxs-lookup"><span data-stu-id="b52ad-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="b52ad-122">Sobald die Seite geladen wurde, wird diese JavaScript-Aufruf der Dropdown-Liste, aber nur wenn sind Elemente darin hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b52ad-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="b52ad-123">Wenn in der Liste keine Elemente vorhanden sind, wird das Steuerelement-Toolkit derzeit, geladen und der JavaScript-Code verwendet einen Timeout in einer halben Sekunde erneut versucht.</span><span class="sxs-lookup"><span data-stu-id="b52ad-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="b52ad-124">Auf diese Weise wird ein Postback nur ausgeführt, wenn in der Liste tatsächlich Elemente vorhanden sind und der Benutzer einen Eintrag wählt.</span><span class="sxs-lookup"><span data-stu-id="b52ad-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="b52ad-125">[![Wählen ein Listenelement auslöst ein postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b52ad-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="b52ad-126">Wenn Sie ein Listenelement wird einen Postback ([klicken Sie, um das Bild in voller Größe anzeigen](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b52ad-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b52ad-127">Vorherige</span><span class="sxs-lookup"><span data-stu-id="b52ad-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
