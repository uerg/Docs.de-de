---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und web Forms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886822"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="00b30-104">Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und WebForms</span><span class="sxs-lookup"><span data-stu-id="00b30-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="00b30-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="00b30-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="00b30-106">Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="00b30-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="00b30-107">Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="00b30-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="00b30-108">Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="00b30-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="00b30-109">Dieses Lernprogramm veranschaulicht, übergeben Sie den Wert in der Abfragezeichenfolge und diesen Wert zum Abrufen von Daten über die modellbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="00b30-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="00b30-110">Dieses Lernprogramm baut auf das Projekt erstellt, der [früheren](retrieving-data.md) Teile der Reihe.</span><span class="sxs-lookup"><span data-stu-id="00b30-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="00b30-111">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar.</span><span class="sxs-lookup"><span data-stu-id="00b30-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="00b30-112">Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="00b30-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="00b30-113">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="00b30-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="00b30-114">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="00b30-114">What you'll build</span></span>

<span data-ttu-id="00b30-115">In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="00b30-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="00b30-116">Fügen Sie eine neue Seite um ein Student die registrierten Kurse anzeigen</span><span class="sxs-lookup"><span data-stu-id="00b30-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="00b30-117">Abrufen der registrierten Kurse für die ausgewählten Studenten, die anhand eines Werts in der Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="00b30-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="00b30-118">Fügen Sie einen Link mit einem Zeichenfolgenwert für die Abfrage von der Rasteransicht, auf die neue Seite</span><span class="sxs-lookup"><span data-stu-id="00b30-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="00b30-119">Die Schritte in diesem Lernprogramm sind ähnlich, was Sie getan haben in den früheren [Lernprogramm](sorting-paging-and-filtering-data.md) zum Filtern der angezeigten Students, die basierend auf der Auswahl des Benutzers in einer Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="00b30-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="00b30-120">In diesem Lernprogramm verwendet Sie die **Steuerelement** -Attribut in der select-Methode an, dass der Wert des Parameters aus einem Steuerelement stammt.</span><span class="sxs-lookup"><span data-stu-id="00b30-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="00b30-121">In diesem Lernprogramm verwenden Sie die **QueryString** -Attribut in der select-Methode an, dass der Wert des Parameters aus der Abfragezeichenfolge stammt.</span><span class="sxs-lookup"><span data-stu-id="00b30-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="00b30-122">Fügen Sie neue Seite für ein Student Kurse anzeigen</span><span class="sxs-lookup"><span data-stu-id="00b30-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="00b30-123">Fügen Sie eine neue WebForm, die die Stammwebsite verwendet, und nennen Sie die Seite **Kurse**.</span><span class="sxs-lookup"><span data-stu-id="00b30-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="00b30-124">In der **Courses.aspx** Datei, fügen Sie eine Rasteransicht der Kurse für die ausgewählten Studenten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="00b30-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="00b30-125">Definieren Sie die select-Methode</span><span class="sxs-lookup"><span data-stu-id="00b30-125">Define the select method</span></span>

<span data-ttu-id="00b30-126">In **Courses.aspx.cs**, fügen Sie die select-Methode mit dem Namen in der Rasteransicht angegebenen **SelectMethod** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="00b30-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="00b30-127">In dieser Methode können Sie die Abfrage zum Abrufen einer Student Kurse zu definieren, und geben an, dass der Parameter einen Wert mit dem gleichen Namen wie der Parameter der Abfragezeichenfolge stammen.</span><span class="sxs-lookup"><span data-stu-id="00b30-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="00b30-128">Sie müssen zuerst, fügen Sie die folgenden **mit** Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="00b30-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="00b30-129">Anschließend fügen Sie den folgenden Code, um Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="00b30-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="00b30-130">Das QueryString-Attribut bedeutet, dass ein mit dem Namen StudentID Wert der Abfragezeichenfolge an den Parameter in dieser Methode automatisch zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="00b30-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="00b30-131">Mit dem Wert der Abfragezeichenfolge Link hinzufügen</span><span class="sxs-lookup"><span data-stu-id="00b30-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="00b30-132">In der Rasteransicht auf Students.aspx fügen Sie ein Hyperlinkfeld, das mit Ihrer neuen Kurse Seite verknüpft.</span><span class="sxs-lookup"><span data-stu-id="00b30-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="00b30-133">Der Hyperlink wird einen Wert der Abfragezeichenfolge mit den Studenten-Id enthalten.</span><span class="sxs-lookup"><span data-stu-id="00b30-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="00b30-134">Fügen Sie in Students.aspx das folgende Feld für das Gesamtguthaben für das Raster Sichtspalten direkt unter dem Feld hinzu.</span><span class="sxs-lookup"><span data-stu-id="00b30-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="00b30-135">Führen Sie die Anwendung, und beachten Sie, dass die Rasteransicht jetzt den Link Kurse enthält.</span><span class="sxs-lookup"><span data-stu-id="00b30-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Link hinzufügen](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="00b30-137">Wenn Sie einen der Links klicken, sehen Sie diese Student registrierten Kurse.</span><span class="sxs-lookup"><span data-stu-id="00b30-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Kurse anzeigen](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="00b30-139">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="00b30-139">Conclusion</span></span>

<span data-ttu-id="00b30-140">In diesem Lernprogramm haben Sie eine Verknüpfung mit einem Abfragezeichenfolgenwert hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="00b30-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="00b30-141">Dieser Wert der Abfragezeichenfolge wird verwendet, für den Wert des Parameters in der select-Methode.</span><span class="sxs-lookup"><span data-stu-id="00b30-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="00b30-142">In der nächsten [Lernprogramm](adding-business-logic-layer.md), Sie den Code aus dem Code-Behind-Dateien in eine Geschäftslogikschicht und eine Datenzugriffsschicht verschoben.</span><span class="sxs-lookup"><span data-stu-id="00b30-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00b30-143">[Zurück](integrating-jquery-ui.md)
> [Weiter](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="00b30-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
