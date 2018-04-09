---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Hinzufügen einer neuen Kategorie unter Verwendung von jQuery UI DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="94ae4-102">Hinzufügen einer neuen Kategorie unter Verwendung von jQuery UI DropDownList</span><span class="sxs-lookup"><span data-stu-id="94ae4-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="94ae4-103">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="94ae4-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="94ae4-104">Der HTML-Code `Select` Tag eignet sich ideal für eine geordnete Liste der festen Kategoriedaten jedoch oft müssen Sie eine neue Kategorie hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="94ae4-105">Nehmen wir an, dass wir den "Genre" "Opera" zu den Kategorien in der Datenbank hinzufügen möchten?</span><span class="sxs-lookup"><span data-stu-id="94ae4-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="94ae4-106">In diesem Abschnitt werden wir jQuery UI verwenden, um ein Dialogfeld hinzufügen, wird eine neue Kategorie hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="94ae4-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="94ae4-107">Die folgende Abbildung zeigt, wie die Benutzeroberfläche im Browser darstellen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="94ae4-108">Wenn ein Benutzer wählt die **Hinzufügen eines neuen "Genre"** Link, ein Popup-Dialogfeld fordert den Benutzer für einen neuen Namen für die "Genre" (und optional eine Beschreibung).</span><span class="sxs-lookup"><span data-stu-id="94ae4-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="94ae4-109">Das Bild unten zeigen die **hinzufügen "Genre"** Popup-Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="94ae4-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="94ae4-110">Wenn ein neuen Namen für die "Genre" eingegeben wird und die **speichern** Schaltfläche abgelegt wird, geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="94ae4-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="94ae4-111">Ein AJAX-Aufruf sendet die Daten in der Create-Methode des Controllers "Genre", die die neue "Genre" in der Datenbank gespeichert und gibt den neuen "Genre"-Informationen ("Genre" Name "und" ID ") als JSON.</span><span class="sxs-lookup"><span data-stu-id="94ae4-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="94ae4-112">JavaScript hinzugefügt der select-Liste die neue Daten für "Genre".</span><span class="sxs-lookup"><span data-stu-id="94ae4-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="94ae4-113">JavaScript wird das neue "Genre" des ausgewählten Elements an.</span><span class="sxs-lookup"><span data-stu-id="94ae4-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="94ae4-114">In der folgenden Abbildung **Opera** wurde der Datenbank hinzugefügt und ausgewählt, die der **"Genre"** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="94ae4-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="94ae4-115">Öffnen der *Views\StoreManager\Create.cshtml* -Datei und Ersetzen Sie durch Folgendes Markup "Genre" den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="94ae4-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="94ae4-116">Die `_ChooseGenre` partielle Ansicht enthält die gesamte Logik, um die JavaScript und jQuery verwendet, um das Hinzufügen neuer "Genre" Feature implementieren verknüpft.</span><span class="sxs-lookup"><span data-stu-id="94ae4-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="94ae4-117">Nachdem wir den Code, der er sehr einfach, identisch mit der Interpret-UI kann abgeschlossen wurden.</span><span class="sxs-lookup"><span data-stu-id="94ae4-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="94ae4-118">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die *Views\StoreManager* Ordner, und wählen **hinzufügen**, klicken Sie dann **Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="94ae4-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="94ae4-119">In der **Sichtname** Eingabe, geben Sie `_ChooseGenre` wählen Sie dann **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="94ae4-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="94ae4-120">Ersetzen Sie das Markup in der *Views\StoreManager\\_ChooseGenre.cshtml* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="94ae4-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="94ae4-121">Die erste Zeile deklariert, dass wir, in übergeben werden eine `Album` als unserem Modell genau in der Create View-Anweisung zu modellieren.</span><span class="sxs-lookup"><span data-stu-id="94ae4-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="94ae4-122">Die nächsten Zeilen sind die **Bezeichnung** Helper Markup.</span><span class="sxs-lookup"><span data-stu-id="94ae4-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="94ae4-123">Die nächste Zeile ist die **DropDownList** Helper aufrufen, genauso wie die ursprüngliche Ansicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="94ae4-124">Die nächste Zeile Fügt eine Verknüpfung mit dem Namen `Add New Genre`, und es sich wie eine Schaltfläche Stile.</span><span class="sxs-lookup"><span data-stu-id="94ae4-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="94ae4-125">Die Zeile mit `ValidationMessageFor` wird direkt über die Erstellungsansicht kopiert.</span><span class="sxs-lookup"><span data-stu-id="94ae4-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="94ae4-126">Die folgenden Zeilen:</span><span class="sxs-lookup"><span data-stu-id="94ae4-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="94ae4-127">erstellt ein ausgeblendetes Div mit der ID `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="94ae4-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="94ae4-128">Verwenden wir jQuery anbinden unsere **hinzufügen "Genre"** Dialogfeld mit der ID `genreDialog` in dieser div.</span><span class="sxs-lookup"><span data-stu-id="94ae4-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="94ae4-129">Die letzten beiden Skripttags enthalten Links zu den JavaScript-Dateien, die verwendet wird um das Hinzufügen neuer "Genre"-Funktion zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="94ae4-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="94ae4-130">Die */Scripts/chooseGenre.js* Datei wird für Sie in das Projekt, ihn später in diesem Lernprogramm untersucht wird.</span><span class="sxs-lookup"><span data-stu-id="94ae4-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="94ae4-131">Führen Sie die Anwendung, und klicken Sie auf die **Hinzufügen eines neuen "Genre"** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="94ae4-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="94ae4-132">In der **hinzufügen "Genre"** Dialogfeld Geben Sie **Opera** in der **Namen** Eingabefeld.</span><span class="sxs-lookup"><span data-stu-id="94ae4-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="94ae4-133">Klicken Sie auf die Schaltfläche **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="94ae4-133">Click the **Save** button.</span></span> <span data-ttu-id="94ae4-134">Ein AJAX-Aufruf den Opera-Kategorie erstellt und anschließend füllt die Dropdownliste mit Opera und legt Opera als der ausgewählte "Genre".</span><span class="sxs-lookup"><span data-stu-id="94ae4-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="94ae4-135">Geben Sie eine Künstler, Titel und Preis, und wählen Sie dann die **erstellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="94ae4-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="94ae4-136">Wenn Sie einen Preis ein, der kleiner als $8,99 eingeben, wird das neue Album am oberen Rand die Indexansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="94ae4-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="94ae4-137">Stellen Sie sicher, dass der neue Album Eintrag in der Datenbank gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="94ae4-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="94ae4-138">Versuchen Sie, eine neue "Genre" mit nur einem Buchstaben zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="94ae4-139">Der folgende code in der *Models\Genre.cs* Datei legt die minimale und maximale Länge des Namens "Genre".</span><span class="sxs-lookup"><span data-stu-id="94ae4-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="94ae4-140">Clientseitige Validierung meldet, dass Sie eine Zeichenfolge zwischen 2 und 20 Zeichen eingeben müssen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="94ae4-141">Untersuchen wie ein neues "Genre" wird die Datenbank und der Select-Liste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="94ae4-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="94ae4-142">Öffnen der *Scripts\chooseGenre.js* Datei, und überprüfen Sie den Code.</span><span class="sxs-lookup"><span data-stu-id="94ae4-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="94ae4-143">Die zweite Zeile verwendet die ID `genreDialog` So erstellen ein Dialogfeld auf das Div-Tag in der *Views\StoreManager\\_ChooseGenre.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="94ae4-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="94ae4-144">Die meisten benannte Parameter sind erklärende.</span><span class="sxs-lookup"><span data-stu-id="94ae4-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="94ae4-145">Die `autoOpen` Parameter auf "false" festgelegt ist Auswählen der **erstellen "Genre"** Schaltfläche Öffnen den Dialog explizit (hierzu finden Sie auf letztere).</span><span class="sxs-lookup"><span data-stu-id="94ae4-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="94ae4-146">Der Dialog besitzt zwei Schaltflächen **speichern** und **"Abbrechen"**.</span><span class="sxs-lookup"><span data-stu-id="94ae4-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="94ae4-147">Die **"Abbrechen"** Schaltfläche wird das Dialogfeld geschlossen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="94ae4-148">Der folgende code zeigt die **speichern** Schaltfläche Funktion.</span><span class="sxs-lookup"><span data-stu-id="94ae4-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="94ae4-149">Die `var createGenreForm` ausgewählt ist, aus der `createGenreForm` -ID.</span><span class="sxs-lookup"><span data-stu-id="94ae4-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="94ae4-150">Die `createGenreForm` -ID wurde festgelegt, in den folgenden Code, der der *Views\Genre\\_CreateGenre.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="94ae4-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="94ae4-151">Die [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) Helper-Überladung, die verwendet wird, der *Views\Genre\\_CreateGenre.cshtml* Datei HTML mit ein, der die URL zum Absenden des Formulars Aktionsattribut generiert.</span><span class="sxs-lookup"><span data-stu-id="94ae4-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="94ae4-152">Sie können dies überprüfen, von der Seite "Album erstellen" in einem Browser anzeigen und Auswählen der anzeigen-Quelle im Browser.</span><span class="sxs-lookup"><span data-stu-id="94ae4-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="94ae4-153">Das folgende Markup zeigt die generierten HTML-Codes, die das Formulartag enthält.</span><span class="sxs-lookup"><span data-stu-id="94ae4-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="94ae4-154">Die jQuery `$.post` Zeile führt einen AJAX-Aufruf an das Action-Attribut (`/StoreManager/Create`) und übergibt die Daten aus der **erstellen "Genre"** (Dialogfeld).</span><span class="sxs-lookup"><span data-stu-id="94ae4-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="94ae4-155">Die Daten bestehen aus den Namen für die neue "Genre" und eine optionale Beschreibung.</span><span class="sxs-lookup"><span data-stu-id="94ae4-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="94ae4-156">Wenn die AJAX-Aufruf erfolgreich ist, neue "Genre" Name und Wert sind Select Markup hinzugefügt, und die neue "Genre" auf den ausgewählten Wert festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="94ae4-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="94ae4-157">Da es sich dynamisch generierten Markup handelt, nicht die neue select-Option durch Anzeigen der Quelle im Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="94ae4-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="94ae4-158">Sie können den neuen HTML-Code mit den Internet Explorer 9 F12 Entwicklertools sehen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="94ae4-159">Um die neue select-Option in Internet Explorer 9 anzuzeigen, drücken Sie F12-Taste, um die F12 Entwicklertools zu starten.</span><span class="sxs-lookup"><span data-stu-id="94ae4-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="94ae4-160">Navigieren Sie zu der Seite "erstellen", und fügen Sie eine neue "Genre" hinzu, damit die neue "Genre" in der Auswahlliste "Genre" ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="94ae4-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="94ae4-161">In den F12-Entwicklungstools:</span><span class="sxs-lookup"><span data-stu-id="94ae4-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="94ae4-162">Wählen Sie die Registerkarte "HTML".</span><span class="sxs-lookup"><span data-stu-id="94ae4-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="94ae4-163">Drücken Sie das Symbol "Aktualisierung".</span><span class="sxs-lookup"><span data-stu-id="94ae4-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="94ae4-164">Geben Sie in das Suchfeld GenreID aus.</span><span class="sxs-lookup"><span data-stu-id="94ae4-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="94ae4-165">Verwenden das Symbol "Weiter",</span><span class="sxs-lookup"><span data-stu-id="94ae4-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="94ae4-166">Navigieren Sie zu der folgenden select-Tags:</span><span class="sxs-lookup"><span data-stu-id="94ae4-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="94ae4-167">Erweitern Sie den letzten Optionswert.</span><span class="sxs-lookup"><span data-stu-id="94ae4-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="94ae4-168">Den folgenden code in die *Scripts\chooseGenre.js* Datei veranschaulicht, wie die **Hinzufügen eines neuen "Genre"** Schaltfläche ruft mit dem Klickereignis verbunden und wie sich das **Hinzufügen eines neuen "Genre"** wird das Dialogfeld erstellt.</span><span class="sxs-lookup"><span data-stu-id="94ae4-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="94ae4-169">Die erste Zeile erstellt eine klicken-Funktion, die angefügt werden, um die **Hinzufügen eines neuen "Genre"** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="94ae4-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="94ae4-170">Das folgende Markup aus der Views\StoreManager\\_ChooseGenre.cshtml-Datei zeigt wie die **Hinzufügen eines neuen "Genre"** Schaltfläche wird erstellt:</span><span class="sxs-lookup"><span data-stu-id="94ae4-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="94ae4-171">Die Load-Methode erstellt und öffnet das Dialogfeld "hinzufügen" Genre "" und dem jQuery ruft `parse` Methode, damit die Clientvalidierung für Daten, die im Dialogfeld eingegebene stattfindet.</span><span class="sxs-lookup"><span data-stu-id="94ae4-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="94ae4-172">In diesem Abschnitt haben Sie gelernt, wie Sie ein Dialogfeld erstellen, die zum Hinzufügen von neuen Kategoriedaten zu einer select-Liste verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="94ae4-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="94ae4-173">Sie können das gleiche Verfahren zum Erstellen von Benutzeroberfläche zum Hinzufügen eines neuen Interpreten der Interpret-Auswahlliste folgen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="94ae4-174">In diesem Lernprogramm gegeben hat einen Überblick über das Arbeiten mit der ASP.NET MVC-HTML-Hilfsobjekt **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="94ae4-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="94ae4-175">Weitere Informationen zum Arbeiten mit der **DropDownList**, finden Sie im Abschnitt Verweise hinzufügen unten.</span><span class="sxs-lookup"><span data-stu-id="94ae4-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="94ae4-176">Bitte lassen Sie uns wissen Sie, ob dieses Lernprogramms hilfreich war.</span><span class="sxs-lookup"><span data-stu-id="94ae4-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="94ae4-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="94ae4-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="94ae4-178">Zusätzliche Referenzen</span><span class="sxs-lookup"><span data-stu-id="94ae4-178">Additional References</span></span>

- <span data-ttu-id="94ae4-179">[ASP.NET MVC – kaskadierende Dropdownlisten Lernprogramm](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) von [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="94ae4-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="94ae4-180">[Ausgewählte](http://harvesthq.github.com/chosen/) eine JavaScript-Plug-Ins, die mehrere Elemente auswählen und Filtern zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="94ae4-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="94ae4-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="94ae4-181">Contributors</span></span>

- [<span data-ttu-id="94ae4-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="94ae4-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="94ae4-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="94ae4-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="94ae4-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="94ae4-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="94ae4-185">Bearbeiter</span><span class="sxs-lookup"><span data-stu-id="94ae4-185">Reviewers</span></span>

- <span data-ttu-id="94ae4-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="94ae4-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="94ae4-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="94ae4-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="94ae4-188">Mike PPPoE</span><span class="sxs-lookup"><span data-stu-id="94ae4-188">Mike Pope</span></span>
- <span data-ttu-id="94ae4-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="94ae4-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="94ae4-190">Vorherige</span><span class="sxs-lookup"><span data-stu-id="94ae4-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
