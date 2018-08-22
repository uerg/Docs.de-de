---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Teil 10: Abschließende Updates für die Navigation und das Websitedesign – Zusammenfassung | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 10 sind die abschließende Updates für die Navigation und S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f32509701dd112053aa4f31d6552601f961c7413
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831490"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="78600-104">Teil 10: Abschließende Updates für die Navigation und das Websitedesign – Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="78600-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="78600-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="78600-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="78600-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="78600-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="78600-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="78600-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="78600-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="78600-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="78600-109">Teil 10 sind die abschließende Updates für die Navigation und das Websitedesign – Zusammenfassung.</span><span class="sxs-lookup"><span data-stu-id="78600-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="78600-110">Wir haben alle wichtige Funktionen für unsere Website abgeschlossen, aber es gibt noch einige Funktionen der Website-Navigation, auf der Startseite und der Store durchsuchen Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="78600-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="78600-111">Erstellen die Shopping Cart teilweise Zusammenfassungsansicht</span><span class="sxs-lookup"><span data-stu-id="78600-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="78600-112">Die Anzahl der Elemente im Einkaufswagen des Benutzers für die gesamte Website verfügbar gemacht werden soll.</span><span class="sxs-lookup"><span data-stu-id="78600-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="78600-113">Wir können einfach implementieren dies durch das Erstellen einer Teilansicht der unsere Site.master hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="78600-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="78600-114">Wie zuvor gezeigt, enthält die ShoppingCart-Controller eine CartSummary Aktionsmethode die Teilansicht zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="78600-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="78600-115">Klicken Sie zum Erstellen der Teilansicht CartSummary mit der rechten Maustaste auf den Ordner Views/ShoppingCart, und wählen Sie die Ansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="78600-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="78600-116">Benennen Sie die Ansicht CartSummary, und aktivieren Sie das Kontrollkästchen "Eine Teilansicht erstellen" aus, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="78600-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="78600-117">Die Teilansicht CartSummary ist ganz einfach – es ist nur ein Link zur der die Anzahl der Elemente zusammen in den ShoppingCart Indexansicht.</span><span class="sxs-lookup"><span data-stu-id="78600-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="78600-118">Der vollständige Code für CartSummary.cshtml lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="78600-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="78600-119">Wir können eine Teilansicht in einer beliebigen Seite in der Website, einschließlich des Masters Standort mithilfe der Methode Html.RenderAction einschließen.</span><span class="sxs-lookup"><span data-stu-id="78600-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="78600-120">RenderAction muss der Name der Aktion ("CartSummary") und des Controllernamens ("ShoppingCart") wie unten angegeben.</span><span class="sxs-lookup"><span data-stu-id="78600-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="78600-121">Vor dem Hinzufügen des Standorts Layout, auch erstellen im Menü "Genre" wir damit wir alle unsere Site.master-Updates auf einmal erstellen können.</span><span class="sxs-lookup"><span data-stu-id="78600-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="78600-122">Erstellen die Teilansicht für "Genre" im Menü</span><span class="sxs-lookup"><span data-stu-id="78600-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="78600-123">Wir können machen es viel einfacher für unsere Benutzer, über den Store zu navigieren, durch das Hinzufügen einer "Genre" im Menü der alle Genres verfügbar in unseres neuen Speichers aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="78600-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="78600-124">Wir befolgen die gleichen Schritte auch eine Teilansicht GenreMenu erstellen, und klicken Sie dann wir können hinzufügen beide mit dem übergeordneten Standort.</span><span class="sxs-lookup"><span data-stu-id="78600-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="78600-125">Fügen Sie zunächst die folgende GenreMenu-Controlleraktion, die StoreController:</span><span class="sxs-lookup"><span data-stu-id="78600-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="78600-126">Diese Aktion gibt eine Liste der Genres, die von der Teilansicht, angezeigt wird, welche als Nächstes erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="78600-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="78600-127">*Hinweis: Wir haben das Attribut [ChildActionOnly] an diese Controlleraktion hinzugefügt gibt an, dass nur diese Aktion aus einer Teilansicht verwendet werden soll. Dieses Attribut wird verhindert, dass die Controlleraktion, die durch Navigieren zu /Store/GenreMenu ausgeführt werden. Dies ist nicht erforderlich für Teilansichten, aber es hat sich bewährt, da Sie sicherstellen, dass unsere Controlleraktionen wie geplant verwendet werden soll. Wir sind auch zurück PartialView statt anzeigen, können Sie die Ansichts-Engine, wissen, dass das Layout sollten nicht für diese Ansicht verwendet werden, wie es in den anderen Ansichten eingefügt wird.*</span><span class="sxs-lookup"><span data-stu-id="78600-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="78600-128">Mit der rechten Maustaste auf die GenreMenu Controlleraktion aus, und erstellen Sie eine Teilansicht mit dem Namen GenreMenu die stark typisiert ist, verwenden die Datenklasse "Genre" anzeigen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="78600-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="78600-129">Aktualisieren Sie den Code anzeigen, für die GenreMenu Teilansicht zum Anzeigen von Elementen, die eine unsortierte Liste wie folgt mit.</span><span class="sxs-lookup"><span data-stu-id="78600-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="78600-130">Layout der Website auf unsere Teilansichten aktualisieren</span><span class="sxs-lookup"><span data-stu-id="78600-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="78600-131">Wir können unsere Teilansichten hinzufügen, um das Layout der Website (/Ansichten/Shared/\_Layout.cshtml) durch Aufrufen von Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="78600-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="78600-132">Wir fügen sie beide an sowie einige zusätzliche Markup angezeigt, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="78600-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="78600-133">Wenn die Anwendung ausgeführt wird, werden wir das Genre im linken Navigationsbereich, und die Zusammenfassung der Warenkorb am oberen sehen.</span><span class="sxs-lookup"><span data-stu-id="78600-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="78600-134">Aktualisieren Sie auf der Seite "Store durchsuchen"</span><span class="sxs-lookup"><span data-stu-id="78600-134">Update to the Store Browse page</span></span>

<span data-ttu-id="78600-135">Die Seite Store durchsuchen ist funktionsfähig, aber nicht sehr gut aussehen.</span><span class="sxs-lookup"><span data-stu-id="78600-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="78600-136">Wir aktualisieren können Sie die Seite, um die Alben in einem besseren Layout anzeigen, indem Sie den Anzeigecode (gefunden in /Views/Store/Browse.cshtml) wie folgt aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="78600-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="78600-137">Wir erstellen hier Url.Action statt Html.ActionLink verwenden werden, sodass wir die spezielle Formatierung auf den Link, um die Grafik Album enthalten anwenden können.</span><span class="sxs-lookup"><span data-stu-id="78600-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="78600-138">*Hinweis: Wir werden eine generische Albumtitel für diese Alben angezeigt. Diese Informationen werden in der Datenbank gespeichert und kann über den Store Manager bearbeitet werden. Sie sind Willkommen, Ihre eigene Grafik hinzufügen.*</span><span class="sxs-lookup"><span data-stu-id="78600-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="78600-139">Nun werden wir ein Genre durchsuchen können, wir die Alben, die in einem Raster mit dem Album Bildmaterial dargestellt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78600-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="78600-140">Aktualisieren die Homepage zum Anzeigen von Selling Alben oben</span><span class="sxs-lookup"><span data-stu-id="78600-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="78600-141">Wir möchten unsere meistverkaufte Alben auf der Startseite auf den Umsatz steigern, feature.</span><span class="sxs-lookup"><span data-stu-id="78600-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="78600-142">Wir stellen einige Updates zu unseren HomeController zu behandeln, die aus, und auch einige zusätzlichen Grafiken hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="78600-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="78600-143">Zunächst fügen wir eine Navigationseigenschaft auf unsere Album-Klasse, sodass EntityFramework weiß, dass sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="78600-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="78600-144">Die letzten Zeilen der unsere **Album** Klasse sollte jetzt wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="78600-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="78600-145">*Hinweis: Dies benötigen Sie eine using-Anweisung im System.Collections.Generic-Namespace zu importieren.*</span><span class="sxs-lookup"><span data-stu-id="78600-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="78600-146">Zunächst fügen wir ein StoreDB-Feld und die using-Anweisungen, wie unsere anderen Controller MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="78600-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="78600-147">Als Nächstes fügen die folgende Methode HomeController wir die unsere Datenbank zum Suchen von Top-selling Alben gemäß OrderDetails abfragt.</span><span class="sxs-lookup"><span data-stu-id="78600-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="78600-148">Dies ist eine private Methode, da wir nicht, als eine Controlleraktion verfügbar zu machen möchten.</span><span class="sxs-lookup"><span data-stu-id="78600-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="78600-149">Wir werden es im HomeController aus Gründen der Einfachheit einschließlich, aber es wird empfohlen, die Ihre Geschäftslogik in separaten Dienstklassen nach Bedarf zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="78600-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="78600-150">Vorhanden, können wir die Index-Controlleraktion zum Abfragen von den Top 5 mit dem Verkauf von Alben, und gibt sie zurück zur Ansicht aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="78600-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="78600-151">Der vollständige Code für die aktualisierte HomeController wird wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="78600-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="78600-152">Abschließend müssen wir unsere Ansicht "Start Index" zu aktualisieren, sodass sie eine Liste der Alben anzeigen kann, durch den Typ des Modells aktualisieren und im unteren Bereich die Albumliste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="78600-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="78600-153">Wir übernehmen die Gelegenheit, um auch eine Überschrift und eine heraufstufung-Abschnitt der Seite hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="78600-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="78600-154">Nun, wenn wir die Anwendung ausführen, sehen wir unsere aktualisierte Startseite mit Top von selling Alben und unsere Werbe-Meldung.</span><span class="sxs-lookup"><span data-stu-id="78600-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="78600-155">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="78600-155">Conclusion</span></span>

<span data-ttu-id="78600-156">Wir haben gesehen, dass ASP.NET MVC erleichtert usw. mit Zugriff auf die Datenbank, Mitgliedschaft, AJAX, eine komplexe Website zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="78600-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="78600-157">sehr schnell.</span><span class="sxs-lookup"><span data-stu-id="78600-157">pretty quickly.</span></span> <span data-ttu-id="78600-158">Hoffentlich hat in diesem Tutorial Sie die Tools angegeben, die Sie erstellen Ihre eigenen ASP.NET MVC-Anwendungen beginnen möchten!</span><span class="sxs-lookup"><span data-stu-id="78600-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="78600-159">Vorherige</span><span class="sxs-lookup"><span data-stu-id="78600-159">Previous</span></span>](mvc-music-store-part-9.md)
