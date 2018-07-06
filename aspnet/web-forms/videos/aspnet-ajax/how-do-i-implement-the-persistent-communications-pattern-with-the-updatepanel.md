---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel
title: '[Gewusst wie:] Implementieren des Musters für persistente Kommunikation mit dem UpdatePanel-Steuerelement? | Microsoft-Dokumentation'
author: JoeStagner
description: In einer herkömmlichen-Website im Browser und dem Server eine laufende Kommunikation nicht beibehalten, jedoch nur in der Antwort an den Benutzer, die Durchführung einer Aktion findet die Kommunikation...
ms.author: aspnetcontent
ms.date: 08/01/2007
ms.assetid: 49c7a74d-dce7-4d5c-8282-c7846f478e11
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel
msc.type: video
ms.openlocfilehash: 89dea5c2c44e8bdd9c12a127864428f681a66fb2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833147"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel"></a><span data-ttu-id="53144-104">[Gewusst wie:] Implementieren des Musters für persistente Kommunikation mit dem UpdatePanel-Steuerelement?</span><span class="sxs-lookup"><span data-stu-id="53144-104">[How Do I:] Implement the Persistent Communications Pattern with the UpdatePanel?</span></span>
====================
<span data-ttu-id="53144-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="53144-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="53144-106">In einer herkömmlichen-Website im Browser und dem Server eine laufende Kommunikation nicht beibehalten, aber nur in der Antwort an den Benutzer, die eine Aktion die Kommunikation.</span><span class="sxs-lookup"><span data-stu-id="53144-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="53144-107">In eine moderne Website, in dem die Seite einen Anwendungscontainer wird, kann es vorteilhaft sein für Browser und dem Server eine laufende Kommunikation verwaltet werden, sodass Seitenupdates auftreten können, ohne dass der Benutzer eine Aktion.</span><span class="sxs-lookup"><span data-stu-id="53144-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="53144-108">Dies wird als des Musters für persistente Kommunikation für AJAX bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="53144-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="53144-109">ASP.NET AJAX bietet zwei Arten für Webentwickler zum Implementieren des Musters für persistente Kommunikation.</span><span class="sxs-lookup"><span data-stu-id="53144-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="53144-110">Dieses Video zeigt die einfachste Möglichkeit, um das UpdatePanel von ASP.NET AJAX als Grundlage für die Implementierung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="53144-110">This video demonstrates the simple way, which is to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="53144-111">In einem später video lernen wir dasselbe Muster ohne die Verwendung von das UpdatePanel von ASP.NET AJAX zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="53144-111">In a later video we will learn how to implement the same pattern without the use of the ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="53144-112">&#9654;Sehen Sie sich Video (12 Minuten)</span><span class="sxs-lookup"><span data-stu-id="53144-112">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="53144-113">[Zurück](how-do-i-use-the-conditional-updatemode-of-the-updatepanel.md)
> [Weiter](how-do-i-localize-an-aspnet-ajax-application.md)</span><span class="sxs-lookup"><span data-stu-id="53144-113">[Previous](how-do-i-use-the-conditional-updatemode-of-the-updatepanel.md)
[Next](how-do-i-localize-an-aspnet-ajax-application.md)</span></span>
