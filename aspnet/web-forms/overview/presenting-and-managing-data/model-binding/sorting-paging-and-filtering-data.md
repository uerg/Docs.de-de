---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortieren, paging und Filtern von Daten mit modellbindung und Web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 7529c811e6196327094f8f735de1bd65be76ee3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398306"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="a3e70-104">Sortieren, paging und Filtern von Daten mit modellbindung und Web forms</span><span class="sxs-lookup"><span data-stu-id="a3e70-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="a3e70-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a3e70-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a3e70-106">Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a3e70-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="a3e70-107">Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement).</span><span class="sxs-lookup"><span data-stu-id="a3e70-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="a3e70-108">Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.</span><span class="sxs-lookup"><span data-stu-id="a3e70-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="a3e70-109">In diesem Tutorial veranschaulicht das Hinzufügen, sortieren, paging und Filterung der Daten über die modellbindung.</span><span class="sxs-lookup"><span data-stu-id="a3e70-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="a3e70-110">Dieses Tutorial baut auf das Projekt erstellt haben, in der ersten [Teil](retrieving-data.md) der Reihe.</span><span class="sxs-lookup"><span data-stu-id="a3e70-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="a3e70-111">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB.</span><span class="sxs-lookup"><span data-stu-id="a3e70-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="a3e70-112">Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a3e70-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="a3e70-113">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a3e70-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="a3e70-114">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="a3e70-114">What you'll build</span></span>

<span data-ttu-id="a3e70-115">In diesem Tutorial müssen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="a3e70-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="a3e70-116">Aktivieren Sie sortieren und paging von Daten</span><span class="sxs-lookup"><span data-stu-id="a3e70-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="a3e70-117">Aktivieren Sie die Filterung der Daten basierend auf der Auswahl des Benutzers</span><span class="sxs-lookup"><span data-stu-id="a3e70-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="a3e70-118">Hinzufügen einer Sortierung</span><span class="sxs-lookup"><span data-stu-id="a3e70-118">Add sorting</span></span>

<span data-ttu-id="a3e70-119">Aktivieren der Sortierung in der GridView ist sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="a3e70-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="a3e70-120">Legen Sie einfach in der Datei Student.aspx **AllowSorting** zu **"true"** in den GridView-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="a3e70-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="a3e70-121">Sie müssen nicht festlegen einer **SortExpression** Wert für jede Spalte, wie die DataField automatisch verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a3e70-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="a3e70-122">Das GridView ändert es sich um die Abfrage enthält Anordnen der Daten durch die ausgewählten Werte.</span><span class="sxs-lookup"><span data-stu-id="a3e70-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="a3e70-123">Der folgende hervorgehobene Code zeigt dem Hinzufügen, die Sie benötigen zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="a3e70-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="a3e70-124">Führen Sie die Webanwendung, und Testen von sortieren Studentendatensätze von den Werten in verschiedenen Spalten.</span><span class="sxs-lookup"><span data-stu-id="a3e70-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Sortierung Schüler/Studenten](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="a3e70-126">Hinzufügen von paging</span><span class="sxs-lookup"><span data-stu-id="a3e70-126">Add paging</span></span>

<span data-ttu-id="a3e70-127">Aktivieren von Paging ist ebenfalls ganz einfach.</span><span class="sxs-lookup"><span data-stu-id="a3e70-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="a3e70-128">Legen Sie in den GridView-Ansicht, die **AllowPaging** Eigenschaft **"true"** und legen Sie die **PageSize** Eigenschaft, um die Anzahl der Datensätze, die Sie auf jeder Seite anzeigen möchten.</span><span class="sxs-lookup"><span data-stu-id="a3e70-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="a3e70-129">In diesem Tutorial können Sie es auf 4 festlegen.</span><span class="sxs-lookup"><span data-stu-id="a3e70-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="a3e70-130">Führen Sie die Webanwendung, und beachten Sie, dass die Datensätze mit mehr als 4 auf einer einzelnen Seite angezeigten Datensätze über mehrere Seiten unterteilt sind.</span><span class="sxs-lookup"><span data-stu-id="a3e70-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Hinzufügen von paging](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="a3e70-132">Verzögerte abfrageausführung verbessert die ANWENDUGNSEFFIZIENZ.</span><span class="sxs-lookup"><span data-stu-id="a3e70-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="a3e70-133">Anstatt durch Abrufen des gesamten Datasets, ändert die GridView für die Abfrage, um nur die Datensätze für die aktuelle Seite abrufen.</span><span class="sxs-lookup"><span data-stu-id="a3e70-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="a3e70-134">Filtern von Datensätzen durch Auswahl des Benutzers</span><span class="sxs-lookup"><span data-stu-id="a3e70-134">Filter records by user selection</span></span>

<span data-ttu-id="a3e70-135">Modellbindung fügt mehrere Attribute, die Sie festlegen, wie Sie den Wert für einen Parameter in einem Modell Bindungsmethode festlegen können.</span><span class="sxs-lookup"><span data-stu-id="a3e70-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="a3e70-136">Diese Attribute sind der **System.Web.ModelBinding** Namespace.</span><span class="sxs-lookup"><span data-stu-id="a3e70-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="a3e70-137">Dazu zählen:</span><span class="sxs-lookup"><span data-stu-id="a3e70-137">They include:</span></span>

- <span data-ttu-id="a3e70-138">Steuerelement</span><span class="sxs-lookup"><span data-stu-id="a3e70-138">Control</span></span>
- <span data-ttu-id="a3e70-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="a3e70-139">Cookie</span></span>
- <span data-ttu-id="a3e70-140">Formular</span><span class="sxs-lookup"><span data-stu-id="a3e70-140">Form</span></span>
- <span data-ttu-id="a3e70-141">Profile</span><span class="sxs-lookup"><span data-stu-id="a3e70-141">Profile</span></span>
- <span data-ttu-id="a3e70-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="a3e70-142">QueryString</span></span>
- <span data-ttu-id="a3e70-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="a3e70-143">RouteData</span></span>
- <span data-ttu-id="a3e70-144">Sitzung</span><span class="sxs-lookup"><span data-stu-id="a3e70-144">Session</span></span>
- <span data-ttu-id="a3e70-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="a3e70-145">UserProfile</span></span>
- <span data-ttu-id="a3e70-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="a3e70-146">ViewState</span></span>

<span data-ttu-id="a3e70-147">In diesem Tutorial verwenden Sie den Wert eines Steuerelements um zu filtern, welche Datensätze in den GridView-Ansicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a3e70-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="a3e70-148">Fügen Sie der **Steuerelement** -Attribut auf die Query-Methode, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="a3e70-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="a3e70-149">In einem [später](using-query-string-values-to-retrieve-data.md) Tutorial übernehmen Sie die **QueryString** -Attribut auf einen Parameter, um anzugeben, dass der Wert des Parameters den Wert einer Abfragezeichenfolge stammen.</span><span class="sxs-lookup"><span data-stu-id="a3e70-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="a3e70-150">Fügen Sie zunächst über die ValidationSummary eine Dropdownliste zum Filtern der Schüler/Studenten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a3e70-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="a3e70-151">Klicken Sie in der CodeBehind-Datei ändern Sie die select-Methode Empfang ein Werts aus dem Steuerelement, und legen Sie den Namen des Parameters auf den Namen des Steuerelements, das den Wert bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="a3e70-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="a3e70-152">Müssen Sie hinzufügen, eine **mit** -Anweisung für die **System.Web.ModelBinding** das Steuerelementattribut aufzulösende Namespace.</span><span class="sxs-lookup"><span data-stu-id="a3e70-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="a3e70-153">Der folgende Code zeigt die select-Methode erneut gearbeitet, um die zurückgegebenen Daten basierend auf dem Wert des Dropdown-Liste zu filtern.</span><span class="sxs-lookup"><span data-stu-id="a3e70-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="a3e70-154">Ein Control-Attribut hinzufügen, bevor ein Parameter gibt an, dass der Wert für diesen Parameter aus einem Steuerelement mit dem gleichen Namen stammt.</span><span class="sxs-lookup"><span data-stu-id="a3e70-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="a3e70-155">Führen Sie die Webanwendung aus, und wählen Sie unterschiedliche Werte aus der Dropdown-Liste aus, um die Liste der Studenten zu filtern.</span><span class="sxs-lookup"><span data-stu-id="a3e70-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filter Schüler/Studenten](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="a3e70-157">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="a3e70-157">Conclusion</span></span>

<span data-ttu-id="a3e70-158">In diesem Tutorial haben aktiviert Sie sortieren und paging von Daten.</span><span class="sxs-lookup"><span data-stu-id="a3e70-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="a3e70-159">Sie wird aktiviert, die Daten durch den Wert eines Steuerelements zu filtern.</span><span class="sxs-lookup"><span data-stu-id="a3e70-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="a3e70-160">In den nächsten [Tutorial](integrating-jquery-ui.md) werden Sie die Benutzeroberfläche durch die Integration eines JQuery UI-Widgets in der Vorlage für die dynamische Daten verbessern.</span><span class="sxs-lookup"><span data-stu-id="a3e70-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3e70-161">[Zurück](updating-deleting-and-creating-data.md)
> [Weiter](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="a3e70-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
