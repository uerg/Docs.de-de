---
title: "Razor-syntaxverweis für ASP.NET Core"
author: rick-anderson
description: "Erfahren Sie Markup Razor-Syntax für das Einbetten von serverbasiertem Code in Webseiten aus."
keywords: ASP.NET Core, Razor, Razor-Direktiven
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: e3c3149254d602db1fcc6d42360690be026189a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="3eac0-104">Razor-Syntax für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3eac0-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="3eac0-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), und [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="3eac0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="3eac0-106">Razor ist eine Markupsyntax für das Einbetten von serverbasiertem Code in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="3eac0-107">Die Razor-Syntax besteht aus Razor-Markup, C#- und HTML.</span><span class="sxs-lookup"><span data-stu-id="3eac0-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="3eac0-108">Dateien, die in der Regel enthält Razor haben eine *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="3eac0-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="3eac0-109">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="3eac0-109">Rendering HTML</span></span>

<span data-ttu-id="3eac0-110">Die Standardsprache für den Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="3eac0-110">The default Razor language is HTML.</span></span> <span data-ttu-id="3eac0-111">Rendern von HTML in Razor-Markuperweiterung unterscheidet sich nicht als HTML-Datei aus einer HTML-Datei gerendert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="3eac0-112">HTML-Markup *cshtml* Razor-Dateien vom Server unverändert gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="3eac0-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="3eac0-113">Razor-syntax</span><span class="sxs-lookup"><span data-stu-id="3eac0-113">Razor syntax</span></span>

<span data-ttu-id="3eac0-114">Razor unterstützt C#- und verwendet die `@` Symbol für den Übergang von HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="3eac0-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="3eac0-115">Razor C#-Ausdrücke ausgewertet und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="3eac0-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="3eac0-116">Wenn ein `@` Symbol wird gefolgt von einer [Razor reserviertes Schlüsselwort](#razor-reserved-keywords), geht in Razor-spezifischer Markups.</span><span class="sxs-lookup"><span data-stu-id="3eac0-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="3eac0-117">Andernfalls geht es in einfachen c#.</span><span class="sxs-lookup"><span data-stu-id="3eac0-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="3eac0-118">Mit Escapezeichen eine `@` symbol in Razor-Markup, verwenden Sie eine zweite `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="3eac0-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="3eac0-119">Der Code wird in HTML gerendert, mit einem einzelnen `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="3eac0-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="3eac0-120">HTML-Attribute und Inhalt mit e-Mail-Adressen nicht behandeln die `@` Symbol als ein Zeichen Übergang.</span><span class="sxs-lookup"><span data-stu-id="3eac0-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="3eac0-121">Die e-Mail-Adressen im folgenden Beispiel werden durch Analysieren von Razor bleiben unverändert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="3eac0-122">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="3eac0-122">Implicit Razor expressions</span></span>

<span data-ttu-id="3eac0-123">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code:</span><span class="sxs-lookup"><span data-stu-id="3eac0-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="3eac0-124">Mit Ausnahme von C#- `await` Schlüsselwort implicit-Ausdrücken keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="3eac0-125">Wenn die C#-Anweisung eine klare beendet wurde, können Leerzeichen vermischt werden:</span><span class="sxs-lookup"><span data-stu-id="3eac0-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="3eac0-126">Implicit-Ausdrücken **kann nicht** C#-Generika, als eines der Zeichen innerhalb der Klammern enthalten (`<>`) als HTML-Tags interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="3eac0-127">Der folgende Code ist **nicht** gültig:</span><span class="sxs-lookup"><span data-stu-id="3eac0-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="3eac0-128">Der vorhergehende Code generiert einen Compilerfehler, der einen der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="3eac0-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="3eac0-129">Das Element "Int" wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="3eac0-129">The "int" element was not closed.</span></span>  <span data-ttu-id="3eac0-130">Alle Elemente muss entweder selbst schließen oder ein entsprechendes Endtag vorhanden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="3eac0-131">Die Methodengruppe "GenericMethod" nicht mit Typ "Object" Delegaten kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="3eac0-132">Wollten Sie die Methode aufrufen? "</span><span class="sxs-lookup"><span data-stu-id="3eac0-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="3eac0-133">Generische Methodenaufrufen, müssen umschlossen werden ein [expliziter Razor-Ausdruck](#explicit-razor-expressions) oder ein [Razor-Codeblock](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="3eac0-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span> <span data-ttu-id="3eac0-134">Diese Einschränkung gilt nicht für *vbhtml* Razor-Dateien, da Visual Basic-Syntax Klammern um generische Typparameter anstelle von Klammern platziert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-134">This restriction doesn't apply to *.vbhtml* Razor files because Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="3eac0-135">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="3eac0-135">Explicit Razor expressions</span></span>

<span data-ttu-id="3eac0-136">Explizite Razor-Ausdrücke bestehen aus einer `@` Symbolbreite ausgeglichene Klammer.</span><span class="sxs-lookup"><span data-stu-id="3eac0-136">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="3eac0-137">Zum Zeitpunkt der letzten Woche zu rendern, wird das folgende Razor-Markup verwendet:</span><span class="sxs-lookup"><span data-stu-id="3eac0-137">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="3eac0-138">Alle Inhalte innerhalb der `@()` Klammern ausgewertet und in die Ausgabe des gerendert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-138">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="3eac0-139">Implizite Ausdrücke, die im vorherigen Abschnitt beschriebenen darf keine Leerzeichen in der Regel enthalten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-139">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="3eac0-140">Im folgenden Code wird nicht eine Woche nach der aktuellen Uhrzeit subtrahiert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-140">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="3eac0-141">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-141">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="3eac0-142">EXPLICIT-Ausdrücken können zum Verketten von Text mit einem Ergebnis des Ausdrucks verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="3eac0-142">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="3eac0-143">Ohne die explizite Ausdruck `<p>Age@joe.Age</p>` so behandelt, als eine e-Mail-Adresse und `<p>Age@joe.Age</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="3eac0-143">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="3eac0-144">Wenn ein expliziter Ausdruck geschrieben `<p>Age33</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="3eac0-144">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="3eac0-145">EXPLICIT-Ausdrücken dienen zum Rendern der Ausgabe von generischen Methoden in *cshtml* Dateien.</span><span class="sxs-lookup"><span data-stu-id="3eac0-145">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="3eac0-146">In einem impliziten Ausdruck, der Zeichen innerhalb der Klammern (`<>`) als HTML-Tags interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-146">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="3eac0-147">Das folgende Markup wird **nicht** Razor gültig:</span><span class="sxs-lookup"><span data-stu-id="3eac0-147">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="3eac0-148">Der vorhergehende Code generiert einen Compilerfehler, der einen der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="3eac0-148">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="3eac0-149">Das Element "Int" wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="3eac0-149">The "int" element was not closed.</span></span>  <span data-ttu-id="3eac0-150">Alle Elemente muss entweder selbst schließen oder ein entsprechendes Endtag vorhanden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-150">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="3eac0-151">Die Methodengruppe "GenericMethod" nicht mit Typ "Object" Delegaten kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-151">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="3eac0-152">Wollten Sie die Methode aufrufen? "</span><span class="sxs-lookup"><span data-stu-id="3eac0-152">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="3eac0-153">Das folgende Markup zeigt die richtige Methode schreiben, diesen Code.</span><span class="sxs-lookup"><span data-stu-id="3eac0-153">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="3eac0-154">Der Code wird als ein expliziter Ausdruck geschrieben:</span><span class="sxs-lookup"><span data-stu-id="3eac0-154">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

<span data-ttu-id="3eac0-155">Hinweis: Diese Einschränkung gilt nicht für *vbhtml* Razor-Dateien.</span><span class="sxs-lookup"><span data-stu-id="3eac0-155">Note: this restriction doesn't apply to *.vbhtml* Razor files.</span></span>  <span data-ttu-id="3eac0-156">Mit *vbhtml* Razor-Dateien, Visual Basic-Syntax platziert Klammern um generische Typparameter anstelle von eckigen Klammern.</span><span class="sxs-lookup"><span data-stu-id="3eac0-156">With *.vbhtml* Razor files, Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="3eac0-157">Expression-Codierung</span><span class="sxs-lookup"><span data-stu-id="3eac0-157">Expression encoding</span></span>

<span data-ttu-id="3eac0-158">C#-Ausdrücke, die zu einer Zeichenfolge ausgewertet werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-158">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="3eac0-159">C#-Ausdrücke, die ergeben `IHtmlContent` sind direkt über gerendert `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="3eac0-159">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="3eac0-160">C#-Ausdrücke, ergeben nicht `IHtmlContent` werden in eine Zeichenfolge konvertiert `ToString` und codiert werden, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-160">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="3eac0-161">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-161">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="3eac0-162">Der HTML-Code wird im Browser als gezeigt:</span><span class="sxs-lookup"><span data-stu-id="3eac0-162">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="3eac0-163">`HtmlHelper.Raw`Ausgabe wird nicht codierte aber als HTML-Markup gerendert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-163">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="3eac0-164">Mit `HtmlHelper.Raw` unsanitized Benutzer Eingabe ist ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="3eac0-164">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="3eac0-165">Benutzereingabe möglicherweise böswilligen JavaScript- oder andere Exploits durchführen enthalten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-165">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="3eac0-166">Es ist schwierig, Bereinigen von Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="3eac0-166">Sanitizing user input is difficult.</span></span> <span data-ttu-id="3eac0-167">Vermeiden Sie die Verwendung `HtmlHelper.Raw` Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="3eac0-167">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="3eac0-168">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-168">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="3eac0-169">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="3eac0-169">Razor code blocks</span></span>

<span data-ttu-id="3eac0-170">Razor-Codeblöcke beginnen Sie mit `@` und eingeschlossen werden `{}`.</span><span class="sxs-lookup"><span data-stu-id="3eac0-170">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="3eac0-171">Im Gegensatz zu Ausdrücken wird nicht C#-Code in Codeblöcken gerendert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-171">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="3eac0-172">Codeblöcke Ausdrücke in einer Sicht gemeinsam zu nutzen die gleichen Bereich und werden in Reihenfolge definiert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-172">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="3eac0-173">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-173">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="3eac0-174">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="3eac0-174">Implicit transitions</span></span>

<span data-ttu-id="3eac0-175">In einem Codeblock die Standardsprache ist c#, aber die Razor-Seite können Übergang zurück zum HTML:</span><span class="sxs-lookup"><span data-stu-id="3eac0-175">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="3eac0-176">Explizite durch Trennzeichen getrennten Übergang</span><span class="sxs-lookup"><span data-stu-id="3eac0-176">Explicit delimited transition</span></span>

<span data-ttu-id="3eac0-177">Um einen Unterabschnitt eines Codeblocks definieren, die HTML gerendert werden soll, umschließen Sie die Zeichen für das Rendering mit den Razor  **\<Text >** Tag:</span><span class="sxs-lookup"><span data-stu-id="3eac0-177">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="3eac0-178">Verwenden Sie diesen Ansatz zum Rendern von HTML, die von einem HTML-Tags eingeschlossen ist nicht an.</span><span class="sxs-lookup"><span data-stu-id="3eac0-178">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="3eac0-179">Tritt auf, ein Razor-Laufzeitfehler, ohne ein HTML- oder Razor-Tag.</span><span class="sxs-lookup"><span data-stu-id="3eac0-179">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="3eac0-180">Die  **\<Text >** Tag ist nützlich, um die Leerzeichen beim Rendern von Inhalt zu steuern:</span><span class="sxs-lookup"><span data-stu-id="3eac0-180">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="3eac0-181">Nur der Inhalt zwischen die  **\<Text >** -Tags wird gerendert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-181">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="3eac0-182">Keine Leerzeichen vor oder nach der  **\<Text >** Tags in der HTML-Ausgabe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3eac0-182">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="3eac0-183">Explizite Zeile Übergang mit @:</span><span class="sxs-lookup"><span data-stu-id="3eac0-183">Explicit Line Transition with @:</span></span>

<span data-ttu-id="3eac0-184">Um den Rest der eine ganze Zeile in einem Codeblock als HTML zu rendern, verwenden die `@:` Syntax:</span><span class="sxs-lookup"><span data-stu-id="3eac0-184">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="3eac0-185">Ohne die `@:` im Code wird ein Razor-Laufzeitfehler generiert.</span><span class="sxs-lookup"><span data-stu-id="3eac0-185">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="3eac0-186">Warnung: Zusätzliche `@` Zeichen in einer Razor-Datei können dazu führen, dass Ursache Compiler-Fehler bei den Anweisungen weiter unten in den Block.</span><span class="sxs-lookup"><span data-stu-id="3eac0-186">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="3eac0-187">Dieser Compilerfehler können schwierig sein, zu verstehen, da der tatsächliche Fehler vor den gemeldeten Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="3eac0-187">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="3eac0-188">Dieser Fehler tritt häufig nach mehrere implizite/explizite Ausdrücke in einen einzelnen Codeblock zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="3eac0-188">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="3eac0-189">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="3eac0-189">Control Structures</span></span>

<span data-ttu-id="3eac0-190">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="3eac0-190">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="3eac0-191">Alle Aspekte von Codeblöcken (Übergang zu Markup Inline-c#) auch gelten die folgenden Strukturen:</span><span class="sxs-lookup"><span data-stu-id="3eac0-191">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="3eac0-192">Konditionelle Abschnitte @if, ElseIf, #else- und@switch</span><span class="sxs-lookup"><span data-stu-id="3eac0-192">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="3eac0-193">`@if`Steuerelemente, wenn der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="3eac0-193">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="3eac0-194">`else`und `else if` erfordern die `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="3eac0-194">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="3eac0-195">Das folgende Markup zeigt, wie eine Switch-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="3eac0-195">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="3eac0-196">Schleifen @for, @foreach, @while, und @do während</span><span class="sxs-lookup"><span data-stu-id="3eac0-196">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="3eac0-197">Auf Vorlagen basierende HTML kann mit Schleifen Steueranweisungen gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="3eac0-197">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="3eac0-198">Um eine Liste der Personen zu rendern:</span><span class="sxs-lookup"><span data-stu-id="3eac0-198">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="3eac0-199">Die folgenden Schleifen Anweisungen werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="3eac0-199">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="3eac0-200">Zusammengesetzte@using</span><span class="sxs-lookup"><span data-stu-id="3eac0-200">Compound @using</span></span>

<span data-ttu-id="3eac0-201">In c# ist ein `using` Anweisung wird verwendet, um sicherzustellen, dass ein Objekt wurde verworfen.</span><span class="sxs-lookup"><span data-stu-id="3eac0-201">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="3eac0-202">In Razor wird derselbe Mechanismus verwendet, um HTML-Hilfsmethoden zu erstellen, zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-202">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="3eac0-203">Im folgenden Code Rendern HTML-Hilfsmethoden ein Formulartag mit der `@using` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="3eac0-203">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="3eac0-204">Bereichsebene Aktionen können ausgeführt werden, mit [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3eac0-204">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="3eac0-205">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="3eac0-205">@try, catch, finally</span></span>

<span data-ttu-id="3eac0-206">Behandlung von Ausnahmen ist vergleichbar mit c#:</span><span class="sxs-lookup"><span data-stu-id="3eac0-206">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="3eac0-207">Razor hat die Möglichkeit, kritische Abschnitte mit Lock-Anweisungen zu schützen:</span><span class="sxs-lookup"><span data-stu-id="3eac0-207">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="3eac0-208">Kommentare</span><span class="sxs-lookup"><span data-stu-id="3eac0-208">Comments</span></span>

<span data-ttu-id="3eac0-209">Razor unterstützt C#- und HTML-Kommentare:</span><span class="sxs-lookup"><span data-stu-id="3eac0-209">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="3eac0-210">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-210">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="3eac0-211">Razor-Kommentare werden vom Server entfernt, bevor auf der Webseite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="3eac0-211">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="3eac0-212">Razor verwendet `@*  *@` zur Begrenzung von Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="3eac0-212">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="3eac0-213">Der folgende Code ist auskommentiert, damit der Server kein Markup Rendern:</span><span class="sxs-lookup"><span data-stu-id="3eac0-213">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="3eac0-214">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="3eac0-214">Directives</span></span>

<span data-ttu-id="3eac0-215">Razor-Direktiven werden durch implicit-Ausdrücken mit reservierten Schlüsselwörtern folgenden dargestellt die `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="3eac0-215">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="3eac0-216">Eine Richtlinie ändert sich normalerweise die Möglichkeit, die eine Sicht wird analysiert oder andere Funktionalität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="3eac0-216">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="3eac0-217">Verstehen, wie der Razor-Code für eine Ansicht generiert erleichtert das Verständnis der Funktionsweise von Direktiven.</span><span class="sxs-lookup"><span data-stu-id="3eac0-217">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="3eac0-218">Der Code generiert eine Klasse, die etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="3eac0-218">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="3eac0-219">Weiter unten in diesem Artikel im Abschnitt [Anzeigen der Razor c#-Klasse, die für eine Sicht generiert](#viewing-the-razor-c-class-generated-for-a-view) wird erläutert, wie diese generierte Klasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3eac0-219">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="3eac0-220">Die `@using` Direktive fügt c# `using` Richtlinie in die generierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="3eac0-220">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="3eac0-221">Die `@model` Richtlinie gibt den Typ des Modells an eine Ansicht übergeben:</span><span class="sxs-lookup"><span data-stu-id="3eac0-221">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="3eac0-222">In einer ASP.NET Core MVC-app mit einzelne Benutzerkonten erstellt die *Views/Account/Login.cshtml* -Ansicht enthält die folgende Deklaration einer Modell:</span><span class="sxs-lookup"><span data-stu-id="3eac0-222">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="3eac0-223">Die generierte Klasse erbt von `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="3eac0-223">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="3eac0-224">Razor macht eine `Model` Eigenschaft für den Zugriff auf das Modell an die Ansicht zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="3eac0-224">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="3eac0-225">Die `@model` Richtlinie gibt den Typ dieser Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="3eac0-225">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="3eac0-226">Die Richtlinie gibt an, die `T` in `RazorPage<T>` , dass die generierte, die Klasse die Sicht abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="3eac0-226">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="3eac0-227">Wenn die `@model` Richtlinie iisn't angegeben, die `Model` -Eigenschaft ist vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="3eac0-227">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="3eac0-228">Der Wert des Modells wird auf dem Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="3eac0-228">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="3eac0-229">Weitere Informationen finden Sie unter [stark typisierte Modelle und die @model Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="3eac0-229">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="3eac0-230">Die `@inherits` Richtlinie bietet vollen Zugriff auf die Klasse erbt von die Sicht:</span><span class="sxs-lookup"><span data-stu-id="3eac0-230">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="3eac0-231">Der folgende Code ist ein benutzerdefinierter Typ des Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="3eac0-231">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="3eac0-232">Die `CustomText` in einer Ansicht angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="3eac0-232">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="3eac0-233">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-233">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="3eac0-234">`@model`und `@inherits` in derselben Ansicht verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="3eac0-234">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="3eac0-235">`@inherits`kann einem *_ViewImports.cshtml* -Datei, die die Sicht importiert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-235">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="3eac0-236">Der folgende Code ist ein Beispiel für eine stark typisierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="3eac0-236">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="3eac0-237">Wenn "rick@contoso.com" übergeben wird im Modell, Ansicht die folgenden HTML-Markup generiert:</span><span class="sxs-lookup"><span data-stu-id="3eac0-237">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="3eac0-238">Die `@inject` Richtlinie ermöglicht die Razor-Seite zum Einfügen von eines Diensts von der [Dienstcontainer](xref:fundamentals/dependency-injection) in eine Sicht.</span><span class="sxs-lookup"><span data-stu-id="3eac0-238">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="3eac0-239">Weitere Informationen finden Sie unter [Abhängigkeitsinjektion in Sichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3eac0-239">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="3eac0-240">Die `@functions` Richtlinie ermöglicht eine Razor-Seite, um eine Sicht Funktionsebene Inhalt hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="3eac0-240">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="3eac0-241">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3eac0-241">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="3eac0-242">Der Code generiert den folgenden HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="3eac0-242">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="3eac0-243">Der folgende Code ist die generierte Razor C#-Klasse:</span><span class="sxs-lookup"><span data-stu-id="3eac0-243">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="3eac0-244">Die `@section` Richtlinie dient in Verbindung mit der [Layout](xref:mvc/views/layout) Ansichten zum Rendern von Inhalt in verschiedenen Teilen der HTML-Seite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="3eac0-244">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="3eac0-245">Weitere Informationen finden Sie unter [Abschnitte](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="3eac0-245">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="3eac0-246">Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="3eac0-246">Tag Helpers</span></span>

<span data-ttu-id="3eac0-247">Es gibt drei Anweisungen, die betreffen [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3eac0-247">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="3eac0-248">Direktive</span><span class="sxs-lookup"><span data-stu-id="3eac0-248">Directive</span></span> | <span data-ttu-id="3eac0-249">Funktion</span><span class="sxs-lookup"><span data-stu-id="3eac0-249">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="3eac0-250">Stellt Tag Hilfen zu einer Ansicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="3eac0-250">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="3eac0-251">Entfernt die Tag-Hilfsprogramme, die zuvor aus einer Sicht hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3eac0-251">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="3eac0-252">Gibt ein Tagpräfix Tag Helper-Unterstützung zu aktivieren und Tag Helper Verwendung als explizite Anforderung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="3eac0-252">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="3eac0-253">Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="3eac0-253">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="3eac0-254">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="3eac0-254">Razor keywords</span></span>

* <span data-ttu-id="3eac0-255">Seite "(erfordert ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="3eac0-255">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="3eac0-256">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="3eac0-256">functions</span></span>
* <span data-ttu-id="3eac0-257">inherits</span><span class="sxs-lookup"><span data-stu-id="3eac0-257">inherits</span></span>
* <span data-ttu-id="3eac0-258">Modell</span><span class="sxs-lookup"><span data-stu-id="3eac0-258">model</span></span>
* <span data-ttu-id="3eac0-259">section</span><span class="sxs-lookup"><span data-stu-id="3eac0-259">section</span></span>
* <span data-ttu-id="3eac0-260">Hilfsprogramm (von ASP.NET Core derzeit nicht unterstützt)</span><span class="sxs-lookup"><span data-stu-id="3eac0-260">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="3eac0-261">Razor-Schlüsselwörter werden mit Escapezeichen versehen `@(Razor Keyword)` (z. B. `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="3eac0-261">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="3eac0-262">C#-Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="3eac0-262">C# Razor keywords</span></span>

* <span data-ttu-id="3eac0-263">case</span><span class="sxs-lookup"><span data-stu-id="3eac0-263">case</span></span>
* <span data-ttu-id="3eac0-264">do</span><span class="sxs-lookup"><span data-stu-id="3eac0-264">do</span></span>
* <span data-ttu-id="3eac0-265">default</span><span class="sxs-lookup"><span data-stu-id="3eac0-265">default</span></span>
* <span data-ttu-id="3eac0-266">for</span><span class="sxs-lookup"><span data-stu-id="3eac0-266">for</span></span>
* <span data-ttu-id="3eac0-267">foreach</span><span class="sxs-lookup"><span data-stu-id="3eac0-267">foreach</span></span>
* <span data-ttu-id="3eac0-268">if</span><span class="sxs-lookup"><span data-stu-id="3eac0-268">if</span></span>
* <span data-ttu-id="3eac0-269">else</span><span class="sxs-lookup"><span data-stu-id="3eac0-269">else</span></span>
* <span data-ttu-id="3eac0-270">lock</span><span class="sxs-lookup"><span data-stu-id="3eac0-270">lock</span></span>
* <span data-ttu-id="3eac0-271">switch</span><span class="sxs-lookup"><span data-stu-id="3eac0-271">switch</span></span>
* <span data-ttu-id="3eac0-272">try</span><span class="sxs-lookup"><span data-stu-id="3eac0-272">try</span></span>
* <span data-ttu-id="3eac0-273">catch</span><span class="sxs-lookup"><span data-stu-id="3eac0-273">catch</span></span>
* <span data-ttu-id="3eac0-274">finally</span><span class="sxs-lookup"><span data-stu-id="3eac0-274">finally</span></span>
* <span data-ttu-id="3eac0-275">Verwenden</span><span class="sxs-lookup"><span data-stu-id="3eac0-275">using</span></span>
* <span data-ttu-id="3eac0-276">while</span><span class="sxs-lookup"><span data-stu-id="3eac0-276">while</span></span>

<span data-ttu-id="3eac0-277">C#-Razor-Schlüsselwörter muss mit doppelten Escapezeichen `@(@C# Razor Keyword)` (z. B. `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="3eac0-277">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="3eac0-278">Die erste `@` versieht den Razor-Parser.</span><span class="sxs-lookup"><span data-stu-id="3eac0-278">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="3eac0-279">Die zweite `@` versieht den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="3eac0-279">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="3eac0-280">Reservierte Schlüsselwörter, die nicht vom Razor verwendet</span><span class="sxs-lookup"><span data-stu-id="3eac0-280">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="3eac0-281">namespace</span><span class="sxs-lookup"><span data-stu-id="3eac0-281">namespace</span></span>
* <span data-ttu-id="3eac0-282">Klasse</span><span class="sxs-lookup"><span data-stu-id="3eac0-282">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="3eac0-283">Anzeigen von für eine Sicht generiert Razor C#-Klasse.</span><span class="sxs-lookup"><span data-stu-id="3eac0-283">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="3eac0-284">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="3eac0-284">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="3eac0-285">Überschreiben Sie die `RazorTemplateEngine` hinzugefügt, indem MVC mit der `CustomTemplateEngine` Klasse:</span><span class="sxs-lookup"><span data-stu-id="3eac0-285">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="3eac0-286">Legen Sie einen Haltepunkt für die `return csharpDocument` SoH `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="3eac0-286">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="3eac0-287">Bei der Ausführung des Programms am Haltepunkt beendet wird, zeigen Sie den Wert des `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="3eac0-287">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Text-Schnellansicht Überblick generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="3eac0-289">Ansicht Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="3eac0-289">View lookups and case sensitivity</span></span>

<span data-ttu-id="3eac0-290">Das Razor-Ansichtsmodul führt die Groß-/Kleinschreibung Suchvorgänge für Ansichten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-290">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="3eac0-291">Allerdings wird die tatsächliche Suche durch das zugrunde liegende Dateisystem bestimmt:</span><span class="sxs-lookup"><span data-stu-id="3eac0-291">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="3eac0-292">Basis-Dateiquelle:</span><span class="sxs-lookup"><span data-stu-id="3eac0-292">File based source:</span></span> 
  * <span data-ttu-id="3eac0-293">Unter Betriebssystemen mit Groß-/Kleinschreibung beachten Dateisystemen (z. B. Windows) sind die physische Datei Anbieter Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="3eac0-293">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="3eac0-294">Beispielsweise `return View("Test")` ergibt Übereinstimmungen für */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, und eine andere Variante der Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="3eac0-294">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="3eac0-295">Auf Dateisystemen, Groß-/Kleinschreibung (z. B. Linux, OS x, und mit `EmbeddedFileProvider`), Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="3eac0-295">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="3eac0-296">Beispielsweise `return View("Test")` speziell Übereinstimmungen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3eac0-296">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="3eac0-297">Vorkompilierte Sichten: Nachschlagen vorkompilierte Sichten mit ASP.NET Core 2.0 und höher, Groß-/Kleinschreibung beachtet, unter allen Betriebssystemen ist.</span><span class="sxs-lookup"><span data-stu-id="3eac0-297">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="3eac0-298">Das Verhalten ist identisch mit dem Verhalten der physischen Datei des Anbieters unter Windows.</span><span class="sxs-lookup"><span data-stu-id="3eac0-298">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="3eac0-299">Wenn zwei vorkompilierte Sichten nur im Fall unterschiedlich sind, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="3eac0-299">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="3eac0-300">Entwicklern wird empfohlen, die Groß-/Kleinschreibung von Datei- und Verzeichnisnamen, die Groß-/Kleinschreibung der entsprechen:</span><span class="sxs-lookup"><span data-stu-id="3eac0-300">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="3eac0-301">Namen von Bereich, Controller und Aktion.</span><span class="sxs-lookup"><span data-stu-id="3eac0-301">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="3eac0-302">Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="3eac0-302">Razor Pages.</span></span>
    
<span data-ttu-id="3eac0-303">Groß-wird sichergestellt, dass die Bereitstellungen ihrer Ansichten unabhängig von der zugrunde liegenden Dateisystem suchen.</span><span class="sxs-lookup"><span data-stu-id="3eac0-303">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
