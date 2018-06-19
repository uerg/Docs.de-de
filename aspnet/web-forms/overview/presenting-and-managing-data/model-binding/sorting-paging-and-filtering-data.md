---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortieren, paging und Filtern von Daten mit modellbindung und WebForms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885405"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="e4721-104">Sortieren, paging und Filtern von Daten mit modellbindung und WebForms</span><span class="sxs-lookup"><span data-stu-id="e4721-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="e4721-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e4721-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e4721-106">Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="e4721-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e4721-107">Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="e4721-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e4721-108">Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="e4721-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e4721-109">Dieses Lernprogramm zeigt, wie sortieren, paging und Filtern der Daten wurden die modellbindung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e4721-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="e4721-110">Dieses Lernprogramm baut auf das Projekt erstellt, in der ersten [Teil](retrieving-data.md) der Reihe.</span><span class="sxs-lookup"><span data-stu-id="e4721-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="e4721-111">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar.</span><span class="sxs-lookup"><span data-stu-id="e4721-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e4721-112">Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e4721-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e4721-113">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4721-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e4721-114">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="e4721-114">What you'll build</span></span>

<span data-ttu-id="e4721-115">In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="e4721-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e4721-116">Aktivieren der Sortierung und paging von Daten</span><span class="sxs-lookup"><span data-stu-id="e4721-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="e4721-117">Aktivieren Sie Filtern der Daten basierend auf der Auswahl des Benutzers</span><span class="sxs-lookup"><span data-stu-id="e4721-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="e4721-118">Sortierung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="e4721-118">Add sorting</span></span>

<span data-ttu-id="e4721-119">Aktivieren der Sortierung in die GridView ist sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="e4721-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="e4721-120">Legen Sie einfach in die Datei Student.aspx **AllowSorting** auf **"true"** in die GridView.</span><span class="sxs-lookup"><span data-stu-id="e4721-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="e4721-121">Sie müssen nicht festlegen einer **SortExpression** Wert für jede Spalte, wie die DataField automatisch verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e4721-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="e4721-122">Die GridView ändert die Abfrage zum Anordnen der Daten nach dem ausgewählten Wert enthalten.</span><span class="sxs-lookup"><span data-stu-id="e4721-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="e4721-123">Der hervorgehobene Code unten zeigt dem Hinzufügen, dass Sie zum Aktivieren der Sortierung vornehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="e4721-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="e4721-124">Führen Sie die Webanwendung, und Testen Sie nach den Werten in verschiedenen Spalten sortieren Studentendatensätze.</span><span class="sxs-lookup"><span data-stu-id="e4721-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Studenten sortieren](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="e4721-126">Hinzufügen von paging</span><span class="sxs-lookup"><span data-stu-id="e4721-126">Add paging</span></span>

<span data-ttu-id="e4721-127">Aktivieren von Paging ist sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="e4721-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="e4721-128">Legen Sie in der GridView der **AllowPaging** Eigenschaft **"true"** und legen Sie die **PageSize** Eigenschaft, um die Anzahl der Datensätze, die auf jeder Seite angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e4721-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="e4721-129">In diesem Lernprogramm können Sie es auf 4 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e4721-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="e4721-130">Führen Sie die Webanwendung, und beachten Sie, dass nun die Datensätze mit mehr als 4 auf einer Seite angezeigten Datensätze über mehrere Seiten unterteilt sind.</span><span class="sxs-lookup"><span data-stu-id="e4721-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Hinzufügen von paging](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="e4721-132">Verzögerte abfrageausführung verbessert die anwendungseffizienz.</span><span class="sxs-lookup"><span data-stu-id="e4721-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="e4721-133">Anstatt das gesamte Dataset abgerufen werden, ändert die GridView die Abfrage aus, um nur die Datensätze für die aktuelle Seite abrufen.</span><span class="sxs-lookup"><span data-stu-id="e4721-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="e4721-134">Filtern von Datensätzen durch Auswahl des Benutzers</span><span class="sxs-lookup"><span data-stu-id="e4721-134">Filter records by user selection</span></span>

<span data-ttu-id="e4721-135">Wurden die modellbindung fügt mehrere Attribute, die Sie festlegen, wie Sie den Wert für einen Parameter in einem Modell Bindungsmethode festlegen können.</span><span class="sxs-lookup"><span data-stu-id="e4721-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="e4721-136">Diese Attribute sind der **System.Web.ModelBinding** Namespace.</span><span class="sxs-lookup"><span data-stu-id="e4721-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="e4721-137">Dazu zählen:</span><span class="sxs-lookup"><span data-stu-id="e4721-137">They include:</span></span>

- <span data-ttu-id="e4721-138">Steuerelement</span><span class="sxs-lookup"><span data-stu-id="e4721-138">Control</span></span>
- <span data-ttu-id="e4721-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="e4721-139">Cookie</span></span>
- <span data-ttu-id="e4721-140">Formular</span><span class="sxs-lookup"><span data-stu-id="e4721-140">Form</span></span>
- <span data-ttu-id="e4721-141">Profile</span><span class="sxs-lookup"><span data-stu-id="e4721-141">Profile</span></span>
- <span data-ttu-id="e4721-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="e4721-142">QueryString</span></span>
- <span data-ttu-id="e4721-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="e4721-143">RouteData</span></span>
- <span data-ttu-id="e4721-144">Sitzung</span><span class="sxs-lookup"><span data-stu-id="e4721-144">Session</span></span>
- <span data-ttu-id="e4721-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="e4721-145">UserProfile</span></span>
- <span data-ttu-id="e4721-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="e4721-146">ViewState</span></span>

<span data-ttu-id="e4721-147">In diesem Lernprogramm verwenden Sie den Wert eines Steuerelements um zu filtern, welche Datensätze in die GridView angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4721-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="e4721-148">Fügen Sie der **Steuerelement** -Attribut auf die Abfragemethode, die Sie zuvor erstellt hatten.</span><span class="sxs-lookup"><span data-stu-id="e4721-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="e4721-149">In einem [später](using-query-string-values-to-retrieve-data.md) Lernprogramm übernehmen Sie die **QueryString** -Attribut auf einen Parameter, um anzugeben, dass der Wert des Parameters einen Wert der Abfragezeichenfolge stammen.</span><span class="sxs-lookup"><span data-stu-id="e4721-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="e4721-150">Fügen Sie oberhalb der ValidationSummary zunächst eine Dropdown-Liste für das Filtern, welche Studenten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4721-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="e4721-151">In der Code-Behind-Datei ändern Sie die select-Methode Empfang ein Werts aus dem Steuerelement, und legen Sie den Namen des Parameters auf den Namen des Steuerelements, das den Wert bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="e4721-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="e4721-152">Müssen Sie hinzufügen, eine **mit** -Anweisung für die **System.Web.ModelBinding** Namespace das Steuerelementattribut aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="e4721-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="e4721-153">Der folgende Code zeigt die select-Methode erneut gearbeitet, um die zurückgegebenen Daten basierend auf dem Wert von der Dropdown-Liste zu filtern.</span><span class="sxs-lookup"><span data-stu-id="e4721-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="e4721-154">Hinzufügen eines Steuerelements-Attributs an, bevor ein Parameter gibt an, dass der Wert für diesen Parameter aus einem Steuerelement mit dem gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="e4721-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="e4721-155">Führen Sie die Webanwendung, und wählen Sie unterschiedliche Werte aus der Dropdown-Liste aus, um die Liste der Schüler zu filtern.</span><span class="sxs-lookup"><span data-stu-id="e4721-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filter Studenten](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="e4721-157">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="e4721-157">Conclusion</span></span>

<span data-ttu-id="e4721-158">In diesem Lernprogramm ermöglichte Sortierung und Auslagerung der Daten.</span><span class="sxs-lookup"><span data-stu-id="e4721-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="e4721-159">Sie wird aktiviert, Filtern von Daten durch den Wert eines Steuerelements.</span><span class="sxs-lookup"><span data-stu-id="e4721-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="e4721-160">In der nächsten [Lernprogramm](integrating-jquery-ui.md) Sie werden die Benutzeroberfläche erhöhen, indem das Integrieren von einem JQuery UI-Widget in der dynamic Data-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="e4721-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4721-161">[Zurück](updating-deleting-and-creating-data.md)
> [Weiter](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="e4721-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
