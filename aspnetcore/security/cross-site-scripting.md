---
title: Verhindern von Cross-Site Scripting (XSS) in ASP.NET Core
author: rick-anderson
description: Informationen Sie zu Cross-Site Scripting (XSS) und Techniken zur Behebung dieses Sicherheitsrisiko in ASP.NET Core-Apps.
ms.author: riande
ms.date: 10/14/2016
uid: security/cross-site-scripting
ms.openlocfilehash: 4784b1775d955f0ef00526e50b960fc873ea218d
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342210"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="c683f-103">Verhindern von Cross-Site Scripting (XSS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c683f-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="c683f-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c683f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c683f-105">Cross-Site Scripting (XSS) ist ein Sicherheitsrisiko, wodurch einen Angreifer, clientseitige Skripts (in der Regel "JavaScript") in Webseiten zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="c683f-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="c683f-106">Wenn andere Benutzer betroffene Seiten, die die Angreifer Skripts ausgeführt werden laden, wodurch der Angreifer Cookies und Sitzungstoken, stehlen ändern Sie den Inhalt der Webseite über DOM-Bearbeitung oder zu einer anderen Seite der Browser umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="c683f-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="c683f-107">XSS-Anfälligkeiten finden in der Regel auf, wenn eine Anwendung Benutzereingaben erhält und ihn auf einer Seite gibt ohne überprüfen, Codierung, oder er Escapezeichen.</span><span class="sxs-lookup"><span data-stu-id="c683f-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="c683f-108">Zum Schützen Ihrer Anwendung vor XSS</span><span class="sxs-lookup"><span data-stu-id="c683f-108">Protecting your application against XSS</span></span>

<span data-ttu-id="c683f-109">An einer grundlegenden Ebene XSS funktioniert, indem Sie Ihre Anwendung in das Einfügen von Konten eine `<script>` Tag in die gerenderte Seite oder durch Einfügen einer `On*` Ereignis in ein Element.</span><span class="sxs-lookup"><span data-stu-id="c683f-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="c683f-110">Entwickler sollten die folgenden Schritte für die Verhinderung von verwenden, um zu vermeiden, die Einführung von XSS in ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c683f-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="c683f-111">Nicht vertrauenswürdige Daten abgelegt nie Ihrer HTML-Eingaben, es sei denn, Sie die restlichen Schritte befolgen.</span><span class="sxs-lookup"><span data-stu-id="c683f-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="c683f-112">Nicht vertrauenswürdige Daten ist keine Daten, die von Angreifern, HTML-Formulareingaben, Abfragezeichenfolgen, HTTP-Header selbst Daten stammen aus einer Datenbank, wie ein Angreifer möglicherweise auf Ihre Datenbank zu verletzen, auch wenn sie Ihre Anwendung verletzen können nicht gesteuert werden können.</span><span class="sxs-lookup"><span data-stu-id="c683f-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="c683f-113">Stellen Sie sicher, dass es sich um HTML-codiert ist, vor dem Einfügen von nicht vertrauenswürdiger Daten innerhalb eines HTML-Elements.</span><span class="sxs-lookup"><span data-stu-id="c683f-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="c683f-114">HTML-Codierung die Zeichen wie z. B. dauert &lt; und ändert diese in eine sichere Form wie &amp;Lt;</span><span class="sxs-lookup"><span data-stu-id="c683f-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="c683f-115">Stellen Sie sicher, dass es sich um HTML-Attribut codiert ist, vor dem Einfügen von nicht vertrauenswürdiger Daten in ein HTML-Attribut.</span><span class="sxs-lookup"><span data-stu-id="c683f-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="c683f-116">HTML-attributcodierung eine Obermenge der HTML-Codierung codiert und zusätzliche Zeichen wie z. B. "und".</span><span class="sxs-lookup"><span data-stu-id="c683f-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="c683f-117">Platzieren Sie die Daten vor dem Einfügen von nicht vertrauenswürdiger Daten in JavaScript in einem HTML-Element, dessen Inhalt Sie zur Laufzeit abrufen.</span><span class="sxs-lookup"><span data-stu-id="c683f-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="c683f-118">JavaScript wird codiert, wenn dies ist nicht möglich, die Daten sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="c683f-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="c683f-119">JavaScript-Codierung nimmt gefährliche Zeichen für JavaScript und z. B. durch ihre hexadezimal ersetzt &lt; codiert werden sollen, als `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="c683f-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="c683f-120">Stellen Sie sicher, dass er URL-codiert ist, ehe Sie nicht vertrauenswürdige Daten in eine URL-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c683f-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="c683f-121">HTML-Codierung, die mithilfe von Razor</span><span class="sxs-lookup"><span data-stu-id="c683f-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="c683f-122">Die Razor-Engine, die automatisch verwendet, die in MVC codiert alle Ausgabe stammt von Variablen auf, es sei denn, Sie hart arbeiten, um zu verhindern, dass sie auf diese Weise.</span><span class="sxs-lookup"><span data-stu-id="c683f-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="c683f-123">Er verwendet die Codierungsregeln bei jeder Verwendung von HTML-Attribut der *@* Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="c683f-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="c683f-124">Als HTML ist das attributcodierung eine Obermenge der HTML-Codierung, die Dies bedeutet, dass Sie nicht darum kümmern, selbst, ob Sie HTML-Codierung oder HTML-attributcodierung verwenden sollten.</span><span class="sxs-lookup"><span data-stu-id="c683f-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="c683f-125">Achten Sie darauf, nur @ in einem HTML-Kontext nicht verwendet werden, wenn versucht wird, nicht vertrauenswürdige Eingaben direkt in JavaScript einzufügen.</span><span class="sxs-lookup"><span data-stu-id="c683f-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="c683f-126">Taghilfsprogramme werden auch Eingabe codieren, die Sie Tag-Parametern verwenden.</span><span class="sxs-lookup"><span data-stu-id="c683f-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="c683f-127">Nehmen Sie die folgende Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="c683f-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="c683f-128">Diese Sicht gibt den Inhalt der *UntrustedInput* Variable.</span><span class="sxs-lookup"><span data-stu-id="c683f-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="c683f-129">Diese Variable enthält einige Zeichen, die in der XSS-Angriffen, d. h. verwendet werden &lt;, "und &gt;.</span><span class="sxs-lookup"><span data-stu-id="c683f-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="c683f-130">Untersuchen die Quelle zeigt die gerenderte Ausgabe als codiert:</span><span class="sxs-lookup"><span data-stu-id="c683f-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="c683f-131">ASP.NET Core MVC bietet eine `HtmlString` Klasse, die automatisch bei Ausgabe codiert ist nicht.</span><span class="sxs-lookup"><span data-stu-id="c683f-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="c683f-132">Dies sollte nie in Kombination mit nicht vertrauenswürdigen Eingaben verwendet werden, wie diese eine XSS-Schwachstelle verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="c683f-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="c683f-133">JavaScript-Codierung mithilfe von Razor</span><span class="sxs-lookup"><span data-stu-id="c683f-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="c683f-134">Möglicherweise gibt es Zeiten, die zum Einfügen eines Werts in JavaScript in der Ansicht verarbeitet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c683f-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="c683f-135">Hierfür gibt es zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="c683f-135">There are two ways to do this.</span></span> <span data-ttu-id="c683f-136">Die sicherste Möglichkeit zum Einfügen von Werten ist, platzieren Sie den Wert in einem Datenattribut eines Tags aus, und diese in Ihrer JavaScript-abrufen.</span><span class="sxs-lookup"><span data-stu-id="c683f-136">The safest way to insert  values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="c683f-137">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c683f-137">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="c683f-138">Dadurch wird der folgenden HTML-Code generiert.</span><span class="sxs-lookup"><span data-stu-id="c683f-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="c683f-139">Die, die Folgendes ausgeführt wird, gerendert werden;</span><span class="sxs-lookup"><span data-stu-id="c683f-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="c683f-140">Sie können den JavaScript-Encoder auch direkt aufrufen,</span><span class="sxs-lookup"><span data-stu-id="c683f-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="c683f-141">Dies wird im Browser wie folgt gerendert;</span><span class="sxs-lookup"><span data-stu-id="c683f-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="c683f-142">Verketten Sie keine nicht vertrauenswürdige Eingaben in JavaScript zum Erstellen von DOM-Elementen.</span><span class="sxs-lookup"><span data-stu-id="c683f-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="c683f-143">Verwenden Sie `createElement()` und Zuweisen von Eigenschaftswerten entsprechend z. B. `node.TextContent=`, oder verwenden Sie `element.SetAttribute()` / `element[attribute]=` machen sich andernfalls DOM-basierte XSS.</span><span class="sxs-lookup"><span data-stu-id="c683f-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="c683f-144">Beim Zugriff auf den Encoder in code</span><span class="sxs-lookup"><span data-stu-id="c683f-144">Accessing encoders in code</span></span>

<span data-ttu-id="c683f-145">Die HTML, JavaScript und URL-Encoder für Ihren Code auf zwei Arten verfügbar sind, fügen Sie sie über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) oder Sie können die Standard-Encoder, die innerhalb der `System.Text.Encodings.Web` Namespace.</span><span class="sxs-lookup"><span data-stu-id="c683f-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="c683f-146">Wenn Sie die Standard-Encoder verwenden, und klicken Sie dann auf Sie angewendet Zeichenbereiche, die als sicher betrachtet wird nicht wirksam: die Standard-Encoder verwenden, die am sichersten Codierungsregeln möglich.</span><span class="sxs-lookup"><span data-stu-id="c683f-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="c683f-147">Die konfigurierbaren Encoder mithilfe von DI verwenden Ihre Konstruktoren dauert ein *HtmlEncoder*, *JavaScriptEncoder* und *UrlEncoder* Parameter nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="c683f-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="c683f-148">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c683f-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="c683f-149">Codierung von URL-Parametern</span><span class="sxs-lookup"><span data-stu-id="c683f-149">Encoding URL Parameters</span></span>

<span data-ttu-id="c683f-150">Wenn Sie eine URL-Abfragezeichenfolge mit nicht vertrauenswürdigen Eingaben als Wert Verwendung erstellen möchten, die `UrlEncoder` zum Codieren des Werts.</span><span class="sxs-lookup"><span data-stu-id="c683f-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="c683f-151">Ein auf ein Objekt angewendeter</span><span class="sxs-lookup"><span data-stu-id="c683f-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="c683f-152">Nach der Codierung der EncodedValue Variable enthält `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="c683f-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="c683f-153">Leerzeichen, Anführungszeichen, Interpunktionszeichen und andere unsichere Zeichen Prozent in ihrer hexadezimalen Wert codiert, z. B. wird ein Leerzeichen % 20.</span><span class="sxs-lookup"><span data-stu-id="c683f-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="c683f-154">Verwenden Sie nicht vertrauenswürdige Eingaben nicht als Teil eines URL-Pfads an.</span><span class="sxs-lookup"><span data-stu-id="c683f-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="c683f-155">Übergeben Sie nicht vertrauenswürdige Eingaben werden immer als Wert einer Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c683f-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="c683f-156">Anpassen der Encoder</span><span class="sxs-lookup"><span data-stu-id="c683f-156">Customizing the Encoders</span></span>

<span data-ttu-id="c683f-157">Standard-Encoder verwenden Sie eine Sicherungsliste auf den Basic Latin Unicode-Bereich beschränkt, und codieren Sie aller Zeichen außerhalb dieses Bereichs wie ihre entsprechenden Code.</span><span class="sxs-lookup"><span data-stu-id="c683f-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="c683f-158">Dieses Verhalten wirkt sich auch auf die Razor-TagHelper und HtmlHelper-Rendering aus, da es die Encoder verwendet wird, um Zeichenfolgen auszugeben.</span><span class="sxs-lookup"><span data-stu-id="c683f-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="c683f-159">Die Überlegung dahinter ist zum Schutz unbekannt oder zukünftige Browserfehler (vorherige Browserfehler haben gebracht Analyse basierend auf die Verarbeitung von nicht-englische Zeichen).</span><span class="sxs-lookup"><span data-stu-id="c683f-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="c683f-160">Wenn Ihre Website intensiven Gebrauch von nicht-lateinische Zeichen wie Chinesisch, macht ist Kyrillisch oder andere Personen dies wahrscheinlich nicht das gewünschte Verhalten.</span><span class="sxs-lookup"><span data-stu-id="c683f-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="c683f-161">Sie können anpassen, dass die Encoder-Sicherheitslisten Unicode enthält Bereiche, die für Ihre Anwendung während des Starts in entsprechende `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="c683f-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="c683f-162">Z. B. mit der Konfiguration können Sie eine Razor-HtmlHelper wie.</span><span class="sxs-lookup"><span data-stu-id="c683f-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="c683f-163">Wenn Sie die Quelle der Webseite anzeigen sehen Sie sich, dass es wie folgt mit dem chinesische Text codiert gerendert wurde.</span><span class="sxs-lookup"><span data-stu-id="c683f-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="c683f-164">Erweitern Sie die Zeichen, die als behandelt sicher vom Encoder Sie würde fügen die folgende Zeile in der `ConfigureServices()` -Methode in der `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="c683f-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="c683f-165">In diesem Beispiel wird die Sicherungsliste, um die Unicode-Bereich CjkUnifiedIdeographs einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="c683f-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="c683f-166">Die gerenderte Ausgabe wäre nun werden.</span><span class="sxs-lookup"><span data-stu-id="c683f-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="c683f-167">Liste mit sicheren Bereiche werden als Unicode-codeübersichten, nicht die Sprachen angegeben.</span><span class="sxs-lookup"><span data-stu-id="c683f-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="c683f-168">Die [Unicode-Standard](http://unicode.org/) enthält eine Liste der [code Diagramme](http://www.unicode.org/charts/index.html) können Sie das Diagramm, das mit Ihrer Zeichen gefunden.</span><span class="sxs-lookup"><span data-stu-id="c683f-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="c683f-169">Jeder Encoder, Html, JavaScript und die Url muss separat konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="c683f-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="c683f-170">Anpassung der Liste der sicheren wirkt sich nur Encodern, die mithilfe von DI stammen.</span><span class="sxs-lookup"><span data-stu-id="c683f-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="c683f-171">Wenn Sie direkt einen Encoder über zugreifen `System.Text.Encodings.Web.*Encoder.Default` klicken Sie dann den Standardwert lateinischen nur Listen sicherer Adressen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c683f-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="c683f-172">Wo soll Codierung stattfinden?</span><span class="sxs-lookup"><span data-stu-id="c683f-172">Where should encoding take place?</span></span>

<span data-ttu-id="c683f-173">Die allgemeinen akzeptiert, dass empfiehlt es sich, dass die Codierung wird zum Zeitpunkt der Ausgabe und die codierte Werte sollten nie in einer Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="c683f-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="c683f-174">Zum Zeitpunkt der Ausgabe-Codierung, können Sie die Verwendung von Daten, z. B. von HTML in einen Zeichenfolgenwert für die Abfrage zu ändern.</span><span class="sxs-lookup"><span data-stu-id="c683f-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="c683f-175">Außerdem können Sie Ihre Daten ganz einfach suchen, ohne Werte vor der Suche nach zu codieren und ermöglicht es Ihnen, Änderungen oder Fehlerbehebungen, die versucht, den Encoder zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="c683f-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="c683f-176">Überprüfung als eine XSS-Prevention-Methode</span><span class="sxs-lookup"><span data-stu-id="c683f-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="c683f-177">Überprüfung kann ein hilfreiches Tool bei XSS-Angriffen eingeschränkt sein.</span><span class="sxs-lookup"><span data-stu-id="c683f-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="c683f-178">Beispielsweise wird keine numerische Zeichenfolge, die nur die Zeichen 0-9 enthält einen XSS-Angriff auslösen.</span><span class="sxs-lookup"><span data-stu-id="c683f-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="c683f-179">Überprüfung wird komplizierter HTML Benutzereingaben - akzeptieren möchten die Analyse von HTML-Eingaben schwierig, wenn nicht sogar unmöglich ist.</span><span class="sxs-lookup"><span data-stu-id="c683f-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="c683f-180">MarkDown und andere Textformate wäre eine sicherere Option für umfangreiche Eingaben.</span><span class="sxs-lookup"><span data-stu-id="c683f-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="c683f-181">Sie sollten niemals zur Validierung von allein verlassen.</span><span class="sxs-lookup"><span data-stu-id="c683f-181">You should never rely on validation alone.</span></span> <span data-ttu-id="c683f-182">Nicht vertrauenswürdige Eingaben vor der Ausgabe immer zu codieren, unabhängig davon, was eine Überprüfung durchgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="c683f-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
