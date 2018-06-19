---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Teil 3: Ansichten und ViewModels | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 3 werden die Ansichten und ViewModels behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874849"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="47dd9-104">Teil 3: Ansichten und ViewModels</span><span class="sxs-lookup"><span data-stu-id="47dd9-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="47dd9-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="47dd9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="47dd9-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="47dd9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="47dd9-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="47dd9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="47dd9-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="47dd9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="47dd9-109">Teil 3 werden die Ansichten und ViewModels behandelt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="47dd9-110">Bisher haben wir nur Zeichenfolgen aus Controlleraktionen Zurückgeben von wurde.</span><span class="sxs-lookup"><span data-stu-id="47dd9-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="47dd9-111">Dies ist eine gute Möglichkeit, erhalten einen Überblick über die Funktionsweise von Controllern allerdings handelt es sich nicht wie Sie möchten eine echte Webanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="47dd9-112">Kegel zum eine bessere Möglichkeit zum Generieren von HTML zurück an den Browser besuchen Sie unsere Website soll – eines, in dem wir Vorlagendateien verwenden können, den HTML-Inhalt leichter anpassen, zurücksenden.</span><span class="sxs-lookup"><span data-stu-id="47dd9-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="47dd9-113">Das ist genau vorgehen Ansichten.</span><span class="sxs-lookup"><span data-stu-id="47dd9-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="47dd9-114">Hinzufügen einer Vorlage für eine Sicht</span><span class="sxs-lookup"><span data-stu-id="47dd9-114">Adding a View template</span></span>

<span data-ttu-id="47dd9-115">Um eine Ansicht-Vorlage verwenden, wir ändern die HomeController Index-Methode, um ein ActionResult zurückzugeben und View(), wie im folgenden zurückgeben lassen:</span><span class="sxs-lookup"><span data-stu-id="47dd9-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="47dd9-116">Die oben genannten Änderung gibt an, dass anstelle eine Zeichenfolge zurückgegeben, die wir stattdessen eine "View" zu verwenden, um ein Ergebnis zurück generieren möchten.</span><span class="sxs-lookup"><span data-stu-id="47dd9-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="47dd9-117">Jetzt müssen wir eine entsprechende Ansichtenvorlage unsere Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="47dd9-118">Zu diesem Zweck wir positionieren Sie den Textcursor innerhalb der Index-Aktionsmethode, und klicken Sie dann mit der rechten Maustaste fort und wählen "Ansicht hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="47dd9-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="47dd9-119">Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="47dd9-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="47dd9-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="47dd9-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="47dd9-121">Das Dialogfeld "Ansicht hinzufügen" kann wir schnell und einfach Vorlagendateien Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="47dd9-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="47dd9-122">"Ansicht hinzufügen" in der Standardeinstellung bereits im Vorfeld gefüllt Dialogfeld den Namen der Sicht Vorlage zum Erstellen, damit er die Aktionsmethode findet, die verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="47dd9-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="47dd9-123">Da wir die Aktionsmethode Index() von unseren HomeController im Kontextmenü "Ansicht hinzufügen" verwendet, hat das Dialogfeld "Ansicht hinzufügen" "Ressourcen" "Index", wie der Ansichtsname standardmäßig im Voraus ausgefüllt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="47dd9-124">Wir müssen eine der Optionen in diesem Dialogfeld ändern möchten, klicken Sie auf die Schaltfläche "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="47dd9-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="47dd9-125">Wenn wir die Schaltfläche "hinzufügen" klicken, erstellt Visual Web Developer ein neues Index.cshtml Vorlage anzeigen für uns im Verzeichnis \Views\Home, erstellen den Ordner aus, wenn nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="47dd9-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="47dd9-126">Der Name und Ordner Speicherort der Datei "Index.cshtml" ist wichtig und folgt die Standard-ASP.NET-MVC-Benennungskonventionen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="47dd9-127">Der Name des Verzeichnisses, \Views\Home, den Controller - mit dem Namen HomeController übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="47dd9-128">Vorlage Ansichtsnamen Index entspricht der Aktionsmethode des Controllers, die Ansicht anzeigen werden.</span><span class="sxs-lookup"><span data-stu-id="47dd9-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="47dd9-129">ASP.NET MVC erlaubt es uns zu vermeiden, dass den Namen oder Speicherort der Vorlage für eine Sicht explizit angeben, wenn wir diese Benennungskonvention verwenden, um eine Sicht zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="47dd9-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="47dd9-130">Standardmäßig wird die \Views\Home\Index.cshtml Ansichtenvorlage gerendert, wenn wir in unserer HomeController Code wie im folgenden schreiben:</span><span class="sxs-lookup"><span data-stu-id="47dd9-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="47dd9-131">Visual Web Developer erstellt und die View-Vorlage "Index.cshtml" geöffnet, nachdem es in das Dialogfeld "Ansicht hinzufügen" auf "Hinzufügen" geklickt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="47dd9-132">Der Inhalt des Index.cshtml werden unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="47dd9-133">In dieser Ansicht wird die Razor-Syntax, wobei präziser als die Web Forms-Ansichtsmodul in ASP.NET Web Forms- und früheren Versionen von ASP.NET MVC verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="47dd9-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="47dd9-134">Das WebForms-Anzeigemodul weiterhin in ASP.NET MVC 3 verfügbar ist, aber viele Entwickler gefunden werden, dass das Razor-Ansichtsmodul ASP.NET MVC-Entwicklung sehr gut geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="47dd9-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="47dd9-135">Die ersten drei Zeilen Festlegen der Seitenname ViewBag.Title verwenden.</span><span class="sxs-lookup"><span data-stu-id="47dd9-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="47dd9-136">Wir sehen Sie sich ausführlicher bald jedoch zunächst sehen wir Update Funktionsweise der Überschriftentext Text und die Seite anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="47dd9-137">Update der &lt;h2&gt; Tag "Diese ist die Startseite" sagen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="47dd9-138">Ausführen von die Anwendung zeigt, dass unsere neue Text auf der Startseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="47dd9-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="47dd9-139">Verwenden ein Layout für allgemeine-Websiteelementen</span><span class="sxs-lookup"><span data-stu-id="47dd9-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="47dd9-140">Die meisten Websites haben Inhalte, die viele Seiten gemeinsam verwendet werden: Navigation, Fußzeilen, Logos, Stylesheet Verweise usw. Das Razor-Ansichtsmodul ist ganz einfach, um die Verwaltung mithilfe einer Seite aufgerufen \_Layout.cshtml die/Ansichten/freigegeben Ordner automatisch für uns erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="47dd9-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="47dd9-141">Doppelklicken Sie auf diesen Ordner zum Anzeigen des Inhalts, sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="47dd9-142">Wird der Inhalt aus unserem einzelnen Ansichten angezeigt werden, durch die @RenderBodyBefehl (), und alle allgemeinen Inhalte, die wir außerhalb, die angezeigt werden sollen hinzugefügt werden die \_Layout.cshtml Markup.</span><span class="sxs-lookup"><span data-stu-id="47dd9-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="47dd9-143">Sollten wir unsere MVC Music Store, einen allgemeinen Header mit Links in unsere Startseite und Store Bereich auf allen Seiten der Website, damit wir, die an der Vorlage, die direkt über hinzufügen @RenderBody()-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="47dd9-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="47dd9-144">Aktualisieren das StyleSheet</span><span class="sxs-lookup"><span data-stu-id="47dd9-144">Updating the StyleSheet</span></span>

<span data-ttu-id="47dd9-145">Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die Stile, die zur Anzeige von validierungsmeldungen enthält nur.</span><span class="sxs-lookup"><span data-stu-id="47dd9-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="47dd9-146">Unsere Designer bereitgestellt hat, einige zusätzliche CSS und Bilder um das Aussehen und Verhalten für unsere Site zu definieren, damit wir diese jetzt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="47dd9-147">Die aktualisierte CSS-Datei und die Bilder befinden sich das Inhaltsverzeichnis der MvcMusicStore Assets.zip unter [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="47dd9-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="47dd9-148">Wir wählen beide Zertifikate im Windows-Explorer und löschen sie in der Projektmappe Ordners "Content" in Visual Web Developer, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="47dd9-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="47dd9-149">Sie werden aufgefordert, die zu überprüfen, ob die vorhandene Datei "Site.CSS" überschrieben werden soll.</span><span class="sxs-lookup"><span data-stu-id="47dd9-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="47dd9-150">Klicken Sie auf "Ja".</span><span class="sxs-lookup"><span data-stu-id="47dd9-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="47dd9-151">Die Ordners "Content" der Anwendung wird nun wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="47dd9-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="47dd9-152">Jetzt sehen wir die Anwendung auszuführen, und sehen, wie sich unsere Änderungen an der auf der Startseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="47dd9-153">Betrachten wir, was geändert wird: der HomeController Index Aktionsmethode gefunden, und die Vorlage \Views\Home\Index.cshtmlView angezeigt, obwohl getesteten Codes "return View()" bezeichnet, da unsere Vorlage Anzeigen der standardmäßige Benennungskonvention folgt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="47dd9-154">Auf der Startseite ist eine einfache Begrüßungsnachricht anzeigen, die innerhalb der \Views\Home\Index.cshtml Ansichtenvorlage definiert ist.</span><span class="sxs-lookup"><span data-stu-id="47dd9-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="47dd9-155">Auf der Startseite verwendeten unsere \_Layout.cshtml-Vorlage, und damit die Willkommensnachricht innerhalb der standardmäßigen Website HTML-Layout enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="47dd9-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="47dd9-156">Ein Modell verwenden, um Informationen an unsere Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="47dd9-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="47dd9-157">Eine Vorlage anzeigen, die hartcodierte HTML nur zeigt ist nicht im Begriff, eine Website besonders interessant machen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="47dd9-158">Um eine dynamische Website erstellen, sollten wir stattdessen Informationen aus unserer Controlleraktionen an unsere Ansichtsvorlagen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="47dd9-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="47dd9-159">In der Model-View-Controller-Muster Objekte der Begriff, dem Modell bezieht sich auf, die Daten in der Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="47dd9-160">Häufig Modellobjekte Tabellen in der Datenbank entsprechen, sie jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="47dd9-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="47dd9-161">Controlleraktionsmethoden ein Aktionsergebnis zurückgegeben können Model-Objekts an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="47dd9-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="47dd9-162">Dies ermöglicht einem Controller ordnungsgemäß Paket die Informationen, die erforderlich, um eine Antwort zu generieren, und übergeben dann diese Informationen in eine Vorlage anzeigen, mit die entsprechenden HTML-Antwort generiert.</span><span class="sxs-lookup"><span data-stu-id="47dd9-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="47dd9-163">Dies ist am einfachsten, Einblicke durch diese Aktion zu sehen, wir also beginnen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="47dd9-164">Erstellen Sie zunächst einige Modellklassen zur Darstellung von Genres und Alben in unserem Store.</span><span class="sxs-lookup"><span data-stu-id="47dd9-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="47dd9-165">Beginnen Sie durch Erstellen einer "Genre"-Klasse.</span><span class="sxs-lookup"><span data-stu-id="47dd9-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="47dd9-166">Mit der rechten Maustaste in des Ordners "Modelle" in Ihrem Projekt, wählen Sie die Option "Klasse hinzufügen", und nennen Sie die Datei "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="47dd9-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="47dd9-167">Fügen Sie eine öffentliche Zeichenfolge Name-Eigenschaft klicken Sie dann auf die Klasse, die erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="47dd9-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="47dd9-168">*Hinweis: In Fällen, die Sie Fragen sich, die {abrufen; festlegen;} Notation macht # automatisch implementierte Eigenschaften-Funktion. Dadurch erhalten Sie die Vorteile einer Eigenschaft, ohne dass uns eine dahinter liegende Feld nicht deklariert werden.*</span><span class="sxs-lookup"><span data-stu-id="47dd9-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="47dd9-169">Führen Sie anschließend die Schritten, um eine Album-Klasse, die (mit dem Namen Album.cs) zu erstellen, die einen Titel und eine Eigenschaft "Genre" aufweist:</span><span class="sxs-lookup"><span data-stu-id="47dd9-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="47dd9-170">Jetzt können wir die StoreController um Sichten zu verwenden, die dynamische Informationen aus unserem Modell anzeigen, ändern.</span><span class="sxs-lookup"><span data-stu-id="47dd9-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="47dd9-171">Wenn – zu Demonstrationszwecken Moment - wir unsere Alben basierend auf der Anforderungs-ID mit dem Namen, konnte die Informationen wie in der folgenden Ansicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="47dd9-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="47dd9-172">Wir beginnen mit der Aktion Store Details ändern, damit sie die Informationen für eine einzelnes Album zeigt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="47dd9-173">Fügen Sie eine "using-Anweisung am Anfang der **StoreControllers** Klasse den Namespace MvcMusicStore.Models einbeziehen, daher brauchen wir MvcMusicStore.Models.Album Geben Sie jedes Mal, wenn die Album-Klasse verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="47dd9-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="47dd9-174">Im Abschnitt "Using-Direktiven" dieser Klasse sollte jetzt angezeigt werden folgendermaßen aus.</span><span class="sxs-lookup"><span data-stu-id="47dd9-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="47dd9-175">Als Nächstes werden wir Controlleraktion Details aktualisieren, sodass ein Aktionsergebnis dar, sondern eine Zeichenfolge zurückgegeben wie wir mit der HomeController Index-Methode.</span><span class="sxs-lookup"><span data-stu-id="47dd9-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="47dd9-176">Jetzt können wir die Logik zum Zurückgeben eines Album-Objekts auf die Ansicht ändern.</span><span class="sxs-lookup"><span data-stu-id="47dd9-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="47dd9-177">Weiter unten in diesem Lernprogramm wird werden abgerufenen Daten aus einer Datenbank – für jetzt wir verwenden jedoch "dummy Daten" für den Einstieg.</span><span class="sxs-lookup"><span data-stu-id="47dd9-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="47dd9-178">*Hinweis: Wenn Sie nicht mit c# vertraut sind, können Sie voraussetzen, dass mit Var bedeutet, dass unsere Album Variable spät gebunden. Das ist nicht richtig – der C#-Compiler wird mithilfe von Typrückschluss basierend auf was wir zum Ermitteln des Typs Album Album wird der Variablen zuweisen und die lokale Album-Variable als einen Typ Album kompilieren, damit wir kompilierzeitüberprüfung und Visual Studio Code-Editors erhalten unterstützt.*</span><span class="sxs-lookup"><span data-stu-id="47dd9-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="47dd9-179">Jetzt erstellen wir eine Vorlage anzeigen, die unsere Album verwendet, um eine HTML-Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="47dd9-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="47dd9-180">Bevor wir dies tun müssen wir das Projekt zu erstellen, sodass das Dialogfeld "Ansicht hinzufügen" zu unseren neu erstellten Album Klasse erkennt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="47dd9-181">Sie können das Projekt erstellen durch Auswählen der Debug⇨Build MvcMusicStore Menüelement (für zusätzliche Guthaben können Sie die STRG-UMSCHALT-B-Verknüpfung zum Erstellen des Projekts verwenden).</span><span class="sxs-lookup"><span data-stu-id="47dd9-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="47dd9-182">Nachdem wir unsere unterstützenden Klassen eingerichtet haben, können wir unsere Ansicht-Vorlage erstellen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="47dd9-183">Mit der rechten Maustaste innerhalb der Methode Details, und wählen Sie "Ansicht hinzufügen"</span><span class="sxs-lookup"><span data-stu-id="47dd9-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="47dd9-184">aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="47dd9-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="47dd9-185">Kegel zum Erstellen einer neuen Ansichtenvorlage aus, wie wir mit HomeController hat sich nichts geändert.</span><span class="sxs-lookup"><span data-stu-id="47dd9-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="47dd9-186">Da wir es aus der StoreController erstellen wird er standardmäßig in einer Datei \Views\Store\Index.cshtml erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="47dd9-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="47dd9-187">Im Gegensatz zu werden, wir überprüfen Sie das Kontrollkästchen für "Erstellen, die einen stark typisierten" anzeigen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="47dd9-188">Dann werden wir unsere "Album"-Klasse in der Drop-Downlist von "-Datenklasse anzeigen" auswählen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="47dd9-189">Dadurch wird das Dialogfeld "Ansicht hinzufügen" erstellen eine Ansichtenvorlage, die der erwartet, dass ein Album Objekt hinzu, um verwenden übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="47dd9-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="47dd9-190">Beim Klicken auf die Schaltfläche "Hinzufügen" wird unsere \Views\Store\Details.cshtml Ansicht-Vorlage erstellt werden, mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="47dd9-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="47dd9-191">Beachten Sie die erste Zeile, gibt an, dass diese Ansicht Album-Klasse stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="47dd9-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="47dd9-192">Das Razor-Ansichtsmodul erkennt, dass es ein Album-Objekt übergeben wurde, damit wir können Zugriff auf einfache Weise die Modelleigenschaften und auch die Vorteile von IntelliSense im Visual Web Developer-Editor.</span><span class="sxs-lookup"><span data-stu-id="47dd9-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="47dd9-193">Update der &lt;h2&gt; kennzeichnen, damit es dem Album Title-Eigenschaft zeigt an, durch Ändern dieser Zeile wie folgt angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="47dd9-194">Beachten Sie, dass IntelliSense ausgelöst wird, wenn Sie den Zeitraum nach dem Eingeben der @Model -Schlüsselwort, zeigt die Eigenschaften und Methoden, die die Album-Klasse unterstützt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="47dd9-195">Wir nun unsere Projekt erneut ausführen und besuchen Sie diese Store/Informationen/5.</span><span class="sxs-lookup"><span data-stu-id="47dd9-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="47dd9-196">Wir sehen die Details der ein Album wie unten.</span><span class="sxs-lookup"><span data-stu-id="47dd9-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="47dd9-197">Jetzt stellen wir eine ähnliche Aktualisierung für die Aktionsmethode Store durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="47dd9-198">Aktualisieren Sie die Methode, sodass ein Aktionsergebnis zurückgegeben, und ändern Sie die Logik der Methode, sodass er ein neues Objekt für "Genre erstellt" und gibt ihn an die Sicht zurück.</span><span class="sxs-lookup"><span data-stu-id="47dd9-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="47dd9-199">Mit der rechten Maustaste in der Methode zum Durchsuchen, und wählen Sie "Ansicht hinzufügen"</span><span class="sxs-lookup"><span data-stu-id="47dd9-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="47dd9-200">Klicken Sie im Kontextmenü, fügen Sie dann eine Ansicht, stark typisiert ist, eine stark typisierte der "Genre" Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="47dd9-201">Update der &lt;h2&gt; Element in der Ansicht code (im /Views/Store/Browse.cshtml), um "Genre" Informationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="47dd9-202">Jetzt sehen wir führen Sie das Projekt erneut aus, und navigieren Sie zu der/Store/durchsuchen? "Genre" = Disco-URL.</span><span class="sxs-lookup"><span data-stu-id="47dd9-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="47dd9-203">Wir sehen, dass die Seite "Durchsuchen" wie unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="47dd9-204">Zum Schluss stellen ein etwas komplexes Update für die **Index speichern** Aktionsmethode und können Sie eine Liste mit allen Genres in unserem Shop anzeigen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="47dd9-205">Wir müssen zu diesem Zweck verwenden eine Liste von Genres als unsere Modellobjekt, anstatt nur einen einzelnen "Genre".</span><span class="sxs-lookup"><span data-stu-id="47dd9-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="47dd9-206">Mit der rechten Maustaste in den Columnstore-Index-Aktionsmethode, und wählen Sie die Ansicht hinzufügen, wie zuvor, wählen "Genre" als der Skriptobjektmodell-Klasse, und drücken Sie die Schaltfläche "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="47dd9-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="47dd9-207">Ändern wir zunächst die @model Deklaration, um anzugeben, dass die Sicht mehrere "Genre" erwartet werden, werden Objekte, anstatt nur einen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="47dd9-208">Ändern Sie die erste Zeile der /Store/Index.cshtml wie folgt:</span><span class="sxs-lookup"><span data-stu-id="47dd9-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="47dd9-209">Dies weist das Razor-Ansichtsmodul aus, dem sie einem Modellobjekt arbeiten, die mehrere "Genre"-Objekte enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="47dd9-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="47dd9-210">Wir verwenden eine IEnumerable&lt;"Genre"&gt; anstelle einer Liste&lt;"Genre"&gt; , da es mehr generisch ist, sodass wir unsere Modelltyp später auf einen beliebigen Objekttyp zu ändern, die IEnumerable-Schnittstelle unterstützt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="47dd9-211">Als Nächstes müssen wir die "Genre"-Objekte im Modell wie im Ansichtscode unten abgeschlossenen dargestellt durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="47dd9-212">Beachten Sie, dass wir vollständige IntelliSense-Unterstützung haben wir diesen Code eingeben, damit es bei der Eingabe "@Model."</span><span class="sxs-lookup"><span data-stu-id="47dd9-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="47dd9-213">sehen Sie alle Methoden und Eigenschaften von IEnumerable vom Typ "Genre" unterstützt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="47dd9-214">Innerhalb der Schleife "Foreach" weiß Visual Web Developer an, dass jedes Element vom Typ "Genre" ist, damit wir IntelliSense für die einzelnen finden Sie unter den Typ "Genre".</span><span class="sxs-lookup"><span data-stu-id="47dd9-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="47dd9-215">Als Nächstes die Gerüstbau-Funktion überprüft das Objekt "Genre" und ermittelt, dass jede eine Name-Eigenschaft verfügen wird, damit durchläuft und Dateien schreibt. Es generiert auch bearbeiten, Details und Löschen von Verknüpfungen auf jedes einzelne Element.</span><span class="sxs-lookup"><span data-stu-id="47dd9-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="47dd9-216">Wir führen, die weiter unten in unsere Speicher-Manager nutzen, aber vorläufig wir hätten gerne eine einfache Liste stattdessen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="47dd9-217">Wenn wir führen Sie die Anwendung, und navigieren Sie zu/Store, sehen wir, dass die Anzahl und die Liste von Genres wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="47dd9-218">Hinzufügen von Verknüpfungen zwischen Seiten</span><span class="sxs-lookup"><span data-stu-id="47dd9-218">Adding Links between pages</span></span>

<span data-ttu-id="47dd9-219">Unsere/Store-URL, die derzeit Genres listet führt die "Genre"-Namen einfach als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="47dd9-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="47dd9-220">Wir ändern, damit anstelle von nur-Text wir stattdessen die "Genre" Namen Link zur entsprechenden Store/durchsuchen-URL enthalten, damit durch Klicken auf einen Musik "Genre" wie "Disco" / Store/durchsuchen navigieren soll? "Genre" = Disco-URL.</span><span class="sxs-lookup"><span data-stu-id="47dd9-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="47dd9-221">Wir konnten unseren \Views\Store\Index.cshtml Ansichtenvorlage Ausgabe, die diese Links mit wie unten Code aktualisiert **(Geben Sie hierzu finden Sie unter: Wir werden darauf zu verbessern)**:</span><span class="sxs-lookup"><span data-stu-id="47dd9-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="47dd9-222">Dies funktioniert, aber dies könnte zu später behandeln, da er auf eine hartcodierte Zeichenfolge angewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="47dd9-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="47dd9-223">Wenn wir, benennen Sie den Controller möchten, müssten wir z. B. durchsuchen getesteten Codes, die Suche nach Links, die aktualisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="47dd9-224">Eine alternative Methode, die verwendet werden können, besteht darin ein HTML-Hilfsmethode nutzen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="47dd9-225">ASP.NET MVC umfasst HTML-Hilfsmethoden aus unserem Vorlagencode Ansicht zum Ausführen einer Vielzahl von allgemeinen Aufgaben wie folgt zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="47dd9-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="47dd9-226">Die Hilfsmethode Html.ActionLink() ist eine besonders nützlich, und erleichtert das Erstellen von HTML &lt;ein&gt; verknüpft und übernimmt die ärgerlich Details, wie Sie sicherstellen, dass URL-Pfade sind ordnungsgemäß URL-codiert.</span><span class="sxs-lookup"><span data-stu-id="47dd9-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="47dd9-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span><span class="sxs-lookup"><span data-stu-id="47dd9-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="47dd9-228">Im einfachsten Fall müssen Sie angeben, nur der Text des Links und die Aktionsmethode soll, wenn auf dem Client auf der Link geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="47dd9-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="47dd9-229">Beispielsweise können wir mit "/ Store /" Index()-Methode auf der Detailseite für Speicher mit der Text des Links "Wechseln Sie zu der Columnstore-Index" mithilfe des folgenden Aufrufs verknüpfen:</span><span class="sxs-lookup"><span data-stu-id="47dd9-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="47dd9-230">*Hinweis: In diesem Fall haben nicht wir müssen den Controllernamen angeben, da es nur an eine andere Aktion in den gleichen Controller verknüpfen, die die aktuelle Ansicht gerendert wird.*</span><span class="sxs-lookup"><span data-stu-id="47dd9-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="47dd9-231">Unsere Links auf der Seite "Durchsuchen" müssen jedoch einen Parameter zu übergeben, damit wir eine andere Überladung der Methode Html.ActionLink verwenden, die drei Parameter akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="47dd9-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="47dd9-232">Text des Links, der den Namen "Genre" angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="47dd9-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="47dd9-233">Aktionsname "Controller" (Durchsuchen)</span><span class="sxs-lookup"><span data-stu-id="47dd9-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="47dd9-234">Routenparameterwerten angeben den Namen ("Genre") und der Wert ("Genre"-Name)</span><span class="sxs-lookup"><span data-stu-id="47dd9-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="47dd9-235">Platzieren, dass alle zusammen sieht wie wir diese Links in die Columnstore-Index Ansicht schreiben müssen:</span><span class="sxs-lookup"><span data-stu-id="47dd9-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="47dd9-236">Wenn wir unsere Projekt erneut auszuführen, und greifen auf die URL /Store/ wird wir nun eine Liste von Genres angezeigt.</span><span class="sxs-lookup"><span data-stu-id="47dd9-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="47dd9-237">Jedes Genres steht ein Hyperlink – aus, wenn geklickt dauert es uns, unsere/Store/durchsuchen? "Genre" =*["Genre"]* URL.</span><span class="sxs-lookup"><span data-stu-id="47dd9-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="47dd9-238">Der HTML-Code für die Liste "Genre" sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="47dd9-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="47dd9-239">[Zurück](mvc-music-store-part-2.md)
> [Weiter](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="47dd9-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
