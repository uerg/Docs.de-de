---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Überprüfen der Benutzereingabe in der ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird erläutert, wie Informationen zu überprüfen, Sie von Benutzern erhalten &mdash; , also stellen Sie sicher, dass die Benutzer geben Sie gültige Informationen in HTML forms in einem Auftragsschritt...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 8f049adce33e452896b5e2a444635ff30d18e480
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021520"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="29ad7-103">Überprüfen der Benutzereingabe in ASP.NET Web Pages (Razor)-Websites</span><span class="sxs-lookup"><span data-stu-id="29ad7-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="29ad7-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="29ad7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="29ad7-105">In diesem Artikel wird erläutert, wie Informationen zu überprüfen, Sie von Benutzern erhalten &mdash; , also stellen Sie sicher, dass die Benutzer geben Sie gültige Informationen in HTML forms in einer ASP.NET Web Pages (Razor)-Website.</span><span class="sxs-lookup"><span data-stu-id="29ad7-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="29ad7-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="29ad7-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="29ad7-107">So prüfen Sie, dass eine Benutzereingaben entspricht Überprüfungskriterien, die Sie definieren.</span><span class="sxs-lookup"><span data-stu-id="29ad7-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="29ad7-108">Informationen zum bestimmen, ob alle Überprüfungstests bestanden haben.</span><span class="sxs-lookup"><span data-stu-id="29ad7-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="29ad7-109">Vorgehensweise beim Anzeigen von Validierungsfehlern (und wie sie formatiert).</span><span class="sxs-lookup"><span data-stu-id="29ad7-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="29ad7-110">So überprüfen Sie die Daten, die direkt von Benutzern keine.</span><span class="sxs-lookup"><span data-stu-id="29ad7-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="29ad7-111">Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Konzepten:</span><span class="sxs-lookup"><span data-stu-id="29ad7-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="29ad7-112">Die `Validation` Helper.</span><span class="sxs-lookup"><span data-stu-id="29ad7-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="29ad7-113">Die `Html.ValidationSummary` und `Html.ValidationMessage` Methoden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="29ad7-114">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="29ad7-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="29ad7-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="29ad7-116">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="29ad7-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="29ad7-117">Dieser Artikel enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="29ad7-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="29ad7-118">Übersicht über die Validierung von Benutzereingaben</span><span class="sxs-lookup"><span data-stu-id="29ad7-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="29ad7-119">Überprüfen der Benutzereingabe</span><span class="sxs-lookup"><span data-stu-id="29ad7-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="29ad7-120">Die clientseitige Validierung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="29ad7-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="29ad7-121">Formatieren von Validierungsfehlern</span><span class="sxs-lookup"><span data-stu-id="29ad7-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="29ad7-122">Überprüfen von Daten, die direkt von Benutzern keine</span><span class="sxs-lookup"><span data-stu-id="29ad7-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="29ad7-123">Übersicht über die Validierung von Benutzereingaben</span><span class="sxs-lookup"><span data-stu-id="29ad7-123">Overview of User Input Validation</span></span>

<span data-ttu-id="29ad7-124">Wenn Sie Benutzer zur Eingabe von Informationen auf einer Seite anfordern, z. B. in einem Formular – es ist wichtig, sicherzustellen, dass die Werte, die sie eingeben gültig sind.</span><span class="sxs-lookup"><span data-stu-id="29ad7-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="29ad7-125">Sie möchten z. B. keine Form zu verarbeiten, die wichtigen Informationen fehlen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="29ad7-126">Wenn der Benutzer Werte in einem HTML-Formular eingeben, sind die Werte, die sie eingeben Zeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="29ad7-127">In vielen Fällen sind die Werte, die Sie benötigen einige andere Datentypen, z. B. ganze Zahlen oder Datumsangaben.</span><span class="sxs-lookup"><span data-stu-id="29ad7-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="29ad7-128">Aus diesem Grund müssen Sie auch sicherstellen, dass die Werte, die Benutzer geben Sie ordnungsgemäß in die entsprechenden Datentypen konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="29ad7-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="29ad7-129">Sie können auch bestimmte Einschränkungen für die Werte verfügen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="29ad7-130">Selbst wenn Benutzer ordnungsgemäß eine ganze Zahl eingeben, können beispielsweise müssen Sie sicherstellen, dass der Wert innerhalb eines bestimmten Bereichs liegt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Validierungsfehler, die CSS-Formatklassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="29ad7-132">**Wichtige** Validieren von Benutzereingaben ist auch wichtig für die Sicherheit.</span><span class="sxs-lookup"><span data-stu-id="29ad7-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="29ad7-133">Wenn Sie die Werte, die Benutzer in Formularen eingeben können beschränken, verringern Sie die Wahrscheinlichkeit, dass ein Benutzer einen Wert eingeben kann, der die Sicherheit Ihrer Website gefährden können.</span><span class="sxs-lookup"><span data-stu-id="29ad7-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="29ad7-134">Überprüfen der Benutzereingabe</span><span class="sxs-lookup"><span data-stu-id="29ad7-134">Validating User Input</span></span>

<span data-ttu-id="29ad7-135">In ASP.NET Web Pages 2 können Sie die `Validator` Hilfsmethode zum Testen der Benutzereingabe.</span><span class="sxs-lookup"><span data-stu-id="29ad7-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="29ad7-136">Grundsätzlich wird die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="29ad7-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="29ad7-137">Bestimmen Sie, welche Elemente (Felder) Eingabe-, die Sie überprüfen möchten.</span><span class="sxs-lookup"><span data-stu-id="29ad7-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="29ad7-138">Überprüfen Sie in der Regel Werte, die in `<input>` Elemente in einem Formular.</span><span class="sxs-lookup"><span data-stu-id="29ad7-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="29ad7-139">Allerdings ist es empfiehlt sich, alle Eingaben überprüfen, geben Sie auch, das von einem eingeschränkten Element wie ist eine `<select>` Liste.</span><span class="sxs-lookup"><span data-stu-id="29ad7-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="29ad7-140">Dadurch wird sichergestellt, dass Benutzer nicht umgehen die Steuerelemente auf einer Seite und senden Sie ein Formular.</span><span class="sxs-lookup"><span data-stu-id="29ad7-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="29ad7-141">Fügen Sie einzelne Überprüfungen im Seitencode, für jedes Element mithilfe der Methoden der Eingabe der `Validation` Helper.</span><span class="sxs-lookup"><span data-stu-id="29ad7-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="29ad7-142">Verwenden Sie zum Überprüfen erforderlichen Felder `Validation.RequireField(field, [error message])` (für ein einzelnes Feld) oder `Validation.RequireFields(field1, field2, ...))` (für eine Liste von Feldern).</span><span class="sxs-lookup"><span data-stu-id="29ad7-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="29ad7-143">Verwenden Sie für andere Arten der Validierung, `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="29ad7-144">Für `ValidationType`, Sie können diese Optionen verwenden:</span><span class="sxs-lookup"><span data-stu-id="29ad7-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="29ad7-145">Wenn die Seite gesendet wird, überprüfen Sie, ob Überprüfung, indem Sie überprüfen bestanden hat `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="29ad7-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="29ad7-146">Treten Validierungsfehler auf, überspringen Sie die normale Verarbeitung.</span><span class="sxs-lookup"><span data-stu-id="29ad7-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="29ad7-147">Beispielsweise ist der Zweck der Seite zum Aktualisieren einer Datenbank, tun nicht Sie dies, bis alle Validierungsfehler behoben wurden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="29ad7-148">Wenn Validierungsfehler vorhanden sind, können Sie Fehlermeldungen in das Markup der Seite anzeigen, indem Sie mithilfe von `Html.ValidationSummary` oder `Html.ValidationMessage`, oder beides.</span><span class="sxs-lookup"><span data-stu-id="29ad7-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="29ad7-149">Das folgende Beispiel zeigt eine Seite, die diese Schritte veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="29ad7-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="29ad7-150">Um die Funktionsweise der Validierung angezeigt wird, führen Sie diese Seite, und machen Sie bewusst Fehler.</span><span class="sxs-lookup"><span data-stu-id="29ad7-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="29ad7-151">Beispielsweise sieht die Seite wie nicht vergessen, wenn Sie zur Eingabe eines Namens Kurs bei der Eingabe ein, und wenn Sie ein ungültiges Datum eingeben:</span><span class="sxs-lookup"><span data-stu-id="29ad7-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Überprüfungsfehler in der gerenderten Seite](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="29ad7-153">Die clientseitige Validierung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="29ad7-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="29ad7-154">Standardmäßig wird der Benutzereingabe überprüft, nachdem der Benutzer die Seite senden – d. h. die Validierung wird im Servercode durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="29ad7-155">Ein Nachteil dieses Ansatzes ist, dass Benutzer nicht wissen, dass sie einen Fehler erst nach dem sie die Seite senden vorgenommen wurden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="29ad7-156">Wenn ein Formular lang oder zu komplex ist, kann Fehler melden, erst nach der Übermittlung der Seite für den Benutzer unpraktisch.</span><span class="sxs-lookup"><span data-stu-id="29ad7-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="29ad7-157">Sie können den Support, um die Validierung im Clientskript hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="29ad7-158">In diesem Fall wird die Überprüfung ausgeführt, während die Benutzer im Browser arbeiten.</span><span class="sxs-lookup"><span data-stu-id="29ad7-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="29ad7-159">Nehmen wir beispielsweise an, dass Sie angeben, dass ein Wert eine ganze Zahl sein muss.</span><span class="sxs-lookup"><span data-stu-id="29ad7-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="29ad7-160">Wenn ein Benutzer einen nicht ganzzahligen Wert eingibt, wird der Fehler gemeldet, sobald der Benutzer das Feld lässt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="29ad7-161">Benutzer erhalten unmittelbar Feedback, das für sie zweckmäßig ist.</span><span class="sxs-lookup"><span data-stu-id="29ad7-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="29ad7-162">Client-basierter Validierung kann auch die Anzahl der reduzieren, denen der Benutzer zum Senden des Formulars, um mehrere Fehler zu beheben.</span><span class="sxs-lookup"><span data-stu-id="29ad7-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="29ad7-163">Auch wenn Sie die clientseitige Validierung verwenden, ist die Überprüfung immer auch im Servercode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="29ad7-164">Ausführen der Überprüfung im Server-Code ist eine Sicherheitsmaßnahme, für den Fall, dass Benutzer, Client-basierten Überprüfung umgehen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="29ad7-165">Registrieren Sie die folgenden JavaScript-Bibliotheken auf der Seite an:</span><span class="sxs-lookup"><span data-stu-id="29ad7-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="29ad7-166">Zwei Bibliotheken sind aus der ein Content Delivery Network (CDN) geladen werden kann, müssen Sie unbedingt nicht damit sie auf Ihrem Computer oder Server.</span><span class="sxs-lookup"><span data-stu-id="29ad7-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="29ad7-167">Allerdings benötigen Sie eine lokale Kopie des *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="29ad7-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="29ad7-168">Wenn Sie nicht bereits mit einer WebMatrix-Vorlage arbeiten (z. B. **Starter Site** ), die die Bibliothek enthält, erstellen Sie eine Web Pages-Website auf der Grundlage **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="29ad7-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="29ad7-169">Kopieren Sie dann die *js* Datei, die der aktuellen Website.</span><span class="sxs-lookup"><span data-stu-id="29ad7-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="29ad7-170">Fügen Sie im Markup für jedes Element, das Sie überprüfen können, die einen Aufruf von `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="29ad7-171">Diese Methode gibt die Attribute, die durch die clientseitige Validierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="29ad7-172">(Statt der tatsächlichen JavaScript-Code ausgegeben werden, gibt die Methode Attribute wie `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="29ad7-173">Diese Attribute unterstützen unaufdringlichen Validierung, die jQuery verwendet, um die Arbeit zu erledigen.)</span><span class="sxs-lookup"><span data-stu-id="29ad7-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="29ad7-174">Die folgende Seite zeigt wie das oben gezeigte Beispiel Validierungsfunktionen Client hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="29ad7-175">Nicht alle Überprüfungen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="29ad7-176">Insbesondere nicht die Datentyp-Überprüfung (ganze Zahl, Datum usw.) auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="29ad7-177">Die folgenden Prüfungen auf dem Client und Server arbeiten:</span><span class="sxs-lookup"><span data-stu-id="29ad7-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="29ad7-178">In diesem Beispiel funktioniert nicht der Test für ein gültiges Datum im Clientcode.</span><span class="sxs-lookup"><span data-stu-id="29ad7-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="29ad7-179">Der Test wird jedoch im Server-Code ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="29ad7-180">Formatieren von Validierungsfehlern</span><span class="sxs-lookup"><span data-stu-id="29ad7-180">Formatting Validation Errors</span></span>

<span data-ttu-id="29ad7-181">Sie können steuern, wie Validierungsfehler angezeigt werden, durch Definieren von CSS-Klassen, die die folgenden reservierten Namen aufweisen:</span><span class="sxs-lookup"><span data-stu-id="29ad7-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="29ad7-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-182">`field-validation-error`.</span></span> <span data-ttu-id="29ad7-183">Definiert die Ausgabe der `Html.ValidationMessage` Methode, wenn es einen Fehler angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="29ad7-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="29ad7-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-184">`field-validation-valid`.</span></span> <span data-ttu-id="29ad7-185">Definiert die Ausgabe der `Html.ValidationMessage` Methode, wenn kein Fehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="29ad7-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-186">`input-validation-error`.</span></span> <span data-ttu-id="29ad7-187">Definiert, wie `<input>` Elemente werden gerendert, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="29ad7-188">(Z. B. können diese Klasse, legen Sie die Farbe des Hintergrunds einer &lt;Eingabe&gt; Element in einer anderen Farbe, wenn der Wert ungültig ist.) Diese CSS-Klasse wird nur während der Validierung (in ASP.NET Web Pages 2) verwendet.</span><span class="sxs-lookup"><span data-stu-id="29ad7-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="29ad7-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-189">`input-validation-valid`.</span></span> <span data-ttu-id="29ad7-190">Definiert die Darstellung der `<input>` Elemente, wenn kein Fehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="29ad7-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-191">`validation-summary-errors`.</span></span> <span data-ttu-id="29ad7-192">Definiert die Ausgabe der `Html.ValidationSummary` Methode, die sie eine Liste der Fehler anzeigt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="29ad7-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-193">`validation-summary-valid`.</span></span> <span data-ttu-id="29ad7-194">Definiert die Ausgabe der `Html.ValidationSummary` Methode, wenn kein Fehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="29ad7-195">Die folgenden `<style>` Block Regeln für fehlerbedingungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="29ad7-196">Wenn Sie auf den Beispielseiten von weiter oben im Artikel dieses Stilblock einschließen, wird die Anzeige wie in der folgenden Abbildung aussehen:</span><span class="sxs-lookup"><span data-stu-id="29ad7-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Validierungsfehler, die CSS-Formatklassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="29ad7-198">Wenn Sie nicht die Clientvalidierung in ASP.NET Web Pages 2 verwenden, die CSS-Klassen für die `<input>` Elemente (`input-validation-error` und `input-validation-valid` haben keinerlei Auswirkung.</span><span class="sxs-lookup"><span data-stu-id="29ad7-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="29ad7-199">Statische und dynamische Fehleranzeige</span><span class="sxs-lookup"><span data-stu-id="29ad7-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="29ad7-200">Die CSS-Regeln paarweise, z. B. `validation-summary-errors` und `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="29ad7-201">Diese Paare können Sie die Regeln für beide Bedingungen definieren: eine fehlerbedingung und eine "normale" (nicht-Fehler)-Bedingung.</span><span class="sxs-lookup"><span data-stu-id="29ad7-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="29ad7-202">Es ist wichtig zu verstehen, dass immer das Markup für die Fehleranzeige gerendert wird, auch wenn keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="29ad7-203">Wenn eine Seite hat z. B. eine `Html.ValidationSummary` -Methode in das Markup, den Quellcode der Seite enthält das folgende Markup auch, wenn die Seite zum ersten Mal angefordert wird:</span><span class="sxs-lookup"><span data-stu-id="29ad7-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="29ad7-204">Das heißt, die `Html.ValidationSummary` Methode stellt immer eine `<div>` Element und eine Liste, auch wenn die Fehlerliste leer ist.</span><span class="sxs-lookup"><span data-stu-id="29ad7-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="29ad7-205">Auf ähnliche Weise die `Html.ValidationMessage` Methode stellt immer eine `<span>` Element als Platzhalter für ein einzelnes feldfehler, auch wenn kein Fehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="29ad7-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="29ad7-206">In einigen Situationen können dazu führen, dass die Seite, um dynamisch umgebrochen eine Fehlermeldung angezeigt und können dazu führen, dass Elemente auf der Seite verschieben.</span><span class="sxs-lookup"><span data-stu-id="29ad7-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="29ad7-207">Die CSS-Regeln, die auf Enden `-valid` können Sie ein Layout zu definieren, mit deren Hilfe kann dieses Problem zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="29ad7-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="29ad7-208">Sie können z. B. definieren `field-validation-error` und `field-validation-valid` sowohl haben die gleiche feste Größe.</span><span class="sxs-lookup"><span data-stu-id="29ad7-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="29ad7-209">Auf diese Weise der Anzeigebereich für das Feld ist statisch und die Seitenfluss wird nicht geändert werden, wenn eine Fehlermeldung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="29ad7-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="29ad7-210">Überprüfen von Daten, die direkt von Benutzern keine</span><span class="sxs-lookup"><span data-stu-id="29ad7-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="29ad7-211">Manchmal müssen Sie an, die direkt von einem HTML-Formular keine Informationen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="29ad7-212">Ein typisches Beispiel ist eine Seite, in dem ein Wert in einer Abfragezeichenfolge, wie im folgenden Beispiel übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="29ad7-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="29ad7-213">In diesem Fall soll sicherstellen, dass der Wert, der auf der Seite übergeben wird (hier 1022 für den Wert des `classid`) gültig ist.</span><span class="sxs-lookup"><span data-stu-id="29ad7-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="29ad7-214">Sie können nicht direkt verwenden die `Validation` Hilfsprogramm zum Ausführen dieser Validierung.</span><span class="sxs-lookup"><span data-stu-id="29ad7-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="29ad7-215">Allerdings können Sie andere Funktionen des Systems Validierung, z.B. die Möglichkeit, die Überprüfungsfehlermeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="29ad7-216">**Wichtige** immer überprüfen von Werten, die Sie von erhalten *alle* Quellcode, einschließlich Formularfeld Werte, Abfragezeichenfolgen-Werte und Cookiewerte.</span><span class="sxs-lookup"><span data-stu-id="29ad7-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="29ad7-217">Es ist einfach, Personen diese Werte (z. B. für böswillige Zwecke) zu ändern.</span><span class="sxs-lookup"><span data-stu-id="29ad7-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="29ad7-218">Daher müssen Sie diese Werte prüfen, um Ihre Anwendung zu schützen.</span><span class="sxs-lookup"><span data-stu-id="29ad7-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="29ad7-219">Das folgende Beispiel zeigt, wie Sie einen Wert überprüfen können, der in einer Abfragezeichenfolge übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="29ad7-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="29ad7-220">Der Code überprüft, dass der Wert nicht leer ist und es sich um eine ganze Zahl ist.</span><span class="sxs-lookup"><span data-stu-id="29ad7-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="29ad7-221">Beachten Sie, dass der Test ausgeführt wird, wenn die Anforderung keiner Formularübergabe (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="29ad7-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="29ad7-222">Dieser Test übergeben beim ersten, die die Seite angefordert wird, aber nicht bei die Anforderung ist eine formularübertragung.</span><span class="sxs-lookup"><span data-stu-id="29ad7-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="29ad7-223">Um diesen Fehler anzuzeigen, können Sie den Fehler zur Liste der Validierungsfehler hinzufügen, durch den Aufruf `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="29ad7-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="29ad7-224">Wenn die Seite mit einen Aufruf von enthält die `Html.ValidationSummary` -Methode, der Fehler wird angezeigt, genau wie einen Fehler für die Validierung von Benutzereingaben.</span><span class="sxs-lookup"><span data-stu-id="29ad7-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="29ad7-225">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="29ad7-225">Additional Resources</span></span>

[<span data-ttu-id="29ad7-226">Arbeiten mit HTML-Formularen in ASP.NET Web Pages-Websites</span><span class="sxs-lookup"><span data-stu-id="29ad7-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
