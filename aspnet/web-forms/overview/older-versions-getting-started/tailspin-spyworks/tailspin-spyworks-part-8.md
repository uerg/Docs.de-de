---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Teil 8: Letzte Seiten, Ausnahmebehandlung und Zusammenfassung | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 8 wird eine Kontakte-Seite zu Seite und die Ausnahme hinzugefügt...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3b49ee53e82933de9b50960779c28ca6ab7441e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833503"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="36fe8-104">Teil 8: Letzte Seiten, Ausnahmebehandlung und Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="36fe8-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="36fe8-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="36fe8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="36fe8-106">Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform.</span><span class="sxs-lookup"><span data-stu-id="36fe8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="36fe8-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="36fe8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="36fe8-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="36fe8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="36fe8-109">Teil 8 wird eine wenden Sie sich an-Seite zu Seite und die Ausnahmebehandlung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="36fe8-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="36fe8-110">Dies ist das Ende der Reihe.</span><span class="sxs-lookup"><span data-stu-id="36fe8-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="36fe8-111">Wenden Sie sich an die Seite (Senden von e-Mail von ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="36fe8-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="36fe8-112">Erstellen Sie eine neue Seite mit dem Namen ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="36fe8-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="36fe8-113">Mithilfe des Designers, erstellen Sie das folgende Format, die spezielle notieren, bestehend aus dem ToolkitScriptManager und der Editor-Steuerelement aus der AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="36fe8-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="36fe8-114">sein.</span><span class="sxs-lookup"><span data-stu-id="36fe8-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="36fe8-115">Doppelklicken Sie auf die Schaltfläche "Absenden", um einen Click-Ereignishandler in der CodeBehind-Datei generieren, und implementieren Sie eine Methode, um die Kontaktinformationen als eine e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="36fe8-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="36fe8-116">Dieser Code erfordert, dass die Datei "Web.config" einen Eintrag im Abschnitt "Konfiguration" enthalten, der angibt, den SMTP-Server zum Senden von e-Mail verwendet.</span><span class="sxs-lookup"><span data-stu-id="36fe8-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="36fe8-117">Zu den Seiten</span><span class="sxs-lookup"><span data-stu-id="36fe8-117">About Page</span></span>

<span data-ttu-id="36fe8-118">Erstellen Sie eine Seite mit dem Namen AboutUs.aspx, und fügen Sie den gewünschten Inhalt hinzu.</span><span class="sxs-lookup"><span data-stu-id="36fe8-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="36fe8-119">Globalen Ausnahmehandlers</span><span class="sxs-lookup"><span data-stu-id="36fe8-119">Global Exception Handler</span></span>

<span data-ttu-id="36fe8-120">Und schließlich in der gesamten Anwendung haben wir ausgelöste Ausnahmen aus, und es gibt unvorhergesehene Umstände, kalte auch Ursache, die nicht behandelte Ausnahmen in unserer Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="36fe8-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="36fe8-121">Wir möchten nicht, eine nicht behandelte Ausnahme, die ein Besucher der Website angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="36fe8-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="36fe8-122">Nicht behandelte Ausnahmen können auch ein Sicherheitsproblem sein, abgesehen davon, dass eine schlechte benutzererfahrung.</span><span class="sxs-lookup"><span data-stu-id="36fe8-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="36fe8-123">Wir werden ein globalen ausnahmehandlers implementieren, um dieses Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="36fe8-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="36fe8-124">Zu diesem Zweck öffnen Sie die Datei "Global.asax", und beachten Sie den folgenden vorab generierten Ereignishandler.</span><span class="sxs-lookup"><span data-stu-id="36fe8-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="36fe8-125">Hinzufügen von Code zum Implementieren der Anwendung\_Fehlerhandler wie folgt.</span><span class="sxs-lookup"><span data-stu-id="36fe8-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="36fe8-126">Klicken Sie dann eine Seite namens Error.aspx der Projektmappe, und fügen Sie diesem Markup-Codeausschnitt hinzu.</span><span class="sxs-lookup"><span data-stu-id="36fe8-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="36fe8-127">Jetzt auf der Seite\_Event Handler extrahieren die Fehlermeldungen aus dem Request-Objekt zu laden.</span><span class="sxs-lookup"><span data-stu-id="36fe8-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="36fe8-128">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="36fe8-128">Conclusion</span></span>

<span data-ttu-id="36fe8-129">Wir haben gesehen, dass ASP.NET WebForms erleichtert usw. mit Zugriff auf die Datenbank, Mitgliedschaft, AJAX, eine komplexe Website zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="36fe8-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="36fe8-130">sehr schnell.</span><span class="sxs-lookup"><span data-stu-id="36fe8-130">pretty quickly.</span></span>

<span data-ttu-id="36fe8-131">Hoffentlich hat in diesem Tutorial Ihnen die Tools gegeben, die Sie Ihren eigenen ASP.NET WebForms-Anwendungen zu entwickeln müssen!</span><span class="sxs-lookup"><span data-stu-id="36fe8-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="36fe8-132">Vorherige</span><span class="sxs-lookup"><span data-stu-id="36fe8-132">Previous</span></span>](tailspin-spyworks-part-7.md)
