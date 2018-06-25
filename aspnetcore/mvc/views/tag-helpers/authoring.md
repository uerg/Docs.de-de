---
title: Erstellen von Taghilfsprogrammen in ASP.NET Core
author: rick-anderson
description: Informationen zum Erstellen von Taghilfsprogrammen in ASP.NET Core
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 5873c6dbdeba1b5f2bf7ac85d8992480228b7125
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275316"
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="36cfd-103">Erstellen von Taghilfsprogrammen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36cfd-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="36cfd-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="36cfd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="36cfd-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="36cfd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="36cfd-106">Erste Schritte mit Taghilfsprogrammen</span><span class="sxs-lookup"><span data-stu-id="36cfd-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="36cfd-107">Dieses Tutorial soll als Einführung in das Programmieren von Taghilfsprogrammen dienen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="36cfd-108">Im Artikel [Introduction to Tag Helpers (Einführung in Taghilfsprogramme)](intro.md) werden die Vorteile von Taghilfsprogrammen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="36cfd-109">Bei einem Taghilfsprogramm handelt es sich um eine Klasse, die die `ITagHelper`-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="36cfd-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="36cfd-110">Wenn Sie hingegen ein Taghilfsprogramm erstellen, stellen Sie in der Regel eine Ableitung von `TagHelper` her, wodurch Sie auf die `Process`-Methode zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="36cfd-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="36cfd-111">Erstellen Sie ein neues ASP.NET Core-Projekt mit dem Namen **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="36cfd-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="36cfd-112">Für dieses Projekt benötigen Sie keine Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="36cfd-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="36cfd-113">Erstellen Sie einen Ordner mit dem Namen *TagHelpers*, in dem die Taghilfsprogramme gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="36cfd-114">Dieser Ordner ist *nicht* zwingend erforderlich, allerdings handelt es sich dabei um eine praktische Konvention.</span><span class="sxs-lookup"><span data-stu-id="36cfd-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="36cfd-115">Im Folgenden werden Beispiele zum Schreiben einfacher Taghilfsprogramme beschrieben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="36cfd-116">Ein Taghilfsprogramm mit den mindestens erforderlichen Elementen</span><span class="sxs-lookup"><span data-stu-id="36cfd-116">A minimal Tag Helper</span></span>

<span data-ttu-id="36cfd-117">In diesem Beispiel wird erläutert, wie Sie ein Taghilfsprogramm schreiben, das ein Update für ein E-Mail-Tag ausführt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="36cfd-118">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="36cfd-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="36cfd-119">Der Server verwendet das Taghilfsprogramm für E-Mails, um dieses Markup in Folgendes zu konvertieren:</span><span class="sxs-lookup"><span data-stu-id="36cfd-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="36cfd-120">Dabei handelt es sich um ein Anchor-Tag, das daraus einen E-Mail-Link erstellt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="36cfd-121">Dieses Hilfsprogramm kann sich als nützlich erweisen, wenn Sie eine Blog-Engine schreiben und eine E-Mail an die Marketingabteilung, den Support oder andere Kontakte senden möchten, die alle derselben Domäne angehören.</span><span class="sxs-lookup"><span data-stu-id="36cfd-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="36cfd-122">Fügen Sie dem Ordner *TagHelpers* folgende `EmailTagHelper`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
   <span data-ttu-id="36cfd-123">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="36cfd-123">**Notes:**</span></span>
    
   * <span data-ttu-id="36cfd-124">Taghilfsprogramme verwenden Namenskonventionen, die auf die Elemente des Stammklassennamens ausgerichtet sind. Das Element *TagHelper* ist in dem Klassennamen allerdings nicht enthalten.</span><span class="sxs-lookup"><span data-stu-id="36cfd-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="36cfd-125">In diesem Beispiel lautet der Stammname des **Email**-Taghilfsprogramms *email*, damit das `<email>`-Tag angezielt wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="36cfd-126">Diese Namenskonvention sollte für die meisten Taghilfsprogramme funktionieren. Nachfolgend finden Sie eine Erläuterung dazu, wie Sie diese außer Kraft setzen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
   * <span data-ttu-id="36cfd-127">Die `EmailTagHelper`-Klasse wird von `TagHelper` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="36cfd-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="36cfd-128">Über die `TagHelper`-Klasse werden Methoden und Eigenschaften für das Schreiben von Taghilfsprogrammen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
   * <span data-ttu-id="36cfd-129">Die überschriebene `Process`-Methode steuert die Funktionen des Taghilfsprogramms bei der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="36cfd-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="36cfd-130">Außerdem stellt die `TagHelper`-Klasse eine asynchrone Version (`ProcessAsync`) mit denselben Parametern bereit.</span><span class="sxs-lookup"><span data-stu-id="36cfd-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
   * <span data-ttu-id="36cfd-131">Der Kontextparameter von `Process` (und `ProcessAsync`) enthält Informationen, die der Ausführung des aktuellen HTML-Tags zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
   * <span data-ttu-id="36cfd-132">Der Ausgabeparameter von `Process` (und `ProcessAsync`) enthält ein zustandsbehaftetes HTML-Element, das für die Originalquelle steht, die zum Generieren eines HTML-Tags und von Inhalten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
   * <span data-ttu-id="36cfd-133">Der Klassenname in diesem Beispiel enthält ein **TagHelper**-Suffix, das *nicht* erforderlich ist, aber als bewährter Bestandteil angesehen wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="36cfd-134">Sie können die Klasse wie folgt deklarieren:</span><span class="sxs-lookup"><span data-stu-id="36cfd-134">You could declare the class as:</span></span>
    
   ```csharp
   public class Email : TagHelper
   ```

2. <span data-ttu-id="36cfd-135">Fügen Sie der *Views/_ViewImports.cshtml*-Datei die Anweisung `addTagHelper` hinzu, um die `EmailTagHelper`-Klasse für sämtliche Razor-Ansichten verfügbar zu machen: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="36cfd-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
   <span data-ttu-id="36cfd-136">Im obenstehenden Code wird die Platzhaltersyntax verwendet, um alle Taghilfsprogramme anzugeben, die in der Assembly verfügbar sein sollen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="36cfd-137">In der ersten Zeichenfolge nach `@addTagHelper` wird angegeben, welches Taghilfsprogramm geladen werden soll (Verwenden Sie für alle Taghilfsprogramme „\*“), und die zweite Zeichenfolge „AuthoringTagHelpers“ gibt die Assembly an, in der sich das Taghilfsprogramm befindet.</span><span class="sxs-lookup"><span data-stu-id="36cfd-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="36cfd-138">Beachten Sie, dass in der zweiten Zeile die Taghilfsprogramme für ASP.NET Core MVC angegeben sind, die die Platzhaltersyntax verwendet (Informationen zu diesen Hilfsprogrammen finden Sie unter [Introduction to Tag Helpers (Einführung in Taghilfsprogramme))](intro.md). Die `@addTagHelper`-Anweisung stellt das Taghilfsprogramm für die Razor-Ansicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="36cfd-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="36cfd-139">Stattdessen können Sie auch wie folgt den vollqualifizierten Namen eines Taghilfsprogramms eingeben:</span><span class="sxs-lookup"><span data-stu-id="36cfd-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="36cfd-140">Wenn Sie einer Ansicht über einen vollqualifizierten Namen ein Taghilfsprogramm hinzufügen möchten, müssen Sie zunächst diesen Namen (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) und dann den Assemblynamen (*AuthoringTagHelpers*) hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="36cfd-141">Die meisten Entwickler verwenden am liebsten die Platzhaltersyntax.</span><span class="sxs-lookup"><span data-stu-id="36cfd-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="36cfd-142">Unter [Introduction to Tag Helpers (Einführung in Taghilfsprogramme)](intro.md) finden Sie mehr Details zum Hinzufügen und Entfernen von Taghilfsprogrammen, zur Hierarchie und zur Platzhaltersyntax.</span><span class="sxs-lookup"><span data-stu-id="36cfd-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3. <span data-ttu-id="36cfd-143">Aktualisieren Sie das Markup mit den folgenden Änderungen in der Datei *Views/Home/Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="36cfd-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4. <span data-ttu-id="36cfd-144">Führen Sie die App aus, und verwenden Sie einen beliebigen Browser, um die HTML-Quelle abzurufen, damit Sie überprüfen können, ob die E-Mail-Tags durch Anchor-Markups wie `<a>Support</a>` ersetzt wurden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="36cfd-145">*Support* und *Marketing* werden als Links gerendert. Allerdings besitzen sie kein `href`-Attribut, das sie funktionsfähig macht.</span><span class="sxs-lookup"><span data-stu-id="36cfd-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="36cfd-146">Dies soll im nächsten Abschnitt behoben werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-146">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="36cfd-147">„SetAttribute“ und „SetContent“</span><span class="sxs-lookup"><span data-stu-id="36cfd-147">SetAttribute and SetContent</span></span>

<span data-ttu-id="36cfd-148">In diesem Abschnitt wird erläutert, wie Sie ein Update für `EmailTagHelper` ausführen, sodass ein gültiges Anchor-Tag für die E-Mail erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-148">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="36cfd-149">Für das Hilfsprogramm soll ein Update ausgeführt werden, damit es der Razor-Ansicht Informationen in Form eines `mail-to`-Attributs entnehmen und dieses zum Generieren des Anchors verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="36cfd-149">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="36cfd-150">Führen Sie über folgenden Code ein Update für die `EmailTagHelper`-Klasse aus:</span><span class="sxs-lookup"><span data-stu-id="36cfd-150">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="36cfd-151">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="36cfd-151">**Notes:**</span></span>

* <span data-ttu-id="36cfd-152">Namen von Klassen und Eigenschaften für Taghilfsprogramme, die in Pascal-Schreibweise angegeben sind, werden in [Lower Kebab Case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101) übersetzt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-152">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="36cfd-153">Wenn Sie daher das `MailTo`-Attribut verwenden möchten, müssen Sie das entsprechende `<email mail-to="value"/>`-Äquivalent auswählen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-153">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="36cfd-154">Mit der letzten Zeile wird der Inhalt dieses Taghilfsprogramms fertig gestellt, das alle mindestens erforderlichen Elemente enthält.</span><span class="sxs-lookup"><span data-stu-id="36cfd-154">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="36cfd-155">In der markierten Zeile wird die Syntax zum Hinzufügen von Attributen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="36cfd-155">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="36cfd-156">Dieser Ansatz funktioniert für das Attribut „href“, wenn es zu diesem Zeitpunkt noch nicht in der Attributsammlung enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="36cfd-156">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="36cfd-157">Außerdem können Sie die `output.Attributes.Add`-Methode verwenden, um ein Attribut des Taghilfsprogramms am Ende der Tagattributsammlung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-157">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="36cfd-158">Aktualisieren Sie das Markup mit den folgenden Änderungen in der Datei *Views/Home/Contact.cshtml*: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="36cfd-158">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2. <span data-ttu-id="36cfd-159">Führen Sie die App aus, und überprüfen Sie, ob die richtigen Links generiert werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-159">Run the app and verify that it generates the correct links.</span></span>
    
   > [!NOTE]
   > <span data-ttu-id="36cfd-160">Wenn das E-Mail-Tag selbstschließend sein soll (`<email mail-to="Rick" />`), ist auch die endgültige Ausgabe selbstschließend.</span><span class="sxs-lookup"><span data-stu-id="36cfd-160">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="36cfd-161">Sie müssen die Klasse wie folgt ergänzen, um die Funktion zum Schreiben des Tags nur mit einem Starttag (`<email mail-to="Rick">`) zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="36cfd-161">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
   > 
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
   <span data-ttu-id="36cfd-162">Bei einem selbstschließenden E-Mail-Taghilfsprogramm sieht die Ausgabe wie folgt aus `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="36cfd-162">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="36cfd-163">Bei selbstschließenden Anchor-Tags handelt es sich nicht um gültige HTML-Elemente. Daher sollten Sie diese nicht erstellen. Erstellen Sie stattdessen ein selbstschließendes Taghilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="36cfd-163">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="36cfd-164">Taghilfsprogramme legen den Typ der `TagMode`-Eigenschaft fest, nachdem ein Tag gelesen wurde.</span><span class="sxs-lookup"><span data-stu-id="36cfd-164">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="36cfd-165">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="36cfd-165">ProcessAsync</span></span>

<span data-ttu-id="36cfd-166">In diesem Abschnitt wird beschrieben, wie Sie ein asynchrones E-Mail-Hilfsprogramm schreiben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-166">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="36cfd-167">Ersetzen Sie die Klasse `EmailTagHelper` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="36cfd-167">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="36cfd-168">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="36cfd-168">**Notes:**</span></span>

   * <span data-ttu-id="36cfd-169">Diese Version verwendet die asynchrone `ProcessAsync`-Methode.</span><span class="sxs-lookup"><span data-stu-id="36cfd-169">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="36cfd-170">Die asynchrone `GetChildContentAsync`-Methode gibt `Task` mit `TagHelperContent` zurück.</span><span class="sxs-lookup"><span data-stu-id="36cfd-170">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="36cfd-171">Verwenden Sie den Parameter `output`, um Inhalte des HTML-Elements abzurufen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-171">Use the `output` parameter to get contents of the HTML element.</span></span>

2. <span data-ttu-id="36cfd-172">Nehmen Sie folgende Änderung an der *Views/Home/Contact.cshtml*-Datei vor, damit das Taghilfsprogramm die Ziel-E-Mail abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="36cfd-172">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3. <span data-ttu-id="36cfd-173">Führen Sie die App aus, und überprüfen Sie, ob gültige E-Mail-Links generiert werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-173">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="36cfd-174">„RemoveAll“, „PreContent.SetHtmlContent“ und „PostContent.SetHtmlContent“</span><span class="sxs-lookup"><span data-stu-id="36cfd-174">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="36cfd-175">Fügen Sie dem Ordner *TagHelpers* folgende `BoldTagHelper`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-175">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   <span data-ttu-id="36cfd-176">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="36cfd-176">**Notes:**</span></span>
    
   * <span data-ttu-id="36cfd-177">Das `[HtmlTargetElement]`-Attribut übergibt einen Attributparameter, der angibt, dass diesem ein HTML-Element mit dem HTML-Attribut „bold“ zugeordnet werden kann. Anschließend wird die Überschreibungsmethode `Process` in der Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-177">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="36cfd-178">In diesem Beispiel entfernt die Methode `Process` das Attribut „bold“ und umschließt das enthaltene Markup mit `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="36cfd-178">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
   * <span data-ttu-id="36cfd-179">Da der bestehende Taginhalt nicht ersetzt werden soll, müssen Sie das Starttag `<strong>` über die Methode `PreContent.SetHtmlContent` und das Endtag `</strong>` über die Methode `PostContent.SetHtmlContent` schreiben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-179">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2. <span data-ttu-id="36cfd-180">Verändern Sie die Ansicht *About.cshtml*, damit diese den Attributwert `bold` enthält.</span><span class="sxs-lookup"><span data-stu-id="36cfd-180">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="36cfd-181">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-181">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3. <span data-ttu-id="36cfd-182">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="36cfd-182">Run the app.</span></span> <span data-ttu-id="36cfd-183">Sie können einen beliebigen Browser verwenden, um die Quelle zu untersuchen und das Markup zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-183">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="36cfd-184">Das oben genannte `[HtmlTargetElement]`-Attribut gilt nur für HTML-Markups mit dem Attributnamen „bold“.</span><span class="sxs-lookup"><span data-stu-id="36cfd-184">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="36cfd-185">Das `<bold>`-Element wurde vom Taghilfsprogramm nicht verändert.</span><span class="sxs-lookup"><span data-stu-id="36cfd-185">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="36cfd-186">Kommentieren Sie die Attributzeile `[HtmlTargetElement]` aus, damit standardmäßig `<bold>`-Tags angezielt werden, also HTML-Markups im Format `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="36cfd-186">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="36cfd-187">Beachten Sie, dass die Standardnamenskonvention dem Klassennamen **Bold**TagHelper den `<bold>`-Tags entspricht.</span><span class="sxs-lookup"><span data-stu-id="36cfd-187">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="36cfd-188">Führen Sie die App aus, und überprüfen Sie, ob das `<bold>`-Tag vom Taghilfsprogramm verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-188">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="36cfd-189">Wenn Sie eine Klasse durch mehrere `[HtmlTargetElement]`-Attribute ergänzen, wird ein logischer ODER-Operator der Ziele erstellt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-189">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="36cfd-190">Wenn Sie z.B. den nachfolgenden Code verwenden, wird ein „bold“-Tag oder ein „bold“-Attribut zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="36cfd-190">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="36cfd-191">Wenn mehrere Attribute derselben Anweisung hinzugefügt werden, behandelt die Runtime diese als logisches UND.</span><span class="sxs-lookup"><span data-stu-id="36cfd-191">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="36cfd-192">Beispielsweise muss im nachfolgenden Code ein HTML-Element den Namen „bold“ mit einem Attribut namens „bold“ (`<bold bold />`) enthalten sein, damit dieses zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="36cfd-192">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="36cfd-193">Außerdem können Sie `[HtmlTargetElement]` verwenden, um den Namen des angezielten Elements zu ändern.</span><span class="sxs-lookup"><span data-stu-id="36cfd-193">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="36cfd-194">Wenn Sie z.B. möchten, dass `BoldTagHelper` `<MyBold>`-Tags anzielt, müssen Sie die folgenden Attribute verwenden:</span><span class="sxs-lookup"><span data-stu-id="36cfd-194">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="36cfd-195">Übergeben eines Modells an das Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="36cfd-195">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="36cfd-196">Fügen Sie einen *Models*-Ordner hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-196">Add a *Models* folder.</span></span>

2. <span data-ttu-id="36cfd-197">Fügen Sie dem Ordner *Models* die folgende `WebsiteContext`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="36cfd-197">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3. <span data-ttu-id="36cfd-198">Fügen Sie dem Ordner *TagHelpers* folgende `WebsiteInformationTagHelper`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-198">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
   <span data-ttu-id="36cfd-199">**Hinweise:**</span><span class="sxs-lookup"><span data-stu-id="36cfd-199">**Notes:**</span></span>
    
   * <span data-ttu-id="36cfd-200">Wie obenstehend erwähnt, übersetzen Taghilfsprogramme C#-Klassennamen und -Eigenschaften in der Pascal-Schreibweise in Taghilfsprogramme in [Lower Kebab Case](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="36cfd-200">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="36cfd-201">Wenn Sie `WebsiteInformationTagHelper` in Razor verwenden möchten, schreiben Sie daher `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="36cfd-201">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
   * <span data-ttu-id="36cfd-202">Das Zielelement wird mit dem `[HtmlTargetElement]`-Attribut nicht explizit identifiziert, sodass standardmäßig `website-information` angezielt wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-202">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="36cfd-203">Wenn Sie das folgende Attribut angewendet haben (beachten Sie, dass es sich hierbei nicht um Kebab Case handelt, sondern das Attribut dem Klassennamen zugeordnet wird):</span><span class="sxs-lookup"><span data-stu-id="36cfd-203">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```
    
   <span data-ttu-id="36cfd-204">Das Lower Kebab Case-Tag `<website-information />` wird dann nicht zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="36cfd-204">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="36cfd-205">Wenn Sie das `[HtmlTargetElement]`-Attribut verwenden, müssen Sie wie nachfolgend dargestellt Kebab Case verwenden:</span><span class="sxs-lookup"><span data-stu-id="36cfd-205">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```
    
   * <span data-ttu-id="36cfd-206">Selbstschließende Elemente haben keinen Inhalt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-206">Elements that are self-closing have no content.</span></span> <span data-ttu-id="36cfd-207">In diesem Beispiel verwendet das Razor-Markup ein selbstschließendes Tag, aber das Taghilfsprogramm erstellt ein [section](http://www.w3.org/TR/html5/sections.html#the-section-element)-Element. Dieses ist nicht selbstschließend, und Sie können Inhalt in das `section`-Element schreiben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-207">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="36cfd-208">Aus diesem Grund müssen Sie `TagMode` auf `StartTagAndEndTag` festlegen, um Ausgaben zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-208">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="36cfd-209">Stattdessen können Sie auch die Zeileneinstellung `TagMode` auskommentieren und Markups mit einem Endtag schreiben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-209">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="36cfd-210">(Ein Beispielmarkup wird in einem nachfolgenden Abschnitt dieses Tutorials zur Verfügung gestellt.)</span><span class="sxs-lookup"><span data-stu-id="36cfd-210">(Example markup is provided later in this tutorial.)</span></span>
    
   * <span data-ttu-id="36cfd-211">Das Dollarzeichen (`$`) in der folgenden Zeile verwendet eine [interpolierte Zeichenfolge](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="36cfd-211">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

4. <span data-ttu-id="36cfd-212">Fügen Sie der *About.cshtml*-Ansicht das folgende Markup hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-212">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="36cfd-213">Das markierte Markup zeigt die Websiteinformationen an.</span><span class="sxs-lookup"><span data-stu-id="36cfd-213">The highlighted markup displays the web site information.</span></span>
    
   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]
    
   > [!NOTE]
   > <span data-ttu-id="36cfd-214">Im nachfolgenden Razor-Markup:</span><span class="sxs-lookup"><span data-stu-id="36cfd-214">In the Razor markup shown below:</span></span>
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   > 
   > <span data-ttu-id="36cfd-215">Razor weiß, dass es sich bei dem `info`-Attribut um eine Klasse und nicht um eine Zeichenfolge handelt und Sie C#-Code schreiben wollen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-215">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="36cfd-216">Alle Attribute von Taghilfsprogrammen, die keine Zeichenfolgen darstellen, sollten ohne das Zeichen `@` geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-216">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5. <span data-ttu-id="36cfd-217">Führen Sie die App aus, und navigieren Sie zur Ansicht „Info“, um die Websiteinformationen zu sehen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-217">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="36cfd-218">Sie können das folgende Markup mit dem Endtag verwenden und die Zeile mit `TagMode.StartTagAndEndTag` aus dem Taghilfsprogramm entfernen:</span><span class="sxs-lookup"><span data-stu-id="36cfd-218">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="36cfd-219">Taghilfsprogramm für Bedingungen</span><span class="sxs-lookup"><span data-stu-id="36cfd-219">Condition Tag Helper</span></span>

<span data-ttu-id="36cfd-220">Das Taghilfsprogramm für Bedingungen rendert Ausgaben, wenn ein TRUE-Wert zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-220">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="36cfd-221">Fügen Sie dem Ordner *TagHelpers* folgende `ConditionTagHelper`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-221">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2. <span data-ttu-id="36cfd-222">Ersetzen Sie die Inhalte der Datei *Views/HelloWorld/Index.cshtml* durch das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="36cfd-222">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   ```cshtml
   @using AuthoringTagHelpers.Models
   @model WebsiteContext
    
   @{
       ViewData["Title"] = "Home Page";
   }
    
   <div>
       <h3>Information about our website (outdated):</h3>
       <website-information info=@Model />
       <div condition="@Model.Approved">
           <p>
               This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
               Visit www.contoso.com for more information.
           </p>
       </div>
   </div>
   ```
    
3. <span data-ttu-id="36cfd-223">Ersetzen Sie die `Index`-Methode im `Home`-Controller durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="36cfd-223">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4. <span data-ttu-id="36cfd-224">Führen Sie die App aus, und navigieren Sie zur Homepage.</span><span class="sxs-lookup"><span data-stu-id="36cfd-224">Run the app and browse to the home page.</span></span> <span data-ttu-id="36cfd-225">Das Markup im bedingten `div`-Element wird nicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="36cfd-225">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="36cfd-226">Fügen Sie die Abfragezeichenfolge `?approved=true` an die URL an (z.B. `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="36cfd-226">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="36cfd-227">`approved` ist auf TRUE festgelegt, und das bedingte Markup wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-227">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="36cfd-228">Verwenden Sie den [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)-Operator, um das anzuzielende Attribut wie beim „bold“-Taghilfsprogramm anzugeben, anstatt eine Zeichenfolge anzugeben:</span><span class="sxs-lookup"><span data-stu-id="36cfd-228">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
> 
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
> 
> <span data-ttu-id="36cfd-229">Der [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)-Operator schützt den Code, wenn dieser umgestaltet wird, also wenn z.B. der Name in `RedCondition` geändert wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-229">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="36cfd-230">Vermeiden von Konflikten mit dem Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="36cfd-230">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="36cfd-231">In diesem Abschnitt wird erläutert, wie Sie zwei Taghilfsprogramme für automatische Verlinkungen schreiben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-231">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="36cfd-232">Das erste ersetzt Markups mit einer URL, die mit HTTP beginnt, durch ein HTML-Anchor-Tag mit derselben URL, sodass ein Link zur der URL zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-232">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="36cfd-233">Das zweite führt dieselbe Aktion aus, allerdings für URLs, die mit WWW beginnen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-233">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="36cfd-234">Da dieses beiden Hilfsprogramme sehr ähnlich funktionieren und Sie sie möglicherweise in der Zukunft umgestalten möchten, werden diese in derselben Datei gespeichert.</span><span class="sxs-lookup"><span data-stu-id="36cfd-234">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="36cfd-235">Fügen Sie dem Ordner *TagHelpers* folgende `AutoLinkerHttpTagHelper`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="36cfd-235">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="36cfd-236">Die `AutoLinkerHttpTagHelper`-Klasse zielt `p`-Elemente an und verwendet [RegEx](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference), um den Anchor zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-236">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2. <span data-ttu-id="36cfd-237">Fügen Sie das folgende Markup an das Ende der Datei *Views/Home/Contact.cshtml* an:</span><span class="sxs-lookup"><span data-stu-id="36cfd-237">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3. <span data-ttu-id="36cfd-238">Führen Sie die App aus, und überprüfen Sie, ob das Taghilfsprogramm den Anchor richtig rendert.</span><span class="sxs-lookup"><span data-stu-id="36cfd-238">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4. <span data-ttu-id="36cfd-239">Führen Sie ein Update für die `AutoLinker`-Klasse aus, um `AutoLinkerWwwTagHelper` einzufügen, wodurch WWW-Text in ein Anchor-Tag konvertiert wird, das ebenfalls den ursprünglichen WWW-Text enthält.</span><span class="sxs-lookup"><span data-stu-id="36cfd-239">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="36cfd-240">Der aktualisierte Code wird im folgenden Beispiel markiert:</span><span class="sxs-lookup"><span data-stu-id="36cfd-240">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5. <span data-ttu-id="36cfd-241">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="36cfd-241">Run the app.</span></span> <span data-ttu-id="36cfd-242">Beachten Sie, dass der WWW-Text als Link gerendert wird, der HTTP-Text hingegen nicht.</span><span class="sxs-lookup"><span data-stu-id="36cfd-242">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="36cfd-243">Wenn Sie in beide Klassen einen Haltepunkt einfügen, wird die HTTP-Hilfsprogrammklasse zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-243">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="36cfd-244">Das Problem dabei ist, dass die Ausgabe des Taghilfsprogramms zwischengespeichert wird, und wenn das WWW-Hilfsprogramm ausgeführt wird, wird die zwischengespeicherte Ausgabe des HTTP-Hilfsprogramms überschrieben.</span><span class="sxs-lookup"><span data-stu-id="36cfd-244">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="36cfd-245">Nachfolgend wird erläutert, wie Sie die Reihenfolge kontrollieren, in der Taghilfsprogramme ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-245">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="36cfd-246">Codefehler werden wie folgt behoben:</span><span class="sxs-lookup"><span data-stu-id="36cfd-246">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="36cfd-247">In der ersten Version der Taghilfsprogramme für automatische Verlinkungen konnten Sie den Inhalt des Ziels über folgenden Code abrufen:</span><span class="sxs-lookup"><span data-stu-id="36cfd-247">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   > 
   > <span data-ttu-id="36cfd-248">Das bedeutet, Sie rufen `GetChildContentAsync` über das `TagHelperOutput`-Objekt ab, das in der `ProcessAsync`-Methode übergeben wurde.</span><span class="sxs-lookup"><span data-stu-id="36cfd-248">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="36cfd-249">Wie zuvor bereits erwähnt, gewinnt das letzte Taghilfsprogramm, das ausgeführt wird, da die Ausgabe zwischengespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-249">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="36cfd-250">Sie haben dieses Problem mithilfe des folgenden Codes behoben:</span><span class="sxs-lookup"><span data-stu-id="36cfd-250">You fixed that problem with the following code:</span></span>
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   > 
   > <span data-ttu-id="36cfd-251">Der obenstehende Code prüft, ob der Inhalt verändert wurde. Wenn das der Fall ist, ruft er den Inhalt vom Ausgabepuffer ab.</span><span class="sxs-lookup"><span data-stu-id="36cfd-251">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6. <span data-ttu-id="36cfd-252">Führen Sie die App aus, und überprüfen Sie, ob die beiden Links wie gewünscht funktionieren.</span><span class="sxs-lookup"><span data-stu-id="36cfd-252">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="36cfd-253">Dabei kann es oft sein, dass es scheint, als würde das Taghilfsprogramm für die automatische Verlinkung einwandfrei funktionieren, obwohl ein verstecktes Problem besteht.</span><span class="sxs-lookup"><span data-stu-id="36cfd-253">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="36cfd-254">Wenn das WWW-Taghilfsprogramm zuerst ausgeführt wird, werden die WWW-Links fehlerhaft erstellt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-254">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="36cfd-255">Aktualisieren Sie den Code, indem Sie die Überladung `Order` hinzufügen, um die Reihenfolge zu kontrollieren, in der das Tag ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-255">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="36cfd-256">Die Eigenschaft `Order` bestimmt die Ausführungsreihenfolge, die im Zusammenhang mit anderen Taghilfsprogrammen steht, die auf dasselbe Element ausgerichtet sind.</span><span class="sxs-lookup"><span data-stu-id="36cfd-256">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="36cfd-257">Der Wert der Standardreihenfolge ist 0 (null), und Instanzen mit niedrigeren Werten werden zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="36cfd-257">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
   <span data-ttu-id="36cfd-258">Der obenstehende Code stellt sicher, dass das HTTP-Taghilfsprogramm vor dem WWW-Taghilfsprogramm ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="36cfd-258">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="36cfd-259">Ändern Sie `Order` in `MaxValue`, und überprüfen Sie, ob das für das WWW-Tag generierte Markup falsch ist.</span><span class="sxs-lookup"><span data-stu-id="36cfd-259">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="36cfd-260">Überprüfen und Abrufen von untergeordnetem Inhalt</span><span class="sxs-lookup"><span data-stu-id="36cfd-260">Inspect and retrieve child content</span></span>

<span data-ttu-id="36cfd-261">Die Taghilfsprogramme stellen mehrere Eigenschaften zum Abrufen von Inhalt zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="36cfd-261">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="36cfd-262">Das Ergebnis von `GetChildContentAsync` kann an `output.Content` angefügt werden.</span><span class="sxs-lookup"><span data-stu-id="36cfd-262">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="36cfd-263">Sie können das Ergebnis von `GetChildContentAsync` mit `GetContent` prüfen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-263">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="36cfd-264">Wenn Sie `output.Content` ändern, wird der TagHelper-Text nicht ausgeführt oder gerendert, solange Sie nicht wie im Beispiel zur automatischen Verlinkung `GetChildContentAsync` aufrufen:</span><span class="sxs-lookup"><span data-stu-id="36cfd-264">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="36cfd-265">Bei mehreren Aufrufen von `GetChildContentAsync` wird derselbe Wert zurückgegeben, und der `TagHelper`-Text wird nicht erneut ausgeführt, solange Sie keinen FALSE-Parameter übergeben, der darauf hindeutet, dass die zwischengespeicherten Ergebnisse nicht verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="36cfd-265">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
