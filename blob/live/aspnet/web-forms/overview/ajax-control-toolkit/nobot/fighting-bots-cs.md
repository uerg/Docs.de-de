---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Abwehren von Bots (c#) | Microsoft Docs
author: wenz
description: Automatisierte Bots Stuck Blogs und anderen Websites mit Spam, Kommentar Formen ohne Eingreifen des Benutzers senden. Das NoBot-Steuerelement in der ASP.NET AJAX-Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: b8eedff4691c1115e242be884f9e74663dc0b4f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-c"></a><span data-ttu-id="d1ce5-104">Abwehren von Bots (c#)</span><span class="sxs-lookup"><span data-stu-id="d1ce5-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="d1ce5-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1ce5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1ce5-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1ce5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="d1ce5-107">Automatisierte Bots Stuck Blogs und anderen Websites mit Spam, Kommentar Formen ohne Eingreifen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="d1ce5-108">Das NoBot-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit können diese Bots bekämpfen.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="d1ce5-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="d1ce5-109">Overview</span></span>

<span data-ttu-id="d1ce5-110">Automatisierte Bots Stuck Blogs und anderen Websites mit Spam, Kommentar Formen ohne Eingreifen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="d1ce5-111">Das NoBot-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit können diese Bots bekämpfen.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="d1ce5-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="d1ce5-112">Steps</span></span>

<span data-ttu-id="d1ce5-113">Ein gebräuchliches Verfahren zur Bots zunichte machen werden CAPTCHAs vollständig automatisierte öffentliche Turing Test verwenden, um Computer und Menschen auseinander.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="d1ce5-114">Ein Test Turing wurde ursprünglich einen Test eine Person, zu entscheiden, ob ein Kommunikationspartner ein menschlicher oder ein Computer ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="d1ce5-115">Im Web besteht ein CAPTCHA eines Bildes mit einigen verzerrten Buchstaben darauf in der Regel aus.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="d1ce5-116">Die Idee dabei ist, dass nur ein Benutzer die Buchstaben für das Abbild lesen kann, während OCR Algorithmen fehl.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="d1ce5-117">Es gibt mehrere vor- und Nachteile dieser Vorgehensweise, aber eine Erläuterung dieser sprengt des Rahmen dieses lehrprogramms sprengen.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="d1ce5-118">Es ist jedoch ein Steuerelement in ASP.NET AJAX Control Toolkit, die einen ähnlichen Ansatz bereitstellt: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="d1ce5-119">Es ist einfacher, als ein CAPTCHA zu umgehen, aber ist sehr einfach zu verwenden und Flugpreise sehr gut auf Websites wie Blogs, in denen es Erfolg berücksichtigt ist, wenn die meisten Versuche Spam, sind wird außer Kraft gesetzt, die die `NoBot` Steuerelement möglich.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="d1ce5-120">`NoBot`fängt das Postback von der aktuellen ASP.NET Web Form ab, wenn mindestens eine der folgenden Bedingungen erfüllt ist:</span><span class="sxs-lookup"><span data-stu-id="d1ce5-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="d1ce5-121">Der Browser eine JavaScript-Rätsel lösen fehlschlägt (z. B. wenn JavaScript deaktiviert ist)</span><span class="sxs-lookup"><span data-stu-id="d1ce5-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="d1ce5-122">Der Benutzer übermittelt das Formular, um schnelle</span><span class="sxs-lookup"><span data-stu-id="d1ce5-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="d1ce5-123">Die Client-IP-Adresse übermittelt das Formular zu häufig in einem bestimmten Zeitraum an.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="d1ce5-124">Um diese Bedingungen zu überprüfen der `NoBot` Steuerelement erfordert diese Attribute (alle von ihnen optional):</span><span class="sxs-lookup"><span data-stu-id="d1ce5-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="d1ce5-125">`ResponseMinimumDelaySeconds`minimale Anzahl der Sekunden zwischen postbacks</span><span class="sxs-lookup"><span data-stu-id="d1ce5-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="d1ce5-126">`CutoffWindowSeconds`die Länge des Zeitintervalls, in denen Postbacks aus einem IP-Measures sind</span><span class="sxs-lookup"><span data-stu-id="d1ce5-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="d1ce5-127">`CutoffMaximumInstances`maximale Anzahl der Sekunden pro Zeitintervall</span><span class="sxs-lookup"><span data-stu-id="d1ce5-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="d1ce5-128">Das folgende Markup Forderungen, mindestens zwei Sekunden verstreichen zwischen Postbacks und es sind nur fünf Postbacks oder weniger in einem Intervall von 30 Sekunden:</span><span class="sxs-lookup"><span data-stu-id="d1ce5-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="d1ce5-129">Klicken Sie dann wie üblich stellen Sie sicherstellen, dass die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="d1ce5-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="d1ce5-130">Da die meisten der Überprüfungen `NoBot` ist jedoch auf der Serverseite auftreten, müssen Sie das Ergebnis von diese Überprüfungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="d1ce5-131">Dies kann geschehen, indem Aufrufen `NoBot`des `IsValid()` Methode.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="d1ce5-132">Es wurde ein Argument (wie ein `out` Parameter /`ByRef` Parameter) vom Typ `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="d1ce5-133">Wenn die Überprüfung fehlschlägt, enthält seine Zeichenfolgendarstellung die Ursache und `Valid` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="d1ce5-134">Der folgende Code gibt eine Nachricht entsprechend dem `NoBot`des führen:</span><span class="sxs-lookup"><span data-stu-id="d1ce5-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="d1ce5-135">Schließlich benötigen Sie ein Formular zum Senden und ein Label-Element, um die Meldung ausgegeben, und Sie sind fertig!</span><span class="sxs-lookup"><span data-stu-id="d1ce5-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="d1ce5-136">Wenn Sie dieses Skript ausführen, und Deaktivieren von JavaScript oder des Formulars innerhalb der ersten zwei Sekunden senden oder des Formulars sieben Mal innerhalb von 30 Sekunden senden, erhalten Sie eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="d1ce5-137">Verwenden Sie dieses Steuerelement jedoch mit Bedacht, da JavaScript aktiviert haben, nur ca. 90-95 % der Benutzer, daher 5 bis 10 % der Benutzer fehl `NoBot`des testen.</span><span class="sxs-lookup"><span data-stu-id="d1ce5-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="d1ce5-138">[![Diese Fehlermeldung kann durch einen Bot wurde vorliegt](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1ce5-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="d1ce5-139">Diese Fehlermeldung kann durch einen Bot verursacht worden sein ([klicken Sie hier, um das Bild in voller Größe angezeigt](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1ce5-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d1ce5-140">Nächste</span><span class="sxs-lookup"><span data-stu-id="d1ce5-140">Next</span></span>](fighting-bots-vb.md)
