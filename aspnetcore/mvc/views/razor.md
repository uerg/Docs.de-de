---
title: Razor-Syntaxverweis für ASP.NET Core
author: rick-anderson
description: Informationen zur Razor-Markupsyntax zum Einbetten von serverbasiertem Code in Webseiten
ms.author: riande
ms.date: 10/18/2017
uid: mvc/views/razor
ms.openlocfilehash: d0f4d59cb605cc3cc7cdfa84bfc65399699e475a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272687"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="25da9-103">Razor-Syntaxverweis für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25da9-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="25da9-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) und [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="25da9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="25da9-105">Razor stellt eine Markupsyntax zum Einbetten von serverbasiertem Code in Webseiten dar.</span><span class="sxs-lookup"><span data-stu-id="25da9-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="25da9-106">Die Razor-Syntax besteht aus dem Razor-Markup, C# und HTML.</span><span class="sxs-lookup"><span data-stu-id="25da9-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="25da9-107">Dateien, die Razor enthalten, besitzen in der Regel die Dateierweiterung *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25da9-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="25da9-108">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="25da9-108">Rendering HTML</span></span>

<span data-ttu-id="25da9-109">Die Standardsprache für Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="25da9-109">The default Razor language is HTML.</span></span> <span data-ttu-id="25da9-110">Das Rendern von HTML-Code aus einem Razor-Markup funktioniert genauso wie das Rendern von HTML aus einer HTML-Datei.</span><span class="sxs-lookup"><span data-stu-id="25da9-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="25da9-111">Ein HTML-Markup wird in Razor-Dateien mit der Erweiterung *.cshtml* vom Server unverändert gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="25da9-112">Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="25da9-112">Razor syntax</span></span>

<span data-ttu-id="25da9-113">Razor unterstützt C# und verwendet für den Übergang von HTML zu C# das `@`-Symbol.</span><span class="sxs-lookup"><span data-stu-id="25da9-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="25da9-114">Razor wertet C#-Ausdrücke aus und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="25da9-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="25da9-115">Folgt auf ein `@`-Symbol ein für [Razor reserviertes Schlüsselwort](#razor-reserved-keywords), wechselt es in ein Razor-spezifisches Markup.</span><span class="sxs-lookup"><span data-stu-id="25da9-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="25da9-116">Andernfalls geht es in reines C# über.</span><span class="sxs-lookup"><span data-stu-id="25da9-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="25da9-117">Verwenden Sie ein zweites `@`-Symbol, um im Razor-Markup ein `@`-Symbol mit Escapezeichen zu versehen:</span><span class="sxs-lookup"><span data-stu-id="25da9-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="25da9-118">Der Code wird in HTML mit einem einzelnen `@`-Symbol gerendert:</span><span class="sxs-lookup"><span data-stu-id="25da9-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="25da9-119">Bei HTML-Attributen und Inhalt mit E-Mail-Adressen wird das `@`-Symbol nicht als ein Übergangszeichen behandelt.</span><span class="sxs-lookup"><span data-stu-id="25da9-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="25da9-120">Die E-Mail-Adressen im folgenden Beispiel bleiben von der Razor-Analyse unverändert:</span><span class="sxs-lookup"><span data-stu-id="25da9-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="25da9-121">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="25da9-121">Implicit Razor expressions</span></span>

<span data-ttu-id="25da9-122">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="25da9-123">Mit Ausnahme des `await`-C#-Schlüsselworts dürfen implizite Ausdrücke keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="25da9-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="25da9-124">Wird die C#-Anweisung eindeutig beendet, können auch Leerzeichen verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="25da9-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="25da9-125">Implizite Ausdrücke **dürfen keine** C#-Generika enthalten, da die Zeichen innerhalb der Klammern (`<>`) als HTML-Tag interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="25da9-126">Der folgende Code ist **ungültig**:</span><span class="sxs-lookup"><span data-stu-id="25da9-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="25da9-127">Der vorhergehende Code erzeugt einen Compilerfehler, der folgendermaßen aussehen kann:</span><span class="sxs-lookup"><span data-stu-id="25da9-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="25da9-128">Das Element „int“ wurde nicht geschlossen.</span><span class="sxs-lookup"><span data-stu-id="25da9-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="25da9-129">Alle Elemente müssen entweder selbstschließend sein oder einen übereinstimmenden Endtag besitzen.</span><span class="sxs-lookup"><span data-stu-id="25da9-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="25da9-130">Die Methodengruppe „GenericMethod“ kann nicht in den Nichtdelegattyp „Objekt“ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="25da9-131">Wollten Sie die Methode aufrufen?</span><span class="sxs-lookup"><span data-stu-id="25da9-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="25da9-132">Generische Methodenaufrufe müssen von einem [expliziten Razor-Ausdruck](#explicit-razor-expressions) oder einem [Razor-Codeblock](#razor-code-blocks) umschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="25da9-133">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="25da9-133">Explicit Razor expressions</span></span>

<span data-ttu-id="25da9-134">Explizite Razor-Ausdrücke bestehen aus einem `@`-Symbol mit ausgeglichener Klammer.</span><span class="sxs-lookup"><span data-stu-id="25da9-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="25da9-135">Zum Rendern der Uhrzeit von letzter Woche wird folgendes Razor-Markup verwendet:</span><span class="sxs-lookup"><span data-stu-id="25da9-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="25da9-136">Jeglicher Inhalt innerhalb der `@()`-Klammer wird ausgewertet und in der Ausgabe gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="25da9-137">Die im vorherigen Abschnitt beschriebenen impliziten Ausdrücke dürfen in der Regel keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="25da9-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="25da9-138">Im folgenden Code wird eine Woche nicht von der aktuellen Uhrzeit abgezogen:</span><span class="sxs-lookup"><span data-stu-id="25da9-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="25da9-139">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="25da9-140">Explizite Ausdrücke können zum Verketten von Text mit einem Ergebnis des Ausdrucks verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="25da9-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="25da9-141">Ohne den expliziten Ausdruck wird `<p>Age@joe.Age</p>` als E-Mail-Adresse behandelt und `<p>Age@joe.Age</p>` gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="25da9-142">`<p>Age33</p>` wird gerendert, wenn es als expliziter Ausdruck geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="25da9-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="25da9-143">Explizite Ausdrücke können zum Rendern der Ausgabe von generischen Methoden in *.cshtml*-Dateien verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="25da9-144">Das folgende Markup zeigt, wie der weiter oben gezeigte Fehler behoben wird, der durch die Klammern von C#-Generika verursacht wurde.</span><span class="sxs-lookup"><span data-stu-id="25da9-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="25da9-145">Der Code wird als expliziter Ausdruck geschrieben:</span><span class="sxs-lookup"><span data-stu-id="25da9-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="25da9-146">Codieren von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="25da9-146">Expression encoding</span></span>

<span data-ttu-id="25da9-147">C#-Ausdrücke, die mit einer Zeichenfolge ausgewertet werden, werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="25da9-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="25da9-148">C#-Ausdrücke, die mit `IHtmlContent` ausgewertet werden, werden direkt durch `IHtmlContent.WriteTo` gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="25da9-149">C#-Ausdrücke, die nicht mit `IHtmlContent` ausgewertet werden, werden durch `ToString` in eine Zeichenfolge konvertiert und codiert, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="25da9-150">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="25da9-151">Der HTML-Code wird im Browser folgendermaßen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="25da9-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="25da9-152">Die Ausgabe `HtmlHelper.Raw` wird nicht codiert, sondern als HTML-Markup gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="25da9-153">Die Verwendung von `HtmlHelper.Raw` bei einer nicht bereinigten Benutzereingabe stellt ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="25da9-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="25da9-154">Benutzereingaben können schädlichen JavaScript-Code oder andere Sicherheitsrisiken enthalten.</span><span class="sxs-lookup"><span data-stu-id="25da9-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="25da9-155">Das Bereinigen von Benutzereingaben ist schwierig.</span><span class="sxs-lookup"><span data-stu-id="25da9-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="25da9-156">Vermeiden Sie daher die Verwendung von `HtmlHelper.Raw` bei Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="25da9-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="25da9-157">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="25da9-158">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="25da9-158">Razor code blocks</span></span>

<span data-ttu-id="25da9-159">Razor-Codeblöcke beginnen mit `@` und sind von `{}` eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="25da9-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="25da9-160">Im Gegensatz zu Ausdrücken wird C#-Code in Codeblöcken nicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="25da9-161">Codeblöcke und Ausdrücke in einer Ansicht nutzen den gleichen Bereich und werden der Reihe nach definiert:</span><span class="sxs-lookup"><span data-stu-id="25da9-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="25da9-162">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="25da9-163">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="25da9-163">Implicit transitions</span></span>

<span data-ttu-id="25da9-164">Die Standardsprache in einem Codeblock ist C#. Die Razor Page kann jedoch zurück zu HTML wechseln:</span><span class="sxs-lookup"><span data-stu-id="25da9-164">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="25da9-165">Durch Trennzeichen getrennte explizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="25da9-165">Explicit delimited transition</span></span>

<span data-ttu-id="25da9-166">Soll ein Unterabschnitt eines Codeblocks in HTML gerendert werden, umschließen Sie die Zeichen, die gerendert werden sollen, mit dem Razor-Tag **\<text>**:</span><span class="sxs-lookup"><span data-stu-id="25da9-166">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="25da9-167">Verwenden Sie diese Methode zum Rendern von HTML-Code, der nicht von einem HTML-Tag umschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="25da9-167">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="25da9-168">Ohne HTML- oder Razor-Tag tritt ein Razor-Laufzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="25da9-168">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="25da9-169">Der **\<text>**-Tag ist nützlich, wenn Sie beim Rendern von Inhalt Leerzeichen steuern möchten:</span><span class="sxs-lookup"><span data-stu-id="25da9-169">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="25da9-170">Nur der Inhalt zwischen dem **\<text>**-Tag wird gerendert.</span><span class="sxs-lookup"><span data-stu-id="25da9-170">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="25da9-171">In der HTML-Ausgabe werden keine Leerzeichen vor oder nach dem **\<text>**-Tag angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25da9-171">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="25da9-172">Explizite Zeilenübergänge mit @:</span><span class="sxs-lookup"><span data-stu-id="25da9-172">Explicit Line Transition with @:</span></span>

<span data-ttu-id="25da9-173">Verwenden Sie die `@:`-Syntax, um den Rest einer kompletten Zeile als HTML-Code in einem Codeblock zu rendern:</span><span class="sxs-lookup"><span data-stu-id="25da9-173">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="25da9-174">Ohne das `@:`-Symbol im Code wird ein Razor-Laufzeitfehler erzeugt.</span><span class="sxs-lookup"><span data-stu-id="25da9-174">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="25da9-175">Warnung: Zusätzliche `@`-Zeichen in einer Razor-Datei können zu Compilerfehlern bei späteren Anweisungen im Block führen.</span><span class="sxs-lookup"><span data-stu-id="25da9-175">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="25da9-176">Diese Compilerfehler können dann schwer nachvollziehbar sein, da der tatsächliche Fehler vor dem gemeldeten Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="25da9-176">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="25da9-177">Dieser Fehler tritt häufig auf, wenn mehrere implizite/explizite Ausdrücke in einem einzigen Codeblock kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-177">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="25da9-178">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="25da9-178">Control structures</span></span>

<span data-ttu-id="25da9-179">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="25da9-179">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="25da9-180">Alle Aspekte von Codeblöcken (Übergang zu Markup, Inline-C#) gelten auch für die folgenden Strukturen:</span><span class="sxs-lookup"><span data-stu-id="25da9-180">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="25da9-181">Die Bedingungen @if, else if, else und @switch</span><span class="sxs-lookup"><span data-stu-id="25da9-181">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="25da9-182">`@if` steuert, wenn der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="25da9-182">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="25da9-183">`else` und `else if` erfordern kein `@`-Symbol:</span><span class="sxs-lookup"><span data-stu-id="25da9-183">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="25da9-184">Im folgenden Markup wird die Verwendung einer switch-Anweisung veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="25da9-184">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="25da9-185">Die Schleifen @for, @foreach, @while und @do while</span><span class="sxs-lookup"><span data-stu-id="25da9-185">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="25da9-186">Auf Vorlagen basierender HTML-Code kann mit Schleifen-Steuerungsanweisungen gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-186">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="25da9-187">Zum Rendern einer Liste von Personen:</span><span class="sxs-lookup"><span data-stu-id="25da9-187">To render a list of people:</span></span>

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

<span data-ttu-id="25da9-188">Die folgenden Schleifenanweisungen werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="25da9-188">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="25da9-189">Zusammengesetztes @using</span><span class="sxs-lookup"><span data-stu-id="25da9-189">Compound @using</span></span>

<span data-ttu-id="25da9-190">In C# wird eine `using`-Anweisung verwendet, um sicherzustellen, dass ein Objekt freigegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="25da9-190">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="25da9-191">In Razor wird derselbe Mechanismus verwendet, um HTML-Hilfsprogramme zu erstellen, die zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="25da9-191">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="25da9-192">Im folgenden Code wird ein Formulartag mit der `@using`-Anweisung durch HTML-Hilfsprogramme gerendert:</span><span class="sxs-lookup"><span data-stu-id="25da9-192">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="25da9-193">Aktionen auf Bereichsebene können mit [Taghilfsprogrammen](xref:mvc/views/tag-helpers/intro) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-193">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="25da9-194">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="25da9-194">@try, catch, finally</span></span>

<span data-ttu-id="25da9-195">Die Behandlung von Ausnahmen ist vergleichbar mit C#:</span><span class="sxs-lookup"><span data-stu-id="25da9-195">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="25da9-196">Razor kann kritische Abschnitte mit lock-Anweisungen schützen:</span><span class="sxs-lookup"><span data-stu-id="25da9-196">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="25da9-197">Kommentare</span><span class="sxs-lookup"><span data-stu-id="25da9-197">Comments</span></span>

<span data-ttu-id="25da9-198">Razor unterstützt C#- und HTML-Kommentare:</span><span class="sxs-lookup"><span data-stu-id="25da9-198">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="25da9-199">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-199">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="25da9-200">Razor-Kommentare werden vom Server entfernt, bevor die Webseite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="25da9-200">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="25da9-201">Zur Abgrenzung der Kommentare verwendet Razor `@*  *@`.</span><span class="sxs-lookup"><span data-stu-id="25da9-201">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="25da9-202">Der folgende Code ist auskommentiert, damit vom Server kein Markup gerendert wird:</span><span class="sxs-lookup"><span data-stu-id="25da9-202">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="25da9-203">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="25da9-203">Directives</span></span>

<span data-ttu-id="25da9-204">Razor-Anweisungen werden durch implizite Ausdrücke mit reservierten Schlüsselwörtern nach dem `@`-Symbol dargestellt.</span><span class="sxs-lookup"><span data-stu-id="25da9-204">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="25da9-205">Eine Anweisung ändert in der Regel die Analyse einer Ansicht oder aktiviert unterschiedliche Funktionen.</span><span class="sxs-lookup"><span data-stu-id="25da9-205">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="25da9-206">Wenn Sie wissen, wie von Razor Code für eine Ansicht generiert wird, erleichtert dies das Verständnis dafür, wie Anweisungen funktionieren.</span><span class="sxs-lookup"><span data-stu-id="25da9-206">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="25da9-207">Durch den Code wird eine Klasse ähnlich der folgenden Klasse generiert:</span><span class="sxs-lookup"><span data-stu-id="25da9-207">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="25da9-208">Im Abschnitt [Anzeigen der Razor-C#-Klasse, die für eine Ansicht generiert wurde](#viewing-the-razor-c-class-generated-for-a-view) weiter unten wird erklärt, wie die generierte Klasse angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="25da9-208">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>
### <a name="using"></a>@using

<span data-ttu-id="25da9-209">Die `@using`-Anweisung fügt die `using`-C#-Anweisung der generierten Ansicht hinzu:</span><span class="sxs-lookup"><span data-stu-id="25da9-209">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="25da9-210">Die `@model`-Anweisung gibt den Typ des Modells an, das an eine Ansicht übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="25da9-210">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="25da9-211">In einer mit einzelnen Benutzerkonten erstellten ASP.NET Core MVC-App enthält die Ansicht *Views/Account/Login.cshtml* die folgende Modelldeklaration:</span><span class="sxs-lookup"><span data-stu-id="25da9-211">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="25da9-212">Die generierte Klasse erbt von `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="25da9-212">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="25da9-213">Razor macht eine `Model`-Eigenschaft für den Zugriff auf das an die Ansicht übergebene Modell verfügbar:</span><span class="sxs-lookup"><span data-stu-id="25da9-213">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="25da9-214">Die `@model`-Anweisung gibt den Typ dieser Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="25da9-214">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="25da9-215">Die Anweisung gibt das `T` in `RazorPage<T>` der generierten Klasse an, von der die Ansicht abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="25da9-215">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="25da9-216">Wird die `@model`-Anweisung nicht angegeben, hat die `Model`-Eigenschaft den Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="25da9-216">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="25da9-217">Der Wert des Modells wird vom Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="25da9-217">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="25da9-218">Weitere Informationen finden Sie unter [Stark typisierte Modelle und das ](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)model-Schlüsselwort&commat;.</span><span class="sxs-lookup"><span data-stu-id="25da9-218">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="25da9-219">Die `@inherits`-Anweisung bietet uneingeschränkten Zugriff auf die Klasse, die die Ansicht erbt:</span><span class="sxs-lookup"><span data-stu-id="25da9-219">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="25da9-220">Der folgende Code ist ein benutzerdefinierter Typ der Razor Page:</span><span class="sxs-lookup"><span data-stu-id="25da9-220">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="25da9-221">`CustomText` wird in einer Ansicht angezeigt:</span><span class="sxs-lookup"><span data-stu-id="25da9-221">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="25da9-222">Der Code rendert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="25da9-222">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="25da9-223">`@model` und `@inherits` können in derselben Ansicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-223">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="25da9-224">`@inherits` kann in einer *_ViewImports.cshtml*-Datei verwendet werden, die von der Ansicht importiert wird:</span><span class="sxs-lookup"><span data-stu-id="25da9-224">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="25da9-225">Der folgende Code ist ein Beispiel für eine stark typisierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="25da9-225">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="25da9-226">Wird „rick@contoso.com“ im Modell übergeben, generiert die Ansicht das folgende HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="25da9-226">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="25da9-227">Mit der `@inject`-Anweisung kann die Razor Page einen Dienst vom [Dienstcontainer](xref:fundamentals/dependency-injection) in eine Ansicht einfügen.</span><span class="sxs-lookup"><span data-stu-id="25da9-227">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="25da9-228">Weitere Informationen finden Sie unter [Dependency Injection in Ansichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="25da9-228">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="25da9-229">Mit der `@functions`-Anweisung kann eine Razor-Seite einer Ansicht einen C#-Codeblock hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="25da9-229">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="25da9-230">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="25da9-230">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="25da9-231">Der Code generiert das folgende HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="25da9-231">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="25da9-232">Der folgende Code wird in der Razor-C#-Klasse generiert:</span><span class="sxs-lookup"><span data-stu-id="25da9-232">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="25da9-233">Die `@section`-Anweisung wird in Verbindung mit dem [Layout](xref:mvc/views/layout) verwendet, damit Ansichten Inhalt in verschiedenen Teilen der HTML-Seite rendern können.</span><span class="sxs-lookup"><span data-stu-id="25da9-233">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="25da9-234">Weitere Informationen finden Sie unter [Abschnitte](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="25da9-234">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="25da9-235">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="25da9-235">Tag Helpers</span></span>

<span data-ttu-id="25da9-236">Die folgenden drei Anweisungen gehören zu den [Taghilfsprogrammen](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="25da9-236">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="25da9-237">Direktive</span><span class="sxs-lookup"><span data-stu-id="25da9-237">Directive</span></span> | <span data-ttu-id="25da9-238">Funktion</span><span class="sxs-lookup"><span data-stu-id="25da9-238">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="25da9-239">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="25da9-239">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="25da9-240">Macht Taghilfsprogramme für eine Ansicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="25da9-240">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="25da9-241">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="25da9-241">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="25da9-242">Entfernt die zuvor aus einer Ansicht hinzugefügten Taghilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="25da9-242">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="25da9-243">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="25da9-243">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="25da9-244">Gibt ein Tagpräfix an, um Unterstützung für Taghilfsprogramme zu aktivieren und ihre Verwendung explizit festzulegen.</span><span class="sxs-lookup"><span data-stu-id="25da9-244">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="25da9-245">Für Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="25da9-245">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="25da9-246">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="25da9-246">Razor keywords</span></span>

* <span data-ttu-id="25da9-247">page (ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="25da9-247">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="25da9-248">namespace</span><span class="sxs-lookup"><span data-stu-id="25da9-248">namespace</span></span>
* <span data-ttu-id="25da9-249">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="25da9-249">functions</span></span>
* <span data-ttu-id="25da9-250">inherits</span><span class="sxs-lookup"><span data-stu-id="25da9-250">inherits</span></span>
* <span data-ttu-id="25da9-251">Modell</span><span class="sxs-lookup"><span data-stu-id="25da9-251">model</span></span>
* <span data-ttu-id="25da9-252">section</span><span class="sxs-lookup"><span data-stu-id="25da9-252">section</span></span>
* <span data-ttu-id="25da9-253">helper (wird derzeit nicht von ASP.NET Core unterstützt)</span><span class="sxs-lookup"><span data-stu-id="25da9-253">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="25da9-254">Razor-Schlüsselwörter werden mit dem Escapezeichen `@(Razor Keyword)` versehen (z.B. `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="25da9-254">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="25da9-255">Razor-C#-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="25da9-255">C# Razor keywords</span></span>

* <span data-ttu-id="25da9-256">Groß- und Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="25da9-256">case</span></span>
* <span data-ttu-id="25da9-257">do</span><span class="sxs-lookup"><span data-stu-id="25da9-257">do</span></span>
* <span data-ttu-id="25da9-258">default</span><span class="sxs-lookup"><span data-stu-id="25da9-258">default</span></span>
* <span data-ttu-id="25da9-259">for</span><span class="sxs-lookup"><span data-stu-id="25da9-259">for</span></span>
* <span data-ttu-id="25da9-260">foreach</span><span class="sxs-lookup"><span data-stu-id="25da9-260">foreach</span></span>
* <span data-ttu-id="25da9-261">if</span><span class="sxs-lookup"><span data-stu-id="25da9-261">if</span></span>
* <span data-ttu-id="25da9-262">else</span><span class="sxs-lookup"><span data-stu-id="25da9-262">else</span></span>
* <span data-ttu-id="25da9-263">lock</span><span class="sxs-lookup"><span data-stu-id="25da9-263">lock</span></span>
* <span data-ttu-id="25da9-264">switch</span><span class="sxs-lookup"><span data-stu-id="25da9-264">switch</span></span>
* <span data-ttu-id="25da9-265">try</span><span class="sxs-lookup"><span data-stu-id="25da9-265">try</span></span>
* <span data-ttu-id="25da9-266">catch</span><span class="sxs-lookup"><span data-stu-id="25da9-266">catch</span></span>
* <span data-ttu-id="25da9-267">finally</span><span class="sxs-lookup"><span data-stu-id="25da9-267">finally</span></span>
* <span data-ttu-id="25da9-268">Verwenden</span><span class="sxs-lookup"><span data-stu-id="25da9-268">using</span></span>
* <span data-ttu-id="25da9-269">while</span><span class="sxs-lookup"><span data-stu-id="25da9-269">while</span></span>

<span data-ttu-id="25da9-270">Razor-C#-Schlüsselwörter werden mit dem doppeltem Escapezeichen `@(@C# Razor Keyword)` versehen (z.B. `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="25da9-270">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="25da9-271">Das erste `@` dient als Escapezeichen für den Razor-Parser.</span><span class="sxs-lookup"><span data-stu-id="25da9-271">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="25da9-272">Das zweite `@` dient als Escapezeichen für den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="25da9-272">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="25da9-273">Reservierte Schlüsselwörter, die nicht von Razor verwendet werden</span><span class="sxs-lookup"><span data-stu-id="25da9-273">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="25da9-274">Klasse</span><span class="sxs-lookup"><span data-stu-id="25da9-274">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="25da9-275">Anzeigen der Razor-C#-Klasse, die für eine Ansicht generiert wurde</span><span class="sxs-lookup"><span data-stu-id="25da9-275">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="25da9-276">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="25da9-276">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="25da9-277">Überschreiben Sie das von MVC hinzugefügte `RazorTemplateEngine` mit der `CustomTemplateEngine`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="25da9-277">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="25da9-278">Setzen Sie auf der `return csharpDocument`-Anweisung von `CustomTemplateEngine` einen Haltepunkt.</span><span class="sxs-lookup"><span data-stu-id="25da9-278">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="25da9-279">Wenn die Ausführung des Programms am Haltepunkt angehalten wird, können Sie den Wert von `generatedCode` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="25da9-279">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Ansicht „Text-Schnellansicht“ von „generatedCode“](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="25da9-281">Ansicht der Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="25da9-281">View lookups and case sensitivity</span></span>

<span data-ttu-id="25da9-282">Die Razor-Ansichtsengine führt für Ansichten Suchvorgänge aus, die Groß- und Kleinschreibung berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="25da9-282">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="25da9-283">Der tatsächliche Suchvorgang wird jedoch vom zugrunde liegenden Dateisystem bestimmt:</span><span class="sxs-lookup"><span data-stu-id="25da9-283">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="25da9-284">Dateibasierte Quelle:</span><span class="sxs-lookup"><span data-stu-id="25da9-284">File based source:</span></span> 
  * <span data-ttu-id="25da9-285">Bei Betriebssystemen, die Dateisysteme ohne Berücksichtigung von Groß-/Kleinschreibung verwenden (z.B. Windows), wird bei Suchvorgängen nach physischen Dateianbietern die Groß- und Kleinschreibung nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="25da9-285">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="25da9-286">`return View("Test")` liefert beispielsweise eine Übereinstimmung für */Views/Home/Test.cshtml*, */Views/home/test.cshtml* sowie für jede andere Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="25da9-286">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="25da9-287">Bei Dateisystemen, die Groß-/Kleinschreibung berücksichtigen (z.B. Linux, OSX sowie mit `EmbeddedFileProvider`), wird auch bei Suchvorgängen die Groß- und Kleinschreibung berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="25da9-287">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="25da9-288">`return View("Test")` liefert beispielsweise speziell eine Übereinstimmung mit */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25da9-288">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="25da9-289">Vorkompilierte Ansichten: Ab ASP.NET Core 2.0 wird bei der Suche nach vorkompilierten Ansichten unter allen Betriebssystemen die Groß- und Kleinschreibung nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="25da9-289">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="25da9-290">Das Verhalten ist mit dem Verhalten des physischen Dateianbieters unter Windows identisch.</span><span class="sxs-lookup"><span data-stu-id="25da9-290">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="25da9-291">Unterscheiden sich zwei vorkompilierte Ansichten nur in der Groß-/Kleinschreibung, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="25da9-291">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="25da9-292">Entwicklern wird empfohlen, sich bei der Groß-/Kleinschreibung von Datei- und Verzeichnisnamen an der Schreibweise folgender Begriffe zu orientieren:</span><span class="sxs-lookup"><span data-stu-id="25da9-292">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="25da9-293">Bereichs-, Controller- und Aktionsnamen</span><span class="sxs-lookup"><span data-stu-id="25da9-293">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="25da9-294">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="25da9-294">Razor Pages.</span></span>
    
<span data-ttu-id="25da9-295">Mit einer übereinstimmenden Groß- und Kleinschreibung wird sichergestellt, dass die entsprechenden Ansichten für die Bereitstellungen unabhängig von dem zugrunde liegenden Dateisystem gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="25da9-295">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
