---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Zulassen von nur bestimmten Zeichen in einem Textfeld (c#) | Microsoft-Dokumentation
author: wenz
description: ASP.NET-Steuerelemente zur gültigkeitsprüfung können stellen Sie sicher, dass nur bestimmten Zeichen in einer Benutzereingabe zulässig sind. Jedoch ist dies immer noch nicht über die Eingabe ungültig, dass Benutzer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7203d7f367f275d2d80c86119edc9645c9d24c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400729"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="94026-104">Zulassen von nur bestimmten Zeichen in einem Textfeld (c#)</span><span class="sxs-lookup"><span data-stu-id="94026-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="94026-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="94026-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="94026-106">[Code herunterladen](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="94026-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="94026-107">ASP.NET-Steuerelemente zur gültigkeitsprüfung können stellen Sie sicher, dass nur bestimmten Zeichen in einer Benutzereingabe zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="94026-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="94026-108">Aber dies immer noch Benutzer ungültige Zeichen eingeben, und versuchen, die zum Senden des Formulars verhindert nicht.</span><span class="sxs-lookup"><span data-stu-id="94026-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="94026-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="94026-109">Overview</span></span>

<span data-ttu-id="94026-110">ASP.NET-Steuerelemente zur gültigkeitsprüfung können stellen Sie sicher, dass nur bestimmten Zeichen in einer Benutzereingabe zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="94026-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="94026-111">Aber dies immer noch Benutzer ungültige Zeichen eingeben, und versuchen, die zum Senden des Formulars verhindert nicht.</span><span class="sxs-lookup"><span data-stu-id="94026-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="94026-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="94026-112">Steps</span></span>

<span data-ttu-id="94026-113">Das ASP.NET AJAX Control Toolkit enthält die `FilteredTextBox` steuern, welche in einem Textfeld, das erweitert.</span><span class="sxs-lookup"><span data-stu-id="94026-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="94026-114">Nach der Aktivierung kann nur ein bestimmten Satz von Zeichen in das Feld eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="94026-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="94026-115">Damit dies funktioniert, wir zunächst wie gewohnt das ASP.NET AJAX `ScriptManager` lädt die JavaScript-Bibliotheken, die auch von der ASP.NET AJAX Control Toolkit verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="94026-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="94026-116">Klicken Sie dann benötigen wir ein Textfeld:</span><span class="sxs-lookup"><span data-stu-id="94026-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="94026-117">Zum Schluss die `FilteredTextBoxExtender` Steuerelement übernimmt beschränken, die Zeichen, die Benutzer in den Typ.</span><span class="sxs-lookup"><span data-stu-id="94026-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="94026-118">Legen Sie zuerst die `TargetControlID` -Attribut auf die `ID` von der `TextBox` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="94026-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="94026-119">Wählen Sie eine der verfügbaren `FilterType` Werte:</span><span class="sxs-lookup"><span data-stu-id="94026-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="94026-120">`Custom` Standardwert Sie müssen eine Liste der gültigen Zeichen angeben.</span><span class="sxs-lookup"><span data-stu-id="94026-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="94026-121">`LowercaseLetters` nur Kleinbuchstaben</span><span class="sxs-lookup"><span data-stu-id="94026-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="94026-122">`Numbers` nur Ziffern</span><span class="sxs-lookup"><span data-stu-id="94026-122">`Numbers` digits only</span></span>
- <span data-ttu-id="94026-123">`UppercaseLetters` nur Großbuchstaben</span><span class="sxs-lookup"><span data-stu-id="94026-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="94026-124">Wenn die `Custom FilterType` verwendet wird, die `ValidChars` Eigenschaft muss festgelegt werden, und geben Sie eine Liste von Zeichen, die eingegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="94026-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="94026-125">Übrigens: Wenn Sie versuchen, Text in das Textfeld einzufügen, werden alle ungültige Zeichen entfernt.</span><span class="sxs-lookup"><span data-stu-id="94026-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="94026-126">Dies ist das Markup für die `FilteredTextBoxExtender` -Steuerelement, das nur die Ziffern kann (etwas, das auch mit möglich gewesen wäre `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="94026-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="94026-127">Führen die Seite, und es wurde versucht, einen Buchstaben eingeben, falls JavaScript aktiviert ist, funktioniert es nicht. Ziffern werden jedoch auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="94026-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="94026-128">Beachten Sie jedoch, dass der Schutz `FilteredTextBox` bietet ist nicht hundertprozentig: Wenn JavaScript aktiviert ist, müssen Sie zusätzliche Überprüfungen, z. B. ASP Verwendung, können alle Daten in das Textfeld eingegeben werden. NET Steuerelementen zur gültigkeitsprüfung.</span><span class="sxs-lookup"><span data-stu-id="94026-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="94026-129">[![Es können nur Ziffern eingegeben werden](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="94026-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="94026-130">Es können nur Ziffern eingegeben werden ([klicken Sie, um das Bild in voller Größe anzeigen](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="94026-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="94026-131">Nächste</span><span class="sxs-lookup"><span data-stu-id="94026-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
