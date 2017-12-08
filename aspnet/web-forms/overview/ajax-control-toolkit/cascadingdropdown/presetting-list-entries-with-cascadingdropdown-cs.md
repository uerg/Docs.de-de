---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: "Vorheriges festlegen Listeneinträge mit CascadingDropDown (c#) | Microsoft Docs"
author: wenz
description: "Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d86c5887a8b175a60c32a0970482266b7d690162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="bcbab-103">Vorheriges festlegen Listeneinträge mit CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="bcbab-103">Presetting List Entries with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="bcbab-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bcbab-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bcbab-105">[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bcbab-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="bcbab-106">Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bcbab-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="bcbab-107">Mit ein wenig Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="bcbab-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="bcbab-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bcbab-108">Overview</span></span>

<span data-ttu-id="bcbab-109">Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bcbab-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="bcbab-110">(Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Mit ein wenig Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="bcbab-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="bcbab-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="bcbab-111">Steps</span></span>

<span data-ttu-id="bcbab-112">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="bcbab-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="bcbab-113">Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="bcbab-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="bcbab-114">Für diese Liste wird ein CascadingDropDown Extender Bereitstellung von Web-Dienst-URL und -Methode hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="bcbab-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="bcbab-115">Der Extender CascadingDropDown ruft dann asynchron einen Webdienst mit der folgenden Methodensignatur:</span><span class="sxs-lookup"><span data-stu-id="bcbab-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="bcbab-116">Die Methode gibt ein Array vom Typ CascadingDropDown-Wert.</span><span class="sxs-lookup"><span data-stu-id="bcbab-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="bcbab-117">Typkonstruktor erwartet zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut).</span><span class="sxs-lookup"><span data-stu-id="bcbab-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="bcbab-118">Wenn das dritte Argument festgelegt ist auf "true", wird die Liste Element automatisch im Browser ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="bcbab-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="bcbab-119">Beim Laden der Seite im Browser füllt die Dropdown-Liste mit den drei Anbietern dem zweiten Ausdruck wird vorab ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="bcbab-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="bcbab-120">[![Die Liste gefüllt und standardmäßig automatisch ausgewählt.](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bcbab-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="bcbab-121">Die Liste aufgefüllt und automatisch vorausgewählt ([klicken Sie hier, um das Bild in voller Größe angezeigt](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bcbab-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bcbab-122">[Zurück](using-cascadingdropdown-with-a-database-cs.md)
[Weiter](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bcbab-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
