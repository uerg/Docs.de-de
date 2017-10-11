---
title: "Razor-syntaxverweis für ASP.NET Core"
author: guardrex
description: "Erfahren Sie Markup Razor-Syntax für das Einbetten von serverbasiertem Code in Webseiten aus."
keywords: ASP.NET Core, Razor, Razor-Direktiven
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="d849e-104">Razor-Syntax für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d849e-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="d849e-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), und [Taylor Mullen](https://twitter.com/ntaylormullen)</span><span class="sxs-lookup"><span data-stu-id="d849e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), and [Taylor Mullen](https://twitter.com/ntaylormullen)</span></span>

<span data-ttu-id="d849e-106">Razor ist eine Markupsyntax für das Einbetten von serverbasiertem Code in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="d849e-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="d849e-107">Die Razor-Syntax besteht aus Razor-Markup, C#- und HTML.</span><span class="sxs-lookup"><span data-stu-id="d849e-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="d849e-108">Dateien, die in der Regel enthält Razor haben eine *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="d849e-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="d849e-109">Rendern von HTML</span><span class="sxs-lookup"><span data-stu-id="d849e-109">Rendering HTML</span></span>

<span data-ttu-id="d849e-110">Die Standardsprache für den Razor ist HTML.</span><span class="sxs-lookup"><span data-stu-id="d849e-110">The default Razor language is HTML.</span></span> <span data-ttu-id="d849e-111">Rendern von HTML in Razor-Markuperweiterung unterscheidet sich nicht als HTML-Datei aus einer HTML-Datei gerendert.</span><span class="sxs-lookup"><span data-stu-id="d849e-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="d849e-112">Setzen Sie die HTML-Markup in einem *cshtml* Razor-Datei, die er vom Server unverändert gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d849e-112">If you place HTML markup into a *.cshtml* Razor file, it's rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="d849e-113">Razor-syntax</span><span class="sxs-lookup"><span data-stu-id="d849e-113">Razor syntax</span></span>

<span data-ttu-id="d849e-114">Razor unterstützt C#- und verwendet die `@` Symbol für den Übergang von HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="d849e-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="d849e-115">Razor C#-Ausdrücke ausgewertet und rendert sie in der HTML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="d849e-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="d849e-116">Wenn ein `@` Symbol wird gefolgt von einer [Razor reserviertes Schlüsselwort](#razor-reserved-keywords), geht in Razor-spezifischer Markups.</span><span class="sxs-lookup"><span data-stu-id="d849e-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="d849e-117">Andernfalls geht es in einfachen c#.</span><span class="sxs-lookup"><span data-stu-id="d849e-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="d849e-118">Mit Escapezeichen eine `@` symbol in Razor-Markup, verwenden Sie eine zweite `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="d849e-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="d849e-119">Der Code wird in HTML gerendert, mit einem einzelnen `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="d849e-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="d849e-120">HTML-Attribute und Inhalt mit e-Mail-Adressen nicht behandeln die `@` Symbol als ein Zeichen Übergang.</span><span class="sxs-lookup"><span data-stu-id="d849e-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="d849e-121">Die e-Mail-Adressen im folgenden Beispiel werden durch Analysieren von Razor bleiben unverändert:</span><span class="sxs-lookup"><span data-stu-id="d849e-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="d849e-122">Implizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="d849e-122">Implicit Razor expressions</span></span>

<span data-ttu-id="d849e-123">Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code:</span><span class="sxs-lookup"><span data-stu-id="d849e-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="d849e-124">Mit Ausnahme von C#- `await` Schlüsselwort implicit-Ausdrücken keine Leerzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="d849e-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="d849e-125">Sie können Leerzeichen intermingle, wenn die C#-Anweisung eine klare beendet wurde:</span><span class="sxs-lookup"><span data-stu-id="d849e-125">You can intermingle spaces if the C# statement has a clear ending:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="d849e-126">Explizite Razor-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="d849e-126">Explicit Razor expressions</span></span>

<span data-ttu-id="d849e-127">Explizite Razor-Ausdrücke bestehen aus einer `@` Symbolbreite ausgeglichene Klammer.</span><span class="sxs-lookup"><span data-stu-id="d849e-127">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="d849e-128">Zum Zeitpunkt der letzten Woche zu rendern, wird das folgende Razor-Markup verwendet:</span><span class="sxs-lookup"><span data-stu-id="d849e-128">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="d849e-129">Alle Inhalte innerhalb der `@()` Klammern ausgewertet und in die Ausgabe des gerendert.</span><span class="sxs-lookup"><span data-stu-id="d849e-129">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="d849e-130">Implizite Ausdrücke, die im vorherigen Abschnitt beschriebenen darf keine Leerzeichen in der Regel enthalten.</span><span class="sxs-lookup"><span data-stu-id="d849e-130">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="d849e-131">Im folgenden Code wird nicht eine Woche nach der aktuellen Uhrzeit subtrahiert:</span><span class="sxs-lookup"><span data-stu-id="d849e-131">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="d849e-132">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="d849e-132">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="d849e-133">Sie können einen expliziten Ausdruck zum Verketten von Text mit einem Ergebnis des Ausdrucks verwenden:</span><span class="sxs-lookup"><span data-stu-id="d849e-133">You can use an explicit expression to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="d849e-134">Ohne die explizite Ausdruck `<p>Age@joe.Age</p>` so behandelt, als eine e-Mail-Adresse und `<p>Age@joe.Age</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d849e-134">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="d849e-135">Wenn ein expliziter Ausdruck geschrieben `<p>Age33</p>` gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d849e-135">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="d849e-136">Expression-Codierung</span><span class="sxs-lookup"><span data-stu-id="d849e-136">Expression encoding</span></span>

<span data-ttu-id="d849e-137">C#-Ausdrücke, die zu einer Zeichenfolge ausgewertet werden HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="d849e-137">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="d849e-138">C#-Ausdrücke, die ergeben `IHtmlContent` sind direkt über gerendert `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="d849e-138">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="d849e-139">C#-Ausdrücke, ergeben nicht `IHtmlContent` werden in eine Zeichenfolge konvertiert `ToString` und codiert werden, bevor sie gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="d849e-139">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="d849e-140">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="d849e-140">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="d849e-141">Der HTML-Code wird im Browser als gezeigt:</span><span class="sxs-lookup"><span data-stu-id="d849e-141">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="d849e-142">`HtmlHelper.Raw`Ausgabe wird nicht codierte aber als HTML-Markup gerendert.</span><span class="sxs-lookup"><span data-stu-id="d849e-142">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="d849e-143">Mit `HtmlHelper.Raw` unsanitized Benutzer Eingabe ist ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="d849e-143">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="d849e-144">Benutzereingabe möglicherweise böswilligen JavaScript- oder andere Exploits durchführen enthalten.</span><span class="sxs-lookup"><span data-stu-id="d849e-144">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="d849e-145">Es ist schwierig, Bereinigen von Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="d849e-145">Sanitizing user input is difficult.</span></span> <span data-ttu-id="d849e-146">Vermeiden Sie die Verwendung `HtmlHelper.Raw` Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="d849e-146">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="d849e-147">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="d849e-147">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="d849e-148">Razor-Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="d849e-148">Razor code blocks</span></span>

<span data-ttu-id="d849e-149">Razor-Codeblöcke beginnen Sie mit `@` und eingeschlossen werden `{}`.</span><span class="sxs-lookup"><span data-stu-id="d849e-149">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="d849e-150">Im Gegensatz zu Ausdrücken wird nicht C#-Code in Codeblöcken gerendert.</span><span class="sxs-lookup"><span data-stu-id="d849e-150">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="d849e-151">Codeblöcke Ausdrücke in einer Sicht gemeinsam zu nutzen die gleichen Bereich und werden in Reihenfolge definiert:</span><span class="sxs-lookup"><span data-stu-id="d849e-151">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="d849e-152">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="d849e-152">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="d849e-153">Implizite Übergänge</span><span class="sxs-lookup"><span data-stu-id="d849e-153">Implicit transitions</span></span>

<span data-ttu-id="d849e-154">In einem Codeblock die Standardsprache ist c#, aber Sie können den Übergang zurück zum HTML:</span><span class="sxs-lookup"><span data-stu-id="d849e-154">The default language in a code block is C#, but you can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="d849e-155">Explizite durch Trennzeichen getrennten Übergang</span><span class="sxs-lookup"><span data-stu-id="d849e-155">Explicit delimited transition</span></span>

<span data-ttu-id="d849e-156">Um einen Unterabschnitt eines Codeblocks definieren, die HTML gerendert werden soll, umschließen Sie die Zeichen für das Rendering mit den Razor  **\<Text >** Tag:</span><span class="sxs-lookup"><span data-stu-id="d849e-156">To define a sub-section of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="d849e-157">Verwenden Sie diesen Ansatz, wenn HTML zu rendern, die von einem HTML-Tags eingeschlossen wird nicht verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d849e-157">Use this approach when you want to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="d849e-158">Ohne ein HTML- oder Razor-Tag erhalten Sie einen Razor-Laufzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="d849e-158">Without an HTML or Razor tag, you receive a Razor runtime error.</span></span>

<span data-ttu-id="d849e-159">Die  **\<Text >** Tag eignet sich ebenfalls um Leerzeichen beim Rendern von Inhalt zu steuern.</span><span class="sxs-lookup"><span data-stu-id="d849e-159">The **\<text>** tag is also useful to control whitespace when rendering content.</span></span> <span data-ttu-id="d849e-160">Nur der Inhalt zwischen die  **\<Text >** -Tags wird gerendert, und keine Leerzeichen vor oder nach der  **\<Text >** Tags im HTML-Ausgabe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d849e-160">Only the content between the **\<text>** tags is rendered, and no whitespace before or after the **\<text>** tags appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="d849e-161">Explizite Zeile Übergang mit @:</span><span class="sxs-lookup"><span data-stu-id="d849e-161">Explicit Line Transition with @:</span></span>

<span data-ttu-id="d849e-162">Um den Rest der eine ganze Zeile in einem Codeblock als HTML zu rendern, verwenden die `@:` Syntax:</span><span class="sxs-lookup"><span data-stu-id="d849e-162">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="d849e-163">Ohne die `@:` im Code erhalten Sie einen Razor-Runtime-Fehler.</span><span class="sxs-lookup"><span data-stu-id="d849e-163">Without the `@:` in the code, you recieve a Razor runtime error.</span></span>

## <a name="control-structures"></a><span data-ttu-id="d849e-164">Steuerungsstrukturen</span><span class="sxs-lookup"><span data-stu-id="d849e-164">Control Structures</span></span>

<span data-ttu-id="d849e-165">Steuerungsstrukturen sind eine Erweiterung von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="d849e-165">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="d849e-166">Alle Aspekte von Codeblöcken (Übergang zu Markup Inline-c#) auch gelten für die folgenden Strukturen.</span><span class="sxs-lookup"><span data-stu-id="d849e-166">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="d849e-167">Konditionelle Abschnitte @if, ElseIf, #else- und@switch</span><span class="sxs-lookup"><span data-stu-id="d849e-167">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="d849e-168">`@if`Steuerelemente, wenn der Code ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="d849e-168">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="d849e-169">`else`und `else if` erfordern die `@` Symbol:</span><span class="sxs-lookup"><span data-stu-id="d849e-169">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="d849e-170">Sie können eine Switch-Anweisung wie folgt verwenden:</span><span class="sxs-lookup"><span data-stu-id="d849e-170">You can use a switch statement like this:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="d849e-171">Schleifen @for, @foreach, @while, und @do während</span><span class="sxs-lookup"><span data-stu-id="d849e-171">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="d849e-172">Sie können aus einer Vorlage gebildete HTML mit Schleifen Steueranweisungen rendern.</span><span class="sxs-lookup"><span data-stu-id="d849e-172">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="d849e-173">Um eine Liste der Personen zu rendern:</span><span class="sxs-lookup"><span data-stu-id="d849e-173">To render a list of people:</span></span>

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

<span data-ttu-id="d849e-174">Sie können eine der folgenden Schleifen Anweisungen verwenden:</span><span class="sxs-lookup"><span data-stu-id="d849e-174">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="d849e-175">Zusammengesetzte@using</span><span class="sxs-lookup"><span data-stu-id="d849e-175">Compound @using</span></span>

<span data-ttu-id="d849e-176">In c# ist ein `using` Anweisung wird verwendet, um sicherzustellen, dass ein Objekt wurde verworfen.</span><span class="sxs-lookup"><span data-stu-id="d849e-176">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="d849e-177">In Razor wird derselbe Mechanismus verwendet, um HTML-Hilfsmethoden zu erstellen, zusätzliche Inhalte enthalten.</span><span class="sxs-lookup"><span data-stu-id="d849e-177">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="d849e-178">Sie können z. B. HTML-Hilfsmethoden zum Rendern ein Formulartag mit nutzen die `@using` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="d849e-178">For instance, you can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="d849e-179">Sie können auch Aktionen auf Bereichsebene mit [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="d849e-179">You can also perform scope-level actions with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="d849e-180">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="d849e-180">@try, catch, finally</span></span>

<span data-ttu-id="d849e-181">Behandlung von Ausnahmen ist vergleichbar mit c#:</span><span class="sxs-lookup"><span data-stu-id="d849e-181">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="d849e-182">Razor hat die Möglichkeit, kritische Abschnitte mit Lock-Anweisungen zu schützen:</span><span class="sxs-lookup"><span data-stu-id="d849e-182">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="d849e-183">Kommentare</span><span class="sxs-lookup"><span data-stu-id="d849e-183">Comments</span></span>

<span data-ttu-id="d849e-184">Razor unterstützt C#- und HTML-Kommentare:</span><span class="sxs-lookup"><span data-stu-id="d849e-184">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="d849e-185">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="d849e-185">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="d849e-186">Razor-Kommentare werden vom Server entfernt, bevor auf der Webseite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d849e-186">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="d849e-187">Razor verwendet `@*  *@` zur Begrenzung von Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="d849e-187">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="d849e-188">Der folgende Code ist auskommentiert, damit der Server kein Markup Rendern:</span><span class="sxs-lookup"><span data-stu-id="d849e-188">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="d849e-189">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="d849e-189">Directives</span></span>

<span data-ttu-id="d849e-190">Razor-Direktiven werden durch implicit-Ausdrücken mit reservierten Schlüsselwörtern folgenden dargestellt die `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="d849e-190">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="d849e-191">Eine Richtlinie ändert sich normalerweise die Möglichkeit, die eine Sicht wird analysiert oder andere Funktionalität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="d849e-191">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="d849e-192">Verstehen, wie der Razor-Code für eine Ansicht generiert erleichtert das Verständnis der Funktionsweise von Direktiven.</span><span class="sxs-lookup"><span data-stu-id="d849e-192">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="d849e-193">Der Code generiert eine Klasse, die etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d849e-193">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="d849e-194">Weiter unten in diesem Artikel im Abschnitt [Anzeigen der Razor c#-Klasse, die für eine Sicht generiert](#viewing-the-razor-c-class-generated-for-a-view) wird erläutert, wie diese generierte Klasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d849e-194">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="d849e-195">Die `@using` Direktive fügt c# `using` Richtlinie in die generierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="d849e-195">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="d849e-196">Die `@model` Richtlinie gibt den Typ des Modells an eine Ansicht übergeben:</span><span class="sxs-lookup"><span data-stu-id="d849e-196">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="d849e-197">Wenn Sie eine ASP.NET-MVC-Anwendung Core einzelne Benutzerkonten erstellen die *Views/Account/Login.cshtml* -Ansicht enthält die folgende Deklaration einer Modell:</span><span class="sxs-lookup"><span data-stu-id="d849e-197">If you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="d849e-198">Die generierte Klasse erbt von `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="d849e-198">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="d849e-199">Razor macht eine `Model` Eigenschaft für den Zugriff auf das Modell an die Ansicht zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="d849e-199">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="d849e-200">Die `@model` Richtlinie gibt den Typ dieser Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d849e-200">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="d849e-201">Die Richtlinie gibt an, die `T` in `RazorPage<T>` , dass die generierte, die Klasse die Sicht abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="d849e-201">The directive specifies the `T` in `RazorPage<T>` that the generated class that your view derives from.</span></span> <span data-ttu-id="d849e-202">Wenn Sie nicht angeben, die `@model` Richtlinie, die `Model` -Eigenschaft ist vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d849e-202">If you don't specify the `@model` directive, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="d849e-203">Der Wert des Modells wird auf dem Controller an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="d849e-203">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="d849e-204">Finden Sie unter [stark typisierte Modelle und die @model Schlüsselwort](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="d849e-204">See [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) for more information.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="d849e-205">Die `@inherits` Richtlinie erhalten Sie Vollzugriff auf die Klasse erbt von die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="d849e-205">The `@inherits` directive gives you full control of the class your view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="d849e-206">Im folgenden finden einen benutzerdefinierten Typ des Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="d849e-206">The following is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="d849e-207">Die `CustomText` in einer Ansicht angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="d849e-207">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="d849e-208">Der Code wird der folgenden HTML-Code gerendert:</span><span class="sxs-lookup"><span data-stu-id="d849e-208">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

<span data-ttu-id="d849e-209">Sie können keine `@model` und `@inherits` in derselben Ansicht.</span><span class="sxs-lookup"><span data-stu-id="d849e-209">You can't use `@model` and `@inherits` in the same view.</span></span> <span data-ttu-id="d849e-210">Sie können `@inherits` in eine *_ViewImports.cshtml* -Datei, die die Sicht importiert:</span><span class="sxs-lookup"><span data-stu-id="d849e-210">You can have `@inherits` in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="d849e-211">Im folgenden finden ein Beispiel für eine stark typisierte Ansicht:</span><span class="sxs-lookup"><span data-stu-id="d849e-211">The following is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="d849e-212">Wenn "rick@contoso.com" übergeben wird im Modell, Ansicht die folgenden HTML-Markup generiert:</span><span class="sxs-lookup"><span data-stu-id="d849e-212">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="d849e-213">Die `@inject` Richtlinie ermöglicht das Einfügen eines Diensts aus Ihrer [Dienstcontainer](xref:fundamentals/dependency-injection) in der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="d849e-213">The `@inject` directive enables you to inject a service from your [service container](xref:fundamentals/dependency-injection) into your view.</span></span> <span data-ttu-id="d849e-214">Finden Sie unter [Abhängigkeitsinjektion in Sichten](xref:mvc/views/dependency-injection) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="d849e-214">See [Dependency injection into views](xref:mvc/views/dependency-injection) for more information.</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="d849e-215">Die `@functions` Richtlinie können Sie funktionsebenendetails auf Inhalte zu einer Sicht hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="d849e-215">The `@functions` directive enables you to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="d849e-216">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d849e-216">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="d849e-217">Der Code generiert den folgenden HTML-Markup:</span><span class="sxs-lookup"><span data-stu-id="d849e-217">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="d849e-218">Der folgende Code ist die generierte Razor C#-Klasse:</span><span class="sxs-lookup"><span data-stu-id="d849e-218">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="d849e-219">Die `@section` Richtlinie dient in Verbindung mit der [Layout](xref:mvc/views/layout) Ansichten zum Rendern von Inhalt in verschiedenen Teilen der HTML-Seite zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="d849e-219">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="d849e-220">Finden Sie unter [Abschnitte](xref:mvc/views/layout#layout-sections-label) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="d849e-220">See [Sections](xref:mvc/views/layout#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="d849e-221">Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="d849e-221">Tag Helpers</span></span>

<span data-ttu-id="d849e-222">Es gibt drei Anweisungen, die betreffen [Tag Hilfsprogramme](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="d849e-222">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="d849e-223">Direktive</span><span class="sxs-lookup"><span data-stu-id="d849e-223">Directive</span></span> | <span data-ttu-id="d849e-224">Funktion</span><span class="sxs-lookup"><span data-stu-id="d849e-224">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="d849e-225">Stellt Tag Hilfen zu einer Ansicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d849e-225">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="d849e-226">Entfernt die Tag-Hilfsprogramme, die zuvor aus einer Sicht hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d849e-226">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="d849e-227">Gibt ein Tagpräfix Tag Helper-Unterstützung zu aktivieren und Tag Helper Verwendung als explizite Anforderung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d849e-227">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="d849e-228">Razor reservierte Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="d849e-228">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="d849e-229">Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="d849e-229">Razor keywords</span></span>

* <span data-ttu-id="d849e-230">Seite "(erfordert ASP.NET Core 2.0 und höher)</span><span class="sxs-lookup"><span data-stu-id="d849e-230">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="d849e-231">-Funktionen</span><span class="sxs-lookup"><span data-stu-id="d849e-231">functions</span></span>
* <span data-ttu-id="d849e-232">inherits</span><span class="sxs-lookup"><span data-stu-id="d849e-232">inherits</span></span>
* <span data-ttu-id="d849e-233">Modell</span><span class="sxs-lookup"><span data-stu-id="d849e-233">model</span></span>
* <span data-ttu-id="d849e-234">section</span><span class="sxs-lookup"><span data-stu-id="d849e-234">section</span></span>
* <span data-ttu-id="d849e-235">Hilfsprogramm (von ASP.NET Core derzeit nicht unterstützt)</span><span class="sxs-lookup"><span data-stu-id="d849e-235">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="d849e-236">Razor-Schlüsselwörter werden mit Escapezeichen versehen `@(Razor Keyword)` (z. B. `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="d849e-236">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="d849e-237">C#-Razor-Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="d849e-237">C# Razor keywords</span></span>

* <span data-ttu-id="d849e-238">case</span><span class="sxs-lookup"><span data-stu-id="d849e-238">case</span></span>
* <span data-ttu-id="d849e-239">do</span><span class="sxs-lookup"><span data-stu-id="d849e-239">do</span></span>
* <span data-ttu-id="d849e-240">default</span><span class="sxs-lookup"><span data-stu-id="d849e-240">default</span></span>
* <span data-ttu-id="d849e-241">for</span><span class="sxs-lookup"><span data-stu-id="d849e-241">for</span></span>
* <span data-ttu-id="d849e-242">foreach</span><span class="sxs-lookup"><span data-stu-id="d849e-242">foreach</span></span>
* <span data-ttu-id="d849e-243">if</span><span class="sxs-lookup"><span data-stu-id="d849e-243">if</span></span>
* <span data-ttu-id="d849e-244">else</span><span class="sxs-lookup"><span data-stu-id="d849e-244">else</span></span>
* <span data-ttu-id="d849e-245">lock</span><span class="sxs-lookup"><span data-stu-id="d849e-245">lock</span></span>
* <span data-ttu-id="d849e-246">switch</span><span class="sxs-lookup"><span data-stu-id="d849e-246">switch</span></span>
* <span data-ttu-id="d849e-247">try</span><span class="sxs-lookup"><span data-stu-id="d849e-247">try</span></span>
* <span data-ttu-id="d849e-248">catch</span><span class="sxs-lookup"><span data-stu-id="d849e-248">catch</span></span>
* <span data-ttu-id="d849e-249">finally</span><span class="sxs-lookup"><span data-stu-id="d849e-249">finally</span></span>
* <span data-ttu-id="d849e-250">Verwenden</span><span class="sxs-lookup"><span data-stu-id="d849e-250">using</span></span>
* <span data-ttu-id="d849e-251">while</span><span class="sxs-lookup"><span data-stu-id="d849e-251">while</span></span>

<span data-ttu-id="d849e-252">C#-Razor-Schlüsselwörter muss mit doppelten Escapezeichen `@(@C# Razor Keyword)` (z. B. `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="d849e-252">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="d849e-253">Die erste `@` versieht den Razor-Parser.</span><span class="sxs-lookup"><span data-stu-id="d849e-253">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="d849e-254">Die zweite `@` versieht den C#-Parser.</span><span class="sxs-lookup"><span data-stu-id="d849e-254">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="d849e-255">Reservierte Schlüsselwörter, die nicht vom Razor verwendet</span><span class="sxs-lookup"><span data-stu-id="d849e-255">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="d849e-256">namespace</span><span class="sxs-lookup"><span data-stu-id="d849e-256">namespace</span></span>
* <span data-ttu-id="d849e-257">Klasse</span><span class="sxs-lookup"><span data-stu-id="d849e-257">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="d849e-258">Anzeigen von für eine Sicht generiert Razor C#-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d849e-258">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="d849e-259">Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="d849e-259">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="d849e-260">Überschreiben Sie die `RazorTemplateEngine` hinzugefügt, indem MVC mit der `CustomTemplateEngine` Klasse:</span><span class="sxs-lookup"><span data-stu-id="d849e-260">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="d849e-261">Legen Sie einen Haltepunkt für die `return csharpDocument` SoH `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="d849e-261">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="d849e-262">Bei der Ausführung des Programms am Haltepunkt beendet wird, zeigen Sie den Wert des `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="d849e-262">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Text-Schnellansicht Überblick generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="d849e-264">Ansicht Suchvorgänge und Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="d849e-264">View lookups and case sensitivity</span></span>

<span data-ttu-id="d849e-265">Das Razor-Ansichtsmodul führt die Groß-/Kleinschreibung Suchvorgänge für Ansichten.</span><span class="sxs-lookup"><span data-stu-id="d849e-265">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="d849e-266">Allerdings wird die tatsächliche Suche durch das zugrunde liegende Dateisystem bestimmt:</span><span class="sxs-lookup"><span data-stu-id="d849e-266">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="d849e-267">Basis-Dateiquelle:</span><span class="sxs-lookup"><span data-stu-id="d849e-267">File based source:</span></span> 
  * <span data-ttu-id="d849e-268">Unter Betriebssystemen mit Groß-/Kleinschreibung beachten Dateisystemen (z. B. Windows) sind die physische Datei Anbieter Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="d849e-268">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="d849e-269">Beispielsweise `return View("Test")` ergibt Übereinstimmungen für */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, und eine andere Variante der Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="d849e-269">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="d849e-270">Auf Dateisystemen, Groß-/Kleinschreibung unterschieden (z. B. Linux, OS x, und mit `EmbeddedFileProvider`), Suchvorgänge Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="d849e-270">On case sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case sensitive.</span></span> <span data-ttu-id="d849e-271">Beispielsweise `return View("Test")` speziell Übereinstimmungen */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d849e-271">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="d849e-272">Vorkompilierte Sichten: Nachschlagen vorkompilierte Sichten mit ASP.Net Core 2.0 und höher, Groß-/Kleinschreibung beachtet, unter allen Betriebssystemen ist.</span><span class="sxs-lookup"><span data-stu-id="d849e-272">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="d849e-273">Das Verhalten ist identisch mit dem Verhalten der physischen Datei des Anbieters unter Windows.</span><span class="sxs-lookup"><span data-stu-id="d849e-273">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="d849e-274">Wenn zwei vorkompilierte Sichten nur im Fall unterschiedlich sind, ist das Ergebnis der Suche nicht deterministisch.</span><span class="sxs-lookup"><span data-stu-id="d849e-274">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="d849e-275">Entwicklern wird empfohlen, die Groß-/Kleinschreibung von Datei- und Verzeichnisnamen, die Groß-/Kleinschreibung der Bereich, Controller-und Aktionsnamen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="d849e-275">Developers are encouraged to match the casing of file and directory names to the casing of area, controller, and action names.</span></span> <span data-ttu-id="d849e-276">Dadurch wird sichergestellt, dass Ihre Bereitstellungen ihre Ansichten unabhängig von der zugrunde liegenden Dateisystem gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="d849e-276">This ensures your deployments will find their views regardless of the underlying file system.</span></span>
