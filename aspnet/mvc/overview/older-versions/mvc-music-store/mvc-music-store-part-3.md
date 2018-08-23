---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Teil 3: Ansichten und ViewModels | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 3 sind die Ansichten und ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832702"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="0eeaf-104">Teil 3: Ansichten und ViewModels</span><span class="sxs-lookup"><span data-stu-id="0eeaf-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="0eeaf-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0eeaf-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0eeaf-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0eeaf-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="0eeaf-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0eeaf-109">Teil 3 sind die Ansichten und ViewModels.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="0eeaf-110">Bisher haben wir nur Zeichenfolgen aus Controlleraktionen Zurückgeben von wurde.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="0eeaf-111">Das ist eine gute Möglichkeit, Sie erhalten eine Vorstellung der Funktionsweise von Controllern, aber es ist nicht wie Sie möchten eine echte Webanwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="0eeaf-112">Wir werden eine bessere Möglichkeit zum Generieren von HTML zurück zum Browser besuchen unsere Website möchten – ein, in dem wir Vorlagendateien verwenden können, um den HTML-Inhalt leichter anpassen, zurücksenden.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="0eeaf-113">Das ist genau das, was Ansichten tun.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="0eeaf-114">Hinzufügen einer Vorlage anzeigen</span><span class="sxs-lookup"><span data-stu-id="0eeaf-114">Adding a View template</span></span>

<span data-ttu-id="0eeaf-115">Um eine ansichtsvorlage zu verwenden, wir ändern die HomeController-Index-Methode, um ein Aktionsergebnis zurückgegeben und View(), wie unten zurückgegeben haben:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="0eeaf-116">Die obige Änderung gibt an, dass anstelle eine Zeichenfolge zurückgegeben, wir möchten stattdessen eine "Ansicht" verwenden, um ein Ergebnis zurück zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="0eeaf-117">Wir werden unser Projekt nun eine geeignete ansichtsvorlage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="0eeaf-118">Zu diesem Zweck wir positionieren Sie den Textcursor in den Index-Aktionsmethode, und klicken Sie dann mit der rechten Maustaste und wählen Sie "Ansicht hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="0eeaf-119">Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="0eeaf-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0eeaf-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="0eeaf-121">Das Dialogfeld "Ansicht hinzufügen" kann wir schnell und einfach Vorlagendateien für die Sicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="0eeaf-122">Standardmäßig ist die "Ansicht hinzufügen" füllt Dialogfeld den Namen der Vorlage anzeigen, erstellen, damit diese die Aktionsmethode entspricht, die dazu verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="0eeaf-123">Da wir im Kontextmenü "Ansicht hinzufügen" innerhalb der Aktionsmethode Index() von unserem HomeController verwendet, hat das Dialogfeld "Ansicht hinzufügen" oben "Index" wie der Ansichtsname, der standardmäßig aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="0eeaf-124">Wir müssen keine der Optionen in diesem Dialogfeld ändern möchten, klicken Sie daher auf die Schaltfläche "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="0eeaf-125">Wenn wir auf die Schaltfläche "hinzufügen" klicken, erstellt Visual Web Developer eine neue Datei "Index.cshtml" ansichtsvorlage für uns im Verzeichnis \Views\Home, erstellen den Ordner aus, wenn nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="0eeaf-126">Der Name und Ordner Speicherort der Datei "Index.cshtml" ist wichtig, und die standardmäßige ASP.NET-MVC-Namenskonventionen entspricht.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="0eeaf-127">Der Name des Verzeichnisses, \Views\Home, die Controller - mit dem HomeController Namen entspricht.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="0eeaf-128">Vorlage Ansichtsnamen Index entspricht der Aktionsmethode des Controllers, die Ansicht anzeigen lassen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="0eeaf-129">ASP.NET MVC ermöglicht uns, zu vermeiden, dass den Namen oder Speicherort, der eine ansichtsvorlage explizit angeben, wenn wir diese Namenskonvention verwenden, um eine Ansicht zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="0eeaf-130">Es werden standardmäßig die ansichtsvorlage \Views\Home\Index.cshtml gerendert, wenn wir in unserer HomeController z. B. folgenden Code schreiben:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="0eeaf-131">Visual Web Developer erstellt und die ansichtsvorlage "Index.cshtml" geöffnet, nachdem wir die Schaltfläche "Hinzufügen" im Dialogfeld "Ansicht hinzufügen" geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="0eeaf-132">Der Inhalt der Datei "Index.cshtml" werden unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="0eeaf-133">In dieser Ansicht wird die Razor-Syntax verwendet, die präziser als die Web Forms-Ansichts-Engine, die in ASP.NET Web Forms und früheren Versionen von ASP.NET MVC verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="0eeaf-134">Die Web Forms-Ansichts-Engine ist in ASP.NET MVC 3 weiterhin verfügbar, aber viele Entwickler finden Sie, dass die Razor-ansichtsengine ASP.NET MVC-Entwicklung wirklich gut geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="0eeaf-135">Die ersten drei Zeilen fest mit ViewBag.Title Seitentitel</span><span class="sxs-lookup"><span data-stu-id="0eeaf-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="0eeaf-136">Wir sehen Sie sich ausführlicher in Kürze, aber zuerst sehen wir Update Funktionsweise Text Überschrift und zeigen Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="0eeaf-137">Update der &lt;h2&gt; Tag "Diese ist die Homepage" zu sagen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="0eeaf-138">Ausführen von der Anwendung zeigt, dass der neue Text auf der Startseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="0eeaf-139">Verwenden ein Layout für allgemeine Website-Elemente</span><span class="sxs-lookup"><span data-stu-id="0eeaf-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="0eeaf-140">Die meisten Websites verfügen, Inhalt, der viele Seiten gemeinsam verwendet werden: Navigation, Fußzeilen, Logos, Stylesheet Verweise usw. Die Razor-ansichtsengine macht dies einfach zu verwalten, verwenden eine Seite mit dem \_Layout.cshtml, die im Ordner "/ Views/Shared" automatisch für uns erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="0eeaf-141">Doppelklicken Sie auf diesen Ordner zum Anzeigen des Inhalts, sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="0eeaf-142">Der Inhalt aus die einzelnen Ansichten angezeigt, auf die @RenderBody()-Befehl, und alle allgemeinen Inhalte, die wir außerhalb dieser angezeigt werden sollen können hinzugefügt werden die \_Layout.cshtml Markup.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="0eeaf-143">Sollten wir unsere MVC Music Store, um einen allgemeinen Header mit Links auf unserer Homepage und Store-Bereich auf allen Seiten der Website haben, damit wir, die für die Vorlage, die direkt über hinzufügen @RenderBody()-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="0eeaf-144">Das StyleSheet wird aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-144">Updating the StyleSheet</span></span>

<span data-ttu-id="0eeaf-145">Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei nur enthält Stile verwendet, um Nachrichten zur inhaltsprüfung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="0eeaf-146">Einige zusätzliche CSS- und Images um das Aussehen und Verhalten für unsere Website, zu definieren, damit wir diese jetzt hinzufügen, hat unsere Designer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="0eeaf-147">Die aktualisierte CSS-Datei und die Bilder befinden sich im Verzeichnis mit Inhalt der MvcMusicStore Assets.zip, verfügbar unter [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="0eeaf-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="0eeaf-148">Wir wählen beide im Windows-Explorer und legen Sie sie in unserer Lösung für den Ordner "Content" in Visual Web Developer, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="0eeaf-149">Sie werden aufgefordert zu bestätigen, dass die vorhandene Datei "Site.CSS" überschrieben werden soll.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="0eeaf-150">Klicken Sie auf "Ja".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="0eeaf-151">Der Ordner "Content" der Anwendung wird nun wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="0eeaf-152">Jetzt sehen wir die Anwendung auszuführen, und sehen, wie unsere Änderungen auf der Startseite aussieht.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="0eeaf-153">Betrachten Sie die Änderungen: die HomeController-Index-Aktionsmethode gefunden und die Vorlage \Views\Home\Index.cshtmlView angezeigt, obwohl unseres Codes "return View()" bezeichnet, da es sich bei unserer Vorlage anzeigen, die standardmäßige Namenskonvention folgen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="0eeaf-154">Auf der Startseite wird eine einfache Willkommensnachricht angezeigt, die die ansichtsvorlage \Views\Home\Index.cshtml definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="0eeaf-155">Auf der Startseite verwendet unsere \_Layout.cshtml-Vorlage, und daher die Willkommensnachricht in der Standardwebsite HTML-Layout enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="0eeaf-156">Ein Modell verwenden, um Informationen an unsere Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="0eeaf-157">Eine ansichtsvorlage, die hartcodierte HTML nur zeigt ist nicht die eine Website sehr interessant machen soll.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="0eeaf-158">Um eine dynamische Website erstellen, sollten wir stattdessen zum Übergeben von Informationen über unsere Controlleraktionen an unsere Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="0eeaf-159">Im Model-View-Controller-Muster Objekte der Begriff, dem bezieht sich das Modell auf, die die Daten in der Anwendung darstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="0eeaf-160">Häufig Modellobjekte Tabellen in der Datenbank entsprechen, aber sie müssen keine.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="0eeaf-161">Controlleraktionsmethoden, die ein ActionResult zurückgeben können einem Modellobjekt an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="0eeaf-162">Dies ermöglicht einem Controller sauber Paket alle Informationen erforderlich, um eine Antwort zu generieren und diese Informationen dann in eine ansichtsvorlage übergeben, mit die entsprechenden HTML-Antwort generiert.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="0eeaf-163">Dies ist am einfachsten zu verstehen, indem Sie diese in Aktion zu sehen, also lassen Sie uns loslegen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="0eeaf-164">Zunächst erstellen wir einige Modellklassen Genres und Alben innerhalb unseres neuen Speichers darstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="0eeaf-165">Wir erstellen zunächst eine Klasse "Genre".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="0eeaf-166">Mit der rechten Maustaste in des Ordners "Models" in Ihrem Projekt, wählen Sie die Option "Klasse hinzufügen", und nennen Sie die Datei "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="0eeaf-167">Fügen Sie eine public String Name-Eigenschaft klicken Sie dann auf die Klasse, die erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="0eeaf-168">*Hinweis: In Fällen, die Sie Fragen sich, den {get; festlegen;} Notation besteht darin, Verwendung der Funktion für # automatisch implementierten Eigenschaften. Dadurch erhalten wir die Vorteile einer Eigenschaft, ohne uns um ein dahinter liegendes Feld zu deklarieren.*</span><span class="sxs-lookup"><span data-stu-id="0eeaf-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="0eeaf-169">Als Nächstes führen Sie die gleichen Schritte aus, um ein Album-Klasse, die (mit dem Namen Album.cs) zu erstellen, die einen Titel und eine Eigenschaft "Genre" verfügt:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="0eeaf-170">Jetzt können wir durch Ändern der StoreController um Sichten zu verwenden, die dynamische Informationen von unserem Modell anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="0eeaf-171">Wenn – zu Demonstrationszwecken jetzt – wir unsere Alben basierend auf die Anforderungs-ID mit dem Namen, können wir diese Informationen, wie in der Ansicht unten darstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="0eeaf-172">Zunächst werden die Details der Store-Aktion ändern, sodass die Informationen für ein einzelnes Album angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="0eeaf-173">Fügen Sie eine "using"-Anweisung am Anfang der **StoreControllers** Klasse den Namespace MvcMusicStore.Models einbeziehen, daher müssen wir nicht MvcMusicStore.Models.Album Geben Sie jedes Mal, wenn die Album-Klasse verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="0eeaf-174">Im Abschnitt "Using-Direktiven" dieser Klasse sollte jetzt angezeigt werden wie folgt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="0eeaf-175">Als Nächstes aktualisieren die Controlleraktion Details wir damit, dass ein ActionResult anstelle einer Zeichenfolge zurückgegeben wie wir die HomeController-Index-Methode haben.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="0eeaf-176">Jetzt können wir die Logik zum Zurückgeben einer Album-Objekts an die Ansicht ändern.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="0eeaf-177">Weiter unten in diesem Tutorial wir wird werden zum Abrufen der Daten aus einer Datenbank – aber jetzt verwenden wir "Pseudoupdate von Daten" für den Einstieg.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="0eeaf-178">*Hinweis: Wenn Sie nicht mit c# vertraut sind, können Sie davon ausgehen, dass es sich bei Verwenden von Var bedeutet, dass die Variable Album spät gebunden ist. Das ist nicht richtig – c#-Compiler verwendet den Typrückschluss basierend auf was wir die Variable zuweisen, um zu bestimmen, dass der Typ Album jeweiligen Album zu sehen ist und die lokalen Album-Variable als einen Typ Album, kompilieren, sodass kompilierzeitüberprüfung und Visual Studio Code-Editors abrufen unterstützt.*</span><span class="sxs-lookup"><span data-stu-id="0eeaf-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="0eeaf-179">Nun erstellen wir eine Vorlage anzeigen, die unsere Album verwendet, um eine HTML-Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="0eeaf-180">Zunächst müssen wir das Projekt zu erstellen, damit das Dialogfeld "Ansicht hinzufügen" unserer Klasse der neu erstellten Album bekannt sind.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="0eeaf-181">Sie können das Projekt erstellen durch Auswählen der Debug⇨Build MvcMusicStore Menüelement (zusätzliche Anerkennung, können Sie die Tastenkombination STRG + UMSCHALT + B zum Erstellen des Projekts verwenden).</span><span class="sxs-lookup"><span data-stu-id="0eeaf-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="0eeaf-182">Nun, da wir unsere unterstützenden Klassen eingerichtet haben, können wir unsere Vorlage für die Ansicht zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="0eeaf-183">Mit der rechten Maustaste in die Details-Methode, und wählen Sie "Ansicht hinzufügen" aus dem Kontextmenü aus.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="0eeaf-184">Wir werden eine neue, wie zuvor mit dem HomeController anzeigen-Vorlage erstellen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="0eeaf-185">Da wir es aus der StoreController erstellen wird er standardmäßig in einer Datei \Views\Store\Index.cshtml erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="0eeaf-186">Im Gegensatz zu werden, wir das "Erstellen, die einen stark typisierten" Ansicht-Kontrollkästchen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="0eeaf-187">Dann werden wir unsere Klasse "Album" in der "Ansicht-Datenklasse" Drop-Downlist auswählen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="0eeaf-188">Dadurch wird das Dialogfeld "Ansicht hinzufügen" zum Erstellen einer Vorlage anzeigen, die von der erwartet, dass ein Album, die Objekt an ihn zur Verwendung übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="0eeaf-189">Beim Klicken auf die Schaltfläche "Hinzufügen" wird die Vorlage unserer \Views\Store\Details.cshtml anzeigen erstellt werden, mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="0eeaf-190">Beachten Sie, dass der ersten Zeile gibt an, dass diese Ansicht auf unsere Album-Klasse stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="0eeaf-191">Die Razor-Ansichts-Engine erkennt, dass es ein Album-Objekt, übergeben wurde also können wir ganz einfach Zugriff auf Eigenschaften des Modells und sogar den Vorteil von IntelliSense im Visual Web Developer-Editor haben.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="0eeaf-192">Update der &lt;h2&gt; markieren, damit es zeigt jedoch das Album des Title-Eigenschaft ändern diese Zeile folgendermaßen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="0eeaf-193">Beachten Sie, dass IntelliSense ausgelöst wird, wenn Sie den Zeitraum nach dem Eingeben der @Model -Schlüsselwort, mit den Eigenschaften und Methoden, die die Album-Klasse unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="0eeaf-194">Lassen Sie uns nun führen Sie das Projekt erneut aus und rufen Sie die URL der Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="0eeaf-195">Wir sehen die Details eines Albums wie unten.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="0eeaf-196">Stellen Sie jetzt eine ähnliche Aktualisierung für die Aktionsmethode Store durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="0eeaf-197">Aktualisieren Sie die Methode so, dass ein Aktionsergebnis zurückgegeben, und ändern Sie die Logik der Methode aus, damit es ein neues Objekt für "Genre erstellt" und gibt ihn an die Ansicht zurück.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="0eeaf-198">Mit der rechten Maustaste in der durchsuchen-Methode, und wählen "Ansicht hinzufügen" aus dem Kontextmenü, klicken Sie dann eine Ansicht, die stark typisierte Hinzufügen der Klasse "Genre" eine stark typisierte hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="0eeaf-199">Update der &lt;h2&gt; Element in der Ansicht code (in /Views/Store/Browse.cshtml) zum Anzeigen der Informationen für "Genre".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="0eeaf-200">Jetzt sehen wir führen Sie das Projekt erneut aus, und navigieren Sie zu dem/Store/durchsuchen? Genre = Disco-URL.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="0eeaf-201">Wir sehen, dass die Seite "Durchsuchen" wie unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="0eeaf-202">Abschließend soll ein etwas komplexes Update für die **Store Index** Aktionsmethode und können Sie eine Liste mit allen Genres in unseres neuen Speichers anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="0eeaf-203">Wir werden dies über eine Liste der Genres als das Modellobjekt, anstatt nur eine einzelne "Genre".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="0eeaf-204">Mit der rechten Maustaste in der Store-Index-Aktionsmethode, und wählen Sie die Ansicht hinzufügen, wie zuvor, wählen "Genre" als die Model-Klasse, und drücken Sie die Schaltfläche "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="0eeaf-205">Zuerst ändern wir die @model Deklaration, um anzugeben, dass die Ansicht, verschiedene "Genre" erwartet werden Objekte, anstatt nur eine.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="0eeaf-206">Ändern Sie die erste Zeile der /Store/Index.cshtml wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="0eeaf-207">Dadurch wird der Razor-ansichtsengine, die es einem Modellobjekt verwendet werden, die mehrere "Genre"-Objekte enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="0eeaf-208">Wir verwenden eine IEnumerable&lt;"Genre"&gt; anstelle einer Liste&lt;"Genre"&gt; , da es mehr generisch ist, ermöglicht uns, unsere Modelltyp später auf einen beliebigen Objekttyp ändern, die die IEnumerable-Schnittstelle unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="0eeaf-209">Als Nächstes müssen wir die Objekte "Genre" in das Modell, wie in den abgeschlossen-Code unten gezeigt durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="0eeaf-210">Beachten Sie, dass die vollständige IntelliSense-Unterstützung haben wir diesem Code eingeben, damit wir bei der Eingabe "@Model."</span><span class="sxs-lookup"><span data-stu-id="0eeaf-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="0eeaf-211">sehen Sie alle Methoden und Eigenschaften, die durch ein IEnumerable vom Typ "Genre" unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="0eeaf-212">In unserem "Foreach"-Schleife weiß Visual Web Developer an, dass jedes Element des Typs "Genre", sodass wir IntelliSense für die einzelnen finden Sie unter den Typ "Genre".</span><span class="sxs-lookup"><span data-stu-id="0eeaf-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="0eeaf-213">Als Nächstes gerüstfeature untersucht das Genre-Objekt und bestimmt, von denen jeder eine Name-Eigenschaft, wird damit durchläuft und Dateien schreibt. Es generiert auch bearbeiten, Details und Löschen von Links auf jedes einzelne Element.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="0eeaf-214">Werfen wir nutzen, die später in unsere Geschäftsführer, aber jetzt möchten wir eine einfache Liste stattdessen zu haben.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="0eeaf-215">Wenn wir die Anwendung auszuführen, und navigieren Sie zu/Store, sehen wir, dass die Anzahl und die Liste der Genres wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="0eeaf-216">Hinzufügen von Verknüpfungen zwischen Seiten</span><span class="sxs-lookup"><span data-stu-id="0eeaf-216">Adding Links between pages</span></span>

<span data-ttu-id="0eeaf-217">Unsere/Store-URL, die Genres derzeit listet Listet die "Genre"-Namen als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="0eeaf-218">Lassen Sie uns dies ändern, damit anstelle von nur-Text wir stattdessen den Link "Genre" Namen, auf die entsprechende URL Store/durchsuchen, haben und durch Klicken auf eine Musik "Genre" wie "Disco" zu der/Store/durchsuchen navigiert? Genre = Disco-URL.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="0eeaf-219">Wir konnten unsere \Views\Store\Index.cshtml ansichtsvorlage aktualisieren, Ausgabe, die diesen Links, mit wie unten Code **(nicht in Typ – wir werden diese verbessern)**:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="0eeaf-220">Dies funktioniert, jedoch kann dies zu einem späteren Zeitpunkt Problemen, da dabei auf eine hartcodierte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="0eeaf-221">Wenn wir den Controller umbenennen möchten, müssten wir z. B. durchsuchen Sie unser Code nach Links, die aktualisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="0eeaf-222">Eine alternative Methode, die wir verwenden können, ist eine HTML-Hilfsmethode nutzen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="0eeaf-223">ASP.NET MVC enthält HTML-Hilfsmethoden, die über unsere Vorlagencode anzeigen, um eine Vielzahl von Aufgaben wie folgt ausführen verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="0eeaf-224">Die Hilfsmethode Html.ActionLink() ist besonders nützlich, und erleichtert Ihnen die Erstellung von HTML &lt;eine&gt; verknüpft und übernimmt die lästigen Details wie das sicherstellen, dass URL-Pfade sind ordnungsgemäß URL-codiert.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="0eeaf-225">Html.ActionLink() verfügt über mehrere verschiedene Überladungen zur Angabe so viele Informationen, wie Sie für Ihre Links benötigen.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="0eeaf-226">Im einfachsten Fall müssen Sie angeben nur der Text des Links und die Aktionsmethode auf, wenn auf den Link geklickt wird, auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="0eeaf-227">Beispielsweise können wir mit "/ Store /" Index()-Methode auf der Seite "Store-Details" mit der Text des Links "Wechseln Sie zu der Store Index" mit dem folgenden Aufruf verknüpfen:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="0eeaf-228">*Hinweis: In diesem Fall nicht haben wir müssen den Namen des Controllers an, da wir nur zu einer anderen Aktion im selben Controller erstellen, die die aktuelle Ansicht rendert.*</span><span class="sxs-lookup"><span data-stu-id="0eeaf-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="0eeaf-229">Unsere Links auf der Seite "Durchsuchen" müssen jedoch einen Parameter übergeben, deshalb verwenden wir eine andere Überladung der Html.ActionLink-Methode, die drei Parameter akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="0eeaf-230">Text des Links, der den Namen "Genre" angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="0eeaf-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="0eeaf-231">Aktionsname "Controller" (Durchsuchen)</span><span class="sxs-lookup"><span data-stu-id="0eeaf-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="0eeaf-232">Routenparameterwerten, sowohl den Namen ("Genre") und den Wert ("Genre"-Name) angeben</span><span class="sxs-lookup"><span data-stu-id="0eeaf-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="0eeaf-233">Platzieren, dass alle zusammen sieht wie wir diese Links an den Store Index schreiben:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="0eeaf-234">Jetzt Wenn wir führen das Projekt erneut aus, und greifen Sie auf die URL /Store/ sehen wir eine Liste von Genres.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="0eeaf-235">Jedes Genre wird ein Link – beim Klicken auf dauert es uns, unsere/Store/durchsuchen? Genre =*["Genre"]* URL.</span><span class="sxs-lookup"><span data-stu-id="0eeaf-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="0eeaf-236">Der HTML-Code für die Liste "Genre" sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="0eeaf-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="0eeaf-237">[Zurück](mvc-music-store-part-2.md)
> [Weiter](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="0eeaf-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
