---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Erstellen von benutzerdefinierten HTML-Hilfsmethoden (c#) | Microsoft Docs
author: microsoft
description: Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie benutzerdefinierte HTML-Hilfsmethoden erstellen können, die Sie im MVC-Ansichten zur Verfügung stehen. Durch die Nutzung von HTML-Hilfsobjekt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869724"
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="62b8f-104">Erstellen von benutzerdefinierten HTML-Hilfsmethoden (c#)</span><span class="sxs-lookup"><span data-stu-id="62b8f-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="62b8f-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="62b8f-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="62b8f-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="62b8f-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="62b8f-107">Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie benutzerdefinierte HTML-Hilfsmethoden erstellen können, die Sie im MVC-Ansichten zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="62b8f-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="62b8f-108">Durch die Nutzung von HTML-Hilfsmethoden, können Sie die Menge des mühsam Eingabe der HTML-Tags, die Sie ausführen müssen, um eine standard-HTML-Seite erstellen reduzieren.</span><span class="sxs-lookup"><span data-stu-id="62b8f-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="62b8f-109">Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie benutzerdefinierte HTML-Hilfsmethoden erstellen können, die Sie im MVC-Ansichten zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="62b8f-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="62b8f-110">Durch die Nutzung von HTML-Hilfsmethoden, können Sie die Menge des mühsam Eingabe der HTML-Tags, die Sie ausführen müssen, um eine standard-HTML-Seite erstellen reduzieren.</span><span class="sxs-lookup"><span data-stu-id="62b8f-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="62b8f-111">Im ersten Teil dieses Lernprogramms beschreiben ich einige der vorhandenen HTML-Hilfsmethoden, die mit ASP.NET MVC-Framework enthalten.</span><span class="sxs-lookup"><span data-stu-id="62b8f-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="62b8f-112">Als Nächstes ich werden zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden beschrieben: Ich wird erläutert, wie zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden durch das Erstellen einer statischen Methode und durch Erstellen einer Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="62b8f-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="62b8f-113">Grundlegendes zu HTML-Hilfsmethoden</span><span class="sxs-lookup"><span data-stu-id="62b8f-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="62b8f-114">Ein HTML-Hilfsobjekt ist nur eine Methode, die eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="62b8f-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="62b8f-115">Die Zeichenfolge kann jede Art von Inhalt darstellen, die Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="62b8f-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="62b8f-116">Beispielsweise können Sie HTML-Hilfsmethoden zum Rendern von HTML-Tags wie HTML-standard `<input>` und `<img>` Tags.</span><span class="sxs-lookup"><span data-stu-id="62b8f-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="62b8f-117">HTML-Hilfsmethoden können auch komplexeren Inhalt, z. B. eine Registerkartenleiste oder eine HTML-Tabelle der Datenbankdaten rendern.</span><span class="sxs-lookup"><span data-stu-id="62b8f-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="62b8f-118">ASP.NET MVC-Framework enthält die folgenden standardmäßigen HTML-Hilfsmethoden (Dies ist keine vollständige Liste):</span><span class="sxs-lookup"><span data-stu-id="62b8f-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="62b8f-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="62b8f-119">Html.ActionLink()</span></span>
- <span data-ttu-id="62b8f-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="62b8f-120">Html.BeginForm()</span></span>
- <span data-ttu-id="62b8f-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="62b8f-121">Html.CheckBox()</span></span>
- <span data-ttu-id="62b8f-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="62b8f-122">Html.DropDownList()</span></span>
- <span data-ttu-id="62b8f-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="62b8f-123">Html.EndForm()</span></span>
- <span data-ttu-id="62b8f-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="62b8f-124">Html.Hidden()</span></span>
- <span data-ttu-id="62b8f-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="62b8f-125">Html.ListBox()</span></span>
- <span data-ttu-id="62b8f-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="62b8f-126">Html.Password()</span></span>
- <span data-ttu-id="62b8f-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="62b8f-127">Html.RadioButton()</span></span>
- <span data-ttu-id="62b8f-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="62b8f-128">Html.TextArea()</span></span>
- <span data-ttu-id="62b8f-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="62b8f-129">Html.TextBox()</span></span>

<span data-ttu-id="62b8f-130">Betrachten Sie beispielsweise das Formular im Codebeispiel 1 aus.</span><span class="sxs-lookup"><span data-stu-id="62b8f-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="62b8f-131">Dieses Formular wird mithilfe von zwei der standardmäßigen HTML-Hilfsmethoden gerendert (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="62b8f-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="62b8f-132">Dieses Formular verwendet die `Html.BeginForm()` und `Html.TextBox()` Hilfsmethoden zum Rendern eines einfachen HTML-Formulars.</span><span class="sxs-lookup"><span data-stu-id="62b8f-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="62b8f-133">[![Seite gerendert wird, mit HTML-Hilfsmethoden](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="62b8f-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="62b8f-134">**Abbildung 01**: Seite gerendert wird, mit HTML-Hilfsmethoden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="62b8f-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="62b8f-135">**Auflisten von 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="62b8f-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="62b8f-136">Die Html.BeginForm() Hilfsmethode wird verwendet, um die öffnenden und schließenden HTML erstellen `<form>` Tags.</span><span class="sxs-lookup"><span data-stu-id="62b8f-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="62b8f-137">Beachten Sie, dass die `Html.BeginForm()` Methode wird aufgerufen, in einer mit Anweisung.</span><span class="sxs-lookup"><span data-stu-id="62b8f-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="62b8f-138">Die Anweisung wird sichergestellt, dass die `<form>` Tag geschlossen wird, am Ende der mit Block.</span><span class="sxs-lookup"><span data-stu-id="62b8f-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="62b8f-139">Wunsch statt einer mit blockieren, können Sie die Html.EndForm() Hilfsmethode schließen Aufrufen der `<form>` Tag.</span><span class="sxs-lookup"><span data-stu-id="62b8f-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="62b8f-140">Verwenden Sie unabhängig davon, welche Vorgehensweise erstellen eine öffnende und schließende `<form>` Tag, die für Sie besonders intuitiv erscheint.</span><span class="sxs-lookup"><span data-stu-id="62b8f-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="62b8f-141">Die `Html.TextBox()` Hilfsmethoden werden zum Rendern von HTML in Codebeispiel 1 verwendet `<input>` Tags.</span><span class="sxs-lookup"><span data-stu-id="62b8f-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="62b8f-142">Wenn Sie in Ihrem Browser Quelltext anzeigen auswählen sehen Sie die HTML-Quelle in 2 aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="62b8f-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="62b8f-143">Beachten Sie, dass die Datenquelle und des standard-HTML-Tags enthält.</span><span class="sxs-lookup"><span data-stu-id="62b8f-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62b8f-144">Beachten Sie, dass die `Html.TextBox()`- HTML-Hilfsobjekt wird mit gerendert `<%= %>` anstelle von tags `<% %>` Tags.</span><span class="sxs-lookup"><span data-stu-id="62b8f-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="62b8f-145">Wenn Sie das Gleichheitszeichen nicht einschließen, ruft nichts an den Browser gerendert.</span><span class="sxs-lookup"><span data-stu-id="62b8f-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="62b8f-146">ASP.NET MVC-Framework enthält einen kleinen Satz von Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="62b8f-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="62b8f-147">In den meisten Fällen müssen Sie das MVC-Framework mit der benutzerdefinierten HTML-Hilfsmethoden zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="62b8f-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="62b8f-148">Im weiteren Verlauf dieses Lernprogramms erfahren Sie zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="62b8f-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="62b8f-149">**Auflisten von 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="62b8f-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="62b8f-150">Erstellen von HTML-Hilfsprogrammen mit statischen Methoden</span><span class="sxs-lookup"><span data-stu-id="62b8f-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="62b8f-151">Die einfachste Möglichkeit zum Erstellen neuer HTML-Hilfsobjekt ist eine statische Methode erstellen, die eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="62b8f-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="62b8f-152">Stellen Sie sich vor, z. B., dass Sie eine neue HTML-Hilfsobjekt erstellen, der eine HTML rendert möchten `<label>` Tag.</span><span class="sxs-lookup"><span data-stu-id="62b8f-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="62b8f-153">Sie können mithilfe die Klasse 2 auflisten Rendern einer `<label>` .</span><span class="sxs-lookup"><span data-stu-id="62b8f-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="62b8f-154">**Auflisten von 2 – `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="62b8f-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="62b8f-155">Es gibt keine besonderen über die Klasse 2 auflisten.</span><span class="sxs-lookup"><span data-stu-id="62b8f-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="62b8f-156">Die `Label()` -Methode einfach eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="62b8f-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="62b8f-157">Die geänderte Indexansicht auflisten 3 verwendet den `LabelHelper` zum Rendern von HTML `<label>` Tags.</span><span class="sxs-lookup"><span data-stu-id="62b8f-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="62b8f-158">Beachten Sie, die die Sicht enthält eine `<%@ imports %>` -Direktive, die importiert die `Application1.Helpers` Namespace.</span><span class="sxs-lookup"><span data-stu-id="62b8f-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="62b8f-159">**Auflisten von 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="62b8f-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="62b8f-160">Erstellen von HTML-Hilfsprogrammen mit Erweiterungsmethoden [c#]</span><span class="sxs-lookup"><span data-stu-id="62b8f-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="62b8f-161">HTML-Hilfsmethoden funktioniert wie der standardmäßigen HTML-Hilfsmethoden, die in ASP.NET MVC-Framework einbezogen wird, müssen Sie zum Erstellen von Erweiterungsmethoden [c#] erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="62b8f-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="62b8f-162">Erweiterungsmethoden können Sie einer vorhandenen Klasse neue Methoden hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="62b8f-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="62b8f-163">Wenn Sie ein HTML-Hilfsmethode erstellen, werden neue Methoden hinzufügen, für die HtmlHelper-Klasse, die durch eine Sicht HTML-Eigenschaft dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="62b8f-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="62b8f-164">Die Klasse im Codebeispiel 3 fügt eine Erweiterungsmethode zum der `HtmlHelper` Klasse mit dem Namen `Label()`.</span><span class="sxs-lookup"><span data-stu-id="62b8f-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="62b8f-165">Es gibt mehrere Dinge, die Sie zu dieser Klasse beachten sollten.</span><span class="sxs-lookup"><span data-stu-id="62b8f-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="62b8f-166">Erstens ist zu beachten, dass die Klasse eine statische Klasse.</span><span class="sxs-lookup"><span data-stu-id="62b8f-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="62b8f-167">Sie müssen eine Erweiterungsmethode mit einer statischen Klasse definieren.</span><span class="sxs-lookup"><span data-stu-id="62b8f-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="62b8f-168">Zweitens, beachten Sie, dass der erste Parameter der `Label()` Methode ist das Schlüsselwort vorangestellt `this`.</span><span class="sxs-lookup"><span data-stu-id="62b8f-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="62b8f-169">Der erste Parameter einer Erweiterungsmethode gibt an, die Klasse, die die Erweiterungsmethode erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="62b8f-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="62b8f-170">**Auflisten von 3: `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="62b8f-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="62b8f-171">Nachdem Sie eine Erweiterungsmethode zu erstellen und die Anwendung erfolgreich erstellen, wird die Erweiterungsmethode in Visual Studio Intellisense wie bei allen anderen Methoden einer Klasse angezeigt (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="62b8f-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="62b8f-172">Der einzige Unterschied ist diese Erweiterung, die Methoden mit einem besonderen Symbol neben dem Namen (Symbol nach unten weisenden Pfeil) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="62b8f-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="62b8f-173">[![Verwenden die Erweiterungsmethode Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="62b8f-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="62b8f-174">**Abbildung 02**: mit der Erweiterungsmethode Html.Label() ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="62b8f-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="62b8f-175">Die geänderte Indexansicht Auflisten von 4 verwendet die Erweiterungsmethode Html.Label() aller gerendert seine `<label>` Tags.</span><span class="sxs-lookup"><span data-stu-id="62b8f-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="62b8f-176">**Auflisten von 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="62b8f-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="62b8f-177">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="62b8f-177">Summary</span></span>

<span data-ttu-id="62b8f-178">In diesem Lernprogramm haben Sie gelernt, zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="62b8f-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="62b8f-179">Zunächst haben Sie gelernt, wie erstellen eine benutzerdefinierten `Label()` HTML-Hilfsobjekt durch Erstellen einer statischen Methode, gibt eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="62b8f-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="62b8f-180">Als Nächstes haben Sie gelernt, wie zum Erstellen eines benutzerdefinierten `Label()` HTML-Hilfsmethode durch Erstellen einer Erweiterungsmethode für die `HtmlHelper` Klasse.</span><span class="sxs-lookup"><span data-stu-id="62b8f-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="62b8f-181">In diesem Lernprogramm konzentriert sich ich auf eine sehr einfache HTML-Hilfsmethode erstellen.</span><span class="sxs-lookup"><span data-stu-id="62b8f-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="62b8f-182">Beachten Sie, dass ein HTML-Hilfsobjekt ebenso kompliziert, wie gewünscht werden.</span><span class="sxs-lookup"><span data-stu-id="62b8f-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="62b8f-183">Sie können HTML-Hilfsmethoden erstellen, die Inhalte wie Strukturansichten, Menüs oder Tabellen der Datenbankdaten rendern.</span><span class="sxs-lookup"><span data-stu-id="62b8f-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62b8f-184">[Zurück](asp-net-mvc-views-overview-cs.md)
> [Weiter](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="62b8f-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
