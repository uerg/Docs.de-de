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
ms.openlocfilehash: 6df769069fce52755a57d8404f88203a652a1ab9
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="78fd8-104">Razor-Syntax für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78fd8-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="78fd8-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), und [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="78fd8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="78fd8-106">Razor ist eine Markupsyntax für das Einbetten von serverbasiertem Code in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="78fd8-107">Die Razor-Syntax besteht aus Razor-Markup, C#- und HTML.</span><span class="sxs-lookup"><span data-stu-id="78fd8-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="78fd8-108">Dateien, die in der Regel enthält Razor haben eine *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="78fd8-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="78fd8-109">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="78fd8-109">Rendering HTML</span></span>

<span data-ttu-id="78fd8-110">Die Standardsprache für den Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="78fd8-110">The default Razor language is HTML.</span></span> <span data-ttu-id="78fd8-111">Rendern von HTML in Razor-Markuperweiterung unterscheidet sich nicht als HTML-Datei aus einer HTML-Datei gerendert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="78fd8-112">HTML-Markup *cshtml* Razor-Dateien vom Server unverändert gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="78fd8-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="78fd8-113">Razor-syntax</span><span class="sxs-lookup"><span data-stu-id="78fd8-113">Razor syntax</span></span>

<span data-ttu-id="78fd8-114">Razor unterstützt C#- und verwendet die `@` Symbol für den Übergang von HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="78fd8-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="78fd8-115">Razor C#-Ausdrücke ausgewertet und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="78fd8-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="78fd8-116">Wenn ein `@` Symbol wird gefolgt von einer [Razor reserviertes Schlüsselwort](#razor-reserved-keywords), geht in Razor-spezifischer Markups.</span><span class="sxs-lookup"><span data-stu-id="78fd8-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="78fd8-117">Andernfalls geht es in einfachen c#.</span><span class="sxs-lookup"><span data-stu-id="78fd8-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="78fd8-118">Mit Escapezeichen eine `@` symbol in Razor-Markup, verwenden Sie eine zweite `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="78fd8-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="78fd8-119">Der Code wird in HTML gerendert, mit einem einzelnen `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="78fd8-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="78fd8-120">HTML-Attribute und Inhalt mit e-Mail-Adressen nicht behandeln die `@` Symbol als ein Zeichen Übergang.</span><span class="sxs-lookup"><span data-stu-id="78fd8-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="78fd8-121">Die e-Mail-Adressen im folgenden Beispiel werden durch Analysieren von Razor bleiben unverändert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="78fd8-122">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="78fd8-122">Implicit Razor expressions</span></span>

<span data-ttu-id="78fd8-123">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code:</span><span class="sxs-lookup"><span data-stu-id="78fd8-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="78fd8-124">Mit Ausnahme von C#- `await` Schlüsselwort implicit-Ausdrücken keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="78fd8-125">Wenn die C#-Anweisung eine klare beendet wurde, können Leerzeichen vermischt werden:</span><span class="sxs-lookup"><span data-stu-id="78fd8-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="78fd8-126">Implicit-Ausdrücken **kann nicht** C#-Generika, als eines der Zeichen innerhalb der Klammern enthalten (`<>`) als HTML-Tags interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="78fd8-127">Der folgende Code ist **nicht** gültig:</span><span class="sxs-lookup"><span data-stu-id="78fd8-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="78fd8-128">Der vorhergehende Code generiert einen Compilerfehler, der einen der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="78fd8-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="78fd8-129">Das Element "Int" wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="78fd8-129">The "int" element was not closed.</span></span>  <span data-ttu-id="78fd8-130">Alle Elemente muss entweder selbst schließen oder ein entsprechendes Endtag vorhanden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="78fd8-131">Die Methodengruppe "GenericMethod" nicht mit Typ "Object" Delegaten kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="78fd8-132">Wollten Sie die Methode aufrufen? "</span><span class="sxs-lookup"><span data-stu-id="78fd8-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="78fd8-133">Generische Methodenaufrufen, müssen umschlossen werden ein [expliziter Razor-Ausdruck](#explicit-razor-expressions) oder ein [Razor-Codeblock](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="78fd8-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="78fd8-134">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="78fd8-134">Explicit Razor expressions</span></span>

<span data-ttu-id="78fd8-135">Explizite Razor-Ausdrücke bestehen aus einer `@` Symbolbreite ausgeglichene Klammer.</span><span class="sxs-lookup"><span data-stu-id="78fd8-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="78fd8-136">Zum Zeitpunkt der letzten Woche zu rendern, wird das folgende Razor-Markup verwendet:</span><span class="sxs-lookup"><span data-stu-id="78fd8-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="78fd8-137">Alle Inhalte innerhalb der `@()` Klammern ausgewertet und in die Ausgabe des gerendert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="78fd8-138">Implizite Ausdrücke, die im vorherigen Abschnitt beschriebenen darf keine Leerzeichen in der Regel enthalten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="78fd8-139">Im folgenden Code wird nicht eine Woche nach der aktuellen Uhrzeit subtrahiert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="78fd8-140">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="78fd8-141">EXPLICIT-Ausdrücken können zum Verketten von Text mit einem Ergebnis des Ausdrucks verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="78fd8-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="78fd8-142">Ohne die explizite Ausdruck `<p>Age@joe.Age</p>` so behandelt, als eine e-Mail-Adresse und `<p>Age@joe.Age</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="78fd8-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="78fd8-143">Wenn ein expliziter Ausdruck geschrieben `<p>Age33</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="78fd8-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="78fd8-144">EXPLICIT-Ausdrücken dienen zum Rendern der Ausgabe von generischen Methoden in *cshtml* Dateien.</span><span class="sxs-lookup"><span data-stu-id="78fd8-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="78fd8-145">In einem impliziten Ausdruck, der Zeichen innerhalb der Klammern (`<>`) als HTML-Tags interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-145">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="78fd8-146">Das folgende Markup wird **nicht** Razor gültig:</span><span class="sxs-lookup"><span data-stu-id="78fd8-146">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="78fd8-147">Der vorhergehende Code generiert einen Compilerfehler, der einen der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="78fd8-147">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="78fd8-148">Das Element "Int" wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="78fd8-148">The "int" element was not closed.</span></span>  <span data-ttu-id="78fd8-149">Alle Elemente muss entweder selbst schließen oder ein entsprechendes Endtag vorhanden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-149">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="78fd8-150">Die Methodengruppe "GenericMethod" nicht mit Typ "Object" Delegaten kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-150">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="78fd8-151">Wollten Sie die Methode aufrufen? "</span><span class="sxs-lookup"><span data-stu-id="78fd8-151">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="78fd8-152">Das folgende Markup zeigt die richtige Methode schreiben, diesen Code.</span><span class="sxs-lookup"><span data-stu-id="78fd8-152">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="78fd8-153">Der Code wird als ein expliziter Ausdruck geschrieben:</span><span class="sxs-lookup"><span data-stu-id="78fd8-153">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="78fd8-154">Expression-Codierung</span><span class="sxs-lookup"><span data-stu-id="78fd8-154">Expression encoding</span></span>

<span data-ttu-id="78fd8-155">C#-Ausdrücke, die zu einer Zeichenfolge ausgewertet werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-155">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="78fd8-156">C#-Ausdrücke, die ergeben `IHtmlContent` sind direkt über gerendert `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="78fd8-156">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="78fd8-157">C#-Ausdrücke, ergeben nicht `IHtmlContent` werden in eine Zeichenfolge konvertiert `ToString` und codiert werden, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-157">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="78fd8-158">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-158">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="78fd8-159">Der HTML-Code wird im Browser als gezeigt:</span><span class="sxs-lookup"><span data-stu-id="78fd8-159">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="78fd8-160">`HtmlHelper.Raw`Ausgabe wird nicht codierte aber als HTML-Markup gerendert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-160">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="78fd8-161">Mit `HtmlHelper.Raw` unsanitized Benutzer Eingabe ist ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="78fd8-161">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="78fd8-162">Benutzereingabe möglicherweise böswilligen JavaScript- oder andere Exploits durchführen enthalten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-162">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="78fd8-163">Es ist schwierig, Bereinigen von Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="78fd8-163">Sanitizing user input is difficult.</span></span> <span data-ttu-id="78fd8-164">Vermeiden Sie die Verwendung `HtmlHelper.Raw` Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="78fd8-164">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="78fd8-165">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-165">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="78fd8-166">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="78fd8-166">Razor code blocks</span></span>

<span data-ttu-id="78fd8-167">Razor-Codeblöcke beginnen Sie mit `@` und eingeschlossen werden `{}`.</span><span class="sxs-lookup"><span data-stu-id="78fd8-167">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="78fd8-168">Im Gegensatz zu Ausdrücken wird nicht C#-Code in Codeblöcken gerendert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-168">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="78fd8-169">Codeblöcke Ausdrücke in einer Sicht gemeinsam zu nutzen die gleichen Bereich und werden in Reihenfolge definiert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-169">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="78fd8-170">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-170">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="78fd8-171">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="78fd8-171">Implicit transitions</span></span>

<span data-ttu-id="78fd8-172">In einem Codeblock die Standardsprache ist c#, aber die Razor-Seite können Übergang zurück zum HTML:</span><span class="sxs-lookup"><span data-stu-id="78fd8-172">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="78fd8-173">Explizite durch Trennzeichen getrennten Übergang</span><span class="sxs-lookup"><span data-stu-id="78fd8-173">Explicit delimited transition</span></span>

<span data-ttu-id="78fd8-174">Um einen Unterabschnitt eines Codeblocks definieren, die HTML gerendert werden soll, umschließen Sie die Zeichen für das Rendering mit den Razor  **\<Text >** Tag:</span><span class="sxs-lookup"><span data-stu-id="78fd8-174">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="78fd8-175">Verwenden Sie diesen Ansatz zum Rendern von HTML, die von einem HTML-Tags eingeschlossen ist nicht an.</span><span class="sxs-lookup"><span data-stu-id="78fd8-175">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="78fd8-176">Tritt auf, ein Razor-Laufzeitfehler, ohne ein HTML- oder Razor-Tag.</span><span class="sxs-lookup"><span data-stu-id="78fd8-176">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="78fd8-177">Die  **\<Text >** Tag ist nützlich, um die Leerzeichen beim Rendern von Inhalt zu steuern:</span><span class="sxs-lookup"><span data-stu-id="78fd8-177">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="78fd8-178">Nur der Inhalt zwischen die  **\<Text >** -Tags wird gerendert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-178">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="78fd8-179">Keine Leerzeichen vor oder nach der  **\<Text >** Tags in der HTML-Ausgabe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78fd8-179">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="78fd8-180">Explizite Zeile Übergang mit @:</span><span class="sxs-lookup"><span data-stu-id="78fd8-180">Explicit Line Transition with @:</span></span>

<span data-ttu-id="78fd8-181">Um den Rest der eine ganze Zeile in einem Codeblock als HTML zu rendern, verwenden die `@:` Syntax:</span><span class="sxs-lookup"><span data-stu-id="78fd8-181">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="78fd8-182">Ohne die `@:` im Code wird ein Razor-Laufzeitfehler generiert.</span><span class="sxs-lookup"><span data-stu-id="78fd8-182">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="78fd8-183">Warnung: Zusätzliche `@` Zeichen in einer Razor-Datei können dazu führen, dass Ursache Compiler-Fehler bei den Anweisungen weiter unten in den Block.</span><span class="sxs-lookup"><span data-stu-id="78fd8-183">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="78fd8-184">Dieser Compilerfehler können schwierig sein, zu verstehen, da der tatsächliche Fehler vor den gemeldeten Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="78fd8-184">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="78fd8-185">Dieser Fehler tritt häufig nach mehrere implizite/explizite Ausdrücke in einen einzelnen Codeblock zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="78fd8-185">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="78fd8-186">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="78fd8-186">Control Structures</span></span>

<span data-ttu-id="78fd8-187">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="78fd8-187">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="78fd8-188">Alle Aspekte von Codeblöcken (Übergang zu Markup Inline-c#) auch gelten die folgenden Strukturen:</span><span class="sxs-lookup"><span data-stu-id="78fd8-188">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="78fd8-189">Konditionelle Abschnitte @if, ElseIf, #else- und@switch</span><span class="sxs-lookup"><span data-stu-id="78fd8-189">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="78fd8-190">`@if`Steuerelemente, wenn der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="78fd8-190">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="78fd8-191">`else`und `else if` erfordern die `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="78fd8-191">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="78fd8-192">Das folgende Markup zeigt, wie eine Switch-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="78fd8-192">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="78fd8-193">Schleifen @for, @foreach, @while, und @do während</span><span class="sxs-lookup"><span data-stu-id="78fd8-193">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="78fd8-194">Auf Vorlagen basierende HTML kann mit Schleifen Steueranweisungen gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="78fd8-194">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="78fd8-195">Um eine Liste der Personen zu rendern:</span><span class="sxs-lookup"><span data-stu-id="78fd8-195">To render a list of people:</span></span>

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

<span data-ttu-id="78fd8-196">Die folgenden Schleifen Anweisungen werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="78fd8-196">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="78fd8-197">Zusammengesetzte@using</span><span class="sxs-lookup"><span data-stu-id="78fd8-197">Compound @using</span></span>

<span data-ttu-id="78fd8-198">In c# ist ein `using` Anweisung wird verwendet, um sicherzustellen, dass ein Objekt wurde verworfen.</span><span class="sxs-lookup"><span data-stu-id="78fd8-198">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="78fd8-199">In Razor wird derselbe Mechanismus verwendet, um HTML-Hilfsmethoden zu erstellen, zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-199">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="78fd8-200">Im folgenden Code Rendern HTML-Hilfsmethoden ein Formulartag mit der `@using` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="78fd8-200">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="78fd8-201">Bereichsebene Aktionen können ausgeführt werden, mit [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="78fd8-201">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="78fd8-202">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="78fd8-202">@try, catch, finally</span></span>

<span data-ttu-id="78fd8-203">Behandlung von Ausnahmen ist vergleichbar mit c#:</span><span class="sxs-lookup"><span data-stu-id="78fd8-203">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="78fd8-204">Razor hat die Möglichkeit, kritische Abschnitte mit Lock-Anweisungen zu schützen:</span><span class="sxs-lookup"><span data-stu-id="78fd8-204">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="78fd8-205">Kommentare</span><span class="sxs-lookup"><span data-stu-id="78fd8-205">Comments</span></span>

<span data-ttu-id="78fd8-206">Razor unterstützt C#- und HTML-Kommentare:</span><span class="sxs-lookup"><span data-stu-id="78fd8-206">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="78fd8-207">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-207">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="78fd8-208">Razor-Kommentare werden vom Server entfernt, bevor auf der Webseite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="78fd8-208">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="78fd8-209">Razor verwendet `@*  *@` zur Begrenzung von Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="78fd8-209">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="78fd8-210">Der folgende Code ist auskommentiert, damit der Server kein Markup Rendern:</span><span class="sxs-lookup"><span data-stu-id="78fd8-210">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="78fd8-211">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="78fd8-211">Directives</span></span>

<span data-ttu-id="78fd8-212">Razor-Direktiven werden durch implicit-Ausdrücken mit reservierten Schlüsselwörtern folgenden dargestellt die `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="78fd8-212">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="78fd8-213">Eine Richtlinie ändert sich normalerweise die Möglichkeit, die eine Sicht wird analysiert oder andere Funktionalität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="78fd8-213">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="78fd8-214">Verstehen, wie der Razor-Code für eine Ansicht generiert erleichtert das Verständnis der Funktionsweise von Direktiven.</span><span class="sxs-lookup"><span data-stu-id="78fd8-214">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="78fd8-215">Der Code generiert eine Klasse, die etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="78fd8-215">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="78fd8-216">Weiter unten in diesem Artikel im Abschnitt [Anzeigen der Razor c#-Klasse, die für eine Sicht generiert](#viewing-the-razor-c-class-generated-for-a-view) wird erläutert, wie diese generierte Klasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78fd8-216">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="78fd8-217">Die `@using` Direktive fügt c# `using` Richtlinie in die generierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="78fd8-217">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="78fd8-218">Die `@model` Richtlinie gibt den Typ des Modells an eine Ansicht übergeben:</span><span class="sxs-lookup"><span data-stu-id="78fd8-218">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="78fd8-219">In einer ASP.NET Core MVC-app mit einzelne Benutzerkonten erstellt die *Views/Account/Login.cshtml* -Ansicht enthält die folgende Deklaration einer Modell:</span><span class="sxs-lookup"><span data-stu-id="78fd8-219">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="78fd8-220">Die generierte Klasse erbt von `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="78fd8-220">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="78fd8-221">Razor macht eine `Model` Eigenschaft für den Zugriff auf das Modell an die Ansicht zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="78fd8-221">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="78fd8-222">Die `@model` Richtlinie gibt den Typ dieser Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="78fd8-222">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="78fd8-223">Die Richtlinie gibt an, die `T` in `RazorPage<T>` , dass die generierte, die Klasse die Sicht abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="78fd8-223">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="78fd8-224">Wenn die `@model` Richtlinie iisn't angegeben, die `Model` -Eigenschaft ist vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="78fd8-224">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="78fd8-225">Der Wert des Modells wird auf dem Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="78fd8-225">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="78fd8-226">Weitere Informationen finden Sie unter [stark typisierte Modelle und die @model Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="78fd8-226">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="78fd8-227">Die `@inherits` Richtlinie bietet vollen Zugriff auf die Klasse erbt von die Sicht:</span><span class="sxs-lookup"><span data-stu-id="78fd8-227">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="78fd8-228">Der folgende Code ist ein benutzerdefinierter Typ des Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="78fd8-228">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="78fd8-229">Die `CustomText` in einer Ansicht angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="78fd8-229">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="78fd8-230">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-230">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="78fd8-231">`@model`und `@inherits` in derselben Ansicht verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="78fd8-231">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="78fd8-232">`@inherits`kann einem *_ViewImports.cshtml* -Datei, die die Sicht importiert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-232">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="78fd8-233">Der folgende Code ist ein Beispiel für eine stark typisierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="78fd8-233">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="78fd8-234">Wenn "rick@contoso.com" übergeben wird im Modell, Ansicht die folgenden HTML-Markup generiert:</span><span class="sxs-lookup"><span data-stu-id="78fd8-234">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="78fd8-235">Die `@inject` Richtlinie ermöglicht die Razor-Seite zum Einfügen von eines Diensts von der [Dienstcontainer](xref:fundamentals/dependency-injection) in eine Sicht.</span><span class="sxs-lookup"><span data-stu-id="78fd8-235">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="78fd8-236">Weitere Informationen finden Sie unter [Abhängigkeitsinjektion in Sichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="78fd8-236">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="78fd8-237">Die `@functions` Richtlinie ermöglicht eine Razor-Seite, um eine Sicht Funktionsebene Inhalt hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="78fd8-237">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="78fd8-238">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78fd8-238">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="78fd8-239">Der Code generiert den folgenden HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="78fd8-239">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="78fd8-240">Der folgende Code ist die generierte Razor C#-Klasse:</span><span class="sxs-lookup"><span data-stu-id="78fd8-240">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="78fd8-241">Die `@section` Richtlinie dient in Verbindung mit der [Layout](xref:mvc/views/layout) Ansichten zum Rendern von Inhalt in verschiedenen Teilen der HTML-Seite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="78fd8-241">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="78fd8-242">Weitere Informationen finden Sie unter [Abschnitte](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="78fd8-242">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="78fd8-243">Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="78fd8-243">Tag Helpers</span></span>

<span data-ttu-id="78fd8-244">Es gibt drei Anweisungen, die betreffen [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="78fd8-244">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="78fd8-245">Direktive</span><span class="sxs-lookup"><span data-stu-id="78fd8-245">Directive</span></span> | <span data-ttu-id="78fd8-246">Funktion</span><span class="sxs-lookup"><span data-stu-id="78fd8-246">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="78fd8-247">Stellt Tag Hilfen zu einer Ansicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="78fd8-247">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="78fd8-248">Entfernt die Tag-Hilfsprogramme, die zuvor aus einer Sicht hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="78fd8-248">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="78fd8-249">Gibt ein Tagpräfix Tag Helper-Unterstützung zu aktivieren und Tag Helper Verwendung als explizite Anforderung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="78fd8-249">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="78fd8-250">Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="78fd8-250">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="78fd8-251">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="78fd8-251">Razor keywords</span></span>

* <span data-ttu-id="78fd8-252">Seite "(erfordert ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="78fd8-252">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="78fd8-253">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="78fd8-253">functions</span></span>
* <span data-ttu-id="78fd8-254">inherits</span><span class="sxs-lookup"><span data-stu-id="78fd8-254">inherits</span></span>
* <span data-ttu-id="78fd8-255">Modell</span><span class="sxs-lookup"><span data-stu-id="78fd8-255">model</span></span>
* <span data-ttu-id="78fd8-256">section</span><span class="sxs-lookup"><span data-stu-id="78fd8-256">section</span></span>
* <span data-ttu-id="78fd8-257">Hilfsprogramm (von ASP.NET Core derzeit nicht unterstützt)</span><span class="sxs-lookup"><span data-stu-id="78fd8-257">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="78fd8-258">Razor-Schlüsselwörter werden mit Escapezeichen versehen `@(Razor Keyword)` (z. B. `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="78fd8-258">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="78fd8-259">C#-Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="78fd8-259">C# Razor keywords</span></span>

* <span data-ttu-id="78fd8-260">Groß- und Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="78fd8-260">case</span></span>
* <span data-ttu-id="78fd8-261">do</span><span class="sxs-lookup"><span data-stu-id="78fd8-261">do</span></span>
* <span data-ttu-id="78fd8-262">default</span><span class="sxs-lookup"><span data-stu-id="78fd8-262">default</span></span>
* <span data-ttu-id="78fd8-263">for</span><span class="sxs-lookup"><span data-stu-id="78fd8-263">for</span></span>
* <span data-ttu-id="78fd8-264">foreach</span><span class="sxs-lookup"><span data-stu-id="78fd8-264">foreach</span></span>
* <span data-ttu-id="78fd8-265">if</span><span class="sxs-lookup"><span data-stu-id="78fd8-265">if</span></span>
* <span data-ttu-id="78fd8-266">else</span><span class="sxs-lookup"><span data-stu-id="78fd8-266">else</span></span>
* <span data-ttu-id="78fd8-267">lock</span><span class="sxs-lookup"><span data-stu-id="78fd8-267">lock</span></span>
* <span data-ttu-id="78fd8-268">switch</span><span class="sxs-lookup"><span data-stu-id="78fd8-268">switch</span></span>
* <span data-ttu-id="78fd8-269">try</span><span class="sxs-lookup"><span data-stu-id="78fd8-269">try</span></span>
* <span data-ttu-id="78fd8-270">catch</span><span class="sxs-lookup"><span data-stu-id="78fd8-270">catch</span></span>
* <span data-ttu-id="78fd8-271">finally</span><span class="sxs-lookup"><span data-stu-id="78fd8-271">finally</span></span>
* <span data-ttu-id="78fd8-272">Verwenden</span><span class="sxs-lookup"><span data-stu-id="78fd8-272">using</span></span>
* <span data-ttu-id="78fd8-273">while</span><span class="sxs-lookup"><span data-stu-id="78fd8-273">while</span></span>

<span data-ttu-id="78fd8-274">C#-Razor-Schlüsselwörter muss mit doppelten Escapezeichen `@(@C# Razor Keyword)` (z. B. `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="78fd8-274">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="78fd8-275">Die erste `@` versieht den Razor-Parser.</span><span class="sxs-lookup"><span data-stu-id="78fd8-275">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="78fd8-276">Die zweite `@` versieht den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="78fd8-276">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="78fd8-277">Reservierte Schlüsselwörter, die nicht vom Razor verwendet</span><span class="sxs-lookup"><span data-stu-id="78fd8-277">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="78fd8-278">namespace</span><span class="sxs-lookup"><span data-stu-id="78fd8-278">namespace</span></span>
* <span data-ttu-id="78fd8-279">Klasse</span><span class="sxs-lookup"><span data-stu-id="78fd8-279">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="78fd8-280">Anzeigen von für eine Sicht generiert Razor C#-Klasse.</span><span class="sxs-lookup"><span data-stu-id="78fd8-280">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="78fd8-281">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="78fd8-281">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="78fd8-282">Überschreiben Sie die `RazorTemplateEngine` hinzugefügt, indem MVC mit der `CustomTemplateEngine` Klasse:</span><span class="sxs-lookup"><span data-stu-id="78fd8-282">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="78fd8-283">Legen Sie einen Haltepunkt für die `return csharpDocument` SoH `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="78fd8-283">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="78fd8-284">Bei der Ausführung des Programms am Haltepunkt beendet wird, zeigen Sie den Wert des `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="78fd8-284">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Text-Schnellansicht Überblick generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="78fd8-286">Ansicht Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="78fd8-286">View lookups and case sensitivity</span></span>

<span data-ttu-id="78fd8-287">Das Razor-Ansichtsmodul führt die Groß-/Kleinschreibung Suchvorgänge für Ansichten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-287">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="78fd8-288">Allerdings wird die tatsächliche Suche durch das zugrunde liegende Dateisystem bestimmt:</span><span class="sxs-lookup"><span data-stu-id="78fd8-288">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="78fd8-289">Basis-Dateiquelle:</span><span class="sxs-lookup"><span data-stu-id="78fd8-289">File based source:</span></span> 
  * <span data-ttu-id="78fd8-290">Unter Betriebssystemen mit Groß-/Kleinschreibung beachten Dateisystemen (z. B. Windows) sind die physische Datei Anbieter Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="78fd8-290">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="78fd8-291">Beispielsweise `return View("Test")` ergibt Übereinstimmungen für */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, und eine andere Variante der Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="78fd8-291">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="78fd8-292">Auf Dateisystemen, Groß-/Kleinschreibung (z. B. Linux, OS x, und mit `EmbeddedFileProvider`), Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="78fd8-292">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="78fd8-293">Beispielsweise `return View("Test")` speziell Übereinstimmungen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78fd8-293">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="78fd8-294">Vorkompilierte Sichten: Nachschlagen vorkompilierte Sichten mit ASP.NET Core 2.0 und höher, Groß-/Kleinschreibung beachtet, unter allen Betriebssystemen ist.</span><span class="sxs-lookup"><span data-stu-id="78fd8-294">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="78fd8-295">Das Verhalten ist identisch mit dem Verhalten der physischen Datei des Anbieters unter Windows.</span><span class="sxs-lookup"><span data-stu-id="78fd8-295">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="78fd8-296">Wenn zwei vorkompilierte Sichten nur im Fall unterschiedlich sind, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="78fd8-296">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="78fd8-297">Entwicklern wird empfohlen, die Groß-/Kleinschreibung von Datei- und Verzeichnisnamen, die Groß-/Kleinschreibung der entsprechen:</span><span class="sxs-lookup"><span data-stu-id="78fd8-297">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="78fd8-298">Namen von Bereich, Controller und Aktion.</span><span class="sxs-lookup"><span data-stu-id="78fd8-298">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="78fd8-299">Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="78fd8-299">Razor Pages.</span></span>
    
<span data-ttu-id="78fd8-300">Groß-wird sichergestellt, dass die Bereitstellungen ihrer Ansichten unabhängig von der zugrunde liegenden Dateisystem suchen.</span><span class="sxs-lookup"><span data-stu-id="78fd8-300">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
