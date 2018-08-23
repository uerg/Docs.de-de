---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC-Ansichten – Übersicht (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet es sich von einem HTML-Seite? In diesem Tutorial Stephen Walther bietet eine Einführung zu Ansichten und veranschaulicht, wie Sie t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a7f4afd70a17281123a7448a00896c186b9a00f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833574"
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="777e8-104">ASP.NET MVC-Ansichten – Übersicht (VB)</span><span class="sxs-lookup"><span data-stu-id="777e8-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="777e8-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="777e8-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="777e8-106">Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet es sich von einem HTML-Seite?</span><span class="sxs-lookup"><span data-stu-id="777e8-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="777e8-107">In diesem Tutorial Stephen Walther bietet eine Einführung zu Ansichten und veranschaulicht, wie Sie Daten anzeigen und HTML-Hilfsprogramme in einer Ansicht nutzen können.</span><span class="sxs-lookup"><span data-stu-id="777e8-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="777e8-108">Der Zweck dieses Lernprogramms ist eine kurze Einführung in ASP.NET MVC-Ansichten, Anzeigen von Daten und HTML-Hilfsprogramme bereit.</span><span class="sxs-lookup"><span data-stu-id="777e8-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="777e8-109">Am Ende dieses Tutorials sollten Sie verstehen, wie Sie neue Ansichten erstellen, Daten von einem Controller an eine Ansicht übergeben und HTML-Hilfsprogramme verwenden, um Inhalte in einer Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="777e8-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="777e8-110">Grundlegendes zu Sichten</span><span class="sxs-lookup"><span data-stu-id="777e8-110">Understanding Views</span></span>

<span data-ttu-id="777e8-111">Im Gegensatz zu ASP.NET oder Active Server Pages enthält ASP.NET MVC nicht alles, der direkt auf eine Seite entspricht.</span><span class="sxs-lookup"><span data-stu-id="777e8-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="777e8-112">In einer ASP.NET MVC-Anwendung besteht keine Seite auf dem Datenträger, der den Pfad in der URL entspricht, die Sie in die Adressleiste des Browsers eingeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="777e8-113">Am ehesten mit einer Seite in einer ASP.NET MVC-Anwendung ist etwas bezeichnet ein *Ansicht*.</span><span class="sxs-lookup"><span data-stu-id="777e8-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="777e8-114">In einer ASP.NET MVC-Anwendung werden die eingehenden Browseranforderungen zu Controlleraktionen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="777e8-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="777e8-115">Eine Controlleraktion kann es sich um eine Ansicht zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-115">A controller action might return a view.</span></span> <span data-ttu-id="777e8-116">Eine Controlleraktion kann jedoch eine andere Art von Aktion, z. B. umleiten Sie zu einer anderen Controlleraktion durchführen.</span><span class="sxs-lookup"><span data-stu-id="777e8-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="777e8-117">Codebeispiel 1 enthält einen einfachen Controller, mit dem Namen HomeController.</span><span class="sxs-lookup"><span data-stu-id="777e8-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="777e8-118">HomeController macht zwei Controller-Aktionen, die mit dem Namen Index() und Details() verfügbar.</span><span class="sxs-lookup"><span data-stu-id="777e8-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="777e8-119">**1 – HomeController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="777e8-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="777e8-120">Sie können die erste Aktion, die Aktion Index() aufrufen, indem Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="777e8-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="777e8-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="777e8-121">/Home/Index</span></span>

<span data-ttu-id="777e8-122">Sie können die zweite Aktion, die Aktion Details() aufrufen, indem Sie diese Adresse in Ihren Browser eingeben:</span><span class="sxs-lookup"><span data-stu-id="777e8-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="777e8-123">/ Home/Details</span><span class="sxs-lookup"><span data-stu-id="777e8-123">/Home/Details</span></span>

<span data-ttu-id="777e8-124">Die Index()-Aktion gibt eine Ansicht zurück.</span><span class="sxs-lookup"><span data-stu-id="777e8-124">The Index() action returns a view.</span></span> <span data-ttu-id="777e8-125">Die meisten Aktionen, die Sie erstellen werden Sichten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-125">Most actions that you create will return views.</span></span> <span data-ttu-id="777e8-126">Sie können jedoch eine Aktion andere Arten von Aktionsergebnissen zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="777e8-127">Beispielsweise gibt die Details()-Aktion ein, die eingehende Anforderung an die Aktion Index() leitet RedirectToActionResult zurück.</span><span class="sxs-lookup"><span data-stu-id="777e8-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="777e8-128">Die Aktion Index() enthält die folgende Codezeile:</span><span class="sxs-lookup"><span data-stu-id="777e8-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="777e8-129">View()</span><span class="sxs-lookup"><span data-stu-id="777e8-129">View()</span></span>

<span data-ttu-id="777e8-130">Diese Codezeile gibt es sich um eine Ansicht, die sich unter folgendem Pfad auf dem Webserver sein müssen:</span><span class="sxs-lookup"><span data-stu-id="777e8-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="777e8-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="777e8-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="777e8-132">Der Pfad für die Ansicht wird von den Namen des Controllers und der Name der Controlleraktion abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="777e8-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="777e8-133">Falls gewünscht, können Sie über die Ansicht explizite sein.</span><span class="sxs-lookup"><span data-stu-id="777e8-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="777e8-134">Die folgende Codezeile gibt eine Ansicht namens Fred zurück:</span><span class="sxs-lookup"><span data-stu-id="777e8-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="777e8-135">Ansicht (Fred)</span><span class="sxs-lookup"><span data-stu-id="777e8-135">View( Fred )</span></span>

<span data-ttu-id="777e8-136">Wenn diese Codezeile ausgeführt wird, wird eine Ansicht aus dem folgenden Pfad zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="777e8-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="777e8-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="777e8-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="777e8-138">Wenn Sie planen, Erstellen von Komponententests für Ihre ASP.NET MVC-Anwendung ist es eine gute Idee, die explizit über den Sichtnamen sein.</span><span class="sxs-lookup"><span data-stu-id="777e8-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="777e8-139">Auf diese Weise können Sie einen Komponententest, um sicherzustellen, dass die erwarteten Ansicht durch eine Controlleraktion zurückgegeben wurde erstellen.</span><span class="sxs-lookup"><span data-stu-id="777e8-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="777e8-140">Hinzufügen von Inhalten zu einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="777e8-140">Adding Content to a View</span></span>

<span data-ttu-id="777e8-141">Eine Sicht ist ein Standard (HTML-Dokument, das Skripts enthalten kann X).</span><span class="sxs-lookup"><span data-stu-id="777e8-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="777e8-142">Sie können Skripts verwenden, um dynamischen Inhalt an eine Ansicht hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="777e8-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="777e8-143">Beispielsweise zeigt die Ansicht im Codebeispiel 2 an das aktuelle Datum und die Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="777e8-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="777e8-144">**Codebeispiel 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="777e8-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="777e8-145">Beachten Sie, dass der Text der HTML-Seite in Liste 2 das folgende Skript enthält:</span><span class="sxs-lookup"><span data-stu-id="777e8-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="777e8-146">&lt;Response.Write(DateTime.Now) %&gt;</span><span class="sxs-lookup"><span data-stu-id="777e8-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="777e8-147">Verwenden Sie die Skript-Trennzeichen &lt;% "und"&gt; Anfang und Ende eines Skripts zu markieren.</span><span class="sxs-lookup"><span data-stu-id="777e8-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="777e8-148">Dieses Skript ist in Visual Basic geschrieben.</span><span class="sxs-lookup"><span data-stu-id="777e8-148">This script is written in Visual basic.</span></span> <span data-ttu-id="777e8-149">Es zeigt das aktuelle Datum und die Uhrzeit durch Aufrufen der Response.Write()-Methode, um Inhalt an den Browser zu rendern.</span><span class="sxs-lookup"><span data-stu-id="777e8-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="777e8-150">Die Skript-Trennzeichen &lt;% "und"&gt; können verwendet werden, um eine oder mehrere Anweisungen ausführen.</span><span class="sxs-lookup"><span data-stu-id="777e8-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="777e8-151">Da Sie so oft Response.Write() aufrufen, bietet Microsoft eine Verknüpfung zum Aufrufen der Response.Write()-Methode.</span><span class="sxs-lookup"><span data-stu-id="777e8-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="777e8-152">Die Ansicht in Programmausdruck 3 verwendet die Trennzeichen &lt;% = "und"&gt; als Abkürzung für Response.Write() aufrufen.</span><span class="sxs-lookup"><span data-stu-id="777e8-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="777e8-153">**Codebeispiel 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="777e8-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="777e8-154">Sie können jeder verwenden, um dynamischen Inhalt in einer Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="777e8-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="777e8-155">Normalerweise wird Sie verwenden Visual Basic .NET oder C#-Controllern und Ansichten zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="777e8-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="777e8-156">Verwenden von HTML-Hilfsmethoden zum Generieren von Inhalt anzeigen</span><span class="sxs-lookup"><span data-stu-id="777e8-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="777e8-157">Zum Hinzufügen von Inhalt an eine Ansicht zu vereinfachen, können Sie nutzen der so genannte ein *HTML-Hilfsobjekt*.</span><span class="sxs-lookup"><span data-stu-id="777e8-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="777e8-158">Ein HTML-Hilfsobjekt, ist in der Regel eine Methode, die eine Zeichenfolge generiert.</span><span class="sxs-lookup"><span data-stu-id="777e8-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="777e8-159">Sie können HTML-Hilfsprogramme verwenden, um standard-HTML-Elemente wie z. B. Textfelder, Links, Dropdownlisten und Listenfelder zu generieren.</span><span class="sxs-lookup"><span data-stu-id="777e8-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="777e8-160">Bilden beispielsweise die Ansicht in Listing 4 nutzt drei HTML-Hilfsprogrammen – Hilfsprogramme BeginForm(), TextBox() und Password() – um eine Anmeldung zu generieren (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="777e8-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="777e8-161">**Programmausdruck 4 – \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="777e8-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="777e8-162">[![Das Dialogfeld "Neues Projekt"](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="777e8-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="777e8-163">**Abbildung 01**: ein Anmeldeformular standard ([klicken Sie, um das Bild in voller Größe anzeigen](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="777e8-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="777e8-164">Alle HTML-Hilfsprogramme Methoden werden in der Eigenschaft Html der Ansicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="777e8-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="777e8-165">Rendern Sie z. B. ein Textfeld durch Aufrufen der Html.TextBox()-Methode.</span><span class="sxs-lookup"><span data-stu-id="777e8-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="777e8-166">Beachten Sie, dass Sie die Skript-Trennzeichen verwenden &lt;% = "und"&gt; beim Aufrufen von Html.TextBox() sowohl Html.Password() Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="777e8-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="777e8-167">Diese Hilfsprogramme werden einfach eine Zeichenfolge zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-167">These helpers simply return a string.</span></span> <span data-ttu-id="777e8-168">Sie müssen Response.Write() aufrufen, um die Zeichenfolge an den Browser zu rendern.</span><span class="sxs-lookup"><span data-stu-id="777e8-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="777e8-169">Verwendung von HTML-Hilfsmethoden ist optional.</span><span class="sxs-lookup"><span data-stu-id="777e8-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="777e8-170">Sie erleichtern sich das Leben durch Verringern der Menge von HTML und Skripts, die Sie schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="777e8-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="777e8-171">Die Ansicht in Listing 5 wird genaue dasselbe Format wie die Ansicht in Listing 4 ohne Verwendung von HTML-Hilfsprogramme gerendert.</span><span class="sxs-lookup"><span data-stu-id="777e8-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="777e8-172">**Programmausdruck 5 – \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="777e8-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="777e8-173">Sie können auch das Erstellen eigener HTML-Hilfsprogrammen.</span><span class="sxs-lookup"><span data-stu-id="777e8-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="777e8-174">Beispielsweise können Sie eine Hilfsmethode GridView() erstellen, die einen Satz von Datenbank-Datensätzen in einer HTML-Tabelle automatisch anzeigt.</span><span class="sxs-lookup"><span data-stu-id="777e8-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="777e8-175">Erläutert, in diesem Thema im Lernprogramm **Erstellen von benutzerdefinierten HTML-Hilfsprogrammen**.</span><span class="sxs-lookup"><span data-stu-id="777e8-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="777e8-176">Anzeigen von Daten verwenden, um Daten an eine Ansicht übergeben</span><span class="sxs-lookup"><span data-stu-id="777e8-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="777e8-177">Sie verwenden die Anzeigedaten, Daten von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="777e8-178">Stellen Sie Anzeigedaten, wie ein Paket, die Sie über die e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="777e8-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="777e8-179">Alle Daten, die von einem Controller an eine Ansicht übergeben müssen mit diesem Paket gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="777e8-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="777e8-180">Der Controller im Codebeispiel 6 fügt beispielsweise eine Nachricht zum Anzeigen von Daten.</span><span class="sxs-lookup"><span data-stu-id="777e8-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="777e8-181">**Codebeispiel 6: ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="777e8-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="777e8-182">Der Controller die ViewData-Eigenschaft stellt eine Auflistung von Name-Wert-Paaren dar.</span><span class="sxs-lookup"><span data-stu-id="777e8-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="777e8-183">Auflisten von 6 wurde der Erfassung von Ansicht namens "Message" mit dem Hello World-Wert die Index()-Methode ein Element hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="777e8-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="777e8-184">Wenn die Ansicht von der Index()-Methode zurückgegeben wird, werden die Anzeigen von Daten automatisch an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="777e8-185">Die Ansicht im Codebeispiel 7 Ruft die Nachricht von den Daten ab und rendert die Nachricht an den Browser.</span><span class="sxs-lookup"><span data-stu-id="777e8-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="777e8-186">**Codebeispiel 7: \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="777e8-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="777e8-187">Beachten Sie, dass die Ansicht die Html.Encode() HTML-Hilfsmethode beim Rendern der Nachricht nutzt.</span><span class="sxs-lookup"><span data-stu-id="777e8-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="777e8-188">Das HTML-Hilfsobjekt Html.Encode() codiert Sonderzeichen wie z. B. &lt; und &gt; in Zeichen, die sicher auf einer Webseite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="777e8-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="777e8-189">Wenn Sie Inhalt, die ein Benutzer auf einer Website sendet rendern, sollten Sie den Inhalt, der JavaScript-Injection-Angriffe zu verhindern, dass codieren.</span><span class="sxs-lookup"><span data-stu-id="777e8-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="777e8-190">(Da wir die Nachricht selbst im ProductController erstellt haben, wir Raten t nicht unbedingt die Meldung zu codieren.</span><span class="sxs-lookup"><span data-stu-id="777e8-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="777e8-191">Es ist jedoch ein sollte zur guten Gewohnheit, immer die Html.Encode()-Methode aufgerufen wird, bei der Anzeige von Inhalten aus der sichtdaten in einer Sicht abgerufen.)</span><span class="sxs-lookup"><span data-stu-id="777e8-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="777e8-192">Auflisten von 7 haben wir nutzen Anzeigedaten, eine einfache Zeichenfolge-Nachricht anhand eines Controllers zu einer Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="777e8-193">Sie können auch anzeigen von Daten verwenden, um andere Arten von Daten, z. B. eine Auflistung von Datenbank-Datensätzen von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="777e8-194">Beispielsweise sollten Sie den Inhalt der Datenbank Products-Tabelle in einer Sicht, zeigen Sie die Auflistung der Datenbank übergeben würde Datensätze in der Ansicht Daten.</span><span class="sxs-lookup"><span data-stu-id="777e8-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="777e8-195">Sie können auch die stark typisierte Ansichtsdaten anhand eines Controllers zu einer Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="777e8-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="777e8-196">Erläutert, in diesem Thema im Lernprogramm **Grundlegendes zu stark typisierten Daten anzeigen und Ansichten**.</span><span class="sxs-lookup"><span data-stu-id="777e8-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="777e8-197">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="777e8-197">Summary</span></span>

<span data-ttu-id="777e8-198">In diesem Tutorial bereitgestellt, eine kurze Einführung in ASP.NET MVC-Ansichten, Anzeigen von Daten und HTML-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="777e8-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="777e8-199">Im ersten Abschnitt haben Sie gelernt, wie neue Ansichten zu Ihrem Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="777e8-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="777e8-200">Sie haben gelernt, dass Sie eine Ansicht und den richtigen Ordner hinzufügen, um über einen bestimmten Controller aufrufen.</span><span class="sxs-lookup"><span data-stu-id="777e8-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="777e8-201">Als Nächstes erläutert das Thema des HTML-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="777e8-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="777e8-202">Sie erfahren, wie HTML-Hilfsprogramme standard-HTML-Inhalt einfach erstellen können.</span><span class="sxs-lookup"><span data-stu-id="777e8-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="777e8-203">Schließlich haben Sie Gewusst wie: Anzeigen von Daten zur Datenübergabe von einem Controller zu einer Ansicht nutzen.</span><span class="sxs-lookup"><span data-stu-id="777e8-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="777e8-204">[Zurück](passing-data-to-view-master-pages-cs.md)
> [Weiter](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="777e8-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
