---
title: "Razor-Syntaxverweis für ASP.NET Core"
author: rick-anderson
description: Informationen zur Razor-Markupsyntax zum Einbetten von serverbasiertem Code in Webseiten
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 98021cc76555f0c1402764c845471a4730b01b20
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="34686-103">Razor-Syntax für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34686-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="34686-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) und [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="34686-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="34686-105">Razor stellt eine Markupsyntax zum Einbetten von serverbasiertem Code in Webseiten dar.</span><span class="sxs-lookup"><span data-stu-id="34686-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="34686-106">Die Razor-Syntax besteht aus dem Razor-Markup, C# und HTML.</span><span class="sxs-lookup"><span data-stu-id="34686-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="34686-107">Dateien, die Razor enthalten, besitzen in der Regel die Dateierweiterung *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="34686-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="34686-108">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="34686-108">Rendering HTML</span></span>

<span data-ttu-id="34686-109">Die Standardsprache für Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="34686-109">The default Razor language is HTML.</span></span> <span data-ttu-id="34686-110">Das Rendern von HTML-Code aus einem Razor-Markup funktioniert genauso wie das Rendern von HTML aus einer HTML-Datei.</span><span class="sxs-lookup"><span data-stu-id="34686-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="34686-111">Ein HTML-Markup wird in Razor-Dateien mit der Erweiterung *.cshtml* vom Server unverändert gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="34686-112">Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="34686-112">Razor syntax</span></span>

<span data-ttu-id="34686-113">Razor unterstützt C# und verwendet für den Übergang von HTML zu C# das `@`-Symbol.</span><span class="sxs-lookup"><span data-stu-id="34686-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="34686-114">Razor wertet C#-Ausdrücke aus und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="34686-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="34686-115">Folgt auf ein `@`-Symbol ein für [Razor reserviertes Schlüsselwort](#razor-reserved-keywords), wechselt es in ein Razor-spezifisches Markup.</span><span class="sxs-lookup"><span data-stu-id="34686-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="34686-116">Andernfalls geht es in reines C# über.</span><span class="sxs-lookup"><span data-stu-id="34686-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="34686-117">Verwenden Sie ein zweites `@`-Symbol, um im Razor-Markup ein `@`-Symbol mit Escapezeichen zu versehen:</span><span class="sxs-lookup"><span data-stu-id="34686-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="34686-118">Der Code wird in HTML mit einem einzelnen `@`-Symbol gerendert:</span><span class="sxs-lookup"><span data-stu-id="34686-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="34686-119">Bei HTML-Attributen und Inhalt mit E-Mail-Adressen wird das `@`-Symbol nicht als ein Übergangszeichen behandelt.</span><span class="sxs-lookup"><span data-stu-id="34686-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="34686-120">Die E-Mail-Adressen im folgenden Beispiel bleiben von der Razor-Analyse unverändert:</span><span class="sxs-lookup"><span data-stu-id="34686-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="34686-121">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="34686-121">Implicit Razor expressions</span></span>

<span data-ttu-id="34686-122">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="34686-123">Mit Ausnahme des `await`-C#-Schlüsselworts dürfen implizite Ausdrücke keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="34686-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="34686-124">Wird die C#-Anweisung eindeutig beendet, können auch Leerzeichen verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="34686-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="34686-125">Implizite Ausdrücke **dürfen keine** C#-Generika enthalten, da die Zeichen innerhalb der Klammern (`<>`) als HTML-Tag interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="34686-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="34686-126">Der folgende Code ist **ungültig**:</span><span class="sxs-lookup"><span data-stu-id="34686-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="34686-127">Der vorhergehende Code erzeugt einen Compilerfehler, der folgendermaßen aussehen kann:</span><span class="sxs-lookup"><span data-stu-id="34686-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="34686-128">Das Element „int“ wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="34686-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="34686-129">Alle Elemente müssen entweder selbstschließend sein oder einen übereinstimmenden Endtag besitzen.</span><span class="sxs-lookup"><span data-stu-id="34686-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="34686-130">Die Methodengruppe „GenericMethod“ kann nicht in den Nichtdelegattyp „Objekt“ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="34686-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="34686-131">Wollten Sie die Methode aufrufen?</span><span class="sxs-lookup"><span data-stu-id="34686-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="34686-132">Generische Methodenaufrufe müssen von einem [expliziten Razor-Ausdruck](#explicit-razor-expressions) oder einem [Razor-Codeblock](#razor-code-blocks) umschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="34686-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="34686-133">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="34686-133">Explicit Razor expressions</span></span>

<span data-ttu-id="34686-134">Explizite Razor-Ausdrücke bestehen aus einem `@`-Symbol mit ausgeglichener Klammer.</span><span class="sxs-lookup"><span data-stu-id="34686-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="34686-135">Zum Rendern der Uhrzeit von letzter Woche wird folgendes Razor-Markup verwendet:</span><span class="sxs-lookup"><span data-stu-id="34686-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="34686-136">Jeglicher Inhalt innerhalb der `@()`-Klammer wird ausgewertet und in der Ausgabe gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="34686-137">Die im vorherigen Abschnitt beschriebenen impliziten Ausdrücke dürfen in der Regel keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="34686-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="34686-138">Im folgenden Code wird eine Woche nicht von der aktuellen Uhrzeit abgezogen:</span><span class="sxs-lookup"><span data-stu-id="34686-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="34686-139">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="34686-140">Explizite Ausdrücke können zum Verketten von Text mit einem Ergebnis des Ausdrucks verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="34686-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="34686-141">Ohne den expliziten Ausdruck wird `<p>Age@joe.Age</p>` als E-Mail-Adresse behandelt und `<p>Age@joe.Age</p>` gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="34686-142">`<p>Age33</p>` wird gerendert, wenn es als expliziter Ausdruck geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="34686-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="34686-143">Explizite Ausdrücke können zum Rendern der Ausgabe von generischen Methoden in *.cshtml*-Dateien verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="34686-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="34686-144">Bei impliziten Ausdrücken werden die Zeichen innerhalb der Klammern (`<>`) als HTML-Tag interpretiert.</span><span class="sxs-lookup"><span data-stu-id="34686-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="34686-145">Das folgende Markup ist **keine gültige** Razor-Schreibweise:</span><span class="sxs-lookup"><span data-stu-id="34686-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="34686-146">Der vorhergehende Code erzeugt einen Compilerfehler, der folgendermaßen aussehen kann:</span><span class="sxs-lookup"><span data-stu-id="34686-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="34686-147">Das Element „int“ wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="34686-147">The "int" element wasn't closed.</span></span> <span data-ttu-id="34686-148">Alle Elemente müssen entweder selbstschließend sein oder einen übereinstimmenden Endtag besitzen.</span><span class="sxs-lookup"><span data-stu-id="34686-148">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="34686-149">Die Methodengruppe „GenericMethod“ kann nicht in den Nichtdelegattyp „Objekt“ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="34686-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="34686-150">Wollten Sie die Methode aufrufen?</span><span class="sxs-lookup"><span data-stu-id="34686-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="34686-151">Das folgende Markup zeigt die korrekte Schreibweise dieses Codes.</span><span class="sxs-lookup"><span data-stu-id="34686-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="34686-152">Der Code wird als expliziter Ausdruck geschrieben:</span><span class="sxs-lookup"><span data-stu-id="34686-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="34686-153">Codieren von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="34686-153">Expression encoding</span></span>

<span data-ttu-id="34686-154">C#-Ausdrücke, die mit einer Zeichenfolge ausgewertet werden, werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="34686-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="34686-155">C#-Ausdrücke, die mit `IHtmlContent` ausgewertet werden, werden direkt durch `IHtmlContent.WriteTo` gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="34686-156">C#-Ausdrücke, die nicht mit `IHtmlContent` ausgewertet werden, werden durch `ToString` in eine Zeichenfolge konvertiert und codiert, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="34686-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="34686-157">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="34686-158">Der HTML-Code wird im Browser folgendermaßen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="34686-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="34686-159">Die Ausgabe `HtmlHelper.Raw` wird nicht codiert, sondern als HTML-Markup gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="34686-160">Die Verwendung von `HtmlHelper.Raw` bei einer nicht bereinigten Benutzereingabe stellt ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="34686-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="34686-161">Benutzereingaben können schädlichen JavaScript-Code oder andere Sicherheitsrisiken enthalten.</span><span class="sxs-lookup"><span data-stu-id="34686-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="34686-162">Das Bereinigen von Benutzereingaben ist schwierig.</span><span class="sxs-lookup"><span data-stu-id="34686-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="34686-163">Vermeiden Sie daher die Verwendung von `HtmlHelper.Raw` bei Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="34686-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="34686-164">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="34686-165">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="34686-165">Razor code blocks</span></span>

<span data-ttu-id="34686-166">Razor-Codeblöcke beginnen mit `@` und sind von `{}` eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="34686-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="34686-167">Im Gegensatz zu Ausdrücken wird C#-Code in Codeblöcken nicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="34686-168">Codeblöcke und Ausdrücke in einer Ansicht nutzen den gleichen Bereich und werden der Reihe nach definiert:</span><span class="sxs-lookup"><span data-stu-id="34686-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="34686-169">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="34686-170">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="34686-170">Implicit transitions</span></span>

<span data-ttu-id="34686-171">Die Standardsprache in einem Codeblock ist C#. Die Razor-Seite kann jedoch zurück zu HTML wechseln:</span><span class="sxs-lookup"><span data-stu-id="34686-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="34686-172">Durch Trennzeichen getrennte explizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="34686-172">Explicit delimited transition</span></span>

<span data-ttu-id="34686-173">Soll ein Unterabschnitt eines Codeblocks in HTML gerendert werden, umschließen Sie die Zeichen, die gerendert werden sollen, mit dem Razor-Tag **\<text>**:</span><span class="sxs-lookup"><span data-stu-id="34686-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="34686-174">Verwenden Sie diese Methode zum Rendern von HTML-Code, der nicht von einem HTML-Tag umschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="34686-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="34686-175">Ohne HTML- oder Razor-Tag tritt ein Razor-Laufzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="34686-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="34686-176">Der **\<text>**-Tag ist nützlich, wenn Sie beim Rendern von Inhalt Leerzeichen steuern möchten:</span><span class="sxs-lookup"><span data-stu-id="34686-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="34686-177">Nur der Inhalt zwischen dem **\<text>**-Tag wird gerendert.</span><span class="sxs-lookup"><span data-stu-id="34686-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="34686-178">In der HTML-Ausgabe werden keine Leerzeichen vor oder nach dem **\<text>**-Tag angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34686-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="34686-179">Explizite Zeilenübergänge mit @:</span><span class="sxs-lookup"><span data-stu-id="34686-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="34686-180">Verwenden Sie die `@:`-Syntax, um den Rest einer kompletten Zeile als HTML-Code in einem Codeblock zu rendern:</span><span class="sxs-lookup"><span data-stu-id="34686-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="34686-181">Ohne das `@:`-Symbol im Code wird ein Razor-Laufzeitfehler erzeugt.</span><span class="sxs-lookup"><span data-stu-id="34686-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="34686-182">Warnung: Zusätzliche `@`-Zeichen in einer Razor-Datei können zu Compilerfehlern bei späteren Anweisungen im Block führen.</span><span class="sxs-lookup"><span data-stu-id="34686-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="34686-183">Diese Compilerfehler können dann schwer nachvollziehbar sein, da der tatsächliche Fehler vor dem gemeldeten Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="34686-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="34686-184">Dieser Fehler tritt häufig auf, wenn mehrere implizite/explizite Ausdrücke in einem einzigen Codeblock kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="34686-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="34686-185">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="34686-185">Control Structures</span></span>

<span data-ttu-id="34686-186">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="34686-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="34686-187">Alle Aspekte von Codeblöcken (Übergang zu Markup, Inline-C#) gelten auch für die folgenden Strukturen:</span><span class="sxs-lookup"><span data-stu-id="34686-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="34686-188">Die Bedingungen @if, else if, else und @switch</span><span class="sxs-lookup"><span data-stu-id="34686-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="34686-189">`@if` steuert, wenn der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="34686-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="34686-190">`else` und `else if` erfordern kein `@`-Symbol:</span><span class="sxs-lookup"><span data-stu-id="34686-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="34686-191">Im folgenden Markup wird die Verwendung einer switch-Anweisung veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="34686-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="34686-192">Die Schleifen @for, @foreach, @while und @do while</span><span class="sxs-lookup"><span data-stu-id="34686-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="34686-193">Auf Vorlagen basierender HTML-Code kann mit Schleifen-Steuerungsanweisungen gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="34686-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="34686-194">Zum Rendern einer Liste von Personen:</span><span class="sxs-lookup"><span data-stu-id="34686-194">To render a list of people:</span></span>

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

<span data-ttu-id="34686-195">Die folgenden Schleifenanweisungen werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="34686-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="34686-196">Zusammengesetztes @using</span><span class="sxs-lookup"><span data-stu-id="34686-196">Compound @using</span></span>

<span data-ttu-id="34686-197">In C# wird eine `using`-Anweisung verwendet, um sicherzustellen, dass ein Objekt freigegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="34686-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="34686-198">In Razor wird derselbe Mechanismus verwendet, um HTML-Hilfsprogramme zu erstellen, die zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="34686-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="34686-199">Im folgenden Code wird ein Formulartag mit der `@using`-Anweisung durch HTML-Hilfsprogramme gerendert:</span><span class="sxs-lookup"><span data-stu-id="34686-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="34686-200">Aktionen auf Bereichsebene können mit [Taghilfsprogrammen](xref:mvc/views/tag-helpers/intro) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="34686-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="34686-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="34686-201">@try, catch, finally</span></span>

<span data-ttu-id="34686-202">Die Behandlung von Ausnahmen ist vergleichbar mit C#:</span><span class="sxs-lookup"><span data-stu-id="34686-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="34686-203">Razor kann kritische Abschnitte mit lock-Anweisungen schützen:</span><span class="sxs-lookup"><span data-stu-id="34686-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="34686-204">Kommentare</span><span class="sxs-lookup"><span data-stu-id="34686-204">Comments</span></span>

<span data-ttu-id="34686-205">Razor unterstützt C#- und HTML-Kommentare:</span><span class="sxs-lookup"><span data-stu-id="34686-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="34686-206">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="34686-207">Razor-Kommentare werden vom Server entfernt, bevor die Webseite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="34686-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="34686-208">Zur Abgrenzung der Kommentare verwendet Razor `@*  *@`.</span><span class="sxs-lookup"><span data-stu-id="34686-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="34686-209">Der folgende Code ist auskommentiert, damit vom Server kein Markup gerendert wird:</span><span class="sxs-lookup"><span data-stu-id="34686-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="34686-210">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="34686-210">Directives</span></span>

<span data-ttu-id="34686-211">Razor-Anweisungen werden durch implizite Ausdrücke mit reservierten Schlüsselwörtern nach dem `@`-Symbol dargestellt.</span><span class="sxs-lookup"><span data-stu-id="34686-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="34686-212">Eine Anweisung ändert in der Regel die Analyse einer Ansicht oder aktiviert unterschiedliche Funktionen.</span><span class="sxs-lookup"><span data-stu-id="34686-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="34686-213">Wenn Sie wissen, wie von Razor Code für eine Ansicht generiert wird, erleichtert dies das Verständnis dafür, wie Anweisungen funktionieren.</span><span class="sxs-lookup"><span data-stu-id="34686-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="34686-214">Durch den Code wird eine Klasse ähnlich der folgenden Klasse generiert:</span><span class="sxs-lookup"><span data-stu-id="34686-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="34686-215">Im Abschnitt [Anzeigen der Razor-C#-Klasse, die für eine Ansicht generiert wurde](#viewing-the-razor-c-class-generated-for-a-view) weiter unten wird erklärt, wie die generierte Klasse angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="34686-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="34686-216">Die `@using`-Anweisung fügt die `using`-C#-Anweisung der generierten Ansicht hinzu:</span><span class="sxs-lookup"><span data-stu-id="34686-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="34686-217">Die `@model`-Anweisung gibt den Typ des Modells an, das an eine Ansicht übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="34686-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="34686-218">In einer mit einzelnen Benutzerkonten erstellten ASP.NET Core MVC-App enthält die Ansicht *Views/Account/Login.cshtml* die folgende Modelldeklaration:</span><span class="sxs-lookup"><span data-stu-id="34686-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="34686-219">Die generierte Klasse erbt von `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="34686-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="34686-220">Razor macht eine `Model`-Eigenschaft für den Zugriff auf das an die Ansicht übergebene Modell verfügbar:</span><span class="sxs-lookup"><span data-stu-id="34686-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="34686-221">Die `@model`-Anweisung gibt den Typ dieser Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="34686-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="34686-222">Die Anweisung gibt das `T` in `RazorPage<T>` der generierten Klasse an, von der die Ansicht abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="34686-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="34686-223">Wird die `@model`-Anweisung nicht angegeben, ist die `Model`-Eigenschaft vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="34686-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="34686-224">Der Wert des Modells wird vom Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="34686-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="34686-225">Weitere Informationen finden Sie unter „Stark typisierte Modelle und das Schlüsselwort @model“.</span><span class="sxs-lookup"><span data-stu-id="34686-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="34686-226">Die `@inherits`-Anweisung bietet uneingeschränkten Zugriff auf die Klasse, die die Ansicht erbt:</span><span class="sxs-lookup"><span data-stu-id="34686-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="34686-227">Der folgende Code ist ein benutzerdefinierter Typ der Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="34686-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="34686-228">`CustomText` wird in einer Ansicht angezeigt:</span><span class="sxs-lookup"><span data-stu-id="34686-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="34686-229">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="34686-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="34686-230">`@model` und `@inherits` können in derselben Ansicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="34686-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="34686-231">`@inherits` kann in einer *_ViewImports.cshtml*-Datei verwendet werden, die von der Ansicht importiert wird:</span><span class="sxs-lookup"><span data-stu-id="34686-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="34686-232">Der folgende Code ist ein Beispiel für eine stark typisierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="34686-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="34686-233">Wird „rick@contoso.com“ im Modell übergeben, generiert die Ansicht das folgende HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="34686-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="34686-234">Mit der `@inject`-Anweisung kann die Razor-Seite einen Dienst vom [Dienstcontainer](xref:fundamentals/dependency-injection) in eine Ansicht einfügen.</span><span class="sxs-lookup"><span data-stu-id="34686-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="34686-235">Weitere Informationen finden Sie unter [Dependency Injection in Ansichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="34686-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="34686-236">Mit der `@functions`-Anweisung kann eine Razor-Seite einer Ansicht Inhalt auf Funktionsebene hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="34686-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="34686-237">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="34686-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="34686-238">Der Code generiert das folgende HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="34686-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="34686-239">Der folgende Code wird in der Razor-C#-Klasse generiert:</span><span class="sxs-lookup"><span data-stu-id="34686-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="34686-240">Die `@section`-Anweisung wird in Verbindung mit dem [Layout](xref:mvc/views/layout) verwendet, damit Ansichten Inhalt in verschiedenen Teilen der HTML-Seite rendern können.</span><span class="sxs-lookup"><span data-stu-id="34686-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="34686-241">Weitere Informationen finden Sie unter [Abschnitte](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="34686-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="34686-242">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="34686-242">Tag Helpers</span></span>

<span data-ttu-id="34686-243">Die folgenden drei Anweisungen gehören zu den [Taghilfsprogrammen](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="34686-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="34686-244">Direktive</span><span class="sxs-lookup"><span data-stu-id="34686-244">Directive</span></span> | <span data-ttu-id="34686-245">Funktion</span><span class="sxs-lookup"><span data-stu-id="34686-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="34686-246">Macht Taghilfsprogramme für eine Ansicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="34686-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="34686-247">Entfernt die zuvor aus einer Ansicht hinzugefügten Taghilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="34686-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="34686-248">Gibt ein Tagpräfix an, um Unterstützung für Taghilfsprogramme zu aktivieren und ihre Verwendung explizit festzulegen.</span><span class="sxs-lookup"><span data-stu-id="34686-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="34686-249">Für Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="34686-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="34686-250">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="34686-250">Razor keywords</span></span>

* <span data-ttu-id="34686-251">page (ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="34686-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="34686-252">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="34686-252">functions</span></span>
* <span data-ttu-id="34686-253">inherits</span><span class="sxs-lookup"><span data-stu-id="34686-253">inherits</span></span>
* <span data-ttu-id="34686-254">Modell</span><span class="sxs-lookup"><span data-stu-id="34686-254">model</span></span>
* <span data-ttu-id="34686-255">section</span><span class="sxs-lookup"><span data-stu-id="34686-255">section</span></span>
* <span data-ttu-id="34686-256">helper (wird derzeit nicht von ASP.NET Core unterstützt)</span><span class="sxs-lookup"><span data-stu-id="34686-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="34686-257">Razor-Schlüsselwörter werden mit dem Escapezeichen `@(Razor Keyword)` versehen (z.B. `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="34686-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="34686-258">Razor-C#-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="34686-258">C# Razor keywords</span></span>

* <span data-ttu-id="34686-259">Groß- und Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="34686-259">case</span></span>
* <span data-ttu-id="34686-260">do</span><span class="sxs-lookup"><span data-stu-id="34686-260">do</span></span>
* <span data-ttu-id="34686-261">default</span><span class="sxs-lookup"><span data-stu-id="34686-261">default</span></span>
* <span data-ttu-id="34686-262">for</span><span class="sxs-lookup"><span data-stu-id="34686-262">for</span></span>
* <span data-ttu-id="34686-263">foreach</span><span class="sxs-lookup"><span data-stu-id="34686-263">foreach</span></span>
* <span data-ttu-id="34686-264">if</span><span class="sxs-lookup"><span data-stu-id="34686-264">if</span></span>
* <span data-ttu-id="34686-265">else</span><span class="sxs-lookup"><span data-stu-id="34686-265">else</span></span>
* <span data-ttu-id="34686-266">lock</span><span class="sxs-lookup"><span data-stu-id="34686-266">lock</span></span>
* <span data-ttu-id="34686-267">switch</span><span class="sxs-lookup"><span data-stu-id="34686-267">switch</span></span>
* <span data-ttu-id="34686-268">try</span><span class="sxs-lookup"><span data-stu-id="34686-268">try</span></span>
* <span data-ttu-id="34686-269">catch</span><span class="sxs-lookup"><span data-stu-id="34686-269">catch</span></span>
* <span data-ttu-id="34686-270">finally</span><span class="sxs-lookup"><span data-stu-id="34686-270">finally</span></span>
* <span data-ttu-id="34686-271">Verwenden</span><span class="sxs-lookup"><span data-stu-id="34686-271">using</span></span>
* <span data-ttu-id="34686-272">while</span><span class="sxs-lookup"><span data-stu-id="34686-272">while</span></span>

<span data-ttu-id="34686-273">Razor-C#-Schlüsselwörter werden mit dem doppeltem Escapezeichen `@(@C# Razor Keyword)` versehen (z.B. `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="34686-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="34686-274">Das erste `@` dient als Escapezeichen für den Razor-Parser.</span><span class="sxs-lookup"><span data-stu-id="34686-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="34686-275">Das zweite `@` dient als Escapezeichen für den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="34686-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="34686-276">Reservierte Schlüsselwörter, die nicht von Razor verwendet werden</span><span class="sxs-lookup"><span data-stu-id="34686-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="34686-277">namespace</span><span class="sxs-lookup"><span data-stu-id="34686-277">namespace</span></span>
* <span data-ttu-id="34686-278">Klasse</span><span class="sxs-lookup"><span data-stu-id="34686-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="34686-279">Anzeigen der Razor-C#-Klasse, die für eine Ansicht generiert wurde</span><span class="sxs-lookup"><span data-stu-id="34686-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="34686-280">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="34686-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="34686-281">Überschreiben Sie das von MVC hinzugefügte `RazorTemplateEngine` mit der `CustomTemplateEngine`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="34686-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="34686-282">Setzen Sie auf der `return csharpDocument`-Anweisung von `CustomTemplateEngine` einen Haltepunkt.</span><span class="sxs-lookup"><span data-stu-id="34686-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="34686-283">Wenn die Ausführung des Programms am Haltepunkt angehalten wird, können Sie den Wert von `generatedCode` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="34686-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Ansicht „Text-Schnellansicht“ von „generatedCode“](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="34686-285">Ansicht der Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="34686-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="34686-286">Die Razor-Ansichtsengine führt für Ansichten Suchvorgänge aus, die Groß- und Kleinschreibung berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="34686-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="34686-287">Der tatsächliche Suchvorgang wird jedoch vom zugrunde liegenden Dateisystem bestimmt:</span><span class="sxs-lookup"><span data-stu-id="34686-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="34686-288">Dateibasierte Quelle:</span><span class="sxs-lookup"><span data-stu-id="34686-288">File based source:</span></span> 
  * <span data-ttu-id="34686-289">Bei Betriebssystemen, die Dateisysteme ohne Berücksichtigung von Groß-/Kleinschreibung verwenden (z.B. Windows), wird bei Suchvorgängen nach physischen Dateianbietern die Groß- und Kleinschreibung nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="34686-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="34686-290">`return View("Test")` liefert beispielsweise eine Übereinstimmung für */Views/Home/Test.cshtml*, */Views/home/test.cshtml* sowie für jede andere Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="34686-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="34686-291">Bei Dateisystemen, die Groß-/Kleinschreibung berücksichtigen (z.B. Linux, OSX sowie mit `EmbeddedFileProvider`), wird auch bei Suchvorgängen die Groß- und Kleinschreibung berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="34686-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="34686-292">`return View("Test")` liefert beispielsweise speziell eine Übereinstimmung mit */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="34686-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="34686-293">Vorkompilierte Ansichten: Ab ASP.NET Core 2.0 wird bei der Suche nach vorkompilierten Ansichten unter allen Betriebssystemen die Groß- und Kleinschreibung nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="34686-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="34686-294">Das Verhalten ist mit dem Verhalten des physischen Dateianbieters unter Windows identisch.</span><span class="sxs-lookup"><span data-stu-id="34686-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="34686-295">Unterscheiden sich zwei vorkompilierte Ansichten nur in der Groß-/Kleinschreibung, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="34686-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="34686-296">Entwicklern wird empfohlen, sich bei der Groß-/Kleinschreibung von Datei- und Verzeichnisnamen an der Schreibweise folgender Begriffe zu orientieren:</span><span class="sxs-lookup"><span data-stu-id="34686-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="34686-297">Bereichs-, Controller- und Aktionsnamen</span><span class="sxs-lookup"><span data-stu-id="34686-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="34686-298">Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="34686-298">Razor Pages.</span></span>
    
<span data-ttu-id="34686-299">Mit einer übereinstimmenden Groß- und Kleinschreibung wird sichergestellt, dass die entsprechenden Ansichten für die Bereitstellungen unabhängig von dem zugrunde liegenden Dateisystem gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="34686-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
