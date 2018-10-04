---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Überprüfen, wie ASP.NET MVC die DropDownList-Hilfsprogramms erstellt das Gerüst für | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577846"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="17519-102">Überprüfen, wie ASP.NET MVC die DropDownList-Hilfsprogramms erstellt das Gerüst für</span><span class="sxs-lookup"><span data-stu-id="17519-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="17519-103">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="17519-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="17519-104">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="17519-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="17519-105">Nennen Sie den Controller **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="17519-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="17519-106">Festlegen der Optionen für die **Controller hinzufügen** Dialogfeld wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="17519-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="17519-107">Bearbeiten der *StoreManager\Index.cshtml* anzeigen und entfernen `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="17519-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="17519-108">Entfernen von `AlbumArtUrl` veranlasst die Präsentation besser lesbar.</span><span class="sxs-lookup"><span data-stu-id="17519-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="17519-109">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="17519-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="17519-110">Öffnen der *Controllers\StoreManagerController.cs* Datei, und suchen die `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="17519-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="17519-111">Hinzufügen der `OrderBy` Klausel, damit die Alben nach Preis sortiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="17519-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="17519-112">Der vollständige Code wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="17519-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="17519-113">Sortieren nach Preis wird zum Testen von Änderungen an der Datenbank erleichtern.</span><span class="sxs-lookup"><span data-stu-id="17519-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="17519-114">Beim Testen der Bearbeitung und Methoden erstellen, können Sie einen niedrigen Preis, damit die gespeicherten Daten zuerst angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="17519-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="17519-115">Öffnen der *StoreManager\Edit.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="17519-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="17519-116">Fügen Sie die folgende Zeile direkt nach dem Tag der Legende an.</span><span class="sxs-lookup"><span data-stu-id="17519-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="17519-117">Der folgende Code zeigt die im Rahmen dieser Änderung:</span><span class="sxs-lookup"><span data-stu-id="17519-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="17519-118">Die `AlbumId` ist erforderlich, um einen Datensatz von Album zu ändern.</span><span class="sxs-lookup"><span data-stu-id="17519-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="17519-119">Drücken Sie STRG+F5, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="17519-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="17519-120">Wählen Sie diese Option die **Admin** verknüpfen, und wählen Sie dann die **neu erstellen** Link, um ein neues Album zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="17519-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="17519-121">Stellen Sie sicher, dass die Albuminformationen gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="17519-121">Verify the album information was saved.</span></span> <span data-ttu-id="17519-122">Bearbeiten Sie ein Album, und stellen Sie sicher, dass die vorgenommenen Änderungen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="17519-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="17519-123">Das Album-Schema</span><span class="sxs-lookup"><span data-stu-id="17519-123">The Album Schema</span></span>

<span data-ttu-id="17519-124">Die `StoreManager` Controller, die von der MVC-Gerüstbau erstellten CRUD (Create, Read, Update, Delete), auf die Alben ermöglicht, in der Music Store-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="17519-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="17519-125">Das Schema für Albuminformationen ist unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="17519-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="17519-126">Die `Albums` Tabelle speichert nicht das Album "Genre" und eine Beschreibung, die sie speichert einen Fremdschlüssel für die `Genres` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="17519-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="17519-127">Die `Genres` Tabelle enthält, "Genre" Name und Beschreibung.</span><span class="sxs-lookup"><span data-stu-id="17519-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="17519-128">Ebenso die `Albums` Tabelle nicht enthalten, der Name des Albums-Künstler, jedoch einen Fremdschlüssel für die `Artists` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="17519-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="17519-129">Die `Artists` Tabelle enthält den Namen des Interpreten.</span><span class="sxs-lookup"><span data-stu-id="17519-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="17519-130">Wenn Sie überprüfen, dass die Daten in die `Albums` Tabelle sehen, da Sie jede Zeile enthält einen Fremdschlüssel für die `Genres` Tabelle und einen Fremdschlüssel für die `Artists` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="17519-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="17519-131">Die folgende Abbildung zeigen einige Daten aus der Tabelle die `Albums` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="17519-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="17519-132">Wählen Sie HTML-Tags</span><span class="sxs-lookup"><span data-stu-id="17519-132">The HTML Select Tag</span></span>

<span data-ttu-id="17519-133">Der HTML-Code `<select>` Element (von der HTML-Code erstellt [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Helper) wird verwendet, um eine vollständige Liste der Werte (z. B. die Liste der Genres) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="17519-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="17519-134">Bearbeiten von Formularen Wenn der aktuelle Wert bekannt ist, kann die select-Liste den aktuellen Wert anzeigen.</span><span class="sxs-lookup"><span data-stu-id="17519-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="17519-135">Erläutert, diesem zuvor Wenn wir den ausgewählten Wert festzulegen, um **Komödie**.</span><span class="sxs-lookup"><span data-stu-id="17519-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="17519-136">Die select-Liste ist ideal für das Anzeigen von Kategorie- oder foreign Key-Daten.</span><span class="sxs-lookup"><span data-stu-id="17519-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="17519-137">Die `<select>` -Element für den Fremdschlüssel "Genre" zeigt die Liste der möglichen genrenamen, aber wenn Sie das Formular speichern wird die Eigenschaft "Genre" mit "Genre" Fremdschlüsselwert, nicht den Namen der angezeigten "Genre" aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="17519-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="17519-138">In der folgenden Abbildung wird das ausgewählte Genre **Disco** und Interpreten **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="17519-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="17519-139">Untersuchen von ASP.NET MVC, Gerüst Code</span><span class="sxs-lookup"><span data-stu-id="17519-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="17519-140">Öffnen der *Controllers\StoreManagerController.cs* Datei, und suchen die `HTTP GET Create` Methode.</span><span class="sxs-lookup"><span data-stu-id="17519-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="17519-141">Die `Create` Methode fügt zwei [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) Objekte die `ViewBag`, "Genre" Informationen enthalten, und die Interpret-Daten enthalten.</span><span class="sxs-lookup"><span data-stu-id="17519-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="17519-142">Die [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Konstruktorüberladung, die oben verwendeten akzeptiert drei Argumente:</span><span class="sxs-lookup"><span data-stu-id="17519-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="17519-143">*Elemente*: ein ["IEnumerable"](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) mit den Elementen in der Liste.</span><span class="sxs-lookup"><span data-stu-id="17519-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="17519-144">Im obigen Beispiel wird die Liste der Genres von zurückgegebenen `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="17519-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="17519-145">*DataValueField*: der Name der Eigenschaft in der **"IEnumerable"** Liste, die den Schlüsselwert enthält.</span><span class="sxs-lookup"><span data-stu-id="17519-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="17519-146">Im obigen Beispiel `GenreId` und `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="17519-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="17519-147">*DataTextField*: der Name der Eigenschaft in der **"IEnumerable"** Liste, die die Informationen für die Anzeige enthält.</span><span class="sxs-lookup"><span data-stu-id="17519-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="17519-148">In den Interpreten und Genre-Tabelle die `name` Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="17519-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="17519-149">Öffnen der *Views\StoreManager\Create.cshtml* Datei, und sehen die `Html.DropDownList` Helper-Markup für das Feld "Genre".</span><span class="sxs-lookup"><span data-stu-id="17519-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="17519-150">Die erste Zeile zeigt, dass die Ansicht "erstellen" wird ein `Album` Modell.</span><span class="sxs-lookup"><span data-stu-id="17519-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="17519-151">In der `Create` oben gezeigten Methode, es wurde kein Modell übergeben, damit die Ansicht Ruft eine **null** `Album` Modell.</span><span class="sxs-lookup"><span data-stu-id="17519-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="17519-152">An diesem Punkt erstellt wird, ein neues Album wir noch keinen haben `Album` dafür Daten.</span><span class="sxs-lookup"><span data-stu-id="17519-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="17519-153">Die [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) Überladung, die oben gezeigte nimmt den Namen des Felds an, für das Modell gebunden.</span><span class="sxs-lookup"><span data-stu-id="17519-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="17519-154">Darüber hinaus verwendet diesen Namen auf die Suche nach einem **"ViewBag"** Objekt mit einer [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) Objekt.</span><span class="sxs-lookup"><span data-stu-id="17519-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="17519-155">Verwenden diese Überladung, Sie sind erforderlich, um den Namen der **ViewBag SelectList** Objekt `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="17519-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="17519-156">Der zweite Parameter (`String.Empty`) ist der Text, der angezeigt wird, wenn kein Element ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="17519-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="17519-157">Dies ist genau das, was wir wollen, wenn Sie ein neues Album zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="17519-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="17519-158">Wenn Sie den zweiten Parameter entfernt und den folgenden Code verwendet:</span><span class="sxs-lookup"><span data-stu-id="17519-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="17519-159">Das erste Element bzw. der Rock würde die select-Liste in diesem Beispiel standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="17519-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="17519-160">Untersuchen der `HTTP POST Create` Methode.</span><span class="sxs-lookup"><span data-stu-id="17519-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="17519-161">Diese Überladung von der `Create` -Methode übernimmt eine `album` Objekt, das durch das ASP.NET MVC-modellbindungssystem aus die Formularwerte gebucht erstellt.</span><span class="sxs-lookup"><span data-stu-id="17519-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="17519-162">Wenn Sie ein neues Album, senden, wenn Modellstatus gültig ist und keine Datenbankfehler vorliegen, wird das neue Album die Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="17519-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="17519-163">Die folgende Abbildung zeigt die Erstellung eines neuen Albums.</span><span class="sxs-lookup"><span data-stu-id="17519-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="17519-164">Sie können die [Fiddler-Tool](http://www.fiddler2.com/fiddler2/) untersuchen Sie die bereitgestellten Formularwerte, ASP.NET MVC-modellbindung verwendet werden, um das Album-Objekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="17519-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="17519-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17519-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="17519-166">Refactoring "ViewBag" SelectList erstellen</span><span class="sxs-lookup"><span data-stu-id="17519-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="17519-167">Sowohl die `Edit` Methoden und die `HTTP POST Create` Methode haben identischen Code zum Einrichten der **SelectList** in die **"ViewBag"**.</span><span class="sxs-lookup"><span data-stu-id="17519-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="17519-168">Ganz im Geiste der [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), wir werden diesen Code Umgestalten.</span><span class="sxs-lookup"><span data-stu-id="17519-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="17519-169">Wir erstellen mithilfe dieses Refactoring Code später noch mal.</span><span class="sxs-lookup"><span data-stu-id="17519-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="17519-170">Erstellen Sie eine neue Methode zum Hinzufügen einer "Genre" und Interpreten **SelectList** auf die **"ViewBag"**.</span><span class="sxs-lookup"><span data-stu-id="17519-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="17519-171">Ersetzen Sie die zwei Zeilen mit dem Festlegen der `ViewBag` in jedem der `Create` und `Edit` Methoden mit einem Aufruf von der `SetGenreArtistViewBag` Methode.</span><span class="sxs-lookup"><span data-stu-id="17519-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="17519-172">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="17519-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="17519-173">Erstellen Sie ein neues Album und bearbeiten Sie ein Album, um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="17519-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="17519-174">Übergeben die SelectList explizit zu DropDownList</span><span class="sxs-lookup"><span data-stu-id="17519-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="17519-175">Die bearbeitungs- und änderungsansichten erstellt, durch die Verwendung der ASP.NET MVC-Gerüstbau Folgendes **DropDownList** überladen:</span><span class="sxs-lookup"><span data-stu-id="17519-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="17519-176">Die `DropDownList` Markup für die Ansicht "erstellen" wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="17519-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="17519-177">Da die `ViewBag` -Eigenschaft für die `SelectList` heißt `GenreId`, **DropDownList** Helfer verwendet die `GenreId` **SelectList** in die **"ViewBag"** .</span><span class="sxs-lookup"><span data-stu-id="17519-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="17519-178">In der folgenden **DropDownList** überladen, die `SelectList` explizit übergeben.</span><span class="sxs-lookup"><span data-stu-id="17519-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="17519-179">Öffnen der *Views\StoreManager\Edit.cshtml* Datei, und ändern Sie die **DropDownList** Aufruf explizit übergeben die **SelectList**, mithilfe der Überladung, die oben genannten.</span><span class="sxs-lookup"><span data-stu-id="17519-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="17519-180">Dies gilt für die Kategorie "Genre".</span><span class="sxs-lookup"><span data-stu-id="17519-180">Do this for the Genre category.</span></span> <span data-ttu-id="17519-181">Der vollständige Code wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="17519-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="17519-182">Führen Sie die Anwendung, und klicken Sie auf die **Admin** zu verknüpfen, und klicken Sie dann ein Album Jazz navigieren, und wählen die **bearbeiten** Link.</span><span class="sxs-lookup"><span data-stu-id="17519-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="17519-183">Anstelle Jazz als das derzeit ausgewählte Genre, wird die Rock angezeigt.</span><span class="sxs-lookup"><span data-stu-id="17519-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="17519-184">Wenn das Argument "String" (die Eigenschaft zum Binden) und die **SelectList** Objekt den gleichen Namen haben, wird der ausgewählte Wert nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="17519-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="17519-185">Wenn es kein ausgewählten Wert angegeben ist, Browser standardmäßig das erste Element in der **SelectList**(d.h. **Rock** im obigen Beispiel).</span><span class="sxs-lookup"><span data-stu-id="17519-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="17519-186">Dies ist eine bekannte Einschränkung der **DropDownList** Helper.</span><span class="sxs-lookup"><span data-stu-id="17519-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="17519-187">Öffnen der *Controllers\StoreManagerController.cs* und Ändern der **SelectList** Objektnamen, `Genres` und `Artists`.</span><span class="sxs-lookup"><span data-stu-id="17519-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="17519-188">Der vollständige Code wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="17519-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="17519-189">Die Namen, die Genres und Künstler sind bessere Namen für die Kategorien aus, da sie mehr als nur die ID der einzelnen Kategorien enthalten.</span><span class="sxs-lookup"><span data-stu-id="17519-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="17519-190">Das refactoring, die, das wir zuvor, gelohnt.</span><span class="sxs-lookup"><span data-stu-id="17519-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="17519-191">Ändern, sondern die **"ViewBag"** wurden unsere Änderungen in vier Methoden, auf die `SetGenreArtistViewBag` Methode.</span><span class="sxs-lookup"><span data-stu-id="17519-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="17519-192">Ändern der **DropDownList** rufen Sie in das Erstellen und Bearbeiten von Ansichten, mit denen die Verwendung der neuen **SelectList** Namen.</span><span class="sxs-lookup"><span data-stu-id="17519-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="17519-193">Das neue Markup für die Bearbeitungsansicht ist unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="17519-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="17519-194">Ansicht "erstellen" ist erforderlich, eine leere Zeichenfolge ein, um zu verhindern, dass das erste Element in der SelectList angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="17519-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="17519-195">Erstellen Sie ein neues Album und bearbeiten Sie ein Album, um sicherzustellen, dass Änderungen ordnungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="17519-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="17519-196">Testen Sie den Code bearbeiten, indem Sie die ein Album mit einer "Genre" als Rock auswählen.</span><span class="sxs-lookup"><span data-stu-id="17519-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="17519-197">Verwenden ein Ansichtsmodell mit dem DropDownList-Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="17519-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="17519-198">Erstellen Sie eine neue Klasse im Ordner "ViewModels" mit dem Namen `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="17519-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="17519-199">Ersetzen Sie den Code in die `AlbumSelectListViewModel` Klasse durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="17519-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="17519-200">Die `AlbumSelectListViewModel` -Konstruktor akzeptiert ein Album und eine Liste mit Interpreten und Genres und erstellt ein Objekt mit dem Album und `SelectList` für Genres und Künstler.</span><span class="sxs-lookup"><span data-stu-id="17519-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="17519-201">Erstellen Sie das Projekt daher `AlbumSelectListViewModel` ist verfügbar, wenn wir im nächsten Schritt eine Ansicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="17519-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="17519-202">Hinzufügen einer `EditVM` Methode, um die `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="17519-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="17519-203">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="17519-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="17519-204">Klicken Sie mit der rechten Maustaste auf `AlbumSelectListViewModel`Option **beheben**, klicken Sie dann **mit MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="17519-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="17519-205">Alternativ können Sie hinzufügen, die folgenden using-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="17519-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="17519-206">Klicken Sie mit der rechten Maustaste auf `EditVM` , und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="17519-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="17519-207">Verwenden Sie die nachfolgend aufgeführten Optionen.</span><span class="sxs-lookup"><span data-stu-id="17519-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="17519-208">Wählen Sie **hinzufügen**, ersetzen Sie den Inhalt von der *Views\StoreManager\EditVM.cshtml* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="17519-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="17519-209">Die `EditVM` Markup ist sehr ähnlich, mit dem Original `Edit` Markup mit den folgenden Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="17519-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="17519-210">Modellieren von Eigenschaften in der `Edit` Anzeigen des Formulars werden `model.property`(z. B. `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="17519-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="17519-211">Modellieren von Eigenschaften in der `EditVm` Anzeigen des Formulars werden `model.Album.property`(z. B. `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="17519-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="17519-212">Der Grund dafür ist die `EditVM` Ansicht übergeben wird einen Container für eine `Album`und keine `Album` wie in der `Edit` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="17519-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="17519-213">Die **DropDownList** zweiter Parameter stammt aus dem Ansichtsmodell, nicht die **"ViewBag"**.</span><span class="sxs-lookup"><span data-stu-id="17519-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="17519-214">Die **BeginForm** -Hilfsmethode in den `EditVM` explizit Ansicht sendet eine Rückmeldung an den `Edit` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="17519-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="17519-215">Durch Ausgeben von zurück an die `Edit` Aktion, die wir geschrieben haben kein `HTTP POST EditVM` Aktion. Außerdem können die `HTTP POST` `Edit` Aktion.</span><span class="sxs-lookup"><span data-stu-id="17519-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="17519-216">Führen Sie die Anwendung, und bearbeiten Sie ein Album.</span><span class="sxs-lookup"><span data-stu-id="17519-216">Run the application and edit an album.</span></span> <span data-ttu-id="17519-217">Ändern Sie die URL mit `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="17519-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="17519-218">Ändern Sie ein Feld, und drücken Sie die **speichern** Schaltfläche, um zu überprüfen, ob der Code funktioniert.</span><span class="sxs-lookup"><span data-stu-id="17519-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="17519-219">Welcher Ansatz sollten Sie verwenden?</span><span class="sxs-lookup"><span data-stu-id="17519-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="17519-220">Alle drei Ansätze, die angezeigt werden Überwachungsdatensätze.</span><span class="sxs-lookup"><span data-stu-id="17519-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="17519-221">Viele Entwickler bevorzugen Explictily übergeben die `SelectList` auf die `DropDownList` mithilfe der `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="17519-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="17519-222">Dieser Ansatz hat den zusätzlichen Vorteil bietet Ihnen die Flexibilität, einen besser geeigneten Namen für die Sammlung an.</span><span class="sxs-lookup"><span data-stu-id="17519-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="17519-223">Ein Nachteil ist, Sie können nicht den Namen der `ViewBag SelectList` Objekt den gleichen Namen wie der Modelleigenschaft.</span><span class="sxs-lookup"><span data-stu-id="17519-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="17519-224">Einige Entwickler bevorzugen die ViewModel-Ansatz.</span><span class="sxs-lookup"><span data-stu-id="17519-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="17519-225">Andere sollten Sie die ausführlichere Markup und generiert HTML-Code von der ViewModel-Ansatz einen Nachteil.</span><span class="sxs-lookup"><span data-stu-id="17519-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="17519-226">In diesem Abschnitt haben wir drei Ansätze zum Verwenden der **DropDownList** mit Kategoriedaten.</span><span class="sxs-lookup"><span data-stu-id="17519-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="17519-227">Im nächsten Abschnitt zeigen wir, wie Sie eine neue Kategorie hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="17519-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="17519-228">[Zurück](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Weiter](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="17519-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
