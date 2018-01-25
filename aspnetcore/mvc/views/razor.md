---
title: "Razor-syntaxverweis für ASP.NET Core"
author: rick-anderson
description: "Erfahren Sie Markup Razor-Syntax für das Einbetten von serverbasiertem Code in Webseiten aus."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: abdbb8112533d42f81180abad52f5ee86e3b280f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="94ab3-103">Razor-Syntax für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94ab3-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="94ab3-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), und [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="94ab3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="94ab3-105">Razor ist eine Markupsyntax für das Einbetten von serverbasiertem Code in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="94ab3-106">Die Razor-Syntax besteht aus Razor-Markup, C#- und HTML.</span><span class="sxs-lookup"><span data-stu-id="94ab3-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="94ab3-107">Dateien, die in der Regel enthält Razor haben eine *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="94ab3-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="94ab3-108">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="94ab3-108">Rendering HTML</span></span>

<span data-ttu-id="94ab3-109">Die Standardsprache für den Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="94ab3-109">The default Razor language is HTML.</span></span> <span data-ttu-id="94ab3-110">Rendern von HTML in Razor-Markuperweiterung unterscheidet sich nicht als HTML-Datei aus einer HTML-Datei gerendert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="94ab3-111">HTML-Markup *cshtml* Razor-Dateien vom Server unverändert gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="94ab3-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="94ab3-112">Razor-syntax</span><span class="sxs-lookup"><span data-stu-id="94ab3-112">Razor syntax</span></span>

<span data-ttu-id="94ab3-113">Razor unterstützt C#- und verwendet die `@` Symbol für den Übergang von HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="94ab3-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="94ab3-114">Razor C#-Ausdrücke ausgewertet und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="94ab3-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="94ab3-115">Wenn ein `@` Symbol wird gefolgt von einer [Razor reserviertes Schlüsselwort](#razor-reserved-keywords), geht in Razor-spezifischer Markups.</span><span class="sxs-lookup"><span data-stu-id="94ab3-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="94ab3-116">Andernfalls geht es in einfachen c#.</span><span class="sxs-lookup"><span data-stu-id="94ab3-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="94ab3-117">Mit Escapezeichen eine `@` symbol in Razor-Markup, verwenden Sie eine zweite `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="94ab3-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="94ab3-118">Der Code wird in HTML gerendert, mit einem einzelnen `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="94ab3-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="94ab3-119">HTML-Attribute und Inhalt mit e-Mail-Adressen nicht behandeln die `@` Symbol als ein Zeichen Übergang.</span><span class="sxs-lookup"><span data-stu-id="94ab3-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="94ab3-120">Die e-Mail-Adressen im folgenden Beispiel werden durch Analysieren von Razor bleiben unverändert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="94ab3-121">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="94ab3-121">Implicit Razor expressions</span></span>

<span data-ttu-id="94ab3-122">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code:</span><span class="sxs-lookup"><span data-stu-id="94ab3-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="94ab3-123">Mit Ausnahme von C#- `await` Schlüsselwort implicit-Ausdrücken keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="94ab3-124">Wenn die C#-Anweisung eine klare beendet wurde, können Leerzeichen vermischt werden:</span><span class="sxs-lookup"><span data-stu-id="94ab3-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="94ab3-125">Implicit-Ausdrücken **kann nicht** C#-Generika, als eines der Zeichen innerhalb der Klammern enthalten (`<>`) als HTML-Tags interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="94ab3-126">Der folgende Code ist **nicht** gültig:</span><span class="sxs-lookup"><span data-stu-id="94ab3-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="94ab3-127">Der vorhergehende Code generiert einen Compilerfehler, der einen der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="94ab3-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="94ab3-128">Das Element "Int" wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="94ab3-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="94ab3-129">Alle Elemente muss entweder selbst schließen oder ein entsprechendes Endtag vorhanden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="94ab3-130">Die Methodengruppe "GenericMethod" nicht mit Typ "Object" Delegaten kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="94ab3-131">Wollten Sie die Methode aufrufen? "</span><span class="sxs-lookup"><span data-stu-id="94ab3-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="94ab3-132">Generische Methodenaufrufen, müssen umschlossen werden ein [expliziter Razor-Ausdruck](#explicit-razor-expressions) oder ein [Razor-Codeblock](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="94ab3-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="94ab3-133">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="94ab3-133">Explicit Razor expressions</span></span>

<span data-ttu-id="94ab3-134">Explizite Razor-Ausdrücke bestehen aus einer `@` Symbolbreite ausgeglichene Klammer.</span><span class="sxs-lookup"><span data-stu-id="94ab3-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="94ab3-135">Zum Zeitpunkt der letzten Woche zu rendern, wird das folgende Razor-Markup verwendet:</span><span class="sxs-lookup"><span data-stu-id="94ab3-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="94ab3-136">Alle Inhalte innerhalb der `@()` Klammern ausgewertet und in die Ausgabe des gerendert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="94ab3-137">Implizite Ausdrücke, die im vorherigen Abschnitt beschriebenen darf keine Leerzeichen in der Regel enthalten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="94ab3-138">Im folgenden Code wird nicht eine Woche nach der aktuellen Uhrzeit subtrahiert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="94ab3-139">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="94ab3-140">EXPLICIT-Ausdrücken können zum Verketten von Text mit einem Ergebnis des Ausdrucks verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="94ab3-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="94ab3-141">Ohne die explizite Ausdruck `<p>Age@joe.Age</p>` so behandelt, als eine e-Mail-Adresse und `<p>Age@joe.Age</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="94ab3-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="94ab3-142">Wenn ein expliziter Ausdruck geschrieben `<p>Age33</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="94ab3-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="94ab3-143">EXPLICIT-Ausdrücken dienen zum Rendern der Ausgabe von generischen Methoden in *cshtml* Dateien.</span><span class="sxs-lookup"><span data-stu-id="94ab3-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="94ab3-144">In einem impliziten Ausdruck, der Zeichen innerhalb der Klammern (`<>`) als HTML-Tags interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="94ab3-145">Das folgende Markup wird **nicht** Razor gültig:</span><span class="sxs-lookup"><span data-stu-id="94ab3-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="94ab3-146">Der vorhergehende Code generiert einen Compilerfehler, der einen der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="94ab3-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="94ab3-147">Das Element "Int" wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="94ab3-147">The "int" element wasn't closed.</span></span> <span data-ttu-id="94ab3-148">Alle Elemente muss entweder selbst schließen oder ein entsprechendes Endtag vorhanden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-148">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="94ab3-149">Die Methodengruppe "GenericMethod" nicht mit Typ "Object" Delegaten kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="94ab3-150">Wollten Sie die Methode aufrufen? "</span><span class="sxs-lookup"><span data-stu-id="94ab3-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="94ab3-151">Das folgende Markup zeigt die richtige Methode schreiben, diesen Code.</span><span class="sxs-lookup"><span data-stu-id="94ab3-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="94ab3-152">Der Code wird als ein expliziter Ausdruck geschrieben:</span><span class="sxs-lookup"><span data-stu-id="94ab3-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="94ab3-153">Expression-Codierung</span><span class="sxs-lookup"><span data-stu-id="94ab3-153">Expression encoding</span></span>

<span data-ttu-id="94ab3-154">C#-Ausdrücke, die zu einer Zeichenfolge ausgewertet werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="94ab3-155">C#-Ausdrücke, die ergeben `IHtmlContent` sind direkt über gerendert `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="94ab3-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="94ab3-156">C#-Ausdrücke, ergeben nicht `IHtmlContent` werden in eine Zeichenfolge konvertiert `ToString` und codiert werden, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="94ab3-157">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="94ab3-158">Der HTML-Code wird im Browser als gezeigt:</span><span class="sxs-lookup"><span data-stu-id="94ab3-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="94ab3-159">`HtmlHelper.Raw`Ausgabe wird nicht codierte aber als HTML-Markup gerendert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="94ab3-160">Mit `HtmlHelper.Raw` unsanitized Benutzer Eingabe ist ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="94ab3-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="94ab3-161">Benutzereingabe möglicherweise böswilligen JavaScript- oder andere Exploits durchführen enthalten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="94ab3-162">Es ist schwierig, Bereinigen von Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="94ab3-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="94ab3-163">Vermeiden Sie die Verwendung `HtmlHelper.Raw` Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="94ab3-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="94ab3-164">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="94ab3-165">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="94ab3-165">Razor code blocks</span></span>

<span data-ttu-id="94ab3-166">Razor-Codeblöcke beginnen Sie mit `@` und eingeschlossen werden `{}`.</span><span class="sxs-lookup"><span data-stu-id="94ab3-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="94ab3-167">Im Gegensatz zu Ausdrücken wird nicht C#-Code in Codeblöcken gerendert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="94ab3-168">Codeblöcke Ausdrücke in einer Sicht gemeinsam zu nutzen die gleichen Bereich und werden in Reihenfolge definiert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="94ab3-169">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="94ab3-170">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="94ab3-170">Implicit transitions</span></span>

<span data-ttu-id="94ab3-171">In einem Codeblock die Standardsprache ist c#, aber die Razor-Seite können Übergang zurück zum HTML:</span><span class="sxs-lookup"><span data-stu-id="94ab3-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="94ab3-172">Explizite durch Trennzeichen getrennten Übergang</span><span class="sxs-lookup"><span data-stu-id="94ab3-172">Explicit delimited transition</span></span>

<span data-ttu-id="94ab3-173">Um einen Unterabschnitt eines Codeblocks definieren, die HTML gerendert werden soll, umschließen Sie die Zeichen für das Rendering mit den Razor  **\<Text >** Tag:</span><span class="sxs-lookup"><span data-stu-id="94ab3-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="94ab3-174">Verwenden Sie diesen Ansatz zum Rendern von HTML, die von einem HTML-Tags eingeschlossen ist nicht an.</span><span class="sxs-lookup"><span data-stu-id="94ab3-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="94ab3-175">Tritt auf, ein Razor-Laufzeitfehler, ohne ein HTML- oder Razor-Tag.</span><span class="sxs-lookup"><span data-stu-id="94ab3-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="94ab3-176">Die  **\<Text >** Tag ist nützlich, um die Leerzeichen beim Rendern von Inhalt zu steuern:</span><span class="sxs-lookup"><span data-stu-id="94ab3-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="94ab3-177">Nur der Inhalt zwischen die  **\<Text >** -Tags wird gerendert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="94ab3-178">Keine Leerzeichen vor oder nach der  **\<Text >** Tags in der HTML-Ausgabe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="94ab3-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="94ab3-179">Explizite Zeile Übergang mit @:</span><span class="sxs-lookup"><span data-stu-id="94ab3-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="94ab3-180">Um den Rest der eine ganze Zeile in einem Codeblock als HTML zu rendern, verwenden die `@:` Syntax:</span><span class="sxs-lookup"><span data-stu-id="94ab3-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="94ab3-181">Ohne die `@:` im Code wird ein Razor-Laufzeitfehler generiert.</span><span class="sxs-lookup"><span data-stu-id="94ab3-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="94ab3-182">Warnung: Zusätzliche `@` Zeichen in einer Razor-Datei können dazu führen, dass Ursache Compiler-Fehler bei den Anweisungen weiter unten in den Block.</span><span class="sxs-lookup"><span data-stu-id="94ab3-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="94ab3-183">Dieser Compilerfehler können schwierig sein, zu verstehen, da der tatsächliche Fehler vor den gemeldeten Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="94ab3-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="94ab3-184">Dieser Fehler tritt häufig nach mehrere implizite/explizite Ausdrücke in einen einzelnen Codeblock zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="94ab3-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="94ab3-185">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="94ab3-185">Control Structures</span></span>

<span data-ttu-id="94ab3-186">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="94ab3-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="94ab3-187">Alle Aspekte von Codeblöcken (Übergang zu Markup Inline-c#) auch gelten die folgenden Strukturen:</span><span class="sxs-lookup"><span data-stu-id="94ab3-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="94ab3-188">Konditionelle Abschnitte @if, ElseIf, #else- und@switch</span><span class="sxs-lookup"><span data-stu-id="94ab3-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="94ab3-189">`@if`Steuerelemente, wenn der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="94ab3-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="94ab3-190">`else`und `else if` erfordern die `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="94ab3-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="94ab3-191">Das folgende Markup zeigt, wie eine Switch-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="94ab3-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="94ab3-192">Schleifen @for, @foreach, @while, und @do während</span><span class="sxs-lookup"><span data-stu-id="94ab3-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="94ab3-193">Auf Vorlagen basierende HTML kann mit Schleifen Steueranweisungen gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="94ab3-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="94ab3-194">Um eine Liste der Personen zu rendern:</span><span class="sxs-lookup"><span data-stu-id="94ab3-194">To render a list of people:</span></span>

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

<span data-ttu-id="94ab3-195">Die folgenden Schleifen Anweisungen werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="94ab3-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="94ab3-196">Zusammengesetzte@using</span><span class="sxs-lookup"><span data-stu-id="94ab3-196">Compound @using</span></span>

<span data-ttu-id="94ab3-197">In c# ist ein `using` Anweisung wird verwendet, um sicherzustellen, dass ein Objekt wurde verworfen.</span><span class="sxs-lookup"><span data-stu-id="94ab3-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="94ab3-198">In Razor wird derselbe Mechanismus verwendet, um HTML-Hilfsmethoden zu erstellen, zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="94ab3-199">Im folgenden Code Rendern HTML-Hilfsmethoden ein Formulartag mit der `@using` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="94ab3-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="94ab3-200">Bereichsebene Aktionen können ausgeführt werden, mit [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="94ab3-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="94ab3-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="94ab3-201">@try, catch, finally</span></span>

<span data-ttu-id="94ab3-202">Behandlung von Ausnahmen ist vergleichbar mit c#:</span><span class="sxs-lookup"><span data-stu-id="94ab3-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="94ab3-203">Razor hat die Möglichkeit, kritische Abschnitte mit Lock-Anweisungen zu schützen:</span><span class="sxs-lookup"><span data-stu-id="94ab3-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="94ab3-204">Kommentare</span><span class="sxs-lookup"><span data-stu-id="94ab3-204">Comments</span></span>

<span data-ttu-id="94ab3-205">Razor unterstützt C#- und HTML-Kommentare:</span><span class="sxs-lookup"><span data-stu-id="94ab3-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="94ab3-206">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="94ab3-207">Razor-Kommentare werden vom Server entfernt, bevor auf der Webseite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="94ab3-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="94ab3-208">Razor verwendet `@*  *@` zur Begrenzung von Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="94ab3-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="94ab3-209">Der folgende Code ist auskommentiert, damit der Server kein Markup Rendern:</span><span class="sxs-lookup"><span data-stu-id="94ab3-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="94ab3-210">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="94ab3-210">Directives</span></span>

<span data-ttu-id="94ab3-211">Razor-Direktiven werden durch implicit-Ausdrücken mit reservierten Schlüsselwörtern folgenden dargestellt die `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="94ab3-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="94ab3-212">Eine Richtlinie ändert sich normalerweise die Möglichkeit, die eine Sicht wird analysiert oder andere Funktionalität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="94ab3-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="94ab3-213">Verstehen, wie der Razor-Code für eine Ansicht generiert erleichtert das Verständnis der Funktionsweise von Direktiven.</span><span class="sxs-lookup"><span data-stu-id="94ab3-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="94ab3-214">Der Code generiert eine Klasse, die etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="94ab3-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="94ab3-215">Weiter unten in diesem Artikel im Abschnitt [Anzeigen der Razor c#-Klasse, die für eine Sicht generiert](#viewing-the-razor-c-class-generated-for-a-view) wird erläutert, wie diese generierte Klasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="94ab3-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="94ab3-216">Die `@using` Direktive fügt c# `using` Richtlinie in die generierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="94ab3-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="94ab3-217">Die `@model` Richtlinie gibt den Typ des Modells an eine Ansicht übergeben:</span><span class="sxs-lookup"><span data-stu-id="94ab3-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="94ab3-218">In einer ASP.NET Core MVC-app mit einzelne Benutzerkonten erstellt die *Views/Account/Login.cshtml* -Ansicht enthält die folgende Deklaration einer Modell:</span><span class="sxs-lookup"><span data-stu-id="94ab3-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="94ab3-219">Die generierte Klasse erbt von `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="94ab3-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="94ab3-220">Razor macht eine `Model` Eigenschaft für den Zugriff auf das Modell an die Ansicht zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="94ab3-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="94ab3-221">Die `@model` Richtlinie gibt den Typ dieser Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="94ab3-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="94ab3-222">Die Richtlinie gibt an, die `T` in `RazorPage<T>` , dass die generierte, die Klasse die Sicht abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="94ab3-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="94ab3-223">Wenn die `@model` Richtlinie iisn't angegeben, die `Model` -Eigenschaft ist vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="94ab3-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="94ab3-224">Der Wert des Modells wird auf dem Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="94ab3-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="94ab3-225">Weitere Informationen finden Sie unter [stark typisierte Modelle und die @model Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="94ab3-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="94ab3-226">Die `@inherits` Richtlinie bietet vollen Zugriff auf die Klasse erbt von die Sicht:</span><span class="sxs-lookup"><span data-stu-id="94ab3-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="94ab3-227">Der folgende Code ist ein benutzerdefinierter Typ des Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="94ab3-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="94ab3-228">Die `CustomText` in einer Ansicht angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="94ab3-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="94ab3-229">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="94ab3-230">`@model`und `@inherits` in derselben Ansicht verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="94ab3-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="94ab3-231">`@inherits`kann einem *_ViewImports.cshtml* -Datei, die die Sicht importiert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="94ab3-232">Der folgende Code ist ein Beispiel für eine stark typisierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="94ab3-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="94ab3-233">Wenn "rick@contoso.com" übergeben wird im Modell, Ansicht die folgenden HTML-Markup generiert:</span><span class="sxs-lookup"><span data-stu-id="94ab3-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="94ab3-234">Die `@inject` Richtlinie ermöglicht die Razor-Seite zum Einfügen von eines Diensts von der [Dienstcontainer](xref:fundamentals/dependency-injection) in eine Sicht.</span><span class="sxs-lookup"><span data-stu-id="94ab3-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="94ab3-235">Weitere Informationen finden Sie unter [Abhängigkeitsinjektion in Sichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="94ab3-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="94ab3-236">Die `@functions` Richtlinie ermöglicht eine Razor-Seite, um eine Sicht Funktionsebene Inhalt hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="94ab3-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="94ab3-237">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="94ab3-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="94ab3-238">Der Code generiert den folgenden HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="94ab3-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="94ab3-239">Der folgende Code ist die generierte Razor C#-Klasse:</span><span class="sxs-lookup"><span data-stu-id="94ab3-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="94ab3-240">Die `@section` Richtlinie dient in Verbindung mit der [Layout](xref:mvc/views/layout) Ansichten zum Rendern von Inhalt in verschiedenen Teilen der HTML-Seite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="94ab3-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="94ab3-241">Weitere Informationen finden Sie unter [Abschnitte](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="94ab3-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="94ab3-242">Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="94ab3-242">Tag Helpers</span></span>

<span data-ttu-id="94ab3-243">Es gibt drei Anweisungen, die betreffen [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="94ab3-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="94ab3-244">Direktive</span><span class="sxs-lookup"><span data-stu-id="94ab3-244">Directive</span></span> | <span data-ttu-id="94ab3-245">Funktion</span><span class="sxs-lookup"><span data-stu-id="94ab3-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="94ab3-246">Stellt Tag Hilfen zu einer Ansicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="94ab3-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="94ab3-247">Entfernt die Tag-Hilfsprogramme, die zuvor aus einer Sicht hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="94ab3-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="94ab3-248">Gibt ein Tagpräfix Tag Helper-Unterstützung zu aktivieren und Tag Helper Verwendung als explizite Anforderung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="94ab3-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="94ab3-249">Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="94ab3-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="94ab3-250">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="94ab3-250">Razor keywords</span></span>

* <span data-ttu-id="94ab3-251">Seite "(erfordert ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="94ab3-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="94ab3-252">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="94ab3-252">functions</span></span>
* <span data-ttu-id="94ab3-253">inherits</span><span class="sxs-lookup"><span data-stu-id="94ab3-253">inherits</span></span>
* <span data-ttu-id="94ab3-254">Modell</span><span class="sxs-lookup"><span data-stu-id="94ab3-254">model</span></span>
* <span data-ttu-id="94ab3-255">section</span><span class="sxs-lookup"><span data-stu-id="94ab3-255">section</span></span>
* <span data-ttu-id="94ab3-256">Hilfsprogramm (von ASP.NET Core derzeit nicht unterstützt)</span><span class="sxs-lookup"><span data-stu-id="94ab3-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="94ab3-257">Razor-Schlüsselwörter werden mit Escapezeichen versehen `@(Razor Keyword)` (z. B. `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="94ab3-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="94ab3-258">C#-Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="94ab3-258">C# Razor keywords</span></span>

* <span data-ttu-id="94ab3-259">Groß- und Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="94ab3-259">case</span></span>
* <span data-ttu-id="94ab3-260">do</span><span class="sxs-lookup"><span data-stu-id="94ab3-260">do</span></span>
* <span data-ttu-id="94ab3-261">default</span><span class="sxs-lookup"><span data-stu-id="94ab3-261">default</span></span>
* <span data-ttu-id="94ab3-262">for</span><span class="sxs-lookup"><span data-stu-id="94ab3-262">for</span></span>
* <span data-ttu-id="94ab3-263">foreach</span><span class="sxs-lookup"><span data-stu-id="94ab3-263">foreach</span></span>
* <span data-ttu-id="94ab3-264">if</span><span class="sxs-lookup"><span data-stu-id="94ab3-264">if</span></span>
* <span data-ttu-id="94ab3-265">else</span><span class="sxs-lookup"><span data-stu-id="94ab3-265">else</span></span>
* <span data-ttu-id="94ab3-266">lock</span><span class="sxs-lookup"><span data-stu-id="94ab3-266">lock</span></span>
* <span data-ttu-id="94ab3-267">switch</span><span class="sxs-lookup"><span data-stu-id="94ab3-267">switch</span></span>
* <span data-ttu-id="94ab3-268">try</span><span class="sxs-lookup"><span data-stu-id="94ab3-268">try</span></span>
* <span data-ttu-id="94ab3-269">catch</span><span class="sxs-lookup"><span data-stu-id="94ab3-269">catch</span></span>
* <span data-ttu-id="94ab3-270">finally</span><span class="sxs-lookup"><span data-stu-id="94ab3-270">finally</span></span>
* <span data-ttu-id="94ab3-271">Verwenden</span><span class="sxs-lookup"><span data-stu-id="94ab3-271">using</span></span>
* <span data-ttu-id="94ab3-272">while</span><span class="sxs-lookup"><span data-stu-id="94ab3-272">while</span></span>

<span data-ttu-id="94ab3-273">C#-Razor-Schlüsselwörter muss mit doppelten Escapezeichen `@(@C# Razor Keyword)` (z. B. `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="94ab3-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="94ab3-274">Die erste `@` versieht den Razor-Parser.</span><span class="sxs-lookup"><span data-stu-id="94ab3-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="94ab3-275">Die zweite `@` versieht den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="94ab3-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="94ab3-276">Reservierte Schlüsselwörter, die nicht vom Razor verwendet</span><span class="sxs-lookup"><span data-stu-id="94ab3-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="94ab3-277">namespace</span><span class="sxs-lookup"><span data-stu-id="94ab3-277">namespace</span></span>
* <span data-ttu-id="94ab3-278">Klasse</span><span class="sxs-lookup"><span data-stu-id="94ab3-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="94ab3-279">Anzeigen von für eine Sicht generiert Razor C#-Klasse.</span><span class="sxs-lookup"><span data-stu-id="94ab3-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="94ab3-280">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="94ab3-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="94ab3-281">Überschreiben Sie die `RazorTemplateEngine` hinzugefügt, indem MVC mit der `CustomTemplateEngine` Klasse:</span><span class="sxs-lookup"><span data-stu-id="94ab3-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="94ab3-282">Legen Sie einen Haltepunkt für die `return csharpDocument` SoH `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="94ab3-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="94ab3-283">Bei der Ausführung des Programms am Haltepunkt beendet wird, zeigen Sie den Wert des `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="94ab3-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Text-Schnellansicht Überblick generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="94ab3-285">Ansicht Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="94ab3-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="94ab3-286">Das Razor-Ansichtsmodul führt die Groß-/Kleinschreibung Suchvorgänge für Ansichten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="94ab3-287">Allerdings wird die tatsächliche Suche durch das zugrunde liegende Dateisystem bestimmt:</span><span class="sxs-lookup"><span data-stu-id="94ab3-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="94ab3-288">Basis-Dateiquelle:</span><span class="sxs-lookup"><span data-stu-id="94ab3-288">File based source:</span></span> 
  * <span data-ttu-id="94ab3-289">Unter Betriebssystemen mit Groß-/Kleinschreibung beachten Dateisystemen (z. B. Windows) sind die physische Datei Anbieter Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="94ab3-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="94ab3-290">Beispielsweise `return View("Test")` ergibt Übereinstimmungen für */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, und eine andere Variante der Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="94ab3-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="94ab3-291">Auf Dateisystemen, Groß-/Kleinschreibung (z. B. Linux, OS x, und mit `EmbeddedFileProvider`), Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="94ab3-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="94ab3-292">Beispielsweise `return View("Test")` speziell Übereinstimmungen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="94ab3-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="94ab3-293">Vorkompilierte Sichten: Nachschlagen vorkompilierte Sichten mit ASP.NET Core 2.0 und höher, Groß-/Kleinschreibung beachtet, unter allen Betriebssystemen ist.</span><span class="sxs-lookup"><span data-stu-id="94ab3-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="94ab3-294">Das Verhalten ist identisch mit dem Verhalten der physischen Datei des Anbieters unter Windows.</span><span class="sxs-lookup"><span data-stu-id="94ab3-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="94ab3-295">Wenn zwei vorkompilierte Sichten nur im Fall unterschiedlich sind, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="94ab3-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="94ab3-296">Entwicklern wird empfohlen, die Groß-/Kleinschreibung von Datei- und Verzeichnisnamen, die Groß-/Kleinschreibung der entsprechen:</span><span class="sxs-lookup"><span data-stu-id="94ab3-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="94ab3-297">Namen von Bereich, Controller und Aktion.</span><span class="sxs-lookup"><span data-stu-id="94ab3-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="94ab3-298">Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="94ab3-298">Razor Pages.</span></span>
    
<span data-ttu-id="94ab3-299">Groß-wird sichergestellt, dass die Bereitstellungen ihrer Ansichten unabhängig von der zugrunde liegenden Dateisystem suchen.</span><span class="sxs-lookup"><span data-stu-id="94ab3-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
