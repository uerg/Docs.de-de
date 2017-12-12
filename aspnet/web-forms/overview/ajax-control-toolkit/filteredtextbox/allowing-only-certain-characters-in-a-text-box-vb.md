---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "Ermöglicht nur bestimmte Zeichen in einem Textfeld (VB) | Microsoft Docs"
author: wenz
description: "ASP.NET-Validierungssteuerelementen können sicherstellen, dass nur bestimmte Zeichen im Benutzereingaben zulässig sind. Jedoch ist dies immer noch nicht eingeben ungültige verhindern..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="947d9-104">Ermöglicht nur bestimmte Zeichen in einem Textfeld (VB)</span><span class="sxs-lookup"><span data-stu-id="947d9-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="947d9-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="947d9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="947d9-106">[Herunterladen von Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="947d9-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="947d9-107">ASP.NET-Validierungssteuerelementen können sicherstellen, dass nur bestimmte Zeichen im Benutzereingaben zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="947d9-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="947d9-108">Jedoch ist dies noch nicht ungültige Zeichen eingeben, und versuchen Sie zum Senden des Formulars verhindern.</span><span class="sxs-lookup"><span data-stu-id="947d9-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="947d9-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="947d9-109">Overview</span></span>

<span data-ttu-id="947d9-110">ASP.NET-Validierungssteuerelementen können sicherstellen, dass nur bestimmte Zeichen im Benutzereingaben zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="947d9-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="947d9-111">Jedoch ist dies noch nicht ungültige Zeichen eingeben, und versuchen Sie zum Senden des Formulars verhindern.</span><span class="sxs-lookup"><span data-stu-id="947d9-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="947d9-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="947d9-112">Steps</span></span>

<span data-ttu-id="947d9-113">Das ASP.NET AJAX-Steuerelement-Toolkit enthält die `FilteredTextBox` steuern, welche ein Textfeld, das erweitert.</span><span class="sxs-lookup"><span data-stu-id="947d9-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="947d9-114">Nach der Aktivierung kann nur ein bestimmten Satz von Zeichen in das Feld eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="947d9-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="947d9-115">Damit dies funktioniert, wird zunächst wie gewohnt ASP.NET AJAX `ScriptManager` lädt die JavaScript-Bibliotheken, die auch von ASP.NET AJAX-Steuerelement-Toolkit verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="947d9-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="947d9-116">Klicken Sie dann, benötigen wir ein Textfeld:</span><span class="sxs-lookup"><span data-stu-id="947d9-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="947d9-117">Schließlich die `FilteredTextBoxExtender` übernimmt die Kontrolle einschränken, die Zeichen, die der Benutzer berechtigt ist, auf den Typ.</span><span class="sxs-lookup"><span data-stu-id="947d9-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="947d9-118">Legen Sie zuerst die `TargetControlID` -Attribut auf die `ID` von der `TextBox` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="947d9-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="947d9-119">Wählen Sie dann eine der verfügbaren `FilterType` Werte:</span><span class="sxs-lookup"><span data-stu-id="947d9-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="947d9-120">`Custom`Standardeinstellung; Sie müssen eine Liste der gültigen Zeichen angeben.</span><span class="sxs-lookup"><span data-stu-id="947d9-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="947d9-121">`LowercaseLetters`dazu nur Kleinbuchstaben</span><span class="sxs-lookup"><span data-stu-id="947d9-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="947d9-122">`Numbers`nur Ziffern</span><span class="sxs-lookup"><span data-stu-id="947d9-122">`Numbers` digits only</span></span>
- <span data-ttu-id="947d9-123">`UppercaseLetters`nur Großbuchstaben.</span><span class="sxs-lookup"><span data-stu-id="947d9-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="947d9-124">Wenn die `Custom FilterType` verwendet wird, die `ValidChars` Eigenschaft muss festgelegt werden, und geben Sie eine Liste von Zeichen, die eingegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="947d9-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="947d9-125">Übrigens: Wenn Sie versuchen, den Text in das Textfeld einfügen, werden alle ungültige Zeichen entfernt.</span><span class="sxs-lookup"><span data-stu-id="947d9-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="947d9-126">Hier wird das Markup für die `FilteredTextBoxExtender` Steuerelement, das nur Ziffern zulässt (etwas, das auch mit möglich gewesen wäre `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="947d9-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="947d9-127">Führen Sie die Seite, und es wurde versucht, einen Buchstaben eingeben, wenn JavaScript aktiviert ist, können sie nicht verwendet werden; Ziffern werden jedoch auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="947d9-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="947d9-128">Beachten Sie jedoch, dass der Schutz `FilteredTextBox` bietet ist nicht sichere: Falls JavaScript aktiviert ist, müssen Sie zusätzliche Validierung bedeutet, d. h. ASP verwenden, können keine Daten in das Textfeld eingegeben werden. NET Validierungssteuerelemente.</span><span class="sxs-lookup"><span data-stu-id="947d9-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="947d9-129">[![Es können nur Ziffern eingegeben werden](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="947d9-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="947d9-130">Es können nur Ziffern eingegeben werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="947d9-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="947d9-131">Zurück</span><span class="sxs-lookup"><span data-stu-id="947d9-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
