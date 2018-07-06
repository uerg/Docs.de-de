---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Abwehren von Bots (VB) | Microsoft-Dokumentation
author: wenz
description: Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden. Das Steuerelement "nobot" in der ASP.NET AJAX-Con...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: e79a973f721c1feeddb00ecbf9d6a76786afb4bb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833442"
---
<a name="fighting-bots-vb"></a><span data-ttu-id="dfd61-104">Abwehren von Bots (VB)</span><span class="sxs-lookup"><span data-stu-id="dfd61-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="dfd61-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dfd61-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dfd61-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dfd61-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="dfd61-107">Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="dfd61-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="dfd61-108">Das Steuerelement "nobot" in der ASP.NET AJAX Control Toolkit können diese Bots zu bekämpfen.</span><span class="sxs-lookup"><span data-stu-id="dfd61-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="dfd61-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="dfd61-109">Overview</span></span>

<span data-ttu-id="dfd61-110">Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="dfd61-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="dfd61-111">Das Steuerelement "nobot" in der ASP.NET AJAX Control Toolkit können diese Bots zu bekämpfen.</span><span class="sxs-lookup"><span data-stu-id="dfd61-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="dfd61-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="dfd61-112">Steps</span></span>

<span data-ttu-id="dfd61-113">Ein gebräuchliches Verfahren zur Bots zunichte machen werden CAPTCHAs vollständig automatisierte öffentliche Turing-Test zu verwenden, um Computer und Menschen auseinander anzugeben.</span><span class="sxs-lookup"><span data-stu-id="dfd61-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="dfd61-114">Ein Turing-Test war ursprünglich ein Test eine Person, in denen entscheiden, ob ein Kommunikationspartner für eine Person oder ein Computer ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="dfd61-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="dfd61-115">Im Web besteht ein CAPTCHA eines Bildes mit einigen verzerrten Buchstaben darauf in der Regel aus.</span><span class="sxs-lookup"><span data-stu-id="dfd61-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="dfd61-116">Die Idee ist, dass nur einem Benutzer, der die Buchstaben auf dem Image lesen kann, während die OCR-Algorithmen fehl.</span><span class="sxs-lookup"><span data-stu-id="dfd61-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="dfd61-117">Es gibt verschiedene vor- und Nachteile dieses Ansatzes, aber eine genauere Beschreibung ist, würde den Rahmen dieses Tutorials.</span><span class="sxs-lookup"><span data-stu-id="dfd61-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="dfd61-118">Es ist jedoch ein Steuerelement in ASP.NET AJAX Control Toolkit bietet einen ähnlichen Ansatz: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="dfd61-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="dfd61-119">Es ist einfacher, als ein CAPTCHA zu überwinden und ist sehr einfach zu verwenden und umfassen extrem gut auf Websites wie Blogs, in denen gilt dies erfolgreich, wenn die meisten Versuche Spam, werden außer Kraft gesetzt, die die `NoBot` Steuerelement möglich.</span><span class="sxs-lookup"><span data-stu-id="dfd61-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="dfd61-120">`NoBot` fängt das Postback des aktuellen ASP.NET Web Form an, wenn mindestens eine der folgenden Bedingungen erfüllt ist:</span><span class="sxs-lookup"><span data-stu-id="dfd61-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="dfd61-121">Der Browser ein Fehler auftritt, ein JavaScript-Rätsel lösen (z. B. wenn JavaScript deaktiviert ist)</span><span class="sxs-lookup"><span data-stu-id="dfd61-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="dfd61-122">Der Benutzer das Formular, um schnell gesendet hat</span><span class="sxs-lookup"><span data-stu-id="dfd61-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="dfd61-123">Die Client-IP-Adresse übermittelt das Formular zu häufig in einem bestimmten Zeitraum an.</span><span class="sxs-lookup"><span data-stu-id="dfd61-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="dfd61-124">Um zu prüfen, ob diese Bedingungen, die `NoBot` Steuerelement erfordert diese Attribute (alle optional):</span><span class="sxs-lookup"><span data-stu-id="dfd61-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="dfd61-125">`ResponseMinimumDelaySeconds` minimale Anzahl von Sekunden zwischen postbacks</span><span class="sxs-lookup"><span data-stu-id="dfd61-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="dfd61-126">`CutoffWindowSeconds` die Länge des Zeitfensters, in dem Measures von Postbacks über eine IP-Adresse sind</span><span class="sxs-lookup"><span data-stu-id="dfd61-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="dfd61-127">`CutoffMaximumInstances` maximale Anzahl von Sekunden pro Zeitintervall</span><span class="sxs-lookup"><span data-stu-id="dfd61-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="dfd61-128">Die folgende Markup-Anforderungen, mindestens zwei Sekunden verstreichen zwischen Postbacks und dass es nur fünf Postbacks innerhalb eines Zeitraums von 30 Sekunden maximal:</span><span class="sxs-lookup"><span data-stu-id="dfd61-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="dfd61-129">Klicken Sie dann wie gewohnt stellen Sie sicher, dass die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="dfd61-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="dfd61-130">Da der Großteil der Überprüfungen `NoBot` Aktionen auftreten, auf dem Server, müssen Sie das Ergebnis des diese Überprüfungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="dfd61-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="dfd61-131">Dies ist möglich durch Aufrufen von `NoBot`des `IsValid()` Methode.</span><span class="sxs-lookup"><span data-stu-id="dfd61-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="dfd61-132">Es hat ein Argument (als ein `out` Parameter /`ByRef` Parameter) vom Typ `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="dfd61-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="dfd61-133">Die Zeichenfolgendarstellung enthält den Grund an, wenn die Überprüfung ein Fehler auftritt und `Valid` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="dfd61-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="dfd61-134">Der folgende Code gibt eine Meldung gemäß `NoBot`des führen:</span><span class="sxs-lookup"><span data-stu-id="dfd61-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="dfd61-135">Schließlich benötigen Sie ein Formular zum Senden und ein Label-Element die Meldung ausgegeben, und Sie sind fertig!</span><span class="sxs-lookup"><span data-stu-id="dfd61-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="dfd61-136">Wenn Sie dieses Skript ausführen und Deaktivieren von JavaScript oder senden Sie das Formular in den ersten zwei Sekunden oder senden Sie das Formular sieben Mal innerhalb von 30 Sekunden, erhalten Sie eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dfd61-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="dfd61-137">Verwenden Sie dieses Steuerelement jedoch bedacht, da nur ungefähr 90 bis 95 % der Benutzer über die JavaScript-aktivierte verfügen, daher 5 bis 10 % der Benutzer nicht `NoBot`des testen.</span><span class="sxs-lookup"><span data-stu-id="dfd61-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="dfd61-138">[![Diese Fehlermeldung kann durch einen Bot verursacht werden](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dfd61-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="dfd61-139">Diese Fehlermeldung kann auftreten, durch einen Bot ([klicken Sie, um das Bild in voller Größe anzeigen](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dfd61-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dfd61-140">Vorherige</span><span class="sxs-lookup"><span data-stu-id="dfd61-140">Previous</span></span>](fighting-bots-cs.md)
