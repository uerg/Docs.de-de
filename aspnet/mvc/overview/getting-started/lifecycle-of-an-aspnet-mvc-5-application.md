---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Lebenszyklus einer ASP.NET MVC 5-Anwendung | Microsoft Docs
author: cephalin
description: Laden Sie ein PDF-Dokument, das den Lebenszyklus einer ASP.NET MVC 5-Anwendung Diagramme. Dieses Lebenszyklus-Dokument enthält eine allgemeine Ansicht des MVC-Lebenszyklus einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036491"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="ade9d-104">Lebenszyklus einer ASP.NET MVC 5-Anwendung</span><span class="sxs-lookup"><span data-stu-id="ade9d-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="ade9d-105">durch [Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="ade9d-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="ade9d-106">Herunterladen von PDF-Dokument</span><span class="sxs-lookup"><span data-stu-id="ade9d-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="ade9d-107">Hier können Sie ein PDF-Dokument herunterladen, die Diagramme, die der Lebenszyklus von jeder ASP.NET MVC 5-Anwendung empfängt die HTTP-Anforderung an die HTTP-Antwort senden an den Client zurück.</span><span class="sxs-lookup"><span data-stu-id="ade9d-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="ade9d-108">Es dient sowohl als Bildungseinrichtung Tool für diejenigen, die in ASP.NET MVC vertraut sind, als auch als Referenz für die Benutzer einen Drilldown in bestimmte Aspekte der Anwendung ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="ade9d-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="ade9d-109">Das PDF-Dokument hat die folgenden Funktionen:</span><span class="sxs-lookup"><span data-stu-id="ade9d-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="ade9d-110">Relevante ["HttpApplication"](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) Stufen, um besser zu verstehen, in denen mit MVC integriert ist die [ASP.NET Anwendungslebenszyklus](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="ade9d-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="ade9d-111">Einen allgemeinen Überblick des Lebenszyklus der MVC-Anwendung, das, in dem Sie die wichtigen Phasen verstehen können, die jeder MVC-Anwendung in der Anforderung Verarbeitungspipeline durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="ade9d-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="ade9d-112">Eine Detailansicht, die einen Drilldown nach unten in die Details der Anforderung Verarbeitungspipeline anzeigt.</span><span class="sxs-lookup"><span data-stu-id="ade9d-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="ade9d-113">Sie können allgemeine Ansicht und der Detailansicht, um festzustellen, wie die Lebenszyklen Details gesammelt werden, in den verschiedenen Phasen vergleichen.</span><span class="sxs-lookup"><span data-stu-id="ade9d-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="ade9d-114">[PDF herunterladen](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) um eine größere Ansicht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ade9d-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="ade9d-115">Platzierung und Zweck der allen überschreibbaren Methoden auf die [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) Objekt in die Pipeline zur anforderungsverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="ade9d-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="ade9d-116">Kann oder möglicherweise nicht erforderlich, alle eine Methode zu überschreiben, aber es ist wichtig, dass Sie ihre Rolle im Lebenszyklus Anwendung verstehen, damit Sie in der entsprechenden Lebenszyklusphase Effekt Code schreiben können, die Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="ade9d-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="ade9d-117">Vergrößert-nach-oben-Diagramme anzeigen, wie alle Filtertypen (Authentifizierung, Autorisierung, Aktion und Ergebnis) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ade9d-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="ade9d-118">Link zu einer nützlichen Artikel oder Blog von jeder Punkt in der Detailansicht von Interesse.</span><span class="sxs-lookup"><span data-stu-id="ade9d-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ade9d-119">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ade9d-119">Next Steps</span></span>

<span data-ttu-id="ade9d-120">Kennengelernt dieses Dokument Ihren Bedürfnissen?</span><span class="sxs-lookup"><span data-stu-id="ade9d-120">Does this document meet your need?</span></span> <span data-ttu-id="ade9d-121">Wir würden Ihr Feedback freuen.</span><span class="sxs-lookup"><span data-stu-id="ade9d-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="ade9d-122">Wenn Sie Frage des ASP.NET MVC-Lebenszyklus in Ihrer Anwendung haben [Stackoverflow](http://stackoverflow.com/help) und [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) eignen sich hervorragend, um Unterstützung bitten.</span><span class="sxs-lookup"><span data-stu-id="ade9d-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="ade9d-123">Führen Sie die [me](https://twitter.com/Cephas_MSFT) auf Twitter, sodass Sie Updates auf meine aktuelle Lernprogramme abrufen können.</span><span class="sxs-lookup"><span data-stu-id="ade9d-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
