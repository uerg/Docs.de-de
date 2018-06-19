---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testen die Stärke von Kennwörtern (VB) | Microsoft Docs
author: wenz
description: Kennwörter sind nahezu überall erforderlich, sodass lazy Benutzer einfache Kennwörter auswählen, über den leicht zu unterbrechen, sind tendenziell. In der ASP-Steuerelements "PasswordStrength"-Steuerelement. N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879451"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="36696-104">Testen die Stärke von Kennwörtern (VB)</span><span class="sxs-lookup"><span data-stu-id="36696-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="36696-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="36696-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="36696-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="36696-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="36696-107">Kennwörter sind nahezu überall erforderlich, sodass lazy Benutzer einfache Kennwörter auswählen, über den leicht zu unterbrechen, sind tendenziell.</span><span class="sxs-lookup"><span data-stu-id="36696-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="36696-108">Das Steuerelement Steuerelements "PasswordStrength" in der ASP.NET AJAX-Steuerelement-Toolkit kann überprüfen, wie gut ein Kennwort ist.</span><span class="sxs-lookup"><span data-stu-id="36696-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="36696-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="36696-109">Overview</span></span>

<span data-ttu-id="36696-110">Kennwörter sind nahezu überall erforderlich, sodass lazy Benutzer einfache Kennwörter auswählen, über den leicht zu unterbrechen, sind tendenziell.</span><span class="sxs-lookup"><span data-stu-id="36696-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="36696-111">Die `PasswordStrength` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit kann überprüfen, wie gut ein Kennwort ist.</span><span class="sxs-lookup"><span data-stu-id="36696-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="36696-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="36696-112">Steps</span></span>

<span data-ttu-id="36696-113">Die `PasswordStrength` Steuerelement ein Textfeld, das erweitert und überprüft, ob das Kennwort in es ausreichend ist.</span><span class="sxs-lookup"><span data-stu-id="36696-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="36696-114">Es bietet eine Vielzahl von Optionen über Attribute. Hier sind nur einige von ihnen:</span><span class="sxs-lookup"><span data-stu-id="36696-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="36696-115">`MinimumNumericCharacters` minimale Anzahl von Zeichen im Kennwort erforderlich</span><span class="sxs-lookup"><span data-stu-id="36696-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="36696-116">`MinimumSymbolCharacters` minimale Anzahl von Symbolzeichen (nicht Buchstaben und Ziffern), die im Kennwort erforderlich</span><span class="sxs-lookup"><span data-stu-id="36696-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="36696-117">`PreferredPasswordLength` die Mindestlänge des Kennworts</span><span class="sxs-lookup"><span data-stu-id="36696-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="36696-118">`RequiresUpperAndLowerCaseCharacters` Gibt an, ob das Kennwort muss Großbuchstaben und Kleinbuchstaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="36696-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="36696-119">Die `StrengthIndicatorType` enthält die Informationen wie die Stärke der das Kennwort als Text vorhanden (Wert `"Text"`) oder als eine Art von Statusanzeige (Wert `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="36696-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="36696-120">In der `DisplayPosition` -Attribut, die Sie konfigurieren, in dem die Informationen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="36696-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="36696-121">Hier ist ein vollständiges Beispiel, einschließlich ASP.NET AJAX `ScriptManager` -Steuerelement, das `PasswordStrength` -Steuerelement und natürlich ein Textfeld, in dem der Benutzer ein Kennwort eingeben kann.</span><span class="sxs-lookup"><span data-stu-id="36696-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="36696-122">Aus Gründen der Demo ist letztere Formularfelds ein normales Textfeld und nicht um ein Kennwortfeld, sodass Sie während der Entwicklung sehen können, was Sie eingeben.</span><span class="sxs-lookup"><span data-stu-id="36696-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="36696-123">Führen Sie die Seite, und geben Sie sofort: nur nachdem Kleinbuchstaben, Großbuchstaben, Ziffern und Symbole eingegeben haben, das Kennwort als untrennbares wiederhergestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="36696-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="36696-124">[![Jetzt ist das Kennwort (relativ) gut](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="36696-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="36696-125">Jetzt das Kennwort (relativ) gut ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="36696-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="36696-126">Vorherige</span><span class="sxs-lookup"><span data-stu-id="36696-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
