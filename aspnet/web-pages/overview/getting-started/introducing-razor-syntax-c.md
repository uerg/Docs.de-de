---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Kapitel bietet Ihnen einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax. ASP.NET ist Microsoft Technologie für die dynamische Pa ausgeführt...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b5eb98dfdf3fc013920f45080d4a20e1fa507725
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021780"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="e1ba5-104">Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (c#)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="e1ba5-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e1ba5-106">Dieser Artikel bietet Ihnen einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="e1ba5-107">ASP.NET ist die Technologie von Microsoft für die Ausführung von dynamischen Webseiten auf Webservern.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="e1ba5-108">Dieser Artikel konzentriert sich auf die mit der Programmiersprache c#.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="e1ba5-109">**Sie lernen Folgendes**:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="e1ba5-110">Das obere 8 Programmiertipps für die ersten Schritte mit ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="e1ba5-111">Grundlegende Programmierkonzepte kennen, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="e1ba5-112">Welche ASP.NET-Servercode: und die Razor-Syntax geht.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="e1ba5-113">Software-Versionen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="e1ba5-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="e1ba5-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="e1ba5-115">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="e1ba5-116">Die besten Tipps für 8 Programmierung</span><span class="sxs-lookup"><span data-stu-id="e1ba5-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="e1ba5-117">Dieser Abschnitt enthält einige Tipps, die Sie unbedingt benötigen wissen, wie Sie mithilfe der Razor-Syntax ASP.NET-Servercode: schreibe.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="e1ba5-118">Die Razor-Syntax basiert auf der C#-Programmiersprache, und das ist die Sprache, die am häufigsten mit ASP.NET Web Pages verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="e1ba5-119">Die Razor-Syntax unterstützt jedoch auch Visual Basic-Sprache, und alle Elemente, die Sie sehen, dass Sie auch in Visual Basic durchführen können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="e1ba5-120">Weitere Informationen finden Sie im Anhang [Visual Basic-Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="e1ba5-121">Weitere Informationen über die meisten dieser Techniken zur Programmierung finden Sie später in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="e1ba5-122">1. Sie fügen Code hinzu, um eine Seite mit dem @-Zeichen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="e1ba5-123">Die `@` Inlineausdrücke, einzelne Anweisungsblöcken und mit mehreren Anweisungen Blöcke beginnt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="e1ba5-124">Dies ist, wie diese Anweisungen aussehen, wenn die Seite in einem Browser ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="e1ba5-126">**HTML-Codierung**</span><span class="sxs-lookup"><span data-stu-id="e1ba5-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="e1ba5-127">Wenn Sie Inhalt anzeigen, auf eine Seite mit den `@` Zeichen, wie in den obigen Beispielen ASP.NET HTML-Codierung der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="e1ba5-128">Dies ersetzt die reservierte HTML-Zeichen (z. B. `<` und `>` und `&`) mit Codes, mit denen die Zeichen als Zeichen statt als HTML-Tags oder Entitäten interpretiert wird auf einer Webseite angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="e1ba5-129">Ohne HTML-Codierung, die aus Ihrem Code möglicherweise nicht richtig angezeigt, und möglicherweise eine Seite, um Sicherheitsrisiken ausgesetzt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="e1ba5-130">Wenn Ihr Ziel ist, um HTML-Markup auszugeben, die Tags als Markup rendert (z. B. `<p></p>` für einen Absatz oder `<em></em>` Text hervorgehoben), finden Sie im Abschnitt [Kombinieren von Text, Markup und Code in Codeblöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="e1ba5-131">Erfahren Sie mehr über die HTML-Codierung [arbeiten mit Formularen](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="e1ba5-132">2. Schließen Sie Codeblöcke in geschweifte Klammern</span><span class="sxs-lookup"><span data-stu-id="e1ba5-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="e1ba5-133">Ein *Codeblock* enthält eine oder mehrere codeanweisungen und in geschweifte Klammern eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="e1ba5-134">Das Ergebnis in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="e1ba5-136">3. In einem Block am Ende jeder Anweisung mit einem Semikolon</span><span class="sxs-lookup"><span data-stu-id="e1ba5-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="e1ba5-137">In einem Codeblock muss jede vollständige Code-Anweisung mit einem Semikolon enden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="e1ba5-138">Inline-Ausdrücke enden nicht mit einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="e1ba5-139">4. Sie verwenden Variablen zum Speichern von Werten</span><span class="sxs-lookup"><span data-stu-id="e1ba5-139">4. You use variables to store values</span></span>

<span data-ttu-id="e1ba5-140">Sie können Werte in speichern eine *Variable*, einschließlich Zeichenfolgen, Zahlen und Datumsangaben. Sie erstellen eine neue Variable mit dem `var` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="e1ba5-141">Sie können Werte direkt in eine Seite mit einfügen `@`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="e1ba5-142">Das Ergebnis in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="e1ba5-144">5. Sie einschließen literale Zeichenfolgenwerte in Anführungszeichen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="e1ba5-145">Ein *Zeichenfolge* ist eine Folge von Zeichen, die als Text behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="e1ba5-146">Um eine Zeichenfolge angeben, schließen Sie ihn in doppelte Anführungszeichen ein:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="e1ba5-147">Wenn die Zeichenfolge, die Sie anzeigen möchten, einen umgekehrten Schrägstrich enthält ( `\` ) oder doppelte Anführungszeichen ( `"` ), verwenden Sie eine *ausführliches Zeichenfolgenliteral* , enthält das Präfix der `@` Operator.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="e1ba5-148">(In C# geschrieben, die \ Zeichen eine besonderen Bedeutung hat, es sei denn, Sie ein ausführliches Zeichenfolgenliteral verwenden.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="e1ba5-149">Klicken Sie zum Einbetten von doppelten Anführungszeichen verwenden ein ausführliches Zeichenfolgenliteral, und wiederholen Sie die Anführungszeichen:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="e1ba5-150">So sieht das Ergebnis der Verwendung von diesen beiden Beispielen auf einer Seite aus:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="e1ba5-152">Beachten Sie, dass die `@` Zeichen wird verwendet, markieren Sie wörtliche Zeichenfolgenliterale in C# geschrieben und Markieren von Code in ASP.NET-Seiten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="e1ba5-153">6. Code wird die Groß-/Kleinschreibung beachten</span><span class="sxs-lookup"><span data-stu-id="e1ba5-153">6. Code is case sensitive</span></span>

<span data-ttu-id="e1ba5-154">C#-Schlüsselwörter (z. B. `var`, `true`, und `if`) und Namen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="e1ba5-155">Die folgenden Codezeilen erstellt zwei unterschiedliche Variablen `lastName` und `LastName.`</span><span class="sxs-lookup"><span data-stu-id="e1ba5-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="e1ba5-156">Wenn Sie eine Variable deklarieren `var lastName = "Smith";` und wenn Sie versuchen, diese Variable auf der Seite als verweisen `@LastName`, ein Fehler ausgegeben, da `LastName` wird nicht erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="e1ba5-157">In Visual Basic-Schlüsselwörter und Variablen werden *nicht* Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="e1ba5-158">7. Die Codierung ein Großteil beinhaltet Objekte</span><span class="sxs-lookup"><span data-stu-id="e1ba5-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="e1ba5-159">Ein *Objekt* stellt eine Sache, die Sie programmieren können, mit &#8212; eine Seite, ein Textfeld, eine Datei, ein Bild, eine webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz (Datenbankzeile) usw. Objekte verfügen über Eigenschaften, die ihre Merkmale zu beschreiben, und Sie lesen oder ändern können &#8212; ein Textfeld-Objekt verfügt über eine `Text` -Eigenschaft (unter anderem) eine Request-Objekt verfügt über eine `Url` -Eigenschaft, die eine e-Mail-Nachricht verfügt über eine `From` -Eigenschaft und ein Kundenobjekt verfügt über eine `FirstName` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="e1ba5-160">Objekte verfügen außerdem über Methoden, die die &quot;Verben&quot; ausführen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="e1ba5-161">Beispiele hierfür sind ein Dateiobjekt `Save` -Methode, ein Bildobjekt `Rotate` -Methode und eine e-Mail-Adresse des Objekts `Send` Methode.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="e1ba5-162">Sie arbeiten häufig mit der `Request` Objekt, wo Sie Informationen wie die Werte von Textfeldern (Felder) auf der Seite Art der Browser die Anforderung, die URL der Seite, die Identität des Benutzers usw. gesendet. Das folgende Beispiel zeigt, wie Sie den Zugriff auf Eigenschaften von den `Request` -Objekt und das Aufrufen von der `MapPath` -Methode der der `Request` -Objekt, das Sie den absoluten Pfad der Seite auf dem Server erhalten:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="e1ba5-163">Das Ergebnis in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="e1ba5-165">8. Sie können Code schreiben, die Entscheidungen trifft</span><span class="sxs-lookup"><span data-stu-id="e1ba5-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="e1ba5-166">Ein wichtiges Feature von dynamischen Webseiten ist, dass auf Sie bestimmen können, was zu tun ist basierend auf Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="e1ba5-167">Die gängigste Methode dazu ist die `if` Anweisung (und optional `else` Anweisung).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="e1ba5-168">Die Anweisung `if(IsPost)` ist eine schnelle Möglichkeit der Verfassung `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="e1ba5-169">Zusammen mit `if` Anweisungen, es gibt eine Vielzahl von Möglichkeiten zum Testen von Bedingungen, wiederholen Sie Codeblöcke und usw., die sind weiter unten in diesem Artikel beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="e1ba5-170">Das Ergebnis in einem Browser angezeigt (nach dem Klicken auf **senden**):</span><span class="sxs-lookup"><span data-stu-id="e1ba5-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="e1ba5-172">HTTP GET und POST-Methoden und die IsPost-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e1ba5-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="e1ba5-173">Das Protokoll für Webseiten (HTTP) unterstützt eine sehr begrenzte Anzahl von Methoden (Verben), die verwendet werden, um Anforderungen an den Server zu senden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="e1ba5-174">Die zwei häufigsten Gründe sind GET, die zum Lesen einer Seite verwendet wird, und POST, das verwendet wird, um eine Seite zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="e1ba5-175">Zum ersten Mal, das ein Benutzer eine Seite anfordert, wird im Allgemeinen unter Verwendung von GET eine Seite angefordert.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="e1ba5-176">Wenn der Benutzer in einem Formular ausgefüllt und klickt dann auf eine Schaltfläche "Senden", sendet der Browser eine POST-Anforderung an dem Server an.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="e1ba5-177">In Web-Programmierung ist es oft hilfreich zu wissen, ob eine Seite als GET oder POST angefordert wird, damit Sie wissen, wie auf die Seite zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="e1ba5-178">In ASP.NET Web Pages, können Sie die `IsPost` Eigenschaft, um festzustellen, ob eine Anforderung eine Get- oder POST ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="e1ba5-179">Wenn die Anforderung eine POST-ist der `IsPost` Eigenschaft "true" zurück, und Sie können Aktionen wie lesen die Werte von Textfeldern in einem Formular.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="e1ba5-180">Viele Beispiele, die Sie sehen, in dem Sie zeigen, wie zum Verarbeiten der Seite hängt der Wert des `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="e1ba5-181">Ein einfaches Codebeispiel</span><span class="sxs-lookup"><span data-stu-id="e1ba5-181">A Simple Code Example</span></span>

<span data-ttu-id="e1ba5-182">Dieses Verfahren zeigt, wie Sie eine Seite erstellen, die die grundlegenden Programmiertechniken veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="e1ba5-183">Im Beispiel erstellen Sie eine Seite, mit dem Benutzer, die zwei Zahlen eingeben, dann fügt sie hinzu, und das Ergebnis wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="e1ba5-184">Klicken Sie in Ihrem Editor, erstellen Sie eine neue Datei, und nennen sie *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="e1ba5-185">Kopieren Sie den folgenden Code und Markup auf der Seite, und Ersetzen Sie dabei alle Elemente bereits in der Seite an.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="e1ba5-186">Es gibt einige Dinge beachten Sie:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="e1ba5-187">Die `@` Zeichen beginnt des erste Codeblocks auf der Seite, und ihm vorausgeht der `totalMessage` Variable, die am unteren Rand der Seite eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="e1ba5-188">Der Block am oberen Rand der Seite wird in Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="e1ba5-189">Am Anfang des Blocks enden alle Zeilen mit einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="e1ba5-190">Die Variablen `total`, `num1`, `num2`, und `totalMessage` mehrere Zahlen und einer Zeichenfolge zu speichern.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="e1ba5-191">Der Zeichenfolgenliteral-Wert, der zugewiesen der `totalMessage` Variable ist in doppelte Anführungszeichen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="e1ba5-192">Da der Code beim Groß-/Kleinschreibung beachtet, ist die `totalMessage` Variable, die am unteren Rand der Seite verwendet wird, muss der Name der Variablen am Anfang genau entsprechen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="e1ba5-193">Der Ausdruck `num1.AsInt() + num2.AsInt()` für die Arbeit mit Objekten und Methoden veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="e1ba5-194">Die `AsInt` Methode für jede Variable konvertiert die Zeichenfolge, die von einem Benutzer in eine Zahl (Integer) eingegeben wird, sodass Sie arithmetische Operationen darauf ausführen können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="e1ba5-195">Die `<form>` Tag enthält eine `method="post"` Attribut.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="e1ba5-196">Dies gibt an, dass wenn der Benutzer klickt **hinzufügen**, die Seite an den Server mithilfe der HTTP-POST-Methode gesendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="e1ba5-197">Wenn die Seite gesendet wird, die `if(IsPost)` Test True ergibt, der bedingte code ausgeführt wird, und das Ergebnis der Addition der Zahlen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="e1ba5-198">Speichern Sie die Seite, und führen Sie sie in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="e1ba5-199">(Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Geben Sie zwei ganzen Zahlen, und klicken Sie dann auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="e1ba5-201">Grundlegende Programmierkonzepte</span><span class="sxs-lookup"><span data-stu-id="e1ba5-201">Basic Programming Concepts</span></span>

<span data-ttu-id="e1ba5-202">Dieser Artikel bietet eine Übersicht über ASP.NET Web-Programmierung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="e1ba5-203">Es ist nicht für eine vollständige Prüfung, nur eine kurze Führung zusammengestellt über die Programmierkonzepte kennen, die Sie am häufigsten verwenden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="e1ba5-204">Dennoch werden fast alles, was benötigen Sie für den Einstieg mit ASP.NET Web Pages behandelt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="e1ba5-205">Aber zuerst, eine kleine technischen Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="e1ba5-206">Die Razor-Syntax, Servercode und ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1ba5-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="e1ba5-207">Razor-Syntax ist eine einfache Programmiersyntax zum Einbetten von serverbasiertem Code in einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="e1ba5-208">Auf einer Webseite, die die Razor-Syntax verwendet, es gibt zwei Arten von Inhalten: Clientcode Inhalte und Server.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="e1ba5-209">Clientinhalt ist die Dinge, die Sie, um auf Webseiten verwendet werden: HTML-Markup (Elemente), Informationen zum Schriftschnitt wie CSS, vielleicht einige Clientskripts wie JavaScript und nur-Text.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="e1ba5-210">Razor-Syntax können Sie diese Clients Inhalte Servercode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="e1ba5-211">Liegt Servercode auf der Seite, führt der Server diesen Code zuerst, bevor die Seite an den Browser gesendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="e1ba5-212">Auf dem Server ausführen, kann der Code Aufgaben ausführen, die mit Clientinhalt alleine, wie Sie den Zugriff auf serverbasierte Datenbanken sehr viel komplizierter sein können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="e1ba5-213">Der wichtigste Servercode kann Clientinhalt dynamisch erstellen &#8212; können sie HTML-Markup oder andere Inhalte nebenbei generieren und senden Sie sie an den Browser sowie die statische HTML-Elemente, die die Seite enthalten können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="e1ba5-214">Aus Sicht des Browsers unterscheidet sich Clients Inhalte, die von Ihrem Code generiert wird nicht als jeder andere Client-Inhalte.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="e1ba5-215">Wie Sie bereits gesehen haben, befindet sich der Code, der erforderlich ist recht einfach.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="e1ba5-216">ASP.NET-Webseiten, die die Razor-Syntax enthalten haben eine besondere Dateinamenerweiterung (*.cshtml* oder *vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="e1ba5-217">Der Server erkennt diese Erweiterungen, führt den Code, der mit Razor-Syntax markiert ist, und klicken Sie dann die Seite an den Browser sendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="e1ba5-218">Wo ist ASP.NET angebracht?</span><span class="sxs-lookup"><span data-stu-id="e1ba5-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="e1ba5-219">Razor-Syntax basiert auf einer Technologie von Microsoft, die Namen ASP.NET, die wiederum auf Microsoft .NET Framework basiert.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="e1ba5-220">Der.NET Framework ist ein großes, umfassende Programmierframework von Microsoft für die Entwicklung von praktisch jeder Art von Computer-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="e1ba5-221">ASP.NET ist der Teil von .NET Framework, die speziell zum Erstellen von Webanwendungen konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="e1ba5-222">Entwickler haben die ASP.NET verwendet, um viele der größten und höchsten Datenverkehr Websites in der ganzen Welt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="e1ba5-223">(Jedes Mal, die die Dateinamenerweiterung daraufhin *aspx* als Teil der URL an einem Standort, wissen Sie, dass die Website mit ASP.NET geschrieben wurde.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="e1ba5-224">Die Razor-Syntax bietet Ihnen die Leistungsfähigkeit der ASP.NET, aber verwenden eine vereinfachte Syntax, der leichter erfahren Sie, ob Sie Anfänger sind und Sie produktiver stellt, wenn Sie ein Experte sind.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="e1ba5-225">Obwohl diese Syntax einfach zu verwenden ist, bedeutet die Familie Beziehung zu ASP.NET und .NET Framework, wie Ihre Websites komplexer werden, Sie die Leistungsfähigkeit der größeren Frameworks zur Verfügung haben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="e1ba5-227">**Klassen und Instanzen**</span><span class="sxs-lookup"><span data-stu-id="e1ba5-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="e1ba5-228">ASP.NET-Servercode: verwendet Objekte, die wiederum auf der Idee von Klassen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="e1ba5-229">Die Klasse ist die Definition oder eine Vorlage für ein Objekt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="e1ba5-230">Beispielsweise kann eine Anwendung enthalten eine `Customer` Klasse, die definiert, die Eigenschaften und Methoden, die alle Customer-Objekt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="e1ba5-231">Wenn die Anwendung mit tatsächlichen Kundeninformationen arbeiten muss, erstellt er eine Instanz von (oder *instanziiert*) eine Customer-Objekt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="e1ba5-232">Jeder einzelne Kunde wird eine separate Instanz von der `Customer` Klasse.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="e1ba5-233">Jede Instanz unterstützt die gleichen Eigenschaften und Methoden, aber die Eigenschaftswerte für die einzelnen Instanzen unterscheiden sich in der Regel, da jedes Kundenobjekt eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="e1ba5-234">In einer Customer-Objekt das `LastName` Eigenschaft möglicherweise "Smith"; in einer anderen Customer-Objekt, das `LastName` Eigenschaft möglicherweise "Jones" ab.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="e1ba5-235">Jeder einzelne Webseite auf Ihrer Website auf ähnliche Weise wird eine `Page` -Objekt, das eine Instanz von der `Page` Klasse.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="e1ba5-236">Eine Schaltfläche auf der Seite ist eine `Button` -Objekt, das eine Instanz von der `Button` -Klasse, und So weiter.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="e1ba5-237">Jede Instanz verfügt über eigene Eigenschaften, aber alle basieren auf den in der Definition der Klasse des Objekts angegebenen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="e1ba5-238">Allgemeine Syntax</span><span class="sxs-lookup"><span data-stu-id="e1ba5-238">Basic Syntax</span></span>

<span data-ttu-id="e1ba5-239">Zuvor haben Sie ein einfaches Beispiel zum Erstellen einer ASP.NET Web Pages-Seite, und wie Sie HTML-Markup Servercode hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="e1ba5-240">Hier erfahren Sie die Grundlagen zum Schreiben von ASP.NET-Code-Server mithilfe der Razor-Syntax &#8212; , also die programming Language-Regeln.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="e1ba5-241">Wenn Sie bereits über Erfahrung mit Programmierung (insbesondere, wenn Sie C#, C++, c#, Visual Basic oder JavaScript verwendet haben), werden viele der hier Sie lesen vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="e1ba5-242">Sie müssen wahrscheinlich machen Sie sich nur mit Markup wie Server-Code hinzugefügt wird *.cshtml* Dateien.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="e1ba5-243">Kombinieren von Text, Markup und Code in Codeblöcken</span><span class="sxs-lookup"><span data-stu-id="e1ba5-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="e1ba5-244">In Server-Codeblöcken möchten Sie häufig Ausgabe Text oder Markup (oder beides) auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="e1ba5-245">Wenn ein Server-Codeblock Text enthält, die nicht Code und, stattdessen gerendert werden soll, wie, muss ASP.NET Text von Code zu unterscheiden können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="e1ba5-246">Dafür stehen verschiedene Möglichkeiten zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-246">There are several ways to do this.</span></span>

- <span data-ttu-id="e1ba5-247">Schließen Sie den Text in einem HTML-Element, z. B. `<p></p>` oder `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="e1ba5-248">Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="e1ba5-249">Wenn erkennt ASP.NET das öffnende HTML-Tag (z. B. `<p>`), sondert rendert alle Elemente einschließlich des-Elements und dessen Inhalt als wird an den Browser, und Beheben von Servercode Ausdrücke, wie es geht.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="e1ba5-250">Verwenden der `@:` Operator oder die `<text>` Element.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="e1ba5-251">Die `@:` Ausgaben eine einzelne Zeile des Inhalts, der nur-Text oder HTML-Tags für nicht übereinstimmenden; enthält die `<text>` Element einschließt, mehrere Zeilen ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="e1ba5-252">Diese Optionen sind nützlich, wenn Sie nicht, ein HTML-Element als Teil der Ausgabe gerendert möchten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="e1ba5-253">Wenn Sie mehrere Zeilen Text oder HTML-Tags, die nicht übereinstimmenden ausgeben möchten, können Sie jede Zeile mit dem voranstellen `@:`, oder Sie können einschließen, die Zeile in einer `<text>` Element.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="e1ba5-254">Wie die `@:` Operator`<text>` Tags werden von ASP.NET zum Identifizieren von Textinhalt verwendet und nie in der Seitenausgabe gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="e1ba5-255">Im erste Beispiel wird das vorherige Beispiel wiederholt, aber verwendet ein einzelnes Paar von `<text>` Tags aus, um den Text Rendern einschließen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="e1ba5-256">Im zweiten Beispiel die `<text>` und `</text>` Tags einschließen drei Zeilen, die jeweils einige nicht enthaltene Text und HTML-Tags, die nicht übereinstimmenden sind (`<br />`), sowie der Servercode und HTML-Tags, die übereinstimmende.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="e1ba5-257">In diesem Fall können Sie auch jede Zeile einzeln mit voranstellen der `@:` Operator; beide Weise funktionieren.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e1ba5-258">Wenn Sie Text ausgeben, wie in diesem Abschnitt gezeigt &#8212; mithilfe eines HTML-Elements, die `@:` -Operator, oder die `<text>` Element &#8212; ASP.NET die Ausgabe nicht HTML-Codierung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="e1ba5-259">(Wie bereits erwähnt, ASP.NET codiert die Ausgabe von Codeausdrücken Server und Server-Codeblöcke, die vor `@`, außer in die Sonderfälle, in diesem Abschnitt erwähnt wurden.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="e1ba5-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="e1ba5-260">Whitespace</span></span>

<span data-ttu-id="e1ba5-261">Zusätzliche Leerzeichen in einer Anweisung (und außerhalb eines Zeichenfolgenliterals) wirken sich nicht auf die Anweisung aus:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="e1ba5-262">Ein Zeilenumbruch in einer Anweisung hat keine Auswirkungen auf die Anweisung aus, und können Sie Anweisungen zur besseren Lesbarkeit umbrochen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="e1ba5-263">Die folgenden Anweisungen sind identisch:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="e1ba5-264">Sie können keine jedoch eine Zeile in der Mitte eines Zeichenfolgenliterals umschließen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="e1ba5-265">Das folgende Beispiel funktioniert nicht:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="e1ba5-266">Es gibt zwei Optionen, um eine lange Zeichenfolge zu kombinieren, die in mehrere Zeilen wie im obigen Code umschließt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="e1ba5-267">Sie können den Operator für Verkettungen verwenden (`+`), die Sie später in diesem Artikel sehen werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="e1ba5-268">Sie können auch die `@` Zeichen, das eine wörtliche Zeichenfolge literal erstellt, wie Sie weiter oben in diesem Artikel gesehen haben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="e1ba5-269">Wörtliche Zeichenfolgenliterale können Sie über mehrere Zeilen unterteilen:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="e1ba5-270">Code (und Markup) Kommentare</span><span class="sxs-lookup"><span data-stu-id="e1ba5-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="e1ba5-271">Kommentare können Sie Notizen für sich selbst oder andere zu verlassen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="e1ba5-272">Sie ermöglichen auch deaktivieren (*auskommentieren*) einen Abschnitt des Codes oder Markups, die Sie nicht ausführen möchten, jedoch auf Ihrer Seite vorerst beibehalten möchten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="e1ba5-273">Es gibt verschiedene kommentieren die Syntax für Razor-Code und HTML-Markup.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="e1ba5-274">Wie bei allen Razor-Code werden Razor-Kommentare verarbeitet (und anschließend entfernt) auf dem Server, bevor die Seite an den Browser gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="e1ba5-275">Aus diesem Grund kann die Kommentierung Razor-Syntax Sie Kommentare im Code (oder sogar in das Markup) bereitstellen, die Sie sehen können, wenn Sie die Datei bearbeiten, aber Benutzern nicht angezeigt wird, auch in den Quellcode der Seite.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="e1ba5-276">Für ASP.NET Razor-Kommentare, starten Sie den Kommentar mit `@*` und enden mit `*@`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="e1ba5-277">Der Kommentar kann auf eine oder mehrere Zeilen sein:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="e1ba5-278">Hier ist ein Kommentar in einem Codeblock:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="e1ba5-279">Hier ist der gleiche Codeblock, mit der Zeile des Codes auskommentiert, damit er nicht ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="e1ba5-280">In einem Codeblock können Sie als Alternative zur Verwendung von Razor-Kommentar-Syntax, die Kommentierung Syntax der Programmiersprache verwenden, die Sie, die wie z. B. c# verwenden:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="e1ba5-281">In c# einzeilige Kommentare vorangestellt sind, wird die `//` Zeichen und mehrzeilige Kommentare beginnen mit `/*` und enden mit `*/`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="e1ba5-282">(Wie bei der Razor-Kommentare werden C#-Kommentare nicht an den Browser gerendert.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="e1ba5-283">Für Markup wie Sie wahrscheinlich wissen, können Sie einen HTML-Kommentar erstellen:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="e1ba5-284">HTML-Kommentare beginnen mit `<!--` Zeichen und enden mit `-->`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="e1ba5-285">Sie können HTML-Kommentare verwenden, um umgeben, nicht nur Text, sondern auch für jedes HTML-Markup, die Sie auf der Seite beibehalten möchten, aber nicht gerendert werden soll.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="e1ba5-286">Diese HTML-Kommentar wird den gesamten Inhalt des Tags und den darin enthaltenen Text ausblenden:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="e1ba5-287">Im Gegensatz zu Razor-Kommentare, HTML-Kommentaren *sind* auf der Seite gerendert und der Benutzer kann den Quellcode der Seite anzeigen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="e1ba5-288">Razor kann Einschränkungen auf geschachtelte Blöcke von c#.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="e1ba5-289">Weitere Informationen finden Sie unter [mit dem Namen C#-Variablen und geschachtelte Blöcke unterteilt Code generieren](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="e1ba5-290">Variablen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-290">Variables</span></span>

<span data-ttu-id="e1ba5-291">Eine Variable ist ein benanntes Objekt, das Sie verwenden, um Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="e1ba5-292">Sie können Variablen Namen, aber der Name muss mit einem alphabetischen Zeichen beginnen, und es darf keine Leerzeichen oder reservierte Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="e1ba5-293">Variablen und Datentypen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-293">Variables and Data Types</span></span>

<span data-ttu-id="e1ba5-294">Eine Variable haben einen bestimmten Datentyp, der angibt, welche Art von Daten in der Variablen gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="e1ba5-295">Können Sie Zeichenfolgenvariablen, in denen Zeichenfolgenwerte gespeichert haben (z. B. &quot;Hallo Welt&quot;), Ganzzahl-Variablen, die ganzzahlige Werte (z. B. 3 oder 79) speichern, und die Datenvariablen, die Datumswerte in einer Vielzahl von Formaten (z. B. 4/12/2012 oder März 2009 speichern ).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="e1ba5-296">Und es gibt viele andere Datentypen, die Sie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="e1ba5-297">Allerdings müssen Sie in der Regel einen Typ für eine Variable angeben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="e1ba5-298">In den meisten Fällen, kann ASP.NET ermitteln den Typ basierend auf wie die Daten in der Variablen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="e1ba5-299">(Gelegentlich müssen Sie einen Typ angeben, sehen Sie Beispiele, in denen dies "true".)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="e1ba5-300">Sie deklarieren eine Variable mit dem `var` Schlüsselwort (sofern Sie nicht möchten, dass für einen Typ angegeben wird) oder mit dem Namen des Typs:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="e1ba5-301">Das folgende Beispiel zeigt einige typischen Verwendungen von Variablen in einer Webseite ein:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="e1ba5-302">Wenn Sie die vorherigen Beispielen auf einer Seite kombinieren, sehen Sie dies in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="e1ba5-304">Konvertieren und Testen von Datentypen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="e1ba5-305">Obwohl ASP.NET einen Datentyp in der Regel automatisch ermitteln kann, manchmal kann es nicht.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="e1ba5-306">Aus diesem Grund müssen Sie ASP.NET weiterhelfen, indem Sie eine explizite Konvertierung ausführen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="e1ba5-307">Auch wenn Sie keine Typen zu konvertieren, ist es manchmal hilfreich, testen, welche Art von Daten Sie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="e1ba5-308">Der häufigste Fall ist, dass Sie zum Konvertieren von einer Zeichenfolge in einen anderen Typ, z. B. auf eine ganze Zahl oder Datum.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="e1ba5-309">Das folgende Beispiel zeigt einem typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="e1ba5-310">Als Faustregel gilt wird der Benutzereingabe für Sie als Zeichenfolgen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="e1ba5-311">Auch wenn Sie haben die Benutzer eine Zahl eingeben aufgefordert, und auch wenn sie bei der Eingabe des Benutzers gesendet wird, und Sie finden es im Code eine Ziffer eingegeben haben, die Daten im Zeichenfolgenformat werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="e1ba5-312">Aus diesem Grund müssen Sie die Zeichenfolge in eine Zahl konvertieren.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="e1ba5-313">Im Beispiel wenn Sie versuchen, die Durchführung von Berechnungen auf den Werten, ohne zu konvertieren, führt der folgende Fehler, da zwei Zeichenfolgen von ASP.NET hinzugefügt werden können:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="e1ba5-314">*Typ "String" in "Int" kann nicht implizit konvertiert werden.*</span><span class="sxs-lookup"><span data-stu-id="e1ba5-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="e1ba5-315">Um die Werte in ganzen Zahlen zu konvertieren, rufen Sie die `AsInt` Methode.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="e1ba5-316">Wenn die Konvertierung erfolgreich ist, können Sie dann die Zahlen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="e1ba5-317">Die folgende Tabelle enthält einige allgemeine Konvertierung und Test-Methoden für die Variablen an.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="e1ba5-318"><strong>Methode</strong></span><span class="sxs-lookup"><span data-stu-id="e1ba5-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-319"><strong>Beschreibung</strong></span><span class="sxs-lookup"><span data-stu-id="e1ba5-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-320"><strong>Beispiel</strong></span><span class="sxs-lookup"><span data-stu-id="e1ba5-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-321">Konvertiert eine Zeichenfolge, die eine ganze Zahl (z. B. "593") in eine ganze Zahl darstellt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-322">Konvertiert eine Zeichenfolge wie &quot;"true"&quot; oder &quot;"false"&quot; auf einen booleschen Typ.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-323">Konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Gleitkommazahl.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-324">Konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Dezimalzahl.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="e1ba5-325">(In ASP.NET ist eine Dezimalzahl genauer als eine Gleitkommazahl.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-326">Konvertiert eine Zeichenfolge, die einen Wert für Datum und Uhrzeit der ASP.NET darstellt `DateTime` Typ.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-327">Konvertiert einen anderen Datentyp in eine Zeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="e1ba5-328">Operatoren</span><span class="sxs-lookup"><span data-stu-id="e1ba5-328">Operators</span></span>

<span data-ttu-id="e1ba5-329">Ein Operator ist ein Schlüsselwort oder das Zeichen, das ASP.NET darüber informiert, welche Art von Befehl aus, um in einem Ausdruck durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="e1ba5-330">Der C#-Sprache (und die Razor-Syntax, die darauf basieren) unterstützt viele Operatoren müssen Sie nur ein Paar für den Einstieg zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="e1ba5-331">In der folgende Tabelle werden die am häufigsten verwendeten Operatoren zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-331">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
    <span data-ttu-id="e1ba5-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="e1ba5-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-333"><strong>Beschreibung</strong></span><span class="sxs-lookup"><span data-stu-id="e1ba5-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-334"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="e1ba5-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-335">Mathematische Operatoren, die in numerischen Ausdrücken verwendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-335">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-336">Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-336">Assignment.</span></span> <span data-ttu-id="e1ba5-337">Weist den Wert auf der rechten Seite einer Anweisung, um das Objekt auf der linken Seite.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-337">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-338">Gleichheit.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-338">Equality.</span></span> <span data-ttu-id="e1ba5-339">Gibt `true` , wenn die Werte gleich sind.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-339">Returns `true` if the values are equal.</span></span> <span data-ttu-id="e1ba5-340">(Beachten Sie, dass den Unterschied zwischen der `=` Operator und die `==` Operator.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-340">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-341">Ungleichheit.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-341">Inequality.</span></span> <span data-ttu-id="e1ba5-342">Gibt `true` , wenn die Werte nicht gleich sind.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-342">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-343">Kleiner-als, größer-als, kleiner-als-oder-gleich und größer-als-oder-gleich.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-343">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-344">Verkettung, die zum Verketten von Zeichenfolgen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-344">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="e1ba5-345">ASP.NET ist den Unterschied zwischen diesen Operator und der Additionsoperator, basierend auf den Datentyp des Ausdrucks bekannt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-345">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-346">Die Inkrement- und Dekrement-Operatoren, die Addition und Subtraktion 1 (bzw.) aus einer Variablen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-346">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-347">Punkt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-347">Dot.</span></span> <span data-ttu-id="e1ba5-348">Verwendet, um Objekte und deren Eigenschaften und Methoden zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-348">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-349">Klammern.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-349">Parentheses.</span></span> <span data-ttu-id="e1ba5-350">Verwendet, um Ausdrücke zu gruppieren und zum Übergeben von Parametern zu Methoden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-350">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-351">Eckige Klammern.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-351">Brackets.</span></span> <span data-ttu-id="e1ba5-352">Für den Zugriff auf Werte in Arrays oder Auflistungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-352">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-353">Nicht.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-353">Not.</span></span> <span data-ttu-id="e1ba5-354">Kehrt eine `true` Wert `false` und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-354">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="e1ba5-355">In der Regel als eine schnelle Möglichkeit zum Testen verwendet `false` (d. h. für nicht `true`).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-355">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    <span data-ttu-id="e1ba5-356">Logisches AND und zusammen Bedingungen, die verwendet werden, um zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-356">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="e1ba5-357">Arbeiten mit Datei- und Ordnerpfade in Code</span><span class="sxs-lookup"><span data-stu-id="e1ba5-357">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="e1ba5-358">Sie werden häufig mit Datei- und Ordnerpfade in Ihrem Code arbeiten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-358">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="e1ba5-359">Hier ist ein Beispiel der physischen Ordnerstruktur für eine Website auf, wie es auf Ihrem Entwicklungscomputer angezeigt werden kann:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-359">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="e1ba5-360">Hier sind einige wichtigen Informationen über URLs und Pfade:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-360">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="e1ba5-361">Eine URL beginnt entweder mit einen Domänennamen (`http://www.example.com`) oder einen Servernamen (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-361">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="e1ba5-362">Eine URL entspricht einem physischen Pfad auf einem Hostcomputer.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-362">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="e1ba5-363">Z. B. `http://myserver` möglicherweise zu dem Ordner entsprechen *C:\websites\mywebsite* auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-363">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="e1ba5-364">Ein virtueller Pfad ist die Abkürzung, um Pfade im Code darzustellen, ohne den vollständigen Pfad angeben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-364">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="e1ba5-365">Es umfasst den Teil einer URL, die der Domäne oder Server Name folgt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-365">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="e1ba5-366">Wenn Sie die virtuellen Pfade verwenden, können Sie Ihren Code zu einer anderen Domäne oder Server verschieben, ohne die Pfade aktualisieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-366">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="e1ba5-367">Hier ist ein Beispiel hilft Ihnen die Unterschiede zu verstehen:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-367">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="e1ba5-368">Vollständige URL</span><span class="sxs-lookup"><span data-stu-id="e1ba5-368">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="e1ba5-369">Servername</span><span class="sxs-lookup"><span data-stu-id="e1ba5-369">Server name</span></span> | <span data-ttu-id="e1ba5-370">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="e1ba5-370">*mycompanyserver*</span></span> |
| <span data-ttu-id="e1ba5-371">Virtueller Pfad</span><span class="sxs-lookup"><span data-stu-id="e1ba5-371">Virtual path</span></span> | <span data-ttu-id="e1ba5-372">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="e1ba5-372">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="e1ba5-373">Physischer Pfad</span><span class="sxs-lookup"><span data-stu-id="e1ba5-373">Physical path</span></span> | <span data-ttu-id="e1ba5-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="e1ba5-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="e1ba5-375">Das virtuelle Stammverzeichnis ist /, genau wie der Stamm von Laufwerk C: Laufwerk \.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-375">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="e1ba5-376">(Virtuelle Ordnerpfade verwenden immer Schrägstriche.) Der virtuelle Pfad eines Ordners muss nicht den gleichen Namen wie der physische Ordner haben; Es kann ein Alias sein.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-376">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="e1ba5-377">(Auf Produktionsservern entspricht der virtuelle Pfad nur selten einen genaue physischen Pfad.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-377">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="e1ba5-378">Bei der Arbeit mit Dateien und Ordner im Code, müssen Sie gelegentlich verweisen auf den physischen Pfad und manchmal auf einen virtuellen Pfad, je nachdem welche Objekte, die mit dem Sie arbeiten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-378">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="e1ba5-379">ASP.NET bietet Ihnen diese Tools zum Arbeiten mit Datei- und Ordnerpfade im Code: die `Server.MapPath` -Methode, und die `~` Operator und `Href` Methode.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-379">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="e1ba5-380">Konvertieren von virtuellen und physischen Pfade: die Server.MapPath-Methode</span><span class="sxs-lookup"><span data-stu-id="e1ba5-380">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="e1ba5-381">Die `Server.MapPath` Methode konvertiert einen virtuellen Pfad (z. B. */default.cshtml*), ein absoluter physischer Pfad (z. B. *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-381">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="e1ba5-382">Sie verwenden diese Methode können Sie jederzeit einen vollständigen physischen Pfad.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-382">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="e1ba5-383">Ein typisches Beispiel ist beim Lesen oder Schreiben einer Text- oder Image-Datei auf dem Webserver.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-383">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="e1ba5-384">Sie können die absoluten physischen Pfad der Website auf einer hosting-Site-Server in der Regel nicht kennen, damit diese Methode konvertieren des Pfads können Sie wissen – der virtuelle Pfad, in den entsprechenden Pfad auf dem Server für Sie.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-384">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="e1ba5-385">Sie den virtuellen Pfad zu einer Datei oder Ordner für die Methode übergeben, und es gibt den physischen Pfad:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-385">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="e1ba5-386">Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und Href-Methode</span><span class="sxs-lookup"><span data-stu-id="e1ba5-386">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="e1ba5-387">In einer *.cshtml* oder *vbhtml* -Datei können Sie verweisen, den virtuellen Stamm-Pfad mit der `~` Operator.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-387">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="e1ba5-388">Dies ist sehr praktisch, da Sie Seiten, an einem Standort navigieren können, und alle Links zu anderen Seiten enthaltenen nicht unterbrochen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-388">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="e1ba5-389">Es ist auch praktisch, für den Fall, dass Sie Ihre Website immer an einem anderen Speicherort verschieben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-389">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="e1ba5-390">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-390">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="e1ba5-391">Wenn die Website ist `http://myserver/myapp`, sieht wie ASP.NET diese Pfade behandelt wird, wenn die Seite ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-391">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="e1ba5-392">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="e1ba5-392">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="e1ba5-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="e1ba5-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="e1ba5-394">(Diese Pfade nicht tatsächlich als die Werte der Variablen angezeigt, aber ASP.NET die Pfade behandelt, als ob einstellungsänderungen ist.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-394">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="e1ba5-395">Sie können die `~` Operator sowohl im Server-Code (wie oben erläutert) und im Markup wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-395">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="e1ba5-396">Im Markup, das Sie verwenden die `~` Operator, um Pfade zu den Ressourcen, wie Bilddateien, andere Webseiten und CSS-Dateien zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-396">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="e1ba5-397">Wenn die Seite ausgeführt wird, durchsucht Sie die Seite (Code und Markup) und behebt alle ASP.NET die `~` Verweise auf den entsprechenden Pfad.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-397">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="e1ba5-398">Bedingungen und Schleifen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-398">Conditional Logic and Loops</span></span>

<span data-ttu-id="e1ba5-399">ASP.NET Server-Code können Sie die Aufgaben, die basierend auf Bedingungen, und Schreiben von Code, der Anweisungen wird wiederholt, eine bestimmte Anzahl von Malen (d. h. Code, der eine Schleife ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-399">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="e1ba5-400">Testen von Bedingungen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-400">Testing Conditions</span></span>

<span data-ttu-id="e1ba5-401">Um eine einfache Bedingung zu testen, Sie verwenden, die `if` -Anweisung, die einen Test gibt "true" oder "false" Grundlage Sie angeben:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-401">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="e1ba5-402">Die `if` Schlüsselwort beginnt einen Block.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-402">The `if` keyword starts a block.</span></span> <span data-ttu-id="e1ba5-403">Der eigentliche Test (Bedingung) wird in Klammern angegeben und gibt "true" oder "false" zurück.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-403">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="e1ba5-404">Die Anweisungen, die ausgeführt werden, wenn der Test "true" ist, werden in geschweifte Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-404">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="e1ba5-405">Ein `if` -Anweisung kann enthalten eine `else` Block, der angibt, Anweisungen, die ausgeführt werden, wenn die Bedingung "false" ist:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-405">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="e1ba5-406">Sie können mehrere Bedingungen mit Hinzufügen einer `else if` blockieren:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-406">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="e1ba5-407">In diesem Beispiel, wenn die erste in der If-Bedingung Block ist nicht "true", die `else if` Bedingung aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-407">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="e1ba5-408">Wenn diese Bedingung erfüllt ist, die Anweisungen in der `else if` Block ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-408">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="e1ba5-409">Wenn keine der Bedingungen erfüllt sind, die Anweisungen in der `else` Block ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-409">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="e1ba5-410">Sie können eine beliebige Anzahl von ElseIf hinzufügen blockiert, und schließen Sie dann mit einer `else` als blockiert die &quot;alles&quot; Bedingung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-410">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="e1ba5-411">Um eine große Anzahl von Bedingungen zu testen, verwenden Sie eine `switch` blockieren:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-411">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="e1ba5-412">Der zu testende Wert wird in Klammern angegeben (im Beispiel die `weekday` Variable).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-412">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="e1ba5-413">Jeder einzelne Test verwendet einen `case` -Anweisung, die mit einem Doppelpunkt (:).) endet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-413">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="e1ba5-414">Wenn der Wert des einem `case` Anweisung entspricht dem Testwert, der Code in diesen Fall Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-414">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="e1ba5-415">Schließen Sie jede Case-Anweisung mit einem `break` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-415">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="e1ba5-416">(Wenn Sie vergessen, jede Unterbrechung einschließt `case` blockieren, den Code aus der nächsten `case` Anweisung auch ausgeführt werden.) Ein `switch` Block verfügt oft über eine `default` -Anweisung als im letzten Fall für eine &quot;alles&quot; Option aus, die ausgeführt wird, wenn keine anderen Fällen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-416">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="e1ba5-417">Das Ergebnis der letzten beiden bedingte Blöcke in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-417">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="e1ba5-419">Schleifen-Code</span><span class="sxs-lookup"><span data-stu-id="e1ba5-419">Looping Code</span></span>

<span data-ttu-id="e1ba5-420">Sie müssen häufig die gleichen Anweisungen mehrmals ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-420">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="e1ba5-421">Dazu müssen Sie Schleifen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-421">You do this by looping.</span></span> <span data-ttu-id="e1ba5-422">Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Auflistung von Daten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-422">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="e1ba5-423">Wenn Sie wissen, wie oft Sie möchten eine Schleife, können Sie eine `for` Schleife.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-423">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="e1ba5-424">Diese Art von Schleife eignet sich besonders für zugewiesen, oder nach unten zählen:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-424">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="e1ba5-425">Die Schleife beginnt mit der `for` Schlüsselwort, gefolgt von drei Anweisungen in Klammern ein, jeweils mit einem Semikolon abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-425">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="e1ba5-426">Innerhalb der Klammern, die erste Anweisung (`var i=10;`) einen Leistungsindikator erstellt und initialisiert es mit 10.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-426">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="e1ba5-427">Sie müssen keine benennen Sie den Zähler `i` &#8212; können Sie eine beliebige Variable.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-427">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="e1ba5-428">Wenn die `for` Schleife ausgeführt wird, wird der Indikator wird automatisch aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-428">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="e1ba5-429">Die zweite Anweisung (`i < 21;`) legt die Bedingung für die wie weit Sie zählen möchten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-429">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="e1ba5-430">In diesem Fall möchten Sie ihn an, auf ein Maximum von 20 (d. h. weiternutzen während der Zähler auf weniger als 21 ist).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-430">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="e1ba5-431">Die dritte Anweisung (`i++` ) Inkrement-Operators, der einfach gibt an, dass der Zähler sollte 1 hinzugefügt wird bei jeder Ausführung die Schleife verwendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-431">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="e1ba5-432">Innerhalb der geschweiften Klammern ist Sie der Code, der für jede Iteration der Schleife ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-432">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="e1ba5-433">Das Markup erstellt einen neuen Absatz (`<p>` Element) jedes Mal, und fügt eine Zeile in die Ausgabe, sodass der Wert `i` (den Zähler).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-433">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="e1ba5-434">Wenn Sie diese Seite ausführen, wird im Beispiel 11 Zeilen anzeigen der Ausgabe, mit dem Text in jeder Zeile an, der angibt, der Elementnummer erstellt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-434">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="e1ba5-436">Wenn Sie mit einer Auflistung oder ein Array arbeiten, verwenden Sie häufig eine `foreach` Schleife.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-436">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="e1ba5-437">Eine Auflistung ist eine Gruppe ähnlicher Objekte und die `foreach` Schleife können Sie eine Aufgabe für jedes Element in der Auflistung durchführen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-437">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="e1ba5-438">Dieser Art von Schleife eignet sich für Auflistungen, da im Gegensatz zu einem `for` Schleife, Sie müssen keine Erhöhen des Zählerwerts oder ein Limit festlegen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-438">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="e1ba5-439">Stattdessen die `foreach` Schleifen-Code wird einfach in der Auflistung fortgesetzt, bis er abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-439">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="e1ba5-440">Der folgende Code gibt beispielsweise die Elemente in der `Request.ServerVariables` Auflistung, die ein Objekt, das Informationen über den Webserver enthält.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-440">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="e1ba5-441">Er verwendet eine `foreac` h-Schleife, um den Namen der einzelnen Elemente anzuzeigen, durch Erstellen eines neuen `<li>` Element in einer HTML-Aufzählung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-441">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="e1ba5-442">Die `foreach` Schlüsselwort ist, gefolgt von Klammern deklarieren, in dem Sie eine Variable, die ein einzelnes Element in der Auflistung darstellt (beispielsweise `var item`), gefolgt von der `in` Schlüsselwort, gefolgt von der Auflistung durchlaufen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-442">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="e1ba5-443">Im Hauptteil der `foreach` Schleife können Sie das aktuelle Element, das mit der Variablen, die Sie zuvor deklarierten zugreifen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-443">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="e1ba5-445">Verwenden Sie zum Erstellen einer allgemeinen Schleife die `while` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-445">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="e1ba5-446">Ein `while` Schleife beginnt mit der `while` Schlüsselwort, gefolgt von Klammern für die Sie angeben, wie lange die Schleife fortgesetzt wird (hier, um so lange `countNum` beträgt weniger als 50), klicken Sie dann den Block wiederholt werden soll.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-446">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="e1ba5-447">Schleifen in der Regel erhöhen (hinzugefügt) oder zu verringern (subtrahiert) eine Variable oder ein Objekt, das für die Inventur verwendet.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-447">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="e1ba5-448">Im Beispiel die `+=` Operator addiert 1 zu `countNum` bei jeder Ausführung die Schleife.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-448">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="e1ba5-449">(Um eine Variable in einer Schleife zu verringern, die wird nach unten gezählt, verwenden Sie des Dekrementoperators `-=`).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-449">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="e1ba5-450">Objekte und Auflistungen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-450">Objects and Collections</span></span>

<span data-ttu-id="e1ba5-451">Fast alles in einer ASP.NET-Website ist ein Objekt, einschließlich der Webseite selbst.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-451">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="e1ba5-452">Dieser Abschnitt beschreibt einige wichtige Objekte, die Sie häufig in Ihrem Code verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-452">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="e1ba5-453">Page-Objekte</span><span class="sxs-lookup"><span data-stu-id="e1ba5-453">Page Objects</span></span>

<span data-ttu-id="e1ba5-454">Die meisten grundlegenden Objekts in ASP.NET ist die Seite.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-454">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="e1ba5-455">Sie können die Eigenschaften der Page-Objekt, ohne alle berechtigten Objekte direkt zugreifen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-455">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="e1ba5-456">Der folgende Code Ruft den Dateipfad der Seite mit den `Request` Objekt der Seite:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-456">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="e1ba5-457">Um es zu machen klar, dass Sie auf Eigenschaften und Methoden für das Seitenobjekt der aktuellen verweisen, optional können Sie das Schlüsselwort `this` das Page-Objekt in Ihrem Code darstellen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-457">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="e1ba5-458">Hier ist das vorherige Codebeispiel, bei dem `this` zur Darstellung der Seite hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-458">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="e1ba5-459">Können Sie Eigenschaften der `Page` Objekt um eine Vielzahl von Informationen, wie z. B. zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-459">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="e1ba5-460">`Request`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-460">`Request`.</span></span> <span data-ttu-id="e1ba5-461">Wie Sie bereits gesehen haben, ist dies eine Auflistung von Informationen über die aktuelle Anforderung, einschließlich welche Art von Browser die Anforderung, die URL der Seite, die Identität des Benutzers usw. vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-461">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="e1ba5-462">`Response`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-462">`Response`.</span></span> <span data-ttu-id="e1ba5-463">Dies ist eine Auflistung von Informationen über die Antwort (Seite), die an den Browser gesendet wird, wenn der Code ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-463">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="e1ba5-464">Beispielsweise können Sie diese Eigenschaft, um Informationen in die Antwort zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-464">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="e1ba5-465">Von Auflistungsobjekten (Arrays und Dictionarys)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-465">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="e1ba5-466">Ein *Auflistung* ist eine Gruppe von Objekten desselben Typs, z. B. eine Auflistung von `Customer` Objekte aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-466">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="e1ba5-467">ASP.NET enthält viele integrierte Auflistungen, z.B. die `Request.Files` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-467">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="e1ba5-468">Sie werden häufig in Sammlungen mit Daten arbeiten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-468">You'll often work with data in collections.</span></span> <span data-ttu-id="e1ba5-469">Zwei allgemeine Auflistungstypen sind die *Array* und *Wörterbuch*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-469">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="e1ba5-470">Ein Array ist nützlich, wenn Sie eine Sammlung mit ähnlichen Elementen gespeichert werden soll, aber nicht, erstellen Sie eine separate Variable für jedes Element möchten:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-470">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="e1ba5-471">Mit Arrays, Sie deklarieren einen bestimmten Datentyp, z. B. `string`, `int`, oder `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-471">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="e1ba5-472">Um anzugeben, dass die Variable darf ein Array, das Sie Klammern zur Deklaration hinzufügen (z. B. `string[]` oder `int[]`).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-472">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="e1ba5-473">Es stehen die Elemente in einem Array mit ihrer Position (Index) oder mithilfe der `foreach` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-473">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="e1ba5-474">Arrayindizes sind nullbasiert &#8212; , also das erste Element ist an position 0, das zweite Element ist an Position 1 und So weiter.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-474">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="e1ba5-475">Sie können die Anzahl der Elemente in einem Array zu bestimmen, durch Abrufen der `Length` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-475">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="e1ba5-476">Rufen Sie die Position eines bestimmten Elements im Array (und das Array zu suchen) mit der `Array.IndexOf` Methode.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-476">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="e1ba5-477">Sie können auch Aktionen wie Reverse den Inhalt eines Arrays (die `Array.Reverse` Methode) oder den Inhalt zu sortieren (die `Array.Sort` Methode).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-477">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="e1ba5-478">Die Ausgabe von der Zeichenfolgen-Array-Code in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-478">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="e1ba5-480">Ein Wörterbuch ist eine Auflistung von Schlüssel/Wert-Paare, in dem Sie den Schlüssel (oder Name) festlegen oder Abrufen des entsprechenden Werts angeben:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-480">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="e1ba5-481">Um ein Wörterbuch zu erstellen, verwenden Sie die `new` Schlüsselwort, um anzugeben, dass Sie ein neues Wörterbuchobjekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-481">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="e1ba5-482">Sie können ein Wörterbuch zuweisen, um eine Variable mit dem `var` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-482">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="e1ba5-483">Sie zeigen die Daten der Elemente in das Wörterbuch, das mithilfe der spitzen Klammern ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-483">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="e1ba5-484">Am Ende der Deklaration müssen Sie ein Paar von Klammern, hinzufügen, da es sich eigentlich eine Methode handelt, die ein neues Wörterbuch erstellt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-484">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="e1ba5-485">Um Elemente zum Wörterbuch hinzuzufügen, rufen Sie die `Add` Methode der "Dictionary"-Variable (`myScores` in diesem Fall), und geben Sie einen Schlüssel und Wert.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-485">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="e1ba5-486">Alternativ können Sie eckige Klammern verwenden, um den Schlüssel angeben, und führen eine einfache Zuweisung, wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-486">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="e1ba5-487">Um einen Wert aus dem Wörterbuch abzurufen, geben Sie den Schlüssel in Klammern ein:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-487">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="e1ba5-488">Aufrufen von Methoden mit Parametern</span><span class="sxs-lookup"><span data-stu-id="e1ba5-488">Calling Methods with Parameters</span></span>

<span data-ttu-id="e1ba5-489">Wenn Sie zuvor in diesem Artikel lesen, können die Objekte, denen Sie Programmieren mit Methoden verfügen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-489">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="e1ba5-490">Z. B. eine `Database` Objekt möglicherweise eine `Database.Connect` Methode.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-490">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="e1ba5-491">Viele Methoden verfügen auch über einen oder mehrere Parameter.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-491">Many methods also have one or more parameters.</span></span> <span data-ttu-id="e1ba5-492">Ein *Parameter* ist ein Wert, der Sie an eine Methode übergeben, um die Methode zum Abschließen des Tasks zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-492">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="e1ba5-493">Betrachten Sie z. B. eine Deklaration für die `Request.MapPath` -Methode, die drei Parameter akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-493">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="e1ba5-494">(Die Zeile wurde umgebrochen, um sie besser lesbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-494">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="e1ba5-495">Beachten Sie, dass Sie Zeilenumbrüche einfügen können, fast jeden Ort, mit der Ausnahme, die innerhalb von Zeichenfolgen, in Anführungszeichen eingeschlossen sind.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-495">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="e1ba5-496">Diese Methode gibt den physischen Pfad auf dem Server, der entspricht einem angegebenen virtuellen Pfad zurück.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-496">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="e1ba5-497">Die drei Parameter für die Methode sind `virtualPath`, `baseVirtualDir`, und `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-497">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="e1ba5-498">(Beachten Sie, dass in der Deklaration, die Parameter mit den Datentypen der Daten aufgeführt sind, die eingegeben werden.) Wenn Sie diese Methode aufrufen, müssen Sie Werte für alle drei Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-498">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="e1ba5-499">Die Razor-Syntax bietet Ihnen zwei Optionen zum Übergeben von Parametern an eine Methode: *Positionsparameter* und *benannte Parameter*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-499">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="e1ba5-500">Um eine Methode mithilfe von positionelle Parameter aufzurufen, übergeben Sie die Parameter in einem strikter Reihenfolge, der in der Deklaration der Methode angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-500">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="e1ba5-501">(Sie würden diese Reihenfolge in der Regel kennen, lesen Sie die Dokumentation für die Methode.) Führen Sie die Reihenfolge, und Sie können einen der Parameter nicht überspringen &#8212; erforderlich, eine leere Zeichenfolge übergeben (`""`) oder `null` für einen positionelle Parameter, die einen Wert für nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-501">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="e1ba5-502">Im folgende Beispiel wird angenommen, Sie haben einen Ordner namens *Skripts* auf Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-502">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="e1ba5-503">Der Code Ruft die `Request.MapPath` -Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-503">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="e1ba5-504">Dann wird den resultierenden zugeordneten Pfad angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-504">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="e1ba5-505">Wenn eine Methode über viele Parameter verfügt, können Sie Ihren Code besser lesbar behalten mit benannten Parametern.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-505">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="e1ba5-506">Um eine Methode, die mit benannten Parametern aufzurufen, geben Sie an, gefolgt von der Parameternamen von einem Doppelpunkt (:), und klicken Sie dann den Wert.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-506">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="e1ba5-507">Der Vorteil für benannte Parameter ist, dass Sie sie in beliebiger Reihenfolge übergeben können, werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-507">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="e1ba5-508">(Ein Nachteil ist, dass der Methodenaufruf nicht so kompakt ist).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-508">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="e1ba5-509">Im folgenden Beispiel wird die gleiche Methode wie oben beschrieben, aber verwendet benannte Parameter, die Werte anzugeben:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-509">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="e1ba5-510">Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-510">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="e1ba5-511">Wenn Sie im vorherigen Beispiel und in diesem Beispiel ausführen, werden sie jedoch den gleichen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-511">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="e1ba5-512">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="e1ba5-512">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="e1ba5-513">Try-Catch-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-513">Try-Catch Statements</span></span>

<span data-ttu-id="e1ba5-514">Sie müssen oft Anweisungen in Ihrem Code, die außerhalb Ihrer Kontrolle Gründen fehlschlagen kann.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-514">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="e1ba5-515">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-515">For example:</span></span>

- <span data-ttu-id="e1ba5-516">Wenn Ihr Code versucht, erstellen oder auf eine Datei zugreifen, können alle möglichen Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-516">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="e1ba5-517">Die gewünschte Datei nicht vorhanden sind, wird sie möglicherweise gesperrt, der Code möglicherweise nicht über Berechtigungen verfügen, und so weiter.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-517">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="e1ba5-518">Auf ähnliche Weise Wenn Ihr Code versucht, die Datensätze in einer Datenbank zu aktualisieren, können Berechtigungsprobleme vorhanden sein, kann die Verbindung mit der Datenbank gelöscht werden, die zu speichernden Daten möglicherweise ungültig und so weiter.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-518">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="e1ba5-519">Programmiertechnisch ausgedrückt, werden diese Situationen aufgerufen *Ausnahmen*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-519">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="e1ba5-520">Wenn Ihr Code eine Ausnahme auftritt, generiert er (auslöst) eine Fehlermeldung angezeigt, die es im besten Fall ziemlich ärgerlich, für Benutzer:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-520">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="e1ba5-522">In Situationen, in denen Ihr Code kann Ausnahmen auftreten, und um Fehler Nachrichten dieses Typs zu vermeiden, können Sie `try/catch` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-522">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="e1ba5-523">In der `try` -Anweisung, führen Sie den Code, die Sie überprüfen können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-523">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="e1ba5-524">In einer oder mehreren `catch` -Anweisungen, sehen Sie nach bestimmten Fehlern (bestimmte Arten von Ausnahmen), die aufgetreten sind.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-524">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="e1ba5-525">Sie können beliebig viele einschließen `catch` Anweisungen, wie Sie nach Fehlern zu suchen, die Sie planen müssen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-525">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="e1ba5-526">Es wird empfohlen, dass Sie, verwenden vermeiden die `Response.Redirect` -Methode in der `try/catch` -Anweisungen, da dies eine Ausnahme auf Ihrer Seite führen kann.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-526">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="e1ba5-527">Das folgende Beispiel zeigt eine Seite, die auf der ersten Anforderung eine Textdatei erstellt und zeigt dann eine Schaltfläche der Benutzer die Datei zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-527">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="e1ba5-528">Im Beispiel verwendet absichtlich einen ungültigen Dateinamen ein, damit es eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-528">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="e1ba5-529">Der Code enthält `catch` -Anweisungen für die zwei möglichen Ausnahmen: `FileNotFoundException`, Dies tritt ein, wenn der Dateiname ungültig, ist und `DirectoryNotFoundException`, Dies tritt ein, wenn ASP.NET nicht den Ordner selbst finden können.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-529">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="e1ba5-530">(Sie können die auskommentierung Aufheben einer Anweisung im Beispiel um zu sehen, wie sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-530">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="e1ba5-531">Wenn Ihr Code die Ausnahme behandelt hat nicht, sehen Sie eine Fehlerseite angezeigt, wie im vorherigen Screenshot.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-531">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="e1ba5-532">Allerdings die `try/catch` Abschnitt hilft zu verhindern, dass den Benutzer diese Arten von Fehlern angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-532">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="e1ba5-533">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e1ba5-533">Additional Resources</span></span>

<span data-ttu-id="e1ba5-534">**Programmierung mit Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="e1ba5-534">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="e1ba5-535">Anhang: Visual Basic-Sprache und Syntax</span><span class="sxs-lookup"><span data-stu-id="e1ba5-535">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="e1ba5-536">**Referenzdokumentation**</span><span class="sxs-lookup"><span data-stu-id="e1ba5-536">**Reference Documentation**</span></span>


[<span data-ttu-id="e1ba5-537">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e1ba5-537">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="e1ba5-538">C#-Sprache</span><span class="sxs-lookup"><span data-stu-id="e1ba5-538">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
