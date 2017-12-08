---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Verwenden von Auto-Postback mit CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: "Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f8059f44b4efbf59ebe7b3d2fd5400e886642a90
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="06c86-103">Verwenden von Auto-Postback mit CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="06c86-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="06c86-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="06c86-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="06c86-105">[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="06c86-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="06c86-106">Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="06c86-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="06c86-107">Jedoch bei der CascadingDropDown-Steuerelement ASP verwenden. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da eine (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden.</span><span class="sxs-lookup"><span data-stu-id="06c86-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="06c86-108">Dieser Effekt kann vermieden werden, durch einige JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="06c86-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="06c86-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="06c86-109">Overview</span></span>

<span data-ttu-id="06c86-110">Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="06c86-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="06c86-111">(Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Jedoch bei der CascadingDropDown-Steuerelement ASP verwenden. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da eine (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden.</span><span class="sxs-lookup"><span data-stu-id="06c86-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="06c86-112">Dieser Effekt kann vermieden werden, durch einige JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="06c86-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="06c86-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="06c86-113">Steps</span></span>

<span data-ttu-id="06c86-114">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der &lt; `form` &gt; Element):</span><span class="sxs-lookup"><span data-stu-id="06c86-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="06c86-115">Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="06c86-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="06c86-116">Für diese Liste wird ein CascadingDropDown Extender Bereitstellung von Web-Dienst-URL und -Methode hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="06c86-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="06c86-117">Der Extender CascadingDropDown ruft dann asynchron einen Webdienst mit der folgenden Methodensignatur:</span><span class="sxs-lookup"><span data-stu-id="06c86-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="06c86-118">Die Methode gibt ein Array vom Typ CascadingDropDown-Wert.</span><span class="sxs-lookup"><span data-stu-id="06c86-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="06c86-119">Typkonstruktor erwartet zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut).</span><span class="sxs-lookup"><span data-stu-id="06c86-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="06c86-120">Beim Laden der Seite im Browser füllt die Dropdown-Liste mit den drei Anbietern dem zweiten Ausdruck wird vorab ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="06c86-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="06c86-121">ASP.NET definiert außerdem die `__doPostBack()` JavaScript-Methode.</span><span class="sxs-lookup"><span data-stu-id="06c86-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="06c86-122">Sobald die Seite geladen wurde, wird dieser JavaScript-Funktionsaufruf der Dropdownliste aus, nur wenn jedoch auch noch Elemente darin hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="06c86-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="06c86-123">Wenn in der Liste keine Elemente vorhanden sind, ist die Control-Toolkit derzeit, von lädt, sodass die JavaScript-Code einen Timeout verwendet und in einer halben Sekunde erneut versucht.</span><span class="sxs-lookup"><span data-stu-id="06c86-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="06c86-124">Auf diese Weise wird ein Postback nur ausgeführt, wenn in der Liste tatsächlich Elemente vorhanden sind und der Benutzer einen Eintrag wählt.</span><span class="sxs-lookup"><span data-stu-id="06c86-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="06c86-125">[![Wenn Sie ein Listenelement aktivieren, wird einen postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06c86-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="06c86-126">Wenn Sie ein Listenelement aktivieren, wird einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06c86-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="06c86-127">Zurück</span><span class="sxs-lookup"><span data-stu-id="06c86-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
