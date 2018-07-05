---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 495489479ef912afcb89c267b56fb11e07f959ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374729"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="b68cc-104">Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und Web forms</span><span class="sxs-lookup"><span data-stu-id="b68cc-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="b68cc-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b68cc-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b68cc-106">Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="b68cc-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="b68cc-107">Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement).</span><span class="sxs-lookup"><span data-stu-id="b68cc-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="b68cc-108">Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.</span><span class="sxs-lookup"><span data-stu-id="b68cc-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="b68cc-109">In diesem Tutorial veranschaulicht einen Wert in der Abfragezeichenfolge übergeben, und verwenden diesen Wert zum Abrufen von Daten über die modellbindung an.</span><span class="sxs-lookup"><span data-stu-id="b68cc-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="b68cc-110">Dieses Tutorial baut auf das Projekt erstellt haben, der [früheren](retrieving-data.md) Teilen der Reihe.</span><span class="sxs-lookup"><span data-stu-id="b68cc-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="b68cc-111">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB.</span><span class="sxs-lookup"><span data-stu-id="b68cc-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="b68cc-112">Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b68cc-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="b68cc-113">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b68cc-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="b68cc-114">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="b68cc-114">What you'll build</span></span>

<span data-ttu-id="b68cc-115">In diesem Tutorial müssen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="b68cc-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="b68cc-116">Hinzufügen einer neuen Seite, um die registrierten Kurse für Schüler/Student anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b68cc-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="b68cc-117">Abrufen der registrierten Kurse für den ausgewählten Studenten, die basierend auf einem Wert in der Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="b68cc-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="b68cc-118">Fügen Sie einen Link mit dem Wert einer Abfragezeichenfolge aus der Rasteransicht auf die neue Seite hinzu</span><span class="sxs-lookup"><span data-stu-id="b68cc-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="b68cc-119">Die Schritte in diesem Tutorial sind recht ähnlich wie für Sie in den früheren [Tutorial](sorting-paging-and-filtering-data.md) auf der Grundlage der Benutzerauswahl in einem Dropdown-Liste angezeigten Kursteilnehmer zu filtern.</span><span class="sxs-lookup"><span data-stu-id="b68cc-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="b68cc-120">In diesem Tutorial haben Sie verwendet die **Steuerelement** -Attribut in der select-Methode an, dass der Wert des Parameters aus einem Steuerelement stammt.</span><span class="sxs-lookup"><span data-stu-id="b68cc-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="b68cc-121">In diesem Tutorial verwenden Sie die **QueryString** -Attribut in der select-Methode an, dass der Wert des Parameters aus der Abfragezeichenfolge stammt.</span><span class="sxs-lookup"><span data-stu-id="b68cc-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="b68cc-122">Fügen Sie neue Seite für die Anzeige eines Studenten Kurse</span><span class="sxs-lookup"><span data-stu-id="b68cc-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="b68cc-123">Fügen Sie ein neues Webformular, das die Stammwebsite verwendet, und benennen Sie die Seite **Kurse**.</span><span class="sxs-lookup"><span data-stu-id="b68cc-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="b68cc-124">In der **Courses.aspx** hinzufügen eine Rasteransicht, um die Kurse für den ausgewählten Studenten angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="b68cc-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="b68cc-125">Definieren Sie die select-Methode</span><span class="sxs-lookup"><span data-stu-id="b68cc-125">Define the select method</span></span>

<span data-ttu-id="b68cc-126">In **Courses.aspx.cs**, fügen Sie die select-Methode mit dem Namen, die Sie, in der Rasteransicht angegeben **SelectMethod** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b68cc-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="b68cc-127">In dieser Methode müssen Sie definieren Sie eine Abfrage zum Abrufen eines Studenten, Kurse, und geben an, dass der Parameter den Wert einer Abfragezeichenfolge mit dem gleichen Namen wie der Parameter stammen.</span><span class="sxs-lookup"><span data-stu-id="b68cc-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="b68cc-128">Zunächst müssen Sie die folgende hinzufügen **mit** Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="b68cc-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="b68cc-129">Anschließend fügen Sie den folgenden Code, um Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="b68cc-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="b68cc-130">Das QueryString-Attribut bedeutet, dass der Parameter in dieser Methode automatisch ein Abfragezeichenfolgenwert mit dem Namen "StudentID" zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="b68cc-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="b68cc-131">Link mit dem Wert der Abfragezeichenfolge hinzufügen</span><span class="sxs-lookup"><span data-stu-id="b68cc-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="b68cc-132">In der Rasteransicht auf Students.aspx fügen Sie ein Hyperlink-Feld, das mit Ihrer neuen Kurse-Seite verknüpft.</span><span class="sxs-lookup"><span data-stu-id="b68cc-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="b68cc-133">Der Link enthält einen Abfragezeichenfolgenwert mit die Id des Studierenden.</span><span class="sxs-lookup"><span data-stu-id="b68cc-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="b68cc-134">Fügen Sie in Students.aspx das folgende Feld für die gesamten Gutschriften auf die Spalten im Datenblatt Ansicht direkt unter dem Feld hinzu.</span><span class="sxs-lookup"><span data-stu-id="b68cc-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="b68cc-135">Führen Sie die Anwendung, und beachten Sie, dass die Rasteransicht jetzt den Link für die Kurse enthält.</span><span class="sxs-lookup"><span data-stu-id="b68cc-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Link hinzufügen](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="b68cc-137">Wenn Sie einen der Links klicken, sehen Sie die registrierten Kurse des Studenten.</span><span class="sxs-lookup"><span data-stu-id="b68cc-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Kurse anzeigen](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="b68cc-139">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="b68cc-139">Conclusion</span></span>

<span data-ttu-id="b68cc-140">In diesem Tutorial haben Sie eine Verbindung mit dem Wert einer Abfragezeichenfolge hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b68cc-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="b68cc-141">Sie verwendet, Abfragezeichenfolgen-Werts für den Parameterwert in der select-Methode.</span><span class="sxs-lookup"><span data-stu-id="b68cc-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="b68cc-142">In den nächsten [Tutorial](adding-business-logic-layer.md), Sie den Code aus dem Code-Behind-Dateien in einer Geschäftslogikebene und eine Datenzugriffsschicht verschoben.</span><span class="sxs-lookup"><span data-stu-id="b68cc-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b68cc-143">[Zurück](integrating-jquery-ui.md)
> [Weiter](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="b68cc-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
