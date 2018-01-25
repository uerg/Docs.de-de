---
title: Konfigurieren Sie portable Objekt Lokalisierung
author: sebastienros
description: "Dieser Artikel führt Portable Objektdateien und erläutert die Schritte zu ihrer Verwendung in einer ASP.NET Core-Anwendung mit dem Framework Core Orchard."
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="a37a0-103">Konfigurieren Sie portable Objekt Lokalisierung Orchard Core</span><span class="sxs-lookup"><span data-stu-id="a37a0-103">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="a37a0-104">Durch [Sébastien Ros](https://github.com/sebastienros) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a37a0-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a37a0-105">Dieser Artikel führt Sie durch die Schritte für die Verwendung von portablen Objekt (PO)-Dateien in einer ASP.NET Core-Anwendung mit der [Orchard Core](https://github.com/OrchardCMS/OrchardCore) Framework.</span><span class="sxs-lookup"><span data-stu-id="a37a0-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="a37a0-106">**Hinweis:** Orchard Core wird nicht in ein Microsoft-Produkt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="a37a0-107">Microsoft bietet daher keine Unterstützung für diese Funktion an.</span><span class="sxs-lookup"><span data-stu-id="a37a0-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="a37a0-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a37a0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="a37a0-109">Was ist ein PO-Datei?</span><span class="sxs-lookup"><span data-stu-id="a37a0-109">What is a PO file?</span></span>

<span data-ttu-id="a37a0-110">PO-Dateien werden als Textdateien, die mit den übersetzten Zeichenfolgen für eine bestimmte Sprache verteilt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="a37a0-111">Einige Vorteile der PO-Dateien vorzuziehen *resx* Includedateien:</span><span class="sxs-lookup"><span data-stu-id="a37a0-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="a37a0-112">PO-Dateien unterstützen Pluralisierung; *resx* Dateien bieten keine Unterstützung Pluralisierung.</span><span class="sxs-lookup"><span data-stu-id="a37a0-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="a37a0-113">PO-Dateien werden nicht kompiliert, z. B. *resx* Dateien.</span><span class="sxs-lookup"><span data-stu-id="a37a0-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="a37a0-114">Daher sind spezialisierte Tools und die Build-Schritte nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a37a0-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="a37a0-115">PO-Dateien funktionieren gut mit collaborative online Bearbeitungstools.</span><span class="sxs-lookup"><span data-stu-id="a37a0-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="a37a0-116">Beispiel</span><span class="sxs-lookup"><span data-stu-id="a37a0-116">Example</span></span>

<span data-ttu-id="a37a0-117">Hier ist eine PO-Beispieldatei, die die Übersetzung für zwei Zeichenfolgen in Französisch, einschließlich eines mit der Pluralform enthält:</span><span class="sxs-lookup"><span data-stu-id="a37a0-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="a37a0-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="a37a0-118">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="a37a0-119">In diesem Beispiel wird die folgende Syntax verwendet:</span><span class="sxs-lookup"><span data-stu-id="a37a0-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="a37a0-120">`#:`: Ein Kommentar, der angibt, des Kontexts der Zeichenfolge, die übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="a37a0-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="a37a0-121">Die gleiche Zeichenfolge möglicherweise unterschiedlich übersetzt werden, je nachdem, wo sie verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a37a0-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="a37a0-122">`msgid`: Die nicht übersetzten Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a37a0-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="a37a0-123">`msgstr`: Die übersetzte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a37a0-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="a37a0-124">Im Fall von Pluralisierung-Unterstützung können mehrere Einträge definiert werden.</span><span class="sxs-lookup"><span data-stu-id="a37a0-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="a37a0-125">`msgid_plural`: Die Zeichenfolge, nicht übersetzte im plural.</span><span class="sxs-lookup"><span data-stu-id="a37a0-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="a37a0-126">`msgstr[0]`: Die übersetzte Zeichenfolge für die Groß-/Kleinschreibung 0.</span><span class="sxs-lookup"><span data-stu-id="a37a0-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="a37a0-127">`msgstr[N]`: Die übersetzte Zeichenfolge für die Groß-/Kleinschreibung N.</span><span class="sxs-lookup"><span data-stu-id="a37a0-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="a37a0-128">Die PO-Dateispezifikation verwendbaren [hier](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="a37a0-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="a37a0-129">Konfigurieren der Unterstützung der Bestellung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a37a0-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="a37a0-130">Dieses Beispiel beruht auf einer ASP.NET Core MVC-Anwendung, die aus einer Visual Studio-2017-Projektvorlage generiert.</span><span class="sxs-lookup"><span data-stu-id="a37a0-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="a37a0-131">Verweisen auf das Paket</span><span class="sxs-lookup"><span data-stu-id="a37a0-131">Referencing the package</span></span>

<span data-ttu-id="a37a0-132">Hinzufügen eines Verweises auf die `OrchardCore.Localization.Core` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="a37a0-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="a37a0-133">Es steht auf [MyGet](https://www.myget.org/) am folgenden Paketquelle: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="a37a0-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="a37a0-134">Die *csproj* Datei enthält nun eine Zeile ähnlich der folgenden (die Versionsnummer kann davon abweichen):</span><span class="sxs-lookup"><span data-stu-id="a37a0-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="a37a0-135">Registrierung des Diensts</span><span class="sxs-lookup"><span data-stu-id="a37a0-135">Registering the service</span></span>

<span data-ttu-id="a37a0-136">Fügen Sie die erforderlichen Dienste, die `ConfigureServices` Methode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a37a0-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="a37a0-137">Die erforderliche Middleware zum Hinzufügen der `Configure` Methode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a37a0-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="a37a0-138">Fügen Sie den folgenden Code, um Ihre Wahl Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="a37a0-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="a37a0-139">*About.cshtml* wird in diesem Beispiel verwendet.</span><span class="sxs-lookup"><span data-stu-id="a37a0-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="a37a0-140">Ein `IViewLocalizer` Instanz eingefügt und zum Übersetzen des Texts "Hello World!" verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a37a0-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="a37a0-141">Erstellen einer bestellungsdatei</span><span class="sxs-lookup"><span data-stu-id="a37a0-141">Creating a PO file</span></span>

<span data-ttu-id="a37a0-142">Erstellen Sie eine Datei mit dem Namen  *<culture code>per* Stammordner für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a37a0-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="a37a0-143">In diesem Beispiel wird der Dateiname ist *fr.po* , weil die französische Sprache verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="a37a0-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="a37a0-144">Diese Datei speichert die Zeichenfolge übersetzen und die Zeichenfolge Französisch übersetzt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="a37a0-145">Übersetzungen, die für ihre übergeordnete Kultur zurückgesetzt werden, wenn erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a37a0-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="a37a0-146">In diesem Beispiel wird die *fr.po* Datei wird verwendet, wenn die angeforderte Kultur ist `fr-FR` oder `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="a37a0-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="a37a0-147">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="a37a0-147">Testing the application</span></span>

<span data-ttu-id="a37a0-148">Führen Sie die Anwendung, und navigieren Sie zu der URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="a37a0-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="a37a0-149">Der Text **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="a37a0-149">The text **Hello world!**</span></span> <span data-ttu-id="a37a0-150">wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-150">is displayed.</span></span>

<span data-ttu-id="a37a0-151">Navigieren Sie zu der URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="a37a0-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="a37a0-152">Der Text **Bonjour le Monde!**</span><span class="sxs-lookup"><span data-stu-id="a37a0-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="a37a0-153">wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="a37a0-154">Pluralisierung</span><span class="sxs-lookup"><span data-stu-id="a37a0-154">Pluralization</span></span>

<span data-ttu-id="a37a0-155">PO-Dateien unterstützen Pluralisierung Forms, das ist nützlich, wenn die gleiche Zeichenfolge übersetzt werden Weise basierend auf einer Kardinalität muss.</span><span class="sxs-lookup"><span data-stu-id="a37a0-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="a37a0-156">Diese Aufgabe wird durch die Tatsache komplizierter hergestellt, dass jede Sprache benutzerdefinierte Regeln zum Auswählen der definiert zu verwendende Zeichenfolge auf die Kardinalität basieren.</span><span class="sxs-lookup"><span data-stu-id="a37a0-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="a37a0-157">Das Paket Orchard Lokalisierung bietet eine API, um diese anderen Pluralform automatisch aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a37a0-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="a37a0-158">Erstellen Pluralisierung PO-Dateien</span><span class="sxs-lookup"><span data-stu-id="a37a0-158">Creating pluralization PO files</span></span>

<span data-ttu-id="a37a0-159">Fügen Sie den folgenden Inhalt, der zuvor erwähnten *fr.po* Datei:</span><span class="sxs-lookup"><span data-stu-id="a37a0-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="a37a0-160">Finden Sie unter [was eine PO-Datei ist?](#what-is-a-po-file) eine Erläuterung, was jeder Eintrag in diesem Beispiel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="a37a0-161">Hinzufügen einer Sprache mit verschiedenen Pluralisierung forms</span><span class="sxs-lookup"><span data-stu-id="a37a0-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="a37a0-162">Englisch und Französisch Zeichenfolgen, die im vorherigen Beispiel verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="a37a0-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="a37a0-163">Englisch und Französisch haben nur zwei Formen der Pluralisierung und teilen die gleichen Regeln für die Form, dass die erste Pluralform eine Kardinalität von eins zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="a37a0-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="a37a0-164">Alle anderen Kardinalität wird die zweite Pluralform zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a37a0-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="a37a0-165">Nicht in allen Sprachen gemeinsam die gleichen Regeln.</span><span class="sxs-lookup"><span data-stu-id="a37a0-165">Not all languages share the same rules.</span></span> <span data-ttu-id="a37a0-166">Dies wird mit der Sprache Tschechische veranschaulicht die drei Pluralform hat.</span><span class="sxs-lookup"><span data-stu-id="a37a0-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="a37a0-167">Erstellen der `cs.po` Datei wie folgt und beachten Sie, wie der Pluralisierung drei verschiedene Übersetzungen benötigt:</span><span class="sxs-lookup"><span data-stu-id="a37a0-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="a37a0-168">Für die Aufnahme Tschechische lokalisierten Versionen hinzufügen `"cs"` zur Liste der unterstützten Kulturen in die `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="a37a0-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="a37a0-169">Bearbeiten der *Views/Home/About.cshtml* Datei plural lokalisierte Zeichenfolgen für mehrere Kardinalitäten gerendert:</span><span class="sxs-lookup"><span data-stu-id="a37a0-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="a37a0-170">**Hinweis:** In einem realen Szenario, eine Variable wird verwendet, um die Anzahl darstellt.</span><span class="sxs-lookup"><span data-stu-id="a37a0-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="a37a0-171">Hier wiederholen wir den gleichen Code mit drei verschiedenen Werten, eine sehr speziellen Fall verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="a37a0-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="a37a0-172">Beim Wechsel Kulturen, sehen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a37a0-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="a37a0-173">Für `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="a37a0-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="a37a0-174">Für `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="a37a0-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="a37a0-175">Für `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="a37a0-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="a37a0-176">Beachten Sie, dass für die Tschechische Kultur, die drei Übersetzungen unterschiedlich sind.</span><span class="sxs-lookup"><span data-stu-id="a37a0-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="a37a0-177">Die Kulturen Französisch und Englisch teilen die gleiche Konstruktion für die beiden letzten übersetzten Zeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="a37a0-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="a37a0-178">Erweiterte Aufgaben</span><span class="sxs-lookup"><span data-stu-id="a37a0-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="a37a0-179">Contextualizing Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="a37a0-179">Contextualizing strings</span></span>

<span data-ttu-id="a37a0-180">Anwendungen enthalten häufig die Zeichenfolgen, die an mehreren Stellen übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="a37a0-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="a37a0-181">Dieselbe Zeichenfolge, die möglicherweise einer unterschiedlichen Übersetzung in bestimmte Speicherorte innerhalb einer app (Razor-Ansichten oder Klassendateien).</span><span class="sxs-lookup"><span data-stu-id="a37a0-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="a37a0-182">Eine Datei der Bestellung unterstützt das Konzept eines Kontexts Datei, die verwendet werden können, zum Kategorisieren von der Zeichenfolge dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a37a0-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="a37a0-183">Verwenden einen Kontext, kann eine Zeichenfolge unterschiedlich, je nach Kontext (oder mangelndes ein Kontext) übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="a37a0-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="a37a0-184">Die Bestellung Lokalisierung Dienste verwenden Sie den Namen der vollständigen Klasse oder die Sicht, die verwendet wird, wenn eine Zeichenfolge zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="a37a0-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="a37a0-185">Dies erfolgt durch Festlegen des Werts auf die `msgctxt` Eintrag.</span><span class="sxs-lookup"><span data-stu-id="a37a0-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="a37a0-186">Betrachten Sie eine kleinere zusätzlich zu den vorherigen *fr.po* Beispiel.</span><span class="sxs-lookup"><span data-stu-id="a37a0-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="a37a0-187">Eine Razor-Ansicht finden Sie unter *Views/Home/About.cshtml* kann als Dateikontext definiert werden, durch Festlegen der reservierten `msgctxt` Wert des Eintrags:</span><span class="sxs-lookup"><span data-stu-id="a37a0-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="a37a0-188">Mit der `msgctxt` textübersetzung als solche festgelegt wurde, tritt auf, wenn Navigieren zur `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="a37a0-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="a37a0-189">Die Übersetzung wird nicht ausgeführt, beim Navigieren zur `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="a37a0-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="a37a0-190">Wenn keine bestimmten Eintrag mit einem bestimmten Dateikontext abgeglichen wird, wird eine entsprechende PO-Datei ohne einen Kontext Orchard-Core-fallback-Mechanismus sucht.</span><span class="sxs-lookup"><span data-stu-id="a37a0-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="a37a0-191">Vorausgesetzt, dass keine bestimmte Dateikontext für definiert ist *Views/Home/Contact.cshtml*, navigieren Sie zu `/Home/Contact?culture=fr-FR` lädt Sie z. B. eine bestellungsdatei:</span><span class="sxs-lookup"><span data-stu-id="a37a0-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="a37a0-192">Ändern den Speicherort der PO-Dateien</span><span class="sxs-lookup"><span data-stu-id="a37a0-192">Changing the location of PO files</span></span>

<span data-ttu-id="a37a0-193">Der standardmäßige Speicherort der PO-Dateien kann geändert werden, `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a37a0-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="a37a0-194">In diesem Beispiel die PO-Dateien geladen werden, aus der *Lokalisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="a37a0-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="a37a0-195">Implementieren einen benutzerdefinierten Logik für die Suche nach Lokalisierungsdateien</span><span class="sxs-lookup"><span data-stu-id="a37a0-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="a37a0-196">Wenn komplexer Logik erforderlich ist, um die PO-Dateien, suchen die `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` Schnittstelle implementiert und als Dienst registriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a37a0-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="a37a0-197">Dies ist nützlich, wenn PO-Dateien an unterschiedlichen Speicherorten gespeichert werden können oder die Dateien müssen innerhalb einer Hierarchie von Ordnern gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="a37a0-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="a37a0-198">Verwenden einer anderen pluralisiert Standardsprache</span><span class="sxs-lookup"><span data-stu-id="a37a0-198">Using a different default pluralized language</span></span>

<span data-ttu-id="a37a0-199">Das Paket enthält eine `Plural` Erweiterungsmethode, die für zwei Pluralform spezifisch ist.</span><span class="sxs-lookup"><span data-stu-id="a37a0-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="a37a0-200">Erstellen Sie für Sprachen, die weitere Pluralform erfordern eine Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="a37a0-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="a37a0-201">Mit einer Erweiterungsmethode, müssen Sie keine Lokalisierungsdatei für die Standardsprache angeben &mdash; ursprünglichen Zeichenfolgen bereits direkt im Code verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="a37a0-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="a37a0-202">Sie können den stärker generische `Plural(int count, string[] pluralForms, params object[] arguments)` Überladung, die ein Array von Zeichenfolgen von Übersetzungen akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="a37a0-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
