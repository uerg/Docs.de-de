---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Teil 10: Letzten Updates der Navigation und Standortentwurfs, Schlussfolgerung | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 10 umfasst die letzten Updates der Navigation und e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: af08039de2d810948b9ab64974111b0346c7fa0f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="d76a1-104">Teil 10: Letzten Updates der Navigation und Standortentwurfs, Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d76a1-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="d76a1-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d76a1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d76a1-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="d76a1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d76a1-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="d76a1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="d76a1-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="d76a1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d76a1-109">Teil 10 werden die letzten Updates der Navigation und Standortentwurfs, Schlussfolgerung behandelt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="d76a1-110">Wir alle wichtige Funktionen für unsere Website abgeschlossen haben, jedoch können wir noch einige Features für die Websitenavigation, auf der Startseite und der Seite "Speicher Durchsuchen".</span><span class="sxs-lookup"><span data-stu-id="d76a1-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="d76a1-111">Shopping Cart Zusammenfassung Teilansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="d76a1-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="d76a1-112">Wir möchten die Anzahl der Elemente im Einkaufswagen des Benutzers für die gesamte Website verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="d76a1-113">Wir können einfach implementieren Sie dies durch das Erstellen einer Teilansicht der unsere Site.master hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="d76a1-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="d76a1-114">Wie bereits zuvor gezeigt, enthält die ShoppingCart-Controller eine CartSummary Aktionsmethode eine Teilansicht zurück:</span><span class="sxs-lookup"><span data-stu-id="d76a1-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="d76a1-115">CartSummary Teilansicht erstellen, mit der rechten Maustaste auf den Ordner Ansichten/ShoppingCart, und wählen Sie die Ansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="d76a1-116">Benennen Sie die Ansicht CartSummary, und aktivieren Sie das Kontrollkästchen "Erstellen eine Teilansicht" aus, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="d76a1-117">Die Teilansicht CartSummary ist ganz einfach – es ist nur ein Link auf die ShoppingCart-Indexansicht, die die Anzahl der Elemente in den Einkaufswagen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d76a1-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="d76a1-118">Der vollständige Code für CartSummary.cshtml lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d76a1-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="d76a1-119">Wir können eine Teilansicht in eine andere Seite in der Website, einschließlich der Website-Master mithilfe der Methode Html.RenderAction einschließen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="d76a1-120">RenderAction erfordert, geben Sie den Aktionsnamen ("CartSummary") und den Namen des Controllers ("ShoppingCart") als unten.</span><span class="sxs-lookup"><span data-stu-id="d76a1-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="d76a1-121">Vor dem Hinzufügen dieser an den Standort Layout, wird auch im Menü "Genre" erstellt, damit wir alle unsere Site.master Updates gleichzeitig vornehmen können.</span><span class="sxs-lookup"><span data-stu-id="d76a1-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="d76a1-122">Die "Genre" Menü Teilansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="d76a1-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="d76a1-123">Wir können ändern, es viel einfacher für die Benutzer über den Store durch Hinzufügen einer "Genre" Menü "alle die Genres verfügbar in unserem Store listet navigieren.</span><span class="sxs-lookup"><span data-stu-id="d76a1-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="d76a1-124">Befolgen wir die gleichen Schritte auch eine Teilansicht GenreMenu erstellen, und klicken Sie dann wir hinzugefügt werden können sowohl mit dem übergeordneten Standort.</span><span class="sxs-lookup"><span data-stu-id="d76a1-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="d76a1-125">Fügen Sie zunächst die folgenden GenreMenu Controlleraktion hinzu der StoreController:</span><span class="sxs-lookup"><span data-stu-id="d76a1-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="d76a1-126">Diese Aktion wird eine Liste von Genres der von der Teilansicht angezeigt wird, die wir als Nächstes erstellt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="d76a1-127">*Hinweis: Wir haben das Attribut [ChildActionOnly] diese Controlleraktion hinzugefügt bedeutet, dass wir nur diese Aktion aus einer Teilansicht verwendet werden soll. Dieses Attribut verhindert Controlleraktion durch Navigieren zum /Store/GenreMenu ausgeführt werden. Dies ist nicht erforderlich für Teilansichten, aber es wird empfohlen, da wir möchten sicherstellen, dass unsere Controlleraktionen verwendet werden, wie wir möchten. Wir sind auch zurückgeben PartialView statt anzeigen, können Sie das Ansichtsmodul, dass sie das Layout für diese Ansicht verwenden, sollten nicht wissen, wie sie in den anderen Ansichten enthalten wird, ist.*</span><span class="sxs-lookup"><span data-stu-id="d76a1-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="d76a1-128">Mit der rechten Maustaste auf die GenreMenu-Controlleraktion, und erstellen Sie eine Teilansicht mit dem Namen GenreMenu die stark typisiert ist, verwenden die Datenklasse "Genre" anzeigen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="d76a1-129">Aktualisieren Sie den Code anzeigen, für die GenreMenu partielle Ansicht zum Anzeigen von Elementen, die eine ungeordnete Liste wie folgt verwenden.</span><span class="sxs-lookup"><span data-stu-id="d76a1-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="d76a1-130">Aktualisieren von Site-Layout für unsere Teilansichten</span><span class="sxs-lookup"><span data-stu-id="d76a1-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="d76a1-131">Wir können unsere Teilansichten hinzufügen, zu der Website-Layout (/Ansichten/freigegeben/\_Layout.cshtml) durch Aufrufen von Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="d76a1-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="d76a1-132">Wir fügen sie beide an, als auch einige zusätzliche Markup zum Anzeigen, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="d76a1-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="d76a1-133">Jetzt Wenn wir die Anwendung ausführen, wird es der "Genre" im linken Navigationsbereich und die Zusammenfassung der Warenkorb am oberen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="d76a1-134">Aktualisieren Sie auf der Seite "Speicher Durchsuchen"</span><span class="sxs-lookup"><span data-stu-id="d76a1-134">Update to the Store Browse page</span></span>

<span data-ttu-id="d76a1-135">Die Seite Speicher Durchsuchen ist funktionsfähig, aber nicht sehr gut aussehen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="d76a1-136">Wir aktualisieren können Sie die Seite, um die Alben in einem besser Layout anzeigen, indem Sie den Code anzeigen (in /Views/Store/Browse.cshtml gefunden) wie folgt aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="d76a1-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="d76a1-137">Wir nehmen hier Url.Action statt Html.ActionLink verwenden, sodass wir die spezielle Formatierung auf den Link, um die Grafik Album enthalten anwenden können.</span><span class="sxs-lookup"><span data-stu-id="d76a1-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="d76a1-138">*Hinweis: Es werden eine generische Albumtitel für diese Alben angezeigt. Diese Informationen werden in der Datenbank gespeichert und kann bearbeitet werden, über den Speicher-Manager. Willkommen beim Ihre eigene Grafik hinzufügen werden.*</span><span class="sxs-lookup"><span data-stu-id="d76a1-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="d76a1-139">Wenn wir zu einem "Genre" navigieren, wird es nun die Alben, die in einem Raster mit Album Grafik dargestellt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="d76a1-140">Aktualisieren die Homepage, um nach oben Verkaufsmanager Alben anzeigen</span><span class="sxs-lookup"><span data-stu-id="d76a1-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="d76a1-141">Wir möchten unsere meistverkaufte Alben auf der Startseite den Umsatz steigern, feature.</span><span class="sxs-lookup"><span data-stu-id="d76a1-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="d76a1-142">Wir stellen einige Updates an unsere HomeController zu behandeln, und in sowie einige zusätzlichen Grafiken hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="d76a1-143">Zunächst fügen wir eine Navigationseigenschaft unsere Album-Klasse, sodass EntityFramework erkennt, dass sie verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="d76a1-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="d76a1-144">Die letzten Zeilen der unsere **Album** Klasse sollte jetzt wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="d76a1-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="d76a1-145">*Hinweis: Dies erfordert, dass hinzufügen eine using-Anweisung System.Collections.Generic-Namespace zu importieren.*</span><span class="sxs-lookup"><span data-stu-id="d76a1-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="d76a1-146">Zunächst fügen wir ein StoreDB-Feld und die MvcMusicStore.Models using-Anweisungen, wie Sie in unserer anderen Controller.</span><span class="sxs-lookup"><span data-stu-id="d76a1-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="d76a1-147">Als Nächstes fügen die folgende Methode, die HomeController wir, die abfragt, unsere Datenbank zum obere Verkaufsmanager Alben entsprechend OrderDetails suchen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="d76a1-148">Dies ist eine private Methode, da wir nicht, um es als eine Controlleraktion verfügbar zu machen möchten.</span><span class="sxs-lookup"><span data-stu-id="d76a1-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="d76a1-149">Wir werden diese im HomeController aus Gründen der Einfachheit einschließlich, aber es wird empfohlen, Ihre Geschäftslogik in separaten Dienstklassen nach Bedarf zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="d76a1-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="d76a1-150">Mit diesem erfüllt können wir Controlleraktion Index zum Abfragen der obersten 5 verkaufen Alben und Geräte auf die Ansicht aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d76a1-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="d76a1-151">Der vollständige Code für die aktualisierte HomeController ist wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="d76a1-152">Schließlich müssen wir unsere Startseite Indexansicht aktualisieren, sodass eine Liste von Alben angezeigt werden können, durch Aktualisieren des Typ des Modells und im unteren Bereich die Albumliste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="d76a1-153">Wir übernehmen diese Möglichkeit, die auch einer Überschrift und eine Promotion-Abschnitt der Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="d76a1-154">Wenn wir die Anwendung ausführen, wird wir nun unsere aktualisierten Startseite mit Top Verkaufsmanager Alben und unsere Werbe-Meldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d76a1-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="d76a1-155">Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="d76a1-155">Conclusion</span></span>

<span data-ttu-id="d76a1-156">Wir haben gesehen, dass diese ASP.NET-MVC erleichtert usw. eine anspruchsvolle Website mit Datenbankzugriff, AJAX-Mitgliedschaft zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d76a1-156">We've seen that that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="d76a1-157">ziemlich schnell.</span><span class="sxs-lookup"><span data-stu-id="d76a1-157">pretty quickly.</span></span> <span data-ttu-id="d76a1-158">Hoffentlich hat in diesem Lernprogramm Ihnen die Tools gegeben, die Sie beginnen damit, eigene ASP.NET-MVC Anwendungen erstellen müssen!</span><span class="sxs-lookup"><span data-stu-id="d76a1-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


>[!div class="step-by-step"]
[<span data-ttu-id="d76a1-159">Zurück</span><span class="sxs-lookup"><span data-stu-id="d76a1-159">Previous</span></span>](mvc-music-store-part-9.md)
