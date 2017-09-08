---
title: "Razor-Syntaxverweis für ASP.NET Core"
author: rick-anderson
description: Details-Razor-syntax
keywords: ASP.NET Core Razor
ms.author: riande
manager: wpickett
ms.date: 07/4/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 7648bc2ac7b9efd1653725cda749d6cd271bae77
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="9aaf0-104">Razor-Syntax für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9aaf0-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="9aaf0-105">Durch [Taylor Mullen](https://twitter.com/ntaylormullen) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9aaf0-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="9aaf0-106">Was ist Razor?</span><span class="sxs-lookup"><span data-stu-id="9aaf0-106">What is Razor?</span></span>

<span data-ttu-id="9aaf0-107">Razor ist eine Markupsyntax für das Einbetten von serverbasierten Code in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="9aaf0-108">Die Razor-Syntax besteht aus C#- und HTML-Razor-Markup.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="9aaf0-109">Dateien, die in der Regel enthält Razor haben eine *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="9aaf0-110">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="9aaf0-110">Rendering HTML</span></span>

<span data-ttu-id="9aaf0-111">Die Standardsprache für den Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-111">The default Razor language is HTML.</span></span> <span data-ttu-id="9aaf0-112">Rendern von HTML Razor ist unterscheidet sich nicht in einer HTML-Datei.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="9aaf0-113">Eine Razor-Datei durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
   ```

<span data-ttu-id="9aaf0-114">Wird unverändert gerendert `<p>Hello World</p>` vom Server.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="9aaf0-115">Razor-syntax</span><span class="sxs-lookup"><span data-stu-id="9aaf0-115">Razor syntax</span></span>

<span data-ttu-id="9aaf0-116">Razor unterstützt C#- und verwendet die `@` Symbol für den Übergang von HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="9aaf0-117">Razor C#-Ausdrücke ausgewertet und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="9aaf0-118">Razor kann aus HTML-Code in c# oder einer Razor-spezifischer Markups übergehen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="9aaf0-119">Wenn ein `@` Symbol wird gefolgt von einer [Razor reserviertes Schlüsselwort](#razor-reserved-keywords) in Razor-spezifischer Markups geht, andernfalls geht in einfachen C#.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="9aaf0-120">HTML mit `@` Symbole mit einer zweiten mit Escapezeichen versehen werden müssen möglicherweise `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="9aaf0-121">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-121">For example:</span></span>

```html
<p>@@Username</p>
   ```

<span data-ttu-id="9aaf0-122">würden Sie folgenden HTML-Code zum Rendern:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-122">would render the following HTML:</span></span>

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

<span data-ttu-id="9aaf0-123">HTML-Attribute und Inhalt mit e-Mail-Adressen nicht behandeln die `@` Symbol als ein Zeichen Übergang.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="9aaf0-124">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="9aaf0-124">Implicit Razor expressions</span></span>

<span data-ttu-id="9aaf0-125">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="9aaf0-126">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="9aaf0-127">Mit Ausnahme von C#- `await` Schlüsselwort implicit-Ausdrücken keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="9aaf0-128">Sie können z. B. Leerzeichen intermingle, solange die C#-Anweisung eine klare beendet wurde:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="9aaf0-129">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="9aaf0-129">Explicit Razor expressions</span></span>

<span data-ttu-id="9aaf0-130">Explizite Razor-Ausdrücke besteht ein @-Zeichen ausgeglichene Klammer.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="9aaf0-131">Geben Sie beispielsweise Folgendes ein, um die Uhrzeit der letzten Woche zu rendern:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="9aaf0-132">Alle Inhalte innerhalb der @ Klammer () ausgewertet und in die Ausgabe des gerendert.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="9aaf0-133">Implicit-Ausdrücken enthalten keine Leerzeichen in der Regel.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="9aaf0-134">Beispielsweise wird in den folgenden Code eine Woche nicht von der aktuellen Zeit abgezogen:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

<span data-ttu-id="9aaf0-135">[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-135">[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]</span></span>

<span data-ttu-id="9aaf0-136">Die folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-136">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

<span data-ttu-id="9aaf0-137">Sie können einen expliziten Ausdruck zum Verketten von Text mit einem Ergebnis des Ausdrucks verwenden:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-137">You can use an explicit expression to concatenate text with an expression result:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="9aaf0-138">Ohne die explizite Ausdruck `<p>Age@joe.Age</p>` behandelt werden würde, als eine e-Mail-Adresse und `<p>Age@joe.Age</p>` würden gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-138">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="9aaf0-139">Wenn ein expliziter Ausdruck geschrieben `<p>Age33</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-139">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="9aaf0-140">Expression-Codierung</span><span class="sxs-lookup"><span data-stu-id="9aaf0-140">Expression encoding</span></span>

<span data-ttu-id="9aaf0-141">C#-Ausdrücke, die zu einer Zeichenfolge ausgewertet werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-141">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="9aaf0-142">C#-Ausdrücke, die ergeben `IHtmlContent` sind direkt über gerendert *IHtmlContent.WriteTo*.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-142">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="9aaf0-143">C#-Ausdrücke, ergeben nicht *IHtmlContent* in eine Zeichenfolge konvertiert werden (durch *ToString*) und codiert werden, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-143">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="9aaf0-144">Um beispielsweise das folgende Markup für den Razor:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-144">For example, the following Razor markup:</span></span>

```html
@("<span>Hello World</span>")
   ```

<span data-ttu-id="9aaf0-145">Rendert das HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-145">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

<span data-ttu-id="9aaf0-146">Der Browser als gerendert wird:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-146">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="9aaf0-147">`HtmlHelper``Raw` Ausgabe nicht codierte jedoch als HTML-Markup gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-147">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="9aaf0-148">Mit `HtmlHelper.Raw` unsanitized Benutzer Eingabe ist ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-148">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="9aaf0-149">Benutzereingabe möglicherweise böswilligen JavaScript- oder andere Exploits durchführen enthalten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-149">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="9aaf0-150">Bereinigen von Benutzereingaben ist schwierig, vermeiden Sie die Verwendung `HtmlHelper.Raw` auf Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-150">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="9aaf0-151">Das folgende Razor-Markup:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-151">The following Razor markup:</span></span>

```html
@Html.Raw("<span>Hello World</span>")
   ```

<span data-ttu-id="9aaf0-152">Rendert das HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-152">Renders this HTML:</span></span>

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="9aaf0-153">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="9aaf0-153">Razor code blocks</span></span>

<span data-ttu-id="9aaf0-154">Razor-Codeblöcke beginnen Sie mit `@` und eingeschlossen werden `{}`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-154">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="9aaf0-155">Im Gegensatz zu Ausdrücken wird C#-Code in Codeblöcken nicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-155">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="9aaf0-156">Codeblöcke und Ausdrücke in einer Razor-Seite gemeinsam denselben Bereich und werden in Reihenfolge definiert (Deklarationen in einem Codeblock werden innerhalb des Bereichs bei späteren Codeblöcke und Ausdrücke sein).</span><span class="sxs-lookup"><span data-stu-id="9aaf0-156">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="9aaf0-157">Würden Sie zum Rendern:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-157">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="9aaf0-158">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="9aaf0-158">Implicit transitions</span></span>

<span data-ttu-id="9aaf0-159">In einem Codeblock die Standardsprache ist c#, aber Sie können den Übergang zurück zum HTML.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-159">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="9aaf0-160">HTML in einem Codeblock erfolgt ein Wechsel zurück in das Rendern von HTML:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-160">HTML within a code block will transition back into rendering HTML:</span></span>

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="9aaf0-161">Explizite durch Trennzeichen getrennten Übergang</span><span class="sxs-lookup"><span data-stu-id="9aaf0-161">Explicit delimited transition</span></span>

<span data-ttu-id="9aaf0-162">Um einen Unterabschnitt eines Codeblocks definieren, die HTML gerendert werden soll, umschließen Sie die Zeichen, die mit dem Razor gerendert werden `<text>` Tag:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-162">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="9aaf0-163">Im Allgemeinen verwenden Sie diesen Ansatz, wenn Sie möchten, HTML zu rendern, die nicht von einer HTML-Tags eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-163">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="9aaf0-164">Ohne ein HTML- oder Razor-Tag erhalten Sie einen Razor-Laufzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-164">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="9aaf0-165">Explizite Zeile Übergang mit`@:`</span><span class="sxs-lookup"><span data-stu-id="9aaf0-165">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="9aaf0-166">Um den Rest der eine ganze Zeile in einem Codeblock als HTML zu rendern, verwenden die `@:` Syntax:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-166">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="9aaf0-167">Ohne die `@:` im obigen Code erhalten Sie eine Razor Laufzeitfehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-167">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="9aaf0-168">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="9aaf0-168">Control Structures</span></span>

<span data-ttu-id="9aaf0-169">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-169">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="9aaf0-170">Alle Aspekte von Codeblöcken (Übergang zu Markup Inline-c#) auch gelten für die folgenden Strukturen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-170">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="9aaf0-171">Bedingungen `@if`, `else if`, `else` und`@switch`</span><span class="sxs-lookup"><span data-stu-id="9aaf0-171">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="9aaf0-172">Die `@if` Familie steuert, wann der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-172">The `@if` family controls when code runs:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="9aaf0-173">`else`und `else if` erfordern die `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-173">`else` and `else if` don't require the `@` symbol:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

<span data-ttu-id="9aaf0-174">Sie können eine Switch-Anweisung wie folgt verwenden:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-174">You can use a switch statement like this:</span></span>

```none
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="9aaf0-175">Schleifen `@for`, `@foreach`, `@while`, und`@do while`</span><span class="sxs-lookup"><span data-stu-id="9aaf0-175">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="9aaf0-176">Sie können aus einer Vorlage gebildete HTML mit Schleifen Steueranweisungen rendern.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-176">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="9aaf0-177">Geben Sie beispielsweise Folgendes ein, um eine Liste der Personen zu rendern:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-177">For example, to render a list of people:</span></span>

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="9aaf0-178">Sie können eine der folgenden Schleifen Anweisungen verwenden:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-178">You can use any of the following looping statements:</span></span>

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
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

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="9aaf0-179">Zusammengesetzte`@using`</span><span class="sxs-lookup"><span data-stu-id="9aaf0-179">Compound `@using`</span></span>

<span data-ttu-id="9aaf0-180">In c# eine using-Anweisung wird verwendet, um sicherzustellen, dass ein Objekt wurde verworfen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-180">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="9aaf0-181">In Razor kann denselben Mechanismus verwendet werden, um HTML-Hilfsmethoden zu erstellen, die zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-181">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="9aaf0-182">Es können z. B. HTML-Hilfsmethoden zum Rendern ein Formulartag mit nutzen die `@using` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-182">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

<span data-ttu-id="9aaf0-183">Sie können auch Aktionen Bereich Ebene wie oben mit [Tag Hilfsprogramme](tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9aaf0-183">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="9aaf0-184">`@try`, `catch`, `finally`</span><span class="sxs-lookup"><span data-stu-id="9aaf0-184">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="9aaf0-185">Behandlung von Ausnahmen ist vergleichbar mit c#:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-185">Exception handling is similar to  C#:</span></span>

<span data-ttu-id="9aaf0-186">[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-186">[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]</span></span>

### `@lock`

<span data-ttu-id="9aaf0-187">Razor hat die Möglichkeit, kritische Abschnitte mit Lock-Anweisungen zu schützen:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-187">Razor has the capability to protect critical sections with lock statements:</span></span>

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="9aaf0-188">Kommentare</span><span class="sxs-lookup"><span data-stu-id="9aaf0-188">Comments</span></span>

<span data-ttu-id="9aaf0-189">Razor unterstützt C#- und HTML-Kommentare.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-189">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="9aaf0-190">Das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-190">The following markup:</span></span>

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="9aaf0-191">Wird vom Server als gerendert werden:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-191">Is rendered by the server as:</span></span>

```none
<!-- HTML comment -->
```

<span data-ttu-id="9aaf0-192">Razor-Kommentare werden vom Server entfernt, bevor die Seite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-192">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="9aaf0-193">Razor verwendet `@*  *@` zur Begrenzung von Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-193">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="9aaf0-194">Der folgende Code ist auskommentiert, damit der Server nicht Markup gerendert werden:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-194">The following code is commented out, so the server will not render any markup:</span></span>

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a><span data-ttu-id="9aaf0-195">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="9aaf0-195">Directives</span></span>

<span data-ttu-id="9aaf0-196">Razor-Direktiven werden durch implicit-Ausdrücken mit reservierten Schlüsselwörtern folgenden dargestellt die `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-196">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="9aaf0-197">Eine Richtlinie wird in der Regel ändern, die eine Seite analysiert wird wie oder andere Funktionalität innerhalb der Razor-Seite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-197">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="9aaf0-198">Verstehen, wie der Razor-Code für eine Ansicht generiert so können sie einfacher zu verstehen, wie Anweisungen funktionieren.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-198">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="9aaf0-199">Eine Razor-Seite wird verwendet, um eine C#-Datei zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-199">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="9aaf0-200">Beispielsweise diese Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-200">For example, this Razor page:</span></span>

<span data-ttu-id="9aaf0-201">[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-201">[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]</span></span>

<span data-ttu-id="9aaf0-202">Generiert eine Klasse, die etwa wie folgt an:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-202">Generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="9aaf0-203">[Anzeigen der Razor c#-Klasse, die für eine Sicht generiert](#razor-customcompilationservice-label) wird erläutert, wie diese generierte Klasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-203">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="9aaf0-204">Die `@using` Richtlinie fügen die c#- `using` Richtlinie auf die generierten Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-204">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

<span data-ttu-id="9aaf0-205">[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-205">[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]</span></span>

### `@model`

<span data-ttu-id="9aaf0-206">Die `@model` Richtlinie gibt den Typ des Modells auf der Seite "Razor" übergeben.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-206">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="9aaf0-207">Es verwendet die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-207">It uses the following syntax:</span></span>

```none
@model TypeNameOfModel
   ```

<span data-ttu-id="9aaf0-208">Angenommen, wenn Sie eine ASP.NET-MVC-Anwendung Core einzelne Benutzerkonten erstellen die *Views/Account/Login.cshtml* Razor-Ansicht enthält die folgende Deklaration einer Modell:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-208">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="9aaf0-209">Im vorangehenden Klassenbeispiel die generierten Klasse erbt von `RazorPage<dynamic>`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-209">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="9aaf0-210">Durch Hinzufügen einer `@model` Sie steuern, welche geerbt wird.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-210">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="9aaf0-211">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-211">For example</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="9aaf0-212">Generiert die folgende Klasse</span><span class="sxs-lookup"><span data-stu-id="9aaf0-212">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

<span data-ttu-id="9aaf0-213">Razor-Seiten verfügbar machen eine `Model` Eigenschaft für den Zugriff auf das Modell auf der Seite "übergeben.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-213">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```html
<div>The Login Email: @Model.Email</div>
   ```

<span data-ttu-id="9aaf0-214">Die `@model` -Direktive angegeben, den Typ dieser Eigenschaft (durch Angabe der `T` in `RazorPage<T>` , die die generierte Klasse für die Seite abgeleitet).</span><span class="sxs-lookup"><span data-stu-id="9aaf0-214">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="9aaf0-215">Wenn Sie nicht angeben, die `@model` Richtlinie die `Model` Eigenschaft werden vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-215">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="9aaf0-216">Der Wert des Modells wird auf dem Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-216">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="9aaf0-217">Finden Sie unter [stark typisierte Modelle und die @model Schlüsselwort](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-217">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="9aaf0-218">Die `@inherits` Richtlinie erhalten Sie Vollzugriff auf die Klasse erbt von die Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-218">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```none
@inherits TypeNameOfClassToInheritFrom
   ```

<span data-ttu-id="9aaf0-219">Beispielsweise angenommen, mussten wir die folgenden benutzerdefinierten Razor-Seitentyp:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-219">For instance, let’s say we had the following custom Razor page type:</span></span>

<span data-ttu-id="9aaf0-220">[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-220">[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]</span></span>

<span data-ttu-id="9aaf0-221">Die folgenden Razor generieren würden `<div>Custom text: Hello World</div>`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-221">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

<span data-ttu-id="9aaf0-222">[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-222">[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]</span></span>

<span data-ttu-id="9aaf0-223">Sie können keine `@model` und `@inherits` auf derselben Seite.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-223">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="9aaf0-224">Sie können `@inherits` in einer *_ViewImports.cshtml* -Datei, die die Razor-Seite importiert.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-224">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="9aaf0-225">Wenn die Razor-Ansicht die folgenden importiert z. B. *_ViewImports.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-225">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="9aaf0-226">[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-226">[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]</span></span>

<span data-ttu-id="9aaf0-227">Die folgenden stark typisierte Razor-Seite</span><span class="sxs-lookup"><span data-stu-id="9aaf0-227">The following strongly typed Razor page</span></span>

<span data-ttu-id="9aaf0-228">[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-228">[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]</span></span>

<span data-ttu-id="9aaf0-229">Wird diese HTML-Markup generiert:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-229">Generates this HTML markup:</span></span>

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="9aaf0-230">Beim übergeben "[Rick@contoso.com](mailto:Rick@contoso.com)" im Modell:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-230">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="9aaf0-231">Finden Sie unter [Layout](layout.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-231">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="9aaf0-232">Die `@inject` Richtlinie ermöglicht das Einfügen eines Diensts aus Ihrer [Dienstcontainer](../../fundamentals/dependency-injection.md) in der Razor-Seite für die Verwendung.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-232">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="9aaf0-233">Finden Sie unter [Abhängigkeitsinjektion in Sichten](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="9aaf0-233">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="9aaf0-234">Die `@functions` Richtlinie ermöglicht es Ihnen, die Funktion Inhalt der Razor-Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-234">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="9aaf0-235">Die Syntax lautet:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-235">The syntax is:</span></span>

```none
@functions { // C# Code }
   ```

<span data-ttu-id="9aaf0-236">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-236">For example:</span></span>

<span data-ttu-id="9aaf0-237">[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-237">[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]</span></span>

<span data-ttu-id="9aaf0-238">Wird der folgende HTML-Markup generiert:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-238">Generates the following HTML markup:</span></span>

```none
<div>From method: Hello</div>
   ```

<span data-ttu-id="9aaf0-239">Der generierte Razor c# sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-239">The generated Razor C# looks like:</span></span>

<span data-ttu-id="9aaf0-240">[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-240">[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]</span></span>

### `@section`

<span data-ttu-id="9aaf0-241">Die `@section` Richtlinie dient in Verbindung mit der [Layoutseite](layout.md) Ansichten zum Rendern von Inhalt in verschiedene Teile des gerenderten HTML-Seite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-241">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="9aaf0-242">Finden Sie unter [Abschnitte](layout.md#layout-sections-label) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-242">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="9aaf0-243">Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="9aaf0-243">Tag Helpers</span></span>

<span data-ttu-id="9aaf0-244">Die folgenden [Tag Hilfsprogramme](tag-helpers/index.md) Direktiven werden in die bereitgestellten Links beschrieben.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-244">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="9aaf0-245">Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="9aaf0-245">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="9aaf0-246">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="9aaf0-246">Razor keywords</span></span>

* <span data-ttu-id="9aaf0-247">Seite "(erfordert ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="9aaf0-247">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="9aaf0-248">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="9aaf0-248">functions</span></span>
* <span data-ttu-id="9aaf0-249">inherits</span><span class="sxs-lookup"><span data-stu-id="9aaf0-249">inherits</span></span>
* <span data-ttu-id="9aaf0-250">Modell</span><span class="sxs-lookup"><span data-stu-id="9aaf0-250">model</span></span>
* <span data-ttu-id="9aaf0-251">section</span><span class="sxs-lookup"><span data-stu-id="9aaf0-251">section</span></span>
* <span data-ttu-id="9aaf0-252">Hilfsprogramm (nicht von ASP.NET Core unterstützt.)</span><span class="sxs-lookup"><span data-stu-id="9aaf0-252">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="9aaf0-253">Razor-Schlüsselwörter können mit Escapezeichen versehen werden `@(Razor Keyword)`, z. B. `@(functions)`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-253">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="9aaf0-254">Finden Sie unter dem vollständigen Beispiel unten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-254">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="9aaf0-255">C#-Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="9aaf0-255">C# Razor keywords</span></span>

* <span data-ttu-id="9aaf0-256">case</span><span class="sxs-lookup"><span data-stu-id="9aaf0-256">case</span></span>
* <span data-ttu-id="9aaf0-257">do</span><span class="sxs-lookup"><span data-stu-id="9aaf0-257">do</span></span>
* <span data-ttu-id="9aaf0-258">default</span><span class="sxs-lookup"><span data-stu-id="9aaf0-258">default</span></span>
* <span data-ttu-id="9aaf0-259">for</span><span class="sxs-lookup"><span data-stu-id="9aaf0-259">for</span></span>
* <span data-ttu-id="9aaf0-260">foreach</span><span class="sxs-lookup"><span data-stu-id="9aaf0-260">foreach</span></span>
* <span data-ttu-id="9aaf0-261">if</span><span class="sxs-lookup"><span data-stu-id="9aaf0-261">if</span></span>
* <span data-ttu-id="9aaf0-262">else</span><span class="sxs-lookup"><span data-stu-id="9aaf0-262">else</span></span>
* <span data-ttu-id="9aaf0-263">lock</span><span class="sxs-lookup"><span data-stu-id="9aaf0-263">lock</span></span>
* <span data-ttu-id="9aaf0-264">switch</span><span class="sxs-lookup"><span data-stu-id="9aaf0-264">switch</span></span>
* <span data-ttu-id="9aaf0-265">try</span><span class="sxs-lookup"><span data-stu-id="9aaf0-265">try</span></span>
* <span data-ttu-id="9aaf0-266">catch</span><span class="sxs-lookup"><span data-stu-id="9aaf0-266">catch</span></span>
* <span data-ttu-id="9aaf0-267">finally</span><span class="sxs-lookup"><span data-stu-id="9aaf0-267">finally</span></span>
* <span data-ttu-id="9aaf0-268">Verwenden</span><span class="sxs-lookup"><span data-stu-id="9aaf0-268">using</span></span>
* <span data-ttu-id="9aaf0-269">while</span><span class="sxs-lookup"><span data-stu-id="9aaf0-269">while</span></span>

<span data-ttu-id="9aaf0-270">C#-Razor-Schlüsselwörter müssen doppelte werden mit Escapezeichen versehen `@(@C# Razor Keyword)`, z. B. `@(@case)`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-270">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="9aaf0-271">Die erste `@` versieht den Razor-Parser, der zweite `@` versieht den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-271">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="9aaf0-272">Finden Sie unter dem vollständigen Beispiel unten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-272">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="9aaf0-273">Reservierte Schlüsselwörter, die nicht vom Razor verwendet</span><span class="sxs-lookup"><span data-stu-id="9aaf0-273">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="9aaf0-274">namespace</span><span class="sxs-lookup"><span data-stu-id="9aaf0-274">namespace</span></span>
* <span data-ttu-id="9aaf0-275">Klasse</span><span class="sxs-lookup"><span data-stu-id="9aaf0-275">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="9aaf0-276">Anzeigen von für eine Sicht generiert Razor C#-Klasse.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-276">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="9aaf0-277">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-277">Add the following class to your ASP.NET Core MVC project:</span></span>

<span data-ttu-id="9aaf0-278">[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-278">[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]</span></span>

<span data-ttu-id="9aaf0-279">Überschreiben Sie die `ICompilationService` von MVC hinzugefügt, mit der oben genannten Klasse;</span><span class="sxs-lookup"><span data-stu-id="9aaf0-279">Override the `ICompilationService` added by MVC with the above class;</span></span>

<span data-ttu-id="9aaf0-280">[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]</span><span class="sxs-lookup"><span data-stu-id="9aaf0-280">[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]</span></span>

<span data-ttu-id="9aaf0-281">Legen Sie einen Haltepunkt für die `Compile` Methode `CustomCompilationService` und Ansicht `compilationContent`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-281">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![Text-Schnellansicht Überblick compilationContent](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="9aaf0-283">Ansicht Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="9aaf0-283">View lookups and case sensitivity</span></span>

<span data-ttu-id="9aaf0-284">Das Razor-Ansichtsmodul führt die Groß-/Kleinschreibung Suchvorgänge für Ansichten.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-284">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="9aaf0-285">Allerdings wird die tatsächliche Suche durch die zugrunde liegende Datenquelle bestimmt:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-285">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="9aaf0-286">Basis-Dateiquelle:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-286">File based source:</span></span> 

    * <span data-ttu-id="9aaf0-287">Unter Betriebssystemen mit Groß-/Kleinschreibung beachten Dateisystemen (z. B. Windows) sind die physische Datei Anbieter Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-287">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="9aaf0-288">Z. B. `return View("Test")` würde `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` und alle anderen Schreibweise Varianten würde ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-288">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="9aaf0-289">In Dateisystemen Groß-/Kleinschreibung beachtet, einschließlich Linux, OS x und `EmbeddedFileProvider`, Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-289">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="9aaf0-290">Beispielsweise `return View("Test")` sieht speziell für `/Views/Home/Test.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-290">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="9aaf0-291">Vorkompilierte Sichten:</span><span class="sxs-lookup"><span data-stu-id="9aaf0-291">Precompiled views:</span></span>

   * <span data-ttu-id="9aaf0-292">Mit ASP.Net Core 2.0 und höher ist die vorkompilierte Sichten Nachschlagen Groß-/Kleinschreibung beachtet, unter allen Betriebssystemen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-292">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="9aaf0-293">Das Verhalten ist identisch mit dem Verhalten der physischen Datei des Anbieters unter Windows.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-293">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="9aaf0-294">Hinweis: Wenn zwei vorkompilierte Sichten nur im Fall unterschiedlich sind, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-294">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="9aaf0-295">Entwicklern wird empfohlen, die Groß-/Kleinschreibung von Datei- und Verzeichnisnamen, die Groß-/Kleinschreibung der Bereich, Controller-und Aktionsnamen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-295">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="9aaf0-296">Dadurch wird sichergestellt, dass die Bereitstellungen des zugrunde liegenden Dateisystems agnostisch bleiben.</span><span class="sxs-lookup"><span data-stu-id="9aaf0-296">This would ensure your deployments remain agnostic of the underlying file system.</span></span>
