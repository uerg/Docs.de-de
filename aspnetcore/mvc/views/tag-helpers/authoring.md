---
title: Tag-Hilfsprogramme in ASP.NET Core erstellen
author: rick-anderson
description: "Erfahren Sie mehr über das Tag-Hilfsprogramme in ASP.NET Core erstellen."
keywords: ASP.NET Core, Tag-Hilfsprogramme
ms.author: riande
manager: wpickett
ms.date: 6/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f16af1184a29b891a9aab0b38ab833836c326c44
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="d24cc-104">Erstellen von Tag-Hilfsprogramme in ASP.NET Core, eine exemplarische Vorgehensweise mit Beispielen</span><span class="sxs-lookup"><span data-stu-id="d24cc-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="d24cc-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d24cc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="d24cc-106">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="d24cc-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="d24cc-107">Erste Schritte mit dem Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="d24cc-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="d24cc-108">Dieses Lernprogramm enthält eine Einführung in die Programmierung Tag-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="d24cc-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="d24cc-109">[Einführung in die Tag-Hilfsprogrammen](intro.md) beschreibt die Vorteile, die Tag-Hilfsprogramme bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="d24cc-110">Ein Tag-Hilfsprogramm ist jede Klasse, implementiert die `ITagHelper` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="d24cc-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="d24cc-111">Jedoch wenn Sie eine Tag Hilfsprogramm erstellen, in der Regel von abzuleiten, `TagHelper`, tun dies der Fall ist ermöglicht Ihnen Zugriff auf die `Process` Methode.</span><span class="sxs-lookup"><span data-stu-id="d24cc-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="d24cc-112">Erstellen Sie ein neues ASP.NET Core-Projekt namens **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="d24cc-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="d24cc-113">Sie brauchen nicht Authentifizierung für dieses Projekt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="d24cc-114">Erstellen Sie einen Ordner zum Speichern von Tag Hilfsprogramme aufgerufen *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="d24cc-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="d24cc-115">Die *TagHelpers* Ordner *nicht* erforderlich, aber es ist eine angemessene Konvention.</span><span class="sxs-lookup"><span data-stu-id="d24cc-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="d24cc-116">Nun beginnen wir zunächst einige einfache Tag Hilfsmethoden schreiben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="d24cc-117">Ein minimaler Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="d24cc-117">A minimal Tag Helper</span></span>

<span data-ttu-id="d24cc-118">In diesem Abschnitt schreiben Sie eine Tag-Hilfsprogramm, die eine e-Mail-Tag aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="d24cc-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="d24cc-119">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d24cc-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="d24cc-120">Der Server wird unserer e-Mail-Tag-Hilfsprogramm verwenden, konvertieren Sie das Markup in der folgenden:</span><span class="sxs-lookup"><span data-stu-id="d24cc-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="d24cc-121">D. h., dass dadurch ein Ankertag, die einen e-Mail-Link.</span><span class="sxs-lookup"><span data-stu-id="d24cc-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="d24cc-122">Möglicherweise möchten vorgehen, wenn Sie eine Blog-Engine schreiben und Sie ihn benötigen, um das Senden von e-Mails für Marketing, Support und andere Kontaktpersonen alle an der gleichen Domäne.</span><span class="sxs-lookup"><span data-stu-id="d24cc-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="d24cc-123">Fügen Sie die folgenden `EmailTagHelper` Klasse, um die *TagHelpers* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d24cc-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="d24cc-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span></span>
    
    <span data-ttu-id="d24cc-125">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="d24cc-125">**Notes:**</span></span>
    
    * <span data-ttu-id="d24cc-126">Tag-Hilfen verwenden eine Benennungskonvention, die auf Elemente des Klassennamens Stamm (abzüglich der *taghelpers* Teil des Namens der Klasse).</span><span class="sxs-lookup"><span data-stu-id="d24cc-126">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="d24cc-127">In diesem Beispiel den Namen des **E-Mail**taghelpers ist *e-Mail*, sodass die `<email>` Tag wird als Ziel haben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-127">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="d24cc-128">Diese Namenskonvention sollte für die meisten Tag Hilfsprogramme funktionieren, später werde ich zeigen, wie zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-128">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="d24cc-129">Die `EmailTagHelper`-Klasse wird von `TagHelper` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="d24cc-129">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="d24cc-130">Die `TagHelper` Klasse stellt Methoden und Eigenschaften für das Schreiben von Hilfsprogrammen Tag.</span><span class="sxs-lookup"><span data-stu-id="d24cc-130">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="d24cc-131">Die überschriebene `Process` Methode steuert Wirkungsweise das Tag-Hilfsobjekt, bei der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="d24cc-131">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="d24cc-132">Die `TagHelper` Klasse bietet auch eine asynchrone Version (`ProcessAsync`) mit denselben Parametern.</span><span class="sxs-lookup"><span data-stu-id="d24cc-132">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="d24cc-133">Der Kontextparameter zu `Process` (und `ProcessAsync`) enthält Informationen, die die Ausführung des aktuellen HTML-Tags zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d24cc-133">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="d24cc-134">Die Output-Parameter `Process` (und `ProcessAsync`) enthält eine zustandsbehaftete HTML-Element, die repräsentativ für die ursprüngliche Quelle, die zum Generieren einer HTML-Tags und Inhalt verwendet.</span><span class="sxs-lookup"><span data-stu-id="d24cc-134">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="d24cc-135">Unsere Klassenname wurde als Suffix **taghelpers**, also *nicht* erforderlich, aber es wurde eine best Practice-Konvention angesehen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-135">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="d24cc-136">Sie können die Klasse als deklarieren:</span><span class="sxs-lookup"><span data-stu-id="d24cc-136">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="d24cc-137">Vornehmen der `EmailTagHelper` Klasse für alle Razor-Ansichten verfügbar, fügen Sie der `addTagHelper` -Direktive die *Views/_ViewImports.cshtml* Datei: [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-137">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="d24cc-138">Der obige Code verwendet die Platzhaltersyntax, um anzugeben, dass die Tag-Hilfsprogramme in unsere Assembly zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-138">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="d24cc-139">Die erste Zeichenfolge nach `@addTagHelper` gibt die Tag-Hilfsmethode zum Laden (Verwendung "*" für alle Tags Hilfsprogramme), und die zweite Zeichenfolge "AuthoringTagHelpers" gibt die Assembly, die das Tag-Hilfsprogramm im ist.</span><span class="sxs-lookup"><span data-stu-id="d24cc-139">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="d24cc-140">Beachten Sie, dass die zweite Zeile in die Core ASP.NET-MVC Tag-Hilfsprogramme, die mit der Platzhaltersyntax Schaltet (diese Hilfsprogramme werden im erläutert [Einführung in die Tag-Hilfsprogrammen](intro.md).) Es ist die `@addTagHelper` -Direktive, die die Tag-Hilfsprogramm an der Razor-Ansicht zur Verfügung stellt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-140">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="d24cc-141">Alternativ können Sie den voll gekennzeichneten Namen (FQN) der wie folgt ein Tag-Hilfsprogramm bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="d24cc-141">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    <span data-ttu-id="d24cc-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span></span>
    
    <span data-ttu-id="d24cc-143">Zum Hinzufügen eines Tag-Hilfsprogramms zu einer Ansicht mithilfe einer FQN Sie zunächst die FQN hinzufügen (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), und klicken Sie dann den Assemblynamen (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="d24cc-143">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="d24cc-144">Die Platzhaltersyntax verwenden, werden die meisten Entwickler bevorzugen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-144">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="d24cc-145">[Einführung in die Tag-Hilfsprogrammen](intro.md) im Detail auf Helper hinzufügen, entfernen, Hierarchie- und Platzhalter Tagsyntax wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-145">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="d24cc-146">Aktualisieren Sie das Markup in der *Views/Home/Contact.cshtml* Datei mit diesen Änderungen:</span><span class="sxs-lookup"><span data-stu-id="d24cc-146">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    <span data-ttu-id="d24cc-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

4.  <span data-ttu-id="d24cc-148">Führen Sie die app und Ihrem bevorzugten Browser verwenden, der HTML-Quelle an, damit Sie überprüfen können, dass die e-Mail-Tags mit Anker Markup ersetzt werden (z. B. `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="d24cc-148">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="d24cc-149">*Unterstützung* und *Marketing* werden als Links, gerendert, jedoch nicht über ein `href` Attribut, um funktionsfähig zu machen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-149">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="d24cc-150">Wir beheben, die im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-150">We'll fix that in the next section.</span></span>

<span data-ttu-id="d24cc-151">Hinweis: Wie HTML-Tags und Attribute, Tags, Klassennamen und Attribute in Razor und c# sind keine Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="d24cc-151">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="d24cc-152">SetAttribute und SetContent</span><span class="sxs-lookup"><span data-stu-id="d24cc-152">SetAttribute and SetContent</span></span>

<span data-ttu-id="d24cc-153">Wir werden in diesem Abschnitt Aktualisieren der `EmailTagHelper` , damit eine gültige Ankertag zum Abrufen von e-Mails erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-153">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="d24cc-154">Wir werden aktualisieren, um die Daten aus einer Razor-Ansicht werden (in Form einer `mail-to` Attribut) und verwenden, die beim Generieren des Ankers.</span><span class="sxs-lookup"><span data-stu-id="d24cc-154">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="d24cc-155">Update der `EmailTagHelper` Klasse durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d24cc-155">Update the `EmailTagHelper` class with the following:</span></span>

<span data-ttu-id="d24cc-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span></span>

<span data-ttu-id="d24cc-157">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="d24cc-157">**Notes:**</span></span>

* <span data-ttu-id="d24cc-158">In Pascal-Schreibweise angegeben Klassen- und Eigenschaftennamen für Tag Hilfsprogramme übersetzt ihre [senken Kebab Groß-/Kleinschreibung](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101).</span><span class="sxs-lookup"><span data-stu-id="d24cc-158">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101).</span></span> <span data-ttu-id="d24cc-159">Aus diesem Grund verwendet der `MailTo` -Attribut, verwenden Sie `<email mail-to="value"/>` entspricht.</span><span class="sxs-lookup"><span data-stu-id="d24cc-159">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="d24cc-160">Die letzte Zeile legt die abgeschlossenen für unsere minimal funktionale Tag-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="d24cc-160">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="d24cc-161">Die hervorgehobene Zeile zeigt die Syntax für das Hinzufügen von Attributen:</span><span class="sxs-lookup"><span data-stu-id="d24cc-161">The highlighted line shows the syntax for adding attributes:</span></span>

<span data-ttu-id="d24cc-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span></span>

<span data-ttu-id="d24cc-163">Dieser Ansatz funktioniert für das Attribut "Href" so lange nicht derzeit in der attributauflistung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="d24cc-163">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="d24cc-164">Sie können auch die `output.Attributes.Add` Methode, um eine taghilfsattribut bis zum Ende der Auflistung von Tagattribute hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-164">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="d24cc-165">Aktualisieren Sie das Markup in der *Views/Home/Contact.cshtml* Datei mit diesen Änderungen: [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-165">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="d24cc-166">Führen Sie die app, und stellen Sie sicher, dass sie die richtige Links generiert.</span><span class="sxs-lookup"><span data-stu-id="d24cc-166">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="d24cc-167">Wenn Sie die e-Mail-Tag selbst schließende geschrieben wurden (`<email mail-to="Rick" />`), das endgültige Ergebnis wäre auch selbstschließende.</span><span class="sxs-lookup"><span data-stu-id="d24cc-167">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="d24cc-168">So aktivieren Sie die Möglichkeit zum Schreiben der Transponder mit nur einem Starttag (`<email mail-to="Rick">`) versehen Sie die Klasse durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d24cc-168">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > <span data-ttu-id="d24cc-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span></span>
    
    <span data-ttu-id="d24cc-170">Mit der ein selbstschließende e-Mail-Tag-Hilfsprogramm, wäre die Ausgabe `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-170">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="d24cc-171">Selbstschließende Ankertags sind nicht gültig HTML, empfiehlt es sich um ein Tag-Hilfsprogramm zu erstellen, die selbstschließende ist, sodass Sie nicht empfehlenswert, um eines zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-171">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="d24cc-172">Tag-Hilfsprogrammen legen Sie den Typ des der `TagMode` Eigenschaft nach dem Lesen eines Tags.</span><span class="sxs-lookup"><span data-stu-id="d24cc-172">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="d24cc-173">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="d24cc-173">ProcessAsync</span></span>

<span data-ttu-id="d24cc-174">In diesem Abschnitt schreiben wir eine asynchrone-e-Mail-Hilfe.</span><span class="sxs-lookup"><span data-stu-id="d24cc-174">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="d24cc-175">Ersetzen Sie die `EmailTagHelper` Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d24cc-175">Replace the `EmailTagHelper` class with the following code:</span></span>

    <span data-ttu-id="d24cc-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span></span>

    <span data-ttu-id="d24cc-177">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="d24cc-177">**Notes:**</span></span>

    * <span data-ttu-id="d24cc-178">Diese Version verwendet die asynchrone `ProcessAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="d24cc-178">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="d24cc-179">Der asynchrone `GetChildContentAsync` gibt eine `Task` , enthält die `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-179">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="d24cc-180">Verwenden der `output` Parameter beim Abrufen von Inhalten des HTML-Elements.</span><span class="sxs-lookup"><span data-stu-id="d24cc-180">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="d24cc-181">Nehmen Sie die folgende Änderung an der *Views/Home/Contact.cshtml* Datei, sodass das Tag-Hilfsobjekt, der Ziel-e-Mail abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="d24cc-181">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    <span data-ttu-id="d24cc-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

3.  <span data-ttu-id="d24cc-183">Führen Sie die app, und stellen Sie sicher, dass sie gültige e-Mail-Links generiert.</span><span class="sxs-lookup"><span data-stu-id="d24cc-183">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="d24cc-184">RemoveAll, PreContent.SetHtmlContent und PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="d24cc-184">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="d24cc-185">Fügen Sie die folgenden `BoldTagHelper` Klasse, um die *TagHelpers* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d24cc-185">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="d24cc-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span></span>

    <span data-ttu-id="d24cc-187">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="d24cc-187">**Notes:**</span></span>
    
    * <span data-ttu-id="d24cc-188">Die `[HtmlTargetElement]` -Attribut übergibt ein Attributparameter, der angibt, dass jeder HTML-Element, das ein HTML‑Attribut enthält, mit dem Namen "bold" entspricht, und die `Process` Überschreibungsmethode in der Klasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-188">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="d24cc-189">In diesem Beispiel die `Process` Methode entfernt das Attribut "Fett" und schließt das Markup enthält, mit dem `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-189">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="d24cc-190">Da Sie nicht den vorhandenen Inhalt Tags ersetzen möchten, müssen Sie das öffnende schreiben `<strong>` tag mit der `PreContent.SetHtmlContent` -Methode und das schließende `</strong>` tag mit der `PostContent.SetHtmlContent` Methode.</span><span class="sxs-lookup"><span data-stu-id="d24cc-190">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="d24cc-191">Ändern der *About.cshtml* anzeigen enthalten einen `bold` Attributwert.</span><span class="sxs-lookup"><span data-stu-id="d24cc-191">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="d24cc-192">Der vollständige Code wird unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-192">The completed code is shown below.</span></span>

    <span data-ttu-id="d24cc-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span></span>

3.  <span data-ttu-id="d24cc-194">Führen Sie die app an.</span><span class="sxs-lookup"><span data-stu-id="d24cc-194">Run the app.</span></span> <span data-ttu-id="d24cc-195">Sie können Ihrem bevorzugten Browser verwenden, überprüfen die Quelle, und überprüfen das Markup.</span><span class="sxs-lookup"><span data-stu-id="d24cc-195">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="d24cc-196">Die `[HtmlTargetElement]` Attribut abzielt nur HTML-Markup, das einen Attributnamen an, der "Fett" bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-196">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="d24cc-197">Die `<bold>` Element durch das Hilfsprogramm Tag nicht geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="d24cc-197">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="d24cc-198">Kommentieren Sie Sie aus der `[HtmlTargetElement]` Attributzeile auf, und es werden standardmäßig als Ziel `<bold>` Tags, d. h. HTML-Markup des Formulars `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-198">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="d24cc-199">Beachten Sie, dass die Dateinamenskonvention entspricht der Name der Klasse **fett**taghelpers um `<bold>` Tags.</span><span class="sxs-lookup"><span data-stu-id="d24cc-199">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="d24cc-200">Die app auszuführen, und überprüfen Sie, ob die `<bold>` Tag durch die Tag-Hilfsprogramm verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-200">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="d24cc-201">Indem eine Klasse mit mehreren `[HtmlTargetElement]` Attribute führt eine logische OR-Ziele.</span><span class="sxs-lookup"><span data-stu-id="d24cc-201">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="d24cc-202">Verwenden den folgenden Code, wird z. B. die fett-Tag oder fett-Attribut entspricht.</span><span class="sxs-lookup"><span data-stu-id="d24cc-202">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

<span data-ttu-id="d24cc-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span></span>

<span data-ttu-id="d24cc-204">Wenn mehrere Attribute der gleichen Anweisung hinzugefügt werden, behandelt die Common Language Runtime diese als eine logische and</span><span class="sxs-lookup"><span data-stu-id="d24cc-204">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="d24cc-205">Beispielsweise in den folgenden Code ein HTML-Element muss den Namen "bold" mit einem Attribut mit dem Namen "bold" (`<bold bold />`) übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-205">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="d24cc-206">Sie können auch die `[HtmlTargetElement]` so ändern Sie den Namen des entsprechenden Elements.</span><span class="sxs-lookup"><span data-stu-id="d24cc-206">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="d24cc-207">Angenommen, Sie möchten die `BoldTagHelper` Ziel `<MyBold>` Tags, verwenden Sie das folgende Attribut:</span><span class="sxs-lookup"><span data-stu-id="d24cc-207">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="d24cc-208">Übergeben ein Modell in einem Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="d24cc-208">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="d24cc-209">Hinzufügen einer *Modelle* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d24cc-209">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="d24cc-210">Fügen Sie die folgenden `WebsiteContext` Klasse, um die *Modelle* Ordner:</span><span class="sxs-lookup"><span data-stu-id="d24cc-210">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    <span data-ttu-id="d24cc-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span></span>

3.  <span data-ttu-id="d24cc-212">Fügen Sie die folgenden `WebsiteInformationTagHelper` Klasse, um die *TagHelpers* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d24cc-212">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="d24cc-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span></span>
    
    <span data-ttu-id="d24cc-214">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="d24cc-214">**Notes:**</span></span>
    
    * <span data-ttu-id="d24cc-215">Wie bereits erwähnt, Tag Hilfsprogramme übersetzt Pascal-Schreibweise verwendet C#-Klassennamen und Eigenschaften für den Tag-Hilfsprogramme in [senken Kebab Groß-/Kleinschreibung](http://c2.com/cgi/wiki?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="d24cc-215">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://c2.com/cgi/wiki?KebabCase).</span></span> <span data-ttu-id="d24cc-216">Aus diesem Grund verwendet der `WebsiteInformationTagHelper` in Razor, die Sie schreiben `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-216">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="d24cc-217">Das Zielelement mit explizit nicht identifiziert werden die `[HtmlTargetElement]` Attribut, sodass die Standardeinstellung von `website-information` ist für die.</span><span class="sxs-lookup"><span data-stu-id="d24cc-217">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="d24cc-218">Wenn Sie das folgende Attribut (wobei es handelt es sich nicht um Kebab Groß-/Kleinschreibung jedoch stimmt mit dem Klassennamen) angewendet haben:</span><span class="sxs-lookup"><span data-stu-id="d24cc-218">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="d24cc-219">Der untere Kebab Kiste `<website-information />` stimmen nicht überein.</span><span class="sxs-lookup"><span data-stu-id="d24cc-219">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="d24cc-220">Wenn Sie möchten die `[HtmlTargetElement]` -Attribut, verwenden Sie Kebab Groß-/Kleinschreibung wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="d24cc-220">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="d24cc-221">Elemente, die selbstschließende sind haben keinen Inhalt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-221">Elements that are self-closing have no content.</span></span> <span data-ttu-id="d24cc-222">In diesem Beispiel wird das Markup Razor ein selbstschließendes Tags verwenden, aber das Tag-Hilfsobjekt erstellen eine [Abschnitt](http://www.w3.org/TR/html5/sections.html#the-section-element) Element (nicht selbstschließende ist und Sie werden das Schreiben von Inhalt in die `section` Element).</span><span class="sxs-lookup"><span data-stu-id="d24cc-222">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="d24cc-223">Aus diesem Grund müssen Sie festlegen `TagMode` auf `StartTagAndEndTag` Ausgabe zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-223">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="d24cc-224">Sie können alternativ die Einstellung für die Zeile auskommentieren `TagMode` und Schreiben von Markup mit dem ein Endtag.</span><span class="sxs-lookup"><span data-stu-id="d24cc-224">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="d24cc-225">(Beispielmarkup wird weiter unten in diesem Lernprogramm bereitgestellt.)</span><span class="sxs-lookup"><span data-stu-id="d24cc-225">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="d24cc-226">Die `$` (Dollarzeichen) in der folgenden Zeile verwendet ein [interpoliert Zeichenfolge](https://msdn.microsoft.com/library/Dn961160.aspx):</span><span class="sxs-lookup"><span data-stu-id="d24cc-226">The `$` (dollar sign) in the following line uses an [interpolated string](https://msdn.microsoft.com/library/Dn961160.aspx):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="d24cc-227">Fügen Sie das folgende Markup zum Rendern der *About.cshtml* anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-227">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="d24cc-228">Das hervorgehobene Markup zeigt die Website-Informationen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-228">The highlighted markup displays the web site information.</span></span>
    
    <span data-ttu-id="d24cc-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="d24cc-230">Im unten gezeigten Razor-Markup:</span><span class="sxs-lookup"><span data-stu-id="d24cc-230">In the Razor markup shown below:</span></span>
    >
    ><span data-ttu-id="d24cc-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span></span>
    > 
    ><span data-ttu-id="d24cc-232">Razor weiß der `info` Attribut ist eine Klasse, die keine Zeichenfolge, und C#-Code geschrieben werden soll.</span><span class="sxs-lookup"><span data-stu-id="d24cc-232">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="d24cc-233">Alle keine Zeichenfolgenmethoden taghilfsattribut geschrieben werden soll, ohne die `@` Zeichen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-233">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="d24cc-234">Führen Sie die app, und navigieren Sie zu der Info-Ansicht, um die Website-Informationen finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="d24cc-234">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="d24cc-235">Sie können das folgende Markup wird ein schließendes Tag mit und entfernen Sie die Zeile mit `TagMode.StartTagAndEndTag` in der Tag-Hilfsprogramm:</span><span class="sxs-lookup"><span data-stu-id="d24cc-235">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    ><span data-ttu-id="d24cc-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span></span>

## <a name="condition-tag-helper"></a><span data-ttu-id="d24cc-237">Bedingung Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="d24cc-237">Condition Tag Helper</span></span>

<span data-ttu-id="d24cc-238">Die Bedingung Tag Hilfsprogramm rendert die Ausgabe, wenn der Wert "true" übergeben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-238">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="d24cc-239">Fügen Sie die folgenden `ConditionTagHelper` Klasse, um die *TagHelpers* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d24cc-239">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="d24cc-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span></span>

2.  <span data-ttu-id="d24cc-241">Ersetzen Sie den Inhalt von der *Views/Home/Index.cshtml* Datei durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="d24cc-241">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="d24cc-242">Ersetzen Sie die `Index` Methode in der `Home` -Controller mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d24cc-242">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    <span data-ttu-id="d24cc-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span></span>

4.  <span data-ttu-id="d24cc-244">Führen Sie die app, und navigieren Sie zur Startseite.</span><span class="sxs-lookup"><span data-stu-id="d24cc-244">Run the app and browse to the home page.</span></span> <span data-ttu-id="d24cc-245">Das Markup in der bedingte `div` nicht gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="d24cc-245">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="d24cc-246">Fügen Sie die Abfragezeichenfolge `?approved=true` an die URL (z. B. `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="d24cc-246">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="d24cc-247">`approved`ist auf True festgelegt und die bedingte Markup wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-247">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="d24cc-248">Verwenden der [Nameof](https://msdn.microsoft.com/library/dn986596.aspx) Operator, um das Ziel, statt eine Zeichenfolge angeben, wie Sie mit den fett formatierten Tag-Hilfsprogramm-Attribut angeben:</span><span class="sxs-lookup"><span data-stu-id="d24cc-248">Use the [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
><span data-ttu-id="d24cc-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span></span>
>
><span data-ttu-id="d24cc-250">Die [Nameof](https://msdn.microsoft.com/library/dn986596.aspx) Operator schützt den Code sollte es jemals umgestaltet werden (es sollten so ändern Sie den Namen in `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="d24cc-250">The [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="d24cc-251">Vermeiden von Konflikten Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="d24cc-251">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="d24cc-252">In diesem Abschnitt schreiben Sie ein Paar von Tag-Hilfsprogrammen automatische Verknüpfungen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-252">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="d24cc-253">Die erste ersetzt Markup, enthält eine URL mit HTTP-ein HTML-Anker Tag mit der gleichen URL (und und gibt daher einen Link zu der URL) beginnt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-253">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="d24cc-254">Die zweite wird die für eine URL Kunst WWW ab.</span><span class="sxs-lookup"><span data-stu-id="d24cc-254">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="d24cc-255">Da diese zwei Hilfsmethoden sind eng miteinander verknüpft, und Sie sie in der Zukunft Umgestalten können, müssen wir sie in der gleichen Datei bleiben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-255">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="d24cc-256">Fügen Sie die folgenden `AutoLinkerHttpTagHelper` Klasse, um die *TagHelpers* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d24cc-256">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="d24cc-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="d24cc-258">Die `AutoLinkerHttpTagHelper` -Klasse Ziele `p` Elemente und verwendet [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) Anker zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-258">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) to create the anchor.</span></span>

2.  <span data-ttu-id="d24cc-259">Fügen Sie das folgende Markup bis zum Ende der *Views/Home/Contact.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="d24cc-259">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    <span data-ttu-id="d24cc-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span></span>

3.  <span data-ttu-id="d24cc-261">Führen Sie die app, und stellen Sie sicher, dass das Tag-Hilfsobjekt Anker ordnungsgemäß gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-261">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="d24cc-262">Update der `AutoLinker` Klasse einbeziehen, die `AutoLinkerWwwTagHelper` konvertiert der Www-Text in ein Ankertag, der auch den ursprünglichen Www Text enthält.</span><span class="sxs-lookup"><span data-stu-id="d24cc-262">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="d24cc-263">Im folgenden wird der aktualisierte Code hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="d24cc-263">The updated code is highlighted below:</span></span>

    <span data-ttu-id="d24cc-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span></span>

5.  <span data-ttu-id="d24cc-265">Führen Sie die app an.</span><span class="sxs-lookup"><span data-stu-id="d24cc-265">Run the app.</span></span> <span data-ttu-id="d24cc-266">Beachten Sie, dass der Www-Text wird als Link gerendert der HTTP-Text ist jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="d24cc-266">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="d24cc-267">Wenn Sie einen Haltepunkt in beiden Klassen einfügen, sehen Sie sich, dass die HTTP-Tag-Hilfsklasse zuerst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-267">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="d24cc-268">Das Problem ist, dass die Tag-Helper-Ausgabe zwischengespeichert wird, und wenn der WWW-Tag-Hilfsprogramm ausgeführt wird, werden im Cache gespeicherte Ausgabe aus dem HTTP-Tag-Hilfsprogramm überschrieben.</span><span class="sxs-lookup"><span data-stu-id="d24cc-268">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="d24cc-269">Später in diesem Lernprogramm sehen wir, wie Sie die Reihenfolge steuern, die Tag-Hilfsprogramme in ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-269">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="d24cc-270">Wir beheben den Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d24cc-270">We'll fix the code with the following:</span></span>

    <span data-ttu-id="d24cc-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="d24cc-272">Sie haben in der Ausgabe zuerst die Hilfsprogramme für die automatische Verknüpfungen Tag den Inhalt des Ziels durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d24cc-272">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    ><span data-ttu-id="d24cc-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span></span>
    >
    ><span data-ttu-id="d24cc-274">D. h., Sie rufen `GetChildContentAsync` mithilfe der `TagHelperOutput` übergebenen die `ProcessAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="d24cc-274">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="d24cc-275">Wie bereits erwähnt zuvor, da die Ausgabe zwischengespeichert wird, der letzten tag Helper Wins ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-275">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="d24cc-276">Sie haben dieses Problem behoben, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d24cc-276">You fixed that problem with the following code:</span></span>
    >
    ><span data-ttu-id="d24cc-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span></span>
    >
    ><span data-ttu-id="d24cc-278">Der obige Code überprüft, um festzustellen, ob der Inhalt geändert wurde und aufweist, ruft den Inhalt aus dem Ausgabepuffer ab.</span><span class="sxs-lookup"><span data-stu-id="d24cc-278">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="d24cc-279">Führen Sie die app, und stellen Sie sicher, dass die beiden Links funktionieren wie erwartet.</span><span class="sxs-lookup"><span data-stu-id="d24cc-279">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="d24cc-280">Hat sich zwar als es angezeigt werden kann, dass unsere automatisch Linker Tag Helper richtig und vollständig ist, eine geringfügige Problem.</span><span class="sxs-lookup"><span data-stu-id="d24cc-280">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="d24cc-281">Wenn der WWW-Tag-Hilfsmethode zuerst ausgeführt wird, sind der Www-Links nicht richtig.</span><span class="sxs-lookup"><span data-stu-id="d24cc-281">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="d24cc-282">Aktualisieren Sie den Code durch Hinzufügen der `Order` Überladung zur Steuerung der Reihenfolge, die das Tag in ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-282">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="d24cc-283">Die `Order` Eigenschaft bestimmt die Ausführungsreihenfolge relativ zu anderen Tag-Hilfsprogramme, die für dasselbe Element.</span><span class="sxs-lookup"><span data-stu-id="d24cc-283">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="d24cc-284">Der standardreihenfolgenwert ist 0 (null), und Instanzen mit niedrigeren Werten werden zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-284">The default order value is zero and instances with lower values are executed first.</span></span>

    <span data-ttu-id="d24cc-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span></span>
    
    <span data-ttu-id="d24cc-286">Der Code oben wird sichergestellt, dass das HTTP-Tag-Hilfsobjekt vor dem WWW-Tag-Hilfsprogramm ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d24cc-286">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="d24cc-287">Änderung `Order` auf `MaxValue` und stellen Sie sicher, dass das Markup für den WWW-Transponder generiert falsch ist.</span><span class="sxs-lookup"><span data-stu-id="d24cc-287">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="d24cc-288">Überprüfen und Abrufen von untergeordneten Inhalt</span><span class="sxs-lookup"><span data-stu-id="d24cc-288">Inspecting and retrieving child content</span></span>

<span data-ttu-id="d24cc-289">Der Tag-Hilfsvorlagen können mehrere Eigenschaften zum Abrufen von Inhalt.</span><span class="sxs-lookup"><span data-stu-id="d24cc-289">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="d24cc-290">Das Ergebnis des `GetChildContentAsync` angefügt werden können, um `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-290">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="d24cc-291">Sie können prüfen, dass das Ergebnis des `GetChildContentAsync` mit `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="d24cc-291">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="d24cc-292">Wenn Sie ändern `output.Content`, taghelpers Text wird nicht ausgeführt oder gerendert werden, es sei denn, Sie rufen `GetChildContentAsync` wie unsere Auto-Linker-Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d24cc-292">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

<span data-ttu-id="d24cc-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span><span class="sxs-lookup"><span data-stu-id="d24cc-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span></span>

-  <span data-ttu-id="d24cc-294">Mehrere Aufrufe `GetChildContentAsync` wird derselbe Wert zurückgegeben und wird nicht erneut ausgeführt. die `TagHelper` body, wenn Sie in einem "false"-Parameter, der angibt, verwenden Sie nicht das zwischengespeicherte Resultset bewegen.</span><span class="sxs-lookup"><span data-stu-id="d24cc-294">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
