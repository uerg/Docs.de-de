---
title: "Razor-Syntaxverweis für ASP.NET Core"
author: rick-anderson
description: Details-Razor-syntax
keywords: ASP.NET Core Razor
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: fff2f98592473a9baf6a2d4e360fec3026b7210d
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Razor-Syntax für ASP.NET Core

Durch [Taylor Mullen](https://twitter.com/ntaylormullen) und [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>Was ist Razor?

Razor ist eine Markupsyntax für das Einbetten von serverbasierten Code in Webseiten. Die Razor-Syntax besteht aus C#- und HTML-Razor-Markup. Dateien, die in der Regel enthält Razor haben eine *cshtml* Dateierweiterung.

## <a name="rendering-html"></a>Rendern von HTML

Die Standardsprache für den Razor ist HTML. Rendern von HTML Razor ist unterscheidet sich nicht in einer HTML-Datei. Eine Razor-Datei durch Folgendes Markup:

```html
<p>Hello World</p>
   ```

Wird unverändert gerendert `<p>Hello World</p>` vom Server.

## <a name="razor-syntax"></a>Razor-syntax

Razor unterstützt C#- und verwendet die `@` Symbol für den Übergang von HTML in c#. Razor C#-Ausdrücke ausgewertet und rendert sie in der HTML-Ausgabe. Razor kann aus HTML in C# oder Razor-spezifisches Markup übergehen. Wenn ein `@` Symbol wird gefolgt von einer [Razor reserviertes Schlüsselwort](#razor-reserved-keywords) in Razor-spezifischer Markups geht, andernfalls geht in einfachen C#.

<a name=escape-at-label></a>

HTML mit `@` Symbole mit einer zweiten mit Escapezeichen versehen werden müssen möglicherweise `@` Symbol. Zum Beispiel:

```html
<p>@@Username</p>
   ```

würden Sie folgenden HTML-Code zum Rendern:

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

HTML-Attribute und Inhalt mit e-Mail-Adressen nicht behandeln die `@` Symbol als ein Zeichen Übergang.

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Implizite Razor-Ausdrücke

Implizite Razor-Ausdrücke beginnen mit `@` gefolgt von C#-Code. Zum Beispiel:

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Mit Ausnahme von C#- `await` Schlüsselwort implicit-Ausdrücken keine Leerzeichen enthalten. Sie können z. B. Leerzeichen intermingle, solange die C#-Anweisung eine klare beendet wurde:

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Explizite Razor-Ausdrücke

Explizite Razor-Ausdrücke besteht ein @-Zeichen ausgeglichene Klammer. Geben Sie beispielsweise Folgendes ein, um die Uhrzeit der letzten Woche zu rendern:

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Alle Inhalte innerhalb der @ Klammer () ausgewertet und in die Ausgabe des gerendert.

Implicit-Ausdrücken enthalten keine Leerzeichen in der Regel. Beispielsweise wird in den folgenden Code eine Woche nicht von der aktuellen Zeit abgezogen:

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

Die folgenden HTML-Code gerendert:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

Sie können einen expliziten Ausdruck zum Verketten von Text mit einem Ergebnis des Ausdrucks verwenden:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

Ohne die explizite Ausdruck `<p>Age@joe.Age</p>` behandelt werden würde, als eine e-Mail-Adresse und `<p>Age@joe.Age</p>` würden gerendert werden. Wenn ein expliziter Ausdruck geschrieben `<p>Age33</p>` gerendert wird.

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>Expression-Codierung

C#-Ausdrücke, die zu einer Zeichenfolge ausgewertet werden HTML-codiert. C#-Ausdrücke, die ergeben `IHtmlContent` sind direkt über gerendert *IHtmlContent.WriteTo*. C#-Ausdrücke, ergeben nicht *IHtmlContent* in eine Zeichenfolge konvertiert werden (durch *ToString*) und codiert werden, bevor sie gerendert werden. Um beispielsweise das folgende Markup für den Razor:

```html
@("<span>Hello World</span>")
   ```

Rendert das HTML-Code:

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

Der Browser als gerendert wird:

`<span>Hello World</span>`

`HtmlHelper``Raw` Ausgabe nicht codierte jedoch als HTML-Markup gerendert wird.

>[!WARNING]
> Mit `HtmlHelper.Raw` unsanitized Benutzer Eingabe ist ein Sicherheitsrisiko dar. Benutzereingabe möglicherweise böswilligen JavaScript- oder andere Exploits durchführen enthalten. Bereinigen von Benutzereingaben ist schwierig, vermeiden Sie die Verwendung `HtmlHelper.Raw` auf Benutzereingaben.

Das folgende Razor-Markup:

```html
@Html.Raw("<span>Hello World</span>")
   ```

Rendert das HTML-Code:

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Razor-Codeblöcke

Razor-Codeblöcke beginnen Sie mit `@` und eingeschlossen werden `{}`. Im Gegensatz zu Ausdrücken wird C#-Code in Codeblöcken nicht gerendert. Codeblöcke und Ausdrücke in einer Razor-Seite gemeinsam denselben Bereich und werden in Reihenfolge definiert (Deklarationen in einem Codeblock werden innerhalb des Bereichs bei späteren Codeblöcke und Ausdrücke sein).

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

Würden Sie zum Rendern:

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>Implizite Übergänge

In einem Codeblock die Standardsprache ist c#, aber Sie können den Übergang zurück zum HTML. HTML in einem Codeblock erfolgt ein Wechsel zurück in das Rendern von HTML:

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>Explizite durch Trennzeichen getrennten Übergang

Um einen Unterabschnitt eines Codeblocks definieren, die HTML gerendert werden soll, umschließen Sie die Zeichen, die mit dem Razor gerendert werden `<text>` Tag:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Im Allgemeinen verwenden Sie diesen Ansatz, wenn Sie möchten, HTML zu rendern, die nicht von einer HTML-Tags eingeschlossen ist. Ohne ein HTML- oder Razor-Tag erhalten Sie einen Razor-Laufzeitfehler.

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>Explizite Zeile Übergang mit`@:`

Um den Rest der eine ganze Zeile in einem Codeblock als HTML zu rendern, verwenden die `@:` Syntax:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Ohne die `@:` im obigen Code erhalten Sie eine Razor Laufzeitfehler auftreten.

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>Steuerungsstrukturen

Steuerungsstrukturen sind eine Erweiterung von Codeblöcken. Alle Aspekte von Codeblöcken (Übergang zu Markup Inline-c#) auch gelten für die folgenden Strukturen.

### <a name="conditionals-if-else-if-else-and-switch"></a>Bedingungen `@if`, `else if`, `else` und`@switch`

Die `@if` Familie steuert, wann der Code ausgeführt wird:

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`und `else if` erfordern die `@` Symbol:

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

Sie können eine Switch-Anweisung wie folgt verwenden:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Schleifen `@for`, `@foreach`, `@while`, und`@do while`

Sie können aus einer Vorlage gebildete HTML mit Schleifen Steueranweisungen rendern. Geben Sie beispielsweise Folgendes ein, um eine Liste der Personen zu rendern:

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

Sie können eine der folgenden Schleifen Anweisungen verwenden:

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

### <a name="compound-using"></a>Zusammengesetzte`@using`

In c# eine using-Anweisung wird verwendet, um sicherzustellen, dass ein Objekt wurde verworfen. In Razor kann denselben Mechanismus verwendet werden, um HTML-Hilfsmethoden zu erstellen, die zusätzliche Inhalte enthalten. Es können z. B. HTML-Hilfsmethoden zum Rendern ein Formulartag mit nutzen die `@using` Anweisung:

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

Sie können auch Aktionen Bereich Ebene wie oben mit [Tag Hilfsprogramme](tag-helpers/index.md).

### <a name="try-catch-finally"></a>`@try`, `catch`, `finally`

Behandlung von Ausnahmen ist vergleichbar mit c#:

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor hat die Möglichkeit, kritische Abschnitte mit Lock-Anweisungen zu schützen:

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Kommentare

Razor unterstützt C#- und HTML-Kommentare. Das folgende Markup:

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

Wird vom Server als gerendert werden:

```none
<!-- HTML comment -->
```

Razor-Kommentare werden vom Server entfernt, bevor die Seite gerendert wird. Razor verwendet `@*  *@` zur Begrenzung von Kommentaren. Der folgende Code ist auskommentiert, damit der Server nicht Markup gerendert werden:

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

## <a name="directives"></a>Anweisungen

Razor-Direktiven werden durch implicit-Ausdrücken mit reservierten Schlüsselwörtern folgenden dargestellt die `@` Symbol. Eine Richtlinie wird in der Regel ändern, die eine Seite analysiert wird wie oder andere Funktionalität innerhalb der Razor-Seite zu aktivieren.

Verstehen, wie der Razor-Code für eine Ansicht generiert so können sie einfacher zu verstehen, wie Anweisungen funktionieren. Eine Razor-Seite wird verwendet, um eine C#-Datei zu generieren. Beispielsweise diese Razor-Seite:

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Generiert eine Klasse, die etwa wie folgt an:

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

[Anzeigen der Razor c#-Klasse, die für eine Sicht generiert](#razor-customcompilationservice-label) wird erläutert, wie diese generierte Klasse angezeigt.

### `@using`

Die `@using` Richtlinie fügen die c#- `using` Richtlinie auf die generierten Razor-Seite:

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

Die `@model` Richtlinie gibt den Typ des Modells auf der Seite "Razor" übergeben. Es verwendet die folgende Syntax:

```none
@model TypeNameOfModel
   ```

Angenommen, wenn Sie eine ASP.NET-MVC-Anwendung Core einzelne Benutzerkonten erstellen die *Views/Account/Login.cshtml* Razor-Ansicht enthält die folgende Deklaration einer Modell:

```csharp
@model LoginViewModel
   ```

Im vorangehenden Klassenbeispiel die generierten Klasse erbt von `RazorPage<dynamic>`. Durch Hinzufügen einer `@model` Sie steuern, welche geerbt wird. Beispiel:

```csharp
@model LoginViewModel
   ```

Generiert die folgende Klasse

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

Razor-Seiten verfügbar machen eine `Model` Eigenschaft für den Zugriff auf das Modell auf der Seite "übergeben.

```html
<div>The Login Email: @Model.Email</div>
   ```

Die `@model` -Direktive angegeben, den Typ dieser Eigenschaft (durch Angabe der `T` in `RazorPage<T>` , die die generierte Klasse für die Seite abgeleitet). Wenn Sie nicht angeben, die `@model` Richtlinie die `Model` Eigenschaft werden vom Typ `dynamic`. Der Wert des Modells wird auf dem Controller an die Ansicht übergeben. Finden Sie unter [stark typisierte Modelle und die @model Schlüsselwort](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) für Weitere Informationen.

### `@inherits`

Die `@inherits` Richtlinie erhalten Sie Vollzugriff auf die Klasse erbt von die Razor-Seite:

```none
@inherits TypeNameOfClassToInheritFrom
   ```

Beispielsweise angenommen, mussten wir die folgenden benutzerdefinierten Razor-Seitentyp:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

Die folgenden Razor generieren würden `<div>Custom text: Hello World</div>`.

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

Sie können keine `@model` und `@inherits` auf derselben Seite. Sie können `@inherits` in einer *_ViewImports.cshtml* -Datei, die die Razor-Seite importiert. Wenn die Razor-Ansicht die folgenden importiert z. B. *_ViewImports.cshtml* Datei:

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Die folgenden stark typisierte Razor-Seite

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

Wird diese HTML-Markup generiert:

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

Beim übergeben "[Rick@contoso.com](mailto:Rick@contoso.com)" im Modell:

   Weitere Informationen finden Sie unter [Layout](layout.md).

### `@inject`

Die `@inject` Richtlinie ermöglicht das Einfügen eines Diensts aus Ihrer [Dienstcontainer](../../fundamentals/dependency-injection.md) in der Razor-Seite für die Verwendung. Finden Sie unter [Abhängigkeitsinjektion in Sichten](dependency-injection.md).

<a name="functions"></a>

### `@functions`

Die `@functions` Richtlinie ermöglicht es Ihnen, die Funktion Inhalt der Razor-Seite hinzufügen. Die Syntax lautet:

```none
@functions { // C# Code }
   ```

Zum Beispiel:

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

Wird der folgende HTML-Markup generiert:

```none
<div>From method: Hello</div>
   ```

Der generierte Razor c# sieht wie folgt:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

Die `@section` Richtlinie dient in Verbindung mit der [Layoutseite](layout.md) Ansichten zum Rendern von Inhalt in verschiedene Teile des gerenderten HTML-Seite zu aktivieren. Finden Sie unter [Abschnitte](layout.md#layout-sections-label) für Weitere Informationen.

## <a name="tag-helpers"></a>Tag-Hilfsprogramme

Die folgenden [Tag Hilfsprogramme](tag-helpers/index.md) Direktiven werden in die bereitgestellten Links beschrieben.

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Razor reservierte Schlüsselwörter

### <a name="razor-keywords"></a>Razor-Schlüsselwörter

* Seite "(erfordert ASP.NET Core 2.0 und höher)
* -Funktionen
* inherits
* Modell
* section
* Hilfsprogramm (nicht von ASP.NET Core unterstützt.)

Razor-Schlüsselwörter können mit Escapezeichen versehen werden `@(Razor Keyword)`, z. B. `@(functions)`. Finden Sie unter dem vollständigen Beispiel unten.

### <a name="c-razor-keywords"></a>C#-Razor-Schlüsselwörter

* case
* do
* default
* for
* foreach
* if
* else
* lock
* switch
* try
* catch
* finally
* Verwenden
* while

C#-Razor-Schlüsselwörter müssen doppelte werden mit Escapezeichen versehen `@(@C# Razor Keyword)`, z. B. `@(@case)`. Die erste `@` versieht den Razor-Parser, der zweite `@` versieht den C#-Parser. Finden Sie unter dem vollständigen Beispiel unten.

### <a name="reserved-keywords-not-used-by-razor"></a>Reservierte Schlüsselwörter, die nicht vom Razor verwendet

* namespace
* Klasse

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Anzeigen von für eine Sicht generiert Razor C#-Klasse.

Fügen Sie dem ASP.NET Core MVC-Projekt die folgende Klasse hinzu:

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

Überschreiben Sie die `ICompilationService` von MVC hinzugefügt, mit der oben genannten Klasse;

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

Legen Sie einen Haltepunkt für die `Compile` Methode `CustomCompilationService` und Ansicht `compilationContent`.

![Text-Schnellansicht Überblick compilationContent](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>Ansicht Suchvorgänge und Groß-/Kleinschreibung

Das Razor-Ansichtsmodul führt die Groß-/Kleinschreibung Suchvorgänge für Ansichten. Allerdings wird die tatsächliche Suche durch die zugrunde liegende Datenquelle bestimmt:

* Basis-Dateiquelle: 

    * Unter Betriebssystemen mit Groß-/Kleinschreibung beachten Dateisystemen (z. B. Windows) sind die physische Datei Anbieter Suchvorgänge Groß-/Kleinschreibung beachtet. Z. B. `return View("Test")` würde `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` und alle anderen Schreibweise Varianten würde ermittelt werden.
    * In Dateisystemen Groß-/Kleinschreibung beachtet, einschließlich Linux, OS x und `EmbeddedFileProvider`, Suchvorgänge Groß-/Kleinschreibung beachtet. Beispielsweise `return View("Test")` sieht speziell für `/Views/Home/Test.cshtml`.
        
* Vorkompilierte Sichten:

   * Mit ASP.Net Core 2.0 und höher ist die vorkompilierte Sichten Nachschlagen Groß-/Kleinschreibung beachtet, unter allen Betriebssystemen. Das Verhalten ist identisch mit dem Verhalten der physischen Datei des Anbieters unter Windows. 
   Hinweis: Wenn zwei vorkompilierte Sichten nur im Fall unterschiedlich sind, ist das Ergebnis der Suche nicht deterministisch.

Entwicklern wird empfohlen, die Groß-/Kleinschreibung von Datei- und Verzeichnisnamen, die Groß-/Kleinschreibung der Bereich, Controller-und Aktionsnamen übereinstimmen. Dadurch wird sichergestellt, dass die Bereitstellungen des zugrunde liegenden Dateisystems agnostisch bleiben.
