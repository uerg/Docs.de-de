---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC-Ansichten (Übersicht) (VB) | Microsoft Docs
author: StephenWalther
description: Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet sie von einer HTML-Seite? In diesem Lernprogramm Stephen Walther Ansichten erläutert und veranschaulicht, wie können Sie t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870855"
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="587bf-104">ASP.NET MVC-Ansichten (Übersicht) (VB)</span><span class="sxs-lookup"><span data-stu-id="587bf-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="587bf-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="587bf-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="587bf-106">Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet sie von einer HTML-Seite?</span><span class="sxs-lookup"><span data-stu-id="587bf-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="587bf-107">In diesem Lernprogramm Stephen Walther Ansichten erläutert und veranschaulicht, wie Sie Daten anzeigen und HTML-Hilfsmethoden innerhalb einer Ansicht nutzen können.</span><span class="sxs-lookup"><span data-stu-id="587bf-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="587bf-108">Der Zweck dieses Lernprogramms werden Sie eine kurze Einführung in ASP.NET MVC-Ansichten, Anzeigen von Daten und HTML-Hilfsmethoden zur Verfügung stellen.</span><span class="sxs-lookup"><span data-stu-id="587bf-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="587bf-109">Am Ende dieses Lernprogramms sollten Sie verstehen, wie neue Ansichten erstellen, übergeben von Daten von einem Domänencontroller zu einer Ansicht und Verwenden von HTML-Hilfsmethoden zum Generieren des Inhalts in einer Ansicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="587bf-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="587bf-110">Grundlegendes zu Sichten</span><span class="sxs-lookup"><span data-stu-id="587bf-110">Understanding Views</span></span>

<span data-ttu-id="587bf-111">Im Gegensatz zu ASP.NET oder Active Server Pages schließt ASP.NET MVC nicht nichts ein, die direkt auf eine Seite entspricht.</span><span class="sxs-lookup"><span data-stu-id="587bf-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="587bf-112">In einer ASP.NET MVC-Anwendung besteht keine Seite auf dem Datenträger, der den Pfad in der URL entspricht, die Sie in der Adressleiste des Browsers eingeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="587bf-113">Am ehesten auf eine Seite in einer ASP.NET MVC-Anwendung ist etwas bezeichnet eine *Ansicht*.</span><span class="sxs-lookup"><span data-stu-id="587bf-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="587bf-114">In einer ASP.NET MVC-Anwendung werden eingehende Browseranforderungen Controlleraktionen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="587bf-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="587bf-115">Eine Controlleraktion könnte eine Sicht zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-115">A controller action might return a view.</span></span> <span data-ttu-id="587bf-116">Eine Controlleraktion führen jedoch möglicherweise aus einen anderen Typ der Aktion, z. B. umleiten Sie zu einem anderen Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="587bf-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="587bf-117">Auflisten von 1 enthält einen einfachen Controller mit dem Namen der HomeController.</span><span class="sxs-lookup"><span data-stu-id="587bf-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="587bf-118">HomeController macht zwei benannte Index() und Details() Controlleraktionen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="587bf-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="587bf-119">**1 – HomeController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="587bf-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="587bf-120">Sie können die erste Aktion, die Aktion Index() aufrufen, indem Sie die folgende URL in der Adressleiste des Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="587bf-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="587bf-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="587bf-121">/Home/Index</span></span>

<span data-ttu-id="587bf-122">Sie können die zweite Aktion, die Aktion Details() aufrufen, indem Sie diese Adresse in Ihren Browser eingeben:</span><span class="sxs-lookup"><span data-stu-id="587bf-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="587bf-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="587bf-123">/Home/Details</span></span>

<span data-ttu-id="587bf-124">Die Aktion Index() gibt eine Ansicht zurück.</span><span class="sxs-lookup"><span data-stu-id="587bf-124">The Index() action returns a view.</span></span> <span data-ttu-id="587bf-125">Die meisten Aktionen, die Sie erstellen werden Sichten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-125">Most actions that you create will return views.</span></span> <span data-ttu-id="587bf-126">Sie können jedoch eine Aktion andere Arten von Aktionsergebnisse zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="587bf-127">Die Details()-Aktion gibt z. B. eine RedirectToActionResult, die eingehende Anforderung an die Aktion Index() umleitet.</span><span class="sxs-lookup"><span data-stu-id="587bf-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="587bf-128">Die Aktion Index() enthält die folgende Codezeile:</span><span class="sxs-lookup"><span data-stu-id="587bf-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="587bf-129">View()</span><span class="sxs-lookup"><span data-stu-id="587bf-129">View()</span></span>

<span data-ttu-id="587bf-130">Diese Codezeile gibt eine Sicht, die sich unter folgendem Pfad auf dem Webserver sein müssen:</span><span class="sxs-lookup"><span data-stu-id="587bf-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="587bf-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="587bf-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="587bf-132">Der Pfad zur Ansicht ist von den Namen des Controllers und der Name der Controlleraktion abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="587bf-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="587bf-133">Falls gewünscht, können Sie zur Ansicht explizit sein.</span><span class="sxs-lookup"><span data-stu-id="587bf-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="587bf-134">Die folgende Codezeile gibt eine Ansicht mit dem Namen Fred zurück:</span><span class="sxs-lookup"><span data-stu-id="587bf-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="587bf-135">Anzeigen (Fred)</span><span class="sxs-lookup"><span data-stu-id="587bf-135">View( Fred )</span></span>

<span data-ttu-id="587bf-136">Wenn diese Codezeile ausgeführt wird, wird eine Sicht aus dem folgenden Pfad zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="587bf-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="587bf-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="587bf-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="587bf-138">Wenn Sie planen, erstellen Sie Komponententests für ASP.NET MVC-Anwendung ist es eine gute Idee, explizit zu Sichtnamen sein.</span><span class="sxs-lookup"><span data-stu-id="587bf-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="587bf-139">Auf diese Weise können Sie einen Komponententest, um sicherzustellen, dass die erwarteten Ansicht durch eine Controlleraktion zurückgegebener erstellen.</span><span class="sxs-lookup"><span data-stu-id="587bf-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="587bf-140">Hinzufügen von Inhalt zu einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="587bf-140">Adding Content to a View</span></span>

<span data-ttu-id="587bf-141">Eine Sicht ist ein Standard (X) HTML-Dokument, die Skripts enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="587bf-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="587bf-142">Sie können Skripts verwenden, dynamische Inhalte zu einer Sicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="587bf-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="587bf-143">Beispielsweise zeigt die Ansicht im Codebeispiel 2 das aktuelle Datum und die Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="587bf-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="587bf-144">**Listing 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="587bf-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="587bf-145">Beachten Sie, dass der Text der HTML-Seite in 2 auflisten das folgende Skript enthält:</span><span class="sxs-lookup"><span data-stu-id="587bf-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="587bf-146">&lt;"% Response.Write(DateTime.Now)"&gt;</span><span class="sxs-lookup"><span data-stu-id="587bf-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="587bf-147">Verwenden Sie das Skript-Trennzeichen &lt;"und" %&gt; um Anfang und Ende eines Skripts zu markieren.</span><span class="sxs-lookup"><span data-stu-id="587bf-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="587bf-148">Dieses Skript wird in Visual Basic geschrieben.</span><span class="sxs-lookup"><span data-stu-id="587bf-148">This script is written in Visual basic.</span></span> <span data-ttu-id="587bf-149">Es zeigt das aktuelle Datum und die Uhrzeit an, durch Aufrufen der Methode Response.Write() für das Rendern von Inhalt an den Browser.</span><span class="sxs-lookup"><span data-stu-id="587bf-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="587bf-150">Die Skript-Trennzeichen &lt;"und" %&gt; können verwendet werden, um eine oder mehrere Anweisungen ausführen.</span><span class="sxs-lookup"><span data-stu-id="587bf-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="587bf-151">Da Sie Response.Write() so häufig aufrufen, bietet Microsoft eine Verknüpfung zum Aufrufen der Response.Write()-Methode.</span><span class="sxs-lookup"><span data-stu-id="587bf-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="587bf-152">Die Ansicht im Codebeispiel 3 verwendet die Trennzeichen &lt;% = "und" %&gt; als Kurzform für Response.Write() aufrufen.</span><span class="sxs-lookup"><span data-stu-id="587bf-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="587bf-153">**3 – Views\Home\Index2.aspx auflisten**</span><span class="sxs-lookup"><span data-stu-id="587bf-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="587bf-154">Jeder können Sie um dynamische Inhalte in einer Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="587bf-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="587bf-155">Normalerweise ll Sie verwenden Visual Basic .NET oder C#-, um Ihre Controller und Ansichten zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="587bf-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="587bf-156">Verwenden von HTML-Hilfsmethoden zum Generieren von Inhalt anzeigen</span><span class="sxs-lookup"><span data-stu-id="587bf-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="587bf-157">Hinzufügen von Inhalt zu einer Sicht zu vereinfachen, profitieren Sie von so genannte ein *HTML-Hilfsobjekt*.</span><span class="sxs-lookup"><span data-stu-id="587bf-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="587bf-158">Ein HTML-Hilfsobjekt, ist im Allgemeinen eine Methode, die eine Zeichenfolge generiert.</span><span class="sxs-lookup"><span data-stu-id="587bf-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="587bf-159">Sie können HTML-Hilfsmethoden zum Generieren von standardmäßigen HTML-Elemente, z. B. Textfelder, Links, Dropdownlisten und Listenfelder verwenden.</span><span class="sxs-lookup"><span data-stu-id="587bf-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="587bf-160">Beispielsweise die Ansicht im Codebeispiel 4 nutzt drei HTML-Hilfsmethoden--bilden die Hilfsprogramme BeginForm(), TextBox() und Password() – um eine Anmeldung zu generieren (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="587bf-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="587bf-161">**Listing 4 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="587bf-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="587bf-162">[![Das Dialogfeld "Neues Projekt"](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="587bf-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="587bf-163">**Abbildung 01**: ein Anmeldename-Standardformat ([klicken Sie hier, um das Bild in voller Größe angezeigt](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="587bf-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="587bf-164">Alle Methoden, HTML-Hilfsmethoden sind in der Eigenschaft Html der Ansicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="587bf-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="587bf-165">Rendern Sie beispielsweise ein Textfeld durch Aufrufen der Html.TextBox()-Methode.</span><span class="sxs-lookup"><span data-stu-id="587bf-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="587bf-166">Beachten Sie, dass Sie die Skript-Trennzeichen verwenden &lt;% = "und" %&gt; beim Aufrufen der Html.TextBox() und die Html.Password() Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="587bf-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="587bf-167">Diese Hilfsprogramme werden einfach eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="587bf-167">These helpers simply return a string.</span></span> <span data-ttu-id="587bf-168">In diesem Fall müssen Sie eine Response.Write() aufrufen, um die Zeichenfolge an den Browser zu rendern.</span><span class="sxs-lookup"><span data-stu-id="587bf-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="587bf-169">Mit HTML-Hilfsmethoden ist optional.</span><span class="sxs-lookup"><span data-stu-id="587bf-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="587bf-170">Sie Ihnen das Leben erleichtern durch Reduzieren der HTML- und Skripts, die Sie schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="587bf-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="587bf-171">Die Ansicht im Codebeispiel 5 rendert die genaue dasselbe Format wie die Ansicht im Codebeispiel 4 ohne Verwendung von HTML-Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="587bf-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="587bf-172">**Listing 5 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="587bf-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="587bf-173">Sie haben auch die Möglichkeit, eigene HTML-Hilfsmethoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="587bf-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="587bf-174">Beispielsweise können Sie eine Hilfsmethode GridView() erstellen, die einen Satz von Datenbankdatensätzen automatisch in einer HTML-Tabelle anzeigt.</span><span class="sxs-lookup"><span data-stu-id="587bf-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="587bf-175">In diesem Thema im Lernprogramm werden untersucht **Erstellen von benutzerdefinierten HTML-Hilfsprogrammen**.</span><span class="sxs-lookup"><span data-stu-id="587bf-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="587bf-176">Mithilfe der Ansichtsdaten auf Daten an eine Ansicht zu übergeben</span><span class="sxs-lookup"><span data-stu-id="587bf-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="587bf-177">Sie verwenden die Ansichtsdaten, um Daten von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="587bf-178">Stellen Sie Anzeigedaten, wie ein Paket, das Sie über die e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="587bf-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="587bf-179">Alle Daten, die von einem Controller an eine Ansicht übergeben müssen mit diesem Paket gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="587bf-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="587bf-180">Der Controller im Codebeispiel 6 fügt z. B. eine Meldung, um Daten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="587bf-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="587bf-181">**6 – ProductController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="587bf-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="587bf-182">Der Controller ViewData-Eigenschaft stellt eine Auflistung von Name-Wert-Paaren dar.</span><span class="sxs-lookup"><span data-stu-id="587bf-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="587bf-183">Auflisten von 6 fügt die Index()-Methode ein Element auf die Ansicht Daten-Auflistung, die mit dem Namen Nachricht mit dem Wert Hello World!.</span><span class="sxs-lookup"><span data-stu-id="587bf-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="587bf-184">Wenn die Ansicht durch die Index()-Methode zurückgegeben wird, werden automatisch die Ansichtsdaten in die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="587bf-185">Die Ansicht im Codebeispiel 7 Ruft die Nachricht aus den Ansichtsdaten ab und rendert die Nachricht an den Browser.</span><span class="sxs-lookup"><span data-stu-id="587bf-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="587bf-186">**Listing 7 -- \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="587bf-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="587bf-187">Beachten Sie, dass die Sicht die Html.Encode() HTML-Hilfsmethode beim Rendern der Nachrichteninhalts nutzt.</span><span class="sxs-lookup"><span data-stu-id="587bf-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="587bf-188">Das HTML-Hilfsobjekt Html.Encode() codiert Sonderzeichen wie z. B. &lt; und &gt; in Zeichen, die sicher auf einer Webseite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="587bf-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="587bf-189">Wenn Sie Inhalt nicht, die ein Benutzer auf einer Website sendet rendern, sollten Sie den Inhalt, der JavaScript-Injection-Angriffe zu verhindern, dass codieren.</span><span class="sxs-lookup"><span data-stu-id="587bf-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="587bf-190">(Da die Nachricht selbst in den ProductController erstellt haben, werden wir Verschlüsselungskennwort t zum Codieren der Nachricht eigentlich benötigen.</span><span class="sxs-lookup"><span data-stu-id="587bf-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="587bf-191">Es ist jedoch Gewohnheit immer die Html.Encode()-Methode aufgerufen wird, beim Abrufen der Anzeigen des Inhalts von Daten in einer Ansicht anzeigen.)</span><span class="sxs-lookup"><span data-stu-id="587bf-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="587bf-192">Auflisten von 7 nutzte wir Anzeigedaten, um eine einfache Zeichenfolge-Nachricht von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="587bf-193">Anzeigen von Daten können auch um andere Arten von Daten, z. B. eine Auflistung von Datenbank-Datensätze von einem Domänencontroller zu einer Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="587bf-194">Z. B. wenn der Inhalt der Datenbanktabelle Produkte in einer Ansicht angezeigt werden sollen, und klicken Sie dann die Auflistung der Datenbank zu übergeben, würde Datensätze in der Sicht Daten.</span><span class="sxs-lookup"><span data-stu-id="587bf-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="587bf-195">Sie haben auch die Möglichkeit, stark typisierten Ansichtsdaten aus einem Controller an eine Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="587bf-196">In diesem Thema im Lernprogramm werden untersucht **Grundlegendes zu stark typisierte Daten anzeigen und Ansichten**.</span><span class="sxs-lookup"><span data-stu-id="587bf-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="587bf-197">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="587bf-197">Summary</span></span>

<span data-ttu-id="587bf-198">In diesem Lernprogramm bereitgestellten eine kurze Einführung in ASP.NET MVC-Ansichten, Anzeigen von Daten und HTML-Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="587bf-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="587bf-199">Im ersten Abschnitt haben Sie gelernt, wie neue Ansichten zu Ihrem Projekt hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="587bf-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="587bf-200">Sie haben gelernt, dass Sie eine Ansicht und den richtigen Ordner hinzufügen, um über einen bestimmten Controller aufrufen.</span><span class="sxs-lookup"><span data-stu-id="587bf-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="587bf-201">Als Nächstes besprochen haben wir das Thema des HTML-Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="587bf-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="587bf-202">Sie haben gelernt, wie HTML-Hilfsmethoden einfach generiert werden standard-HTML-Inhalt aktivieren.</span><span class="sxs-lookup"><span data-stu-id="587bf-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="587bf-203">Schließlich haben Sie gelernt, wie das Nutzen von Anzeigedaten, um Daten von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="587bf-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="587bf-204">[Zurück](passing-data-to-view-master-pages-cs.md)
> [Weiter](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="587bf-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
