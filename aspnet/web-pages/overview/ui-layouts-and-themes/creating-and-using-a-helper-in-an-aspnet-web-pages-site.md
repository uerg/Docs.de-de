---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Erstellen und Verwenden eines Hilfsprogramms in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu erstellen. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup Perf enthält...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: c2edc10e01f4d2358dd0b0bdf450348f01eb2bfa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811898"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4062d-104">Erstellen und Verwenden eines Hilfsprogramms in einer ASP.NET Web Pages (Razor)-Website</span><span class="sxs-lookup"><span data-stu-id="4062d-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="4062d-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4062d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4062d-106">Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4062d-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="4062d-107">Ein *Helper* ist eine wiederverwendbare Komponente, die enthält Code und Markup zum Ausführen einer Aufgabe, die mühsam oder komplex sein können.</span><span class="sxs-lookup"><span data-stu-id="4062d-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="4062d-108">**Sie lernen Folgendes:**</span><span class="sxs-lookup"><span data-stu-id="4062d-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="4062d-109">Informationen zum Erstellen und verwenden Sie eine einfache Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="4062d-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="4062d-110">Dies sind die Funktionen von ASP.NET in diesem Artikel:</span><span class="sxs-lookup"><span data-stu-id="4062d-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="4062d-111">Die `@helper` Syntax.</span><span class="sxs-lookup"><span data-stu-id="4062d-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4062d-112">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4062d-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4062d-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="4062d-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="4062d-114">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="4062d-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="4062d-115">Übersicht über die Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="4062d-115">Overview of Helpers</span></span>

<span data-ttu-id="4062d-116">Wenn Sie die gleichen Aufgaben auf verschiedenen Seiten auf Ihrer Website ausführen möchten, können Sie eine Hilfsprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="4062d-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="4062d-117">ASP.NET Web Pages umfasst eine Reihe von Hilfsprogrammen, und es gibt jedoch zahlreiche weitere, die Sie herunterladen und installieren können.</span><span class="sxs-lookup"><span data-stu-id="4062d-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="4062d-118">(Eine Liste der integrierten Hilfsprogramme in ASP.NET Web Pages finden Sie der [ASP.NET-API-Kurzübersicht](https://go.microsoft.com/fwlink/?LinkId=202907).) Wenn keine der vorhandenen Hilfsprogramme Ihre Anforderungen erfüllt, können Sie Ihre eigenen Hilfsprogramm erstellen.</span><span class="sxs-lookup"><span data-stu-id="4062d-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="4062d-119">Eine Hilfsprogramm können Sie einen allgemeinen Block von Code über mehrere Seiten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="4062d-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="4062d-120">Nehmen wir an, dass auf Ihrer Seite häufig ein Hinweis-Element erstellen möchten, die abgesehen von der normale Absätze festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="4062d-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="4062d-121">Vielleicht wird als Hinweis erstellt eine `<div>` Element, das formatiert wurde, wird als Feld mit einem Rahmen.</span><span class="sxs-lookup"><span data-stu-id="4062d-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="4062d-122">Anstatt diese dasselbe Markup zu einer Seite hinzufügen, jedes Mal, wenn Sie sich anzeigen möchten, können Sie das Markup als Hilfsprogramm packen.</span><span class="sxs-lookup"><span data-stu-id="4062d-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="4062d-123">Sie können anschließend den Hinweis mit einer einzelnen Codezeile einfügen an einer beliebigen Stelle Sie ihn benötigen.</span><span class="sxs-lookup"><span data-stu-id="4062d-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="4062d-124">Verwenden eines Hilfsprogramms wie folgt, wird der Code in den einzelnen Seiten einfacher und leichter zu lesen.</span><span class="sxs-lookup"><span data-stu-id="4062d-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="4062d-125">Es erleichtert auch Wartung Ihres Standorts, da Sie bei Bedarf ändern, wie die Anmerkungen zu dieser zu suchen, das Markup an einem Ort ändern können.</span><span class="sxs-lookup"><span data-stu-id="4062d-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="4062d-126">Erstellen eines Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="4062d-126">Creating a Helper</span></span>

<span data-ttu-id="4062d-127">Dieses Verfahren zeigt, wie Sie das Hilfsprogramm zu erstellen, das die Beachten Sie, wie gerade beschrieben erstellt.</span><span class="sxs-lookup"><span data-stu-id="4062d-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="4062d-128">Dies ist ein einfaches Beispiel, aber die benutzerdefinierten Hilfsmethoden zählen alle Markups und ASP-Code, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="4062d-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="4062d-129">Erstellen Sie im Stammordner der Website, einen Ordner namens *App\_Code*.</span><span class="sxs-lookup"><span data-stu-id="4062d-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="4062d-130">Dies ist ein reservierter Ordnername in ASP.NET können Code für Komponenten wie Hilfsprogramme ordnen Sie.</span><span class="sxs-lookup"><span data-stu-id="4062d-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="4062d-131">In der *App\_Code* Ordner erstellen Sie ein neues *.cshtml* Datei und nennen Sie sie *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4062d-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="4062d-132">Ersetzen Sie den vorhandenen Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="4062d-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="4062d-133">Der Code verwendet die `@helper` Syntax zum Deklarieren Sie einer neuen Hilfsprogramms, mit dem Namen `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="4062d-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="4062d-134">Diese bestimmte-Hilfsobjekt ermöglicht Ihnen das Übergeben eines Parameters namens `content` , die eine Kombination von Text und Markup enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="4062d-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="4062d-135">Das Hilfsprogramm fügt die Zeichenfolge in die Hinweis-Text mithilfe der `@content` Variable.</span><span class="sxs-lookup"><span data-stu-id="4062d-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="4062d-136">Beachten Sie, dass die Datei heißt *MyHelpers.cshtml*, aber das Hilfsprogramm wird mit dem Namen `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="4062d-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="4062d-137">Sie können mehrere benutzerdefinierte Hilfen in einer einzelnen Datei einfügen.</span><span class="sxs-lookup"><span data-stu-id="4062d-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="4062d-138">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="4062d-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="4062d-139">Unter Verwendung des Hilfsprogramms auf einer Seite</span><span class="sxs-lookup"><span data-stu-id="4062d-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="4062d-140">In den Stammordner, erstellen Sie eine neue leere Datei namens *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4062d-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="4062d-141">Fügen Sie folgenden Code in die Datei ein:</span><span class="sxs-lookup"><span data-stu-id="4062d-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="4062d-142">Verwenden Sie zum Aufrufen der Hilfsmethode Erstellung `@` gefolgt von den Dateinamen, in dem das Hilfsprogramm ist, einen Punkt, und klicken Sie dann den Hilfsprogramm-Namen.</span><span class="sxs-lookup"><span data-stu-id="4062d-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="4062d-143">(Wenn Sie mehrere Ordner, in haben der *App\_Code* Ordner können Sie die Syntax `@FolderName.FileName.HelperName` aufrufen, die Hilfsmethode in einer beliebigen geschachtelte Ordner der obersten Ebene).</span><span class="sxs-lookup"><span data-stu-id="4062d-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="4062d-144">Der Text, den Sie in Anführungszeichen innerhalb der Klammern hinzufügen, ist der Text, den das Hilfsprogramm als Teil der Notiz auf der Webseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="4062d-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="4062d-145">Speichern Sie die Seite, und führen Sie sie in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="4062d-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="4062d-146">Das Hilfsprogramm generiert das Hinweis Element direkt, wenn Sie das Hilfsprogramm aufgerufen: zwischen den beiden Absätze tauschen.</span><span class="sxs-lookup"><span data-stu-id="4062d-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Screenshot die Seite im Browser, und wie das Hilfsprogramm Markup generiert, die einen Rahmen um den angegebenen Text wird angezeigt.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="4062d-148">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4062d-148">Additional Resources</span></span>


<span data-ttu-id="4062d-149">[Horizontales Menü als ein Razor-Hilfsprogramm](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="4062d-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="4062d-150">In diesem Blogeintrag von Mike Pope veranschaulicht, wie ein horizontales Menü als ein Hilfsprogramm über Markup, CSS und Code zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4062d-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="4062d-151">[Nutzen HTML5 in der ASP.NET Web Helpers für WebMatrix und ASP.NET MVC3 Seiten](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="4062d-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="4062d-152">In diesem Blogeintrag von Sam Abraham zeigt eine Hilfsprogramm, das ein HTML5 rendert `Canvas` Element.</span><span class="sxs-lookup"><span data-stu-id="4062d-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="4062d-153">[Der Unterschied zwischen @Helpers und @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="4062d-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="4062d-154">In diesem Blogeintrag von Mike Brind beschreibt `@helper` Syntax und `@function` Syntax und wann Sie jeweils zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="4062d-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
