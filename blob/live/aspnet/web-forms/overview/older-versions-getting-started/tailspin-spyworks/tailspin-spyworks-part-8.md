---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Teil 8: Endgültige Seiten, Ausnahmebehandlung und Abschluss | Microsoft Docs"
author: JoeStagner
description: "Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 8 Fügt eine wenden Sie sich an-Seite zu Seite und die Ausnahme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="fa2a9-104">Teil 8: Endgültige Seiten, Ausnahmebehandlung und Abschluss</span><span class="sxs-lookup"><span data-stu-id="fa2a9-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="fa2a9-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="fa2a9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="fa2a9-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="fa2a9-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="fa2a9-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="fa2a9-109">Teil 8 Fügt eine wenden Sie sich an-Seite zu Seite und die Ausnahmebehandlung.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="fa2a9-110">Dies ist das Ende der Reihe.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a><span data-ttu-id="fa2a9-111">Wenden Sie sich an die Seite (Senden von e-Mails von ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="fa2a9-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="fa2a9-112">Erstellen Sie eine neue Seite mit dem Namen ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="fa2a9-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="fa2a9-113">Mithilfe des Designers, erstellen Sie das folgende Format, die spezielle notieren, die ToolkitScriptManager und die Editor-Steuerelement aus der AjaxdControlToolkit aufgenommen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="fa2a9-114">.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="fa2a9-115">Doppelklicken Sie auf die Schaltfläche "Submit", um einen Click-Ereignishandler in der CodeBehind-Datei generieren und implementieren Sie eine Methode, um die Kontaktinformationen als eine e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="fa2a9-116">Dieser Code erfordert, dass die Datei "Web.config" einen Eintrag im Abschnitt "Konfiguration" enthalten, der angibt, den SMTP-Server zum Senden von e-Mail verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="fa2a9-117">Zu den Seiten</span><span class="sxs-lookup"><span data-stu-id="fa2a9-117">About Page</span></span>

<span data-ttu-id="fa2a9-118">Erstellen Sie eine Seite mit dem Namen AboutUs.aspx und fügen Sie beliebige Inhalte, die Ihnen gefällt.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="fa2a9-119">Globalen Ausnahmehandlers</span><span class="sxs-lookup"><span data-stu-id="fa2a9-119">Global Exception Handler</span></span>

<span data-ttu-id="fa2a9-120">Schließlich in der gesamten Anwendung haben wir Ausnahmen ausgelöst, und es sind unvorhergesehenen Umstände, kalte auch Ursache, die nicht behandelte Ausnahmen in der vorliegenden Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="fa2a9-121">Wir möchten nie eine nicht behandelte Ausnahme, die ein Besucher der Website angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="fa2a9-122">Nicht behandelte Ausnahmen können auch ein Sicherheitsproblem sein, abgesehen von einer schrecklichen Benutzeroberfläche wird.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="fa2a9-123">Um dieses Problem zu lösen implementieren wir ein globalen ausnahmehandlers.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="fa2a9-124">Zu diesem Zweck öffnen Sie die Datei "Global.asax" aus, und notieren Sie sich den folgenden vorgenerierten Ereignishandler.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="fa2a9-125">Fügen Sie Code zum Implementieren der Anwendung\_Fehlerhandler wie folgt.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="fa2a9-126">Anschließend fügen Sie eine Seite mit dem Namen Error.aspx der Projektmappe, und fügen Sie diesen markupcodeausschnitt hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="fa2a9-127">Jetzt auf der Seite\_Ereignishandler extrahieren die Fehlermeldungen aus dem Request-Objekt zu laden.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="fa2a9-128">Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="fa2a9-128">Conclusion</span></span>

<span data-ttu-id="fa2a9-129">Wir haben gesehen, dass diese ASP.NET Web Forms erleichtert usw. eine anspruchsvolle Website mit Datenbankzugriff, AJAX-Mitgliedschaft zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-129">We've seen that that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="fa2a9-130">ziemlich schnell.</span><span class="sxs-lookup"><span data-stu-id="fa2a9-130">pretty quickly.</span></span>

<span data-ttu-id="fa2a9-131">Hoffentlich hat in diesem Lernprogramm Ihnen die Tools gegeben, die Sie beginnen damit, eigene ASP.NET WebForms-Anwendungen erstellen müssen!</span><span class="sxs-lookup"><span data-stu-id="fa2a9-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="fa2a9-132">Zurück</span><span class="sxs-lookup"><span data-stu-id="fa2a9-132">Previous</span></span>](tailspin-spyworks-part-7.md)
