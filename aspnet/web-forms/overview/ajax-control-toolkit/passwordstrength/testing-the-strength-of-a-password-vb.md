---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testen der Sicherheit eines Kennworts (VB) | Microsoft-Dokumentation
author: wenz
description: Kennwörter sind nahezu überall erforderlich, so, dass verzögerte Benutzer neigen dazu, einfache Kennwörter sind leicht zu entschlüsseln. Das Steuerelement, in der ASP-Steuerelements "PasswordStrength". N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 3faf9996c73fb5aaa427b515d396f36663cf1801
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379701"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="90b91-104">Testen der Sicherheit eines Kennworts (VB)</span><span class="sxs-lookup"><span data-stu-id="90b91-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="90b91-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="90b91-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="90b91-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="90b91-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="90b91-107">Kennwörter sind nahezu überall erforderlich, so, dass verzögerte Benutzer neigen dazu, einfache Kennwörter sind leicht zu entschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="90b91-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="90b91-108">Das Steuerelement "PasswordStrength" in ASP.NET AJAX Control Toolkit kann überprüfen, wie gut ein Kennwort ist.</span><span class="sxs-lookup"><span data-stu-id="90b91-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="90b91-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="90b91-109">Overview</span></span>

<span data-ttu-id="90b91-110">Kennwörter sind nahezu überall erforderlich, so, dass verzögerte Benutzer neigen dazu, einfache Kennwörter sind leicht zu entschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="90b91-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="90b91-111">Die `PasswordStrength` -Steuerelement in ASP.NET AJAX Control Toolkit kann überprüfen, wie gut ein Kennwort ist.</span><span class="sxs-lookup"><span data-stu-id="90b91-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="90b91-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="90b91-112">Steps</span></span>

<span data-ttu-id="90b91-113">Die `PasswordStrength` Steuerelement erweitert ein Textfeld, und überprüft, ob es sich bei das Kennwort an gut genug ist.</span><span class="sxs-lookup"><span data-stu-id="90b91-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="90b91-114">Er bietet eine Fülle von Optionen mithilfe von Attributen; Hier sind nur einige davon:</span><span class="sxs-lookup"><span data-stu-id="90b91-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="90b91-115">`MinimumNumericCharacters` minimale Anzahl der Zeichen im Kennwort erforderlich</span><span class="sxs-lookup"><span data-stu-id="90b91-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="90b91-116">`MinimumSymbolCharacters` minimale Anzahl von Symbolzeichen (nicht Buchstaben und Ziffern), die im Kennwort erforderlich</span><span class="sxs-lookup"><span data-stu-id="90b91-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="90b91-117">`PreferredPasswordLength` Mindestlänge des Kennworts</span><span class="sxs-lookup"><span data-stu-id="90b91-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="90b91-118">`RequiresUpperAndLowerCaseCharacters` Gibt an, ob das Kennwort muss Groß-und Kleinbuchstaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="90b91-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="90b91-119">Die `StrengthIndicatorType` stellt die Informationen bereit, wie Sie die Stärke des Kennworts verwendet, als Text vorhanden (Wert `"Text"`) oder als eine Art der Statusanzeige (Wert `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="90b91-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="90b91-120">In der `DisplayPosition` -Attribut, die Sie konfigurieren, in dem die Informationen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="90b91-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="90b91-121">Hier ist ein vollständiges Beispiel, einschließlich der ASP.NET AJAX `ScriptManager` -Steuerelement, das `PasswordStrength` -Steuerelement, und natürlich ein Textfeld, in denen der Benutzer ein Kennwort eingeben kann.</span><span class="sxs-lookup"><span data-stu-id="90b91-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="90b91-122">Die Zwecke dieser Demo ist das Feld für die letztgenannte Form ein normales Textfeld und nicht in ein Kennwortfeld, damit Sie während der Entwicklung sehen können, was Sie eingeben.</span><span class="sxs-lookup"><span data-stu-id="90b91-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="90b91-123">Führen Sie die Seite, und geben Sie den Weg: nur nach dem Sie Kleinbuchstaben, Großbuchstaben, Ziffern und Symbole eingegeben haben, das Kennwort als unbreakable angesehen wird.</span><span class="sxs-lookup"><span data-stu-id="90b91-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="90b91-124">[![Nachdem das Kennwort (Recht) gut ist.](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="90b91-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="90b91-125">Nachdem das Kennwort (Recht) gut ist ([klicken Sie, um das Bild in voller Größe anzeigen](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="90b91-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="90b91-126">Vorherige</span><span class="sxs-lookup"><span data-stu-id="90b91-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
