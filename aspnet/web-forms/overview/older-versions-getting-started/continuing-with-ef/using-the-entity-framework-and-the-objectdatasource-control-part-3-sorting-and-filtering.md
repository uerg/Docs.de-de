---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 3: Sortieren und Filtern | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0-Tutorial-Reihe erstellt wird. ICH...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 15d9a7ecd3119af02576cbcf728828353748a513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806997"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="954eb-104">Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 3: Sortieren und Filtern</span><span class="sxs-lookup"><span data-stu-id="954eb-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="954eb-105">durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="954eb-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="954eb-106">Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Tutorial-Reihe.</span><span class="sxs-lookup"><span data-stu-id="954eb-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="954eb-107">Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden.</span><span class="sxs-lookup"><span data-stu-id="954eb-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="954eb-108">Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="954eb-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="954eb-109">Wenn Sie Fragen zu den Lernprogrammen haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="954eb-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="954eb-110">Im vorherigen Tutorial, die Sie implementiert des Repository-Musters in einer n-schichtige Webanwendung, die das Entity Framework verwendet und die `ObjectDataSource` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="954eb-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="954eb-111">Dieses Tutorial veranschaulicht das Sortieren und Filtern und Verarbeiten von Master / Detail-Szenarien.</span><span class="sxs-lookup"><span data-stu-id="954eb-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="954eb-112">Fügen Sie die folgenden Erweiterungen der *Departments.aspx* Seite:</span><span class="sxs-lookup"><span data-stu-id="954eb-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="954eb-113">Ein Textfeld, um den Benutzern ermöglichen, Abteilungen anhand des Namens auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="954eb-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="954eb-114">Eine Liste der Kurse jeder Abteilung, die im Raster angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="954eb-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="954eb-115">Die Fähigkeit, indem Sie auf Spaltenüberschriften sortieren.</span><span class="sxs-lookup"><span data-stu-id="954eb-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="954eb-116">[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="954eb-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="954eb-117">Hinzufügen der Funktion zum Sortieren von GridView-Spalten</span><span class="sxs-lookup"><span data-stu-id="954eb-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="954eb-118">Öffnen der *Departments.aspx* Seite, und fügen eine `SortParameterName="sortExpression"` -Attribut auf die `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="954eb-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="954eb-119">(Später erstellen Sie eine `GetDepartments` -Methode, die einen namens Parameter `sortExpression`.) Das Markup für das Anfangstag des Steuerelements ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="954eb-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="954eb-120">Hinzufügen der `AllowSorting="true"` Attribut an das öffnende Tag das `GridView` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="954eb-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="954eb-121">Das Markup für das Anfangstag des Steuerelements ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="954eb-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="954eb-122">In *Departments.aspx.cs*, legen Sie die Standardsortierreihenfolge durch Aufrufen der `GridView` des Steuerelements `Sort` Methode aus der `Page_Load` Methode:</span><span class="sxs-lookup"><span data-stu-id="954eb-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="954eb-123">Sie können den Code, der zum Sortieren oder Filtern in der Business Logic-Klasse oder die "Repository"-Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="954eb-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="954eb-124">Wenn der Business Logic-Klasse verwendet wird, Sortier- oder Filterkriterien Arbeit ausgeführt, nachdem die Daten aus der Datenbank abgerufen werden, da die Business Logic-Klasse mit arbeitet ein `IEnumerable` Repository zurückgegebenes Objekt.</span><span class="sxs-lookup"><span data-stu-id="954eb-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="954eb-125">Wenn Sie sortieren und Filtern von Code in der repositoryklasse hinzufügen, und Sie dies, bevor Sie eine LINQ-Ausdruck tun oder Objektabfrage konvertiert wurde ein `IEnumerable` Objekts, die Befehle werden durch übergeben werden, in der Datenbank für die Verarbeitung, die in der Regel effizienter ist.</span><span class="sxs-lookup"><span data-stu-id="954eb-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="954eb-126">In diesem Tutorial implementieren Sie sortieren und Filtern in einer Weise, die bewirkt, dass die Verarbeitung von der Datenbank ausgeführt werden – d. h. im Repository.</span><span class="sxs-lookup"><span data-stu-id="954eb-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="954eb-127">Um Sortierung Funktion hinzuzufügen, müssen Sie die Repository-Schnittstelle und Repositoryklassen auch hinsichtlich der Business Logic-Klasse eine neue Methode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="954eb-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="954eb-128">In der *ISchoolRepository.cs* Datei, fügen Sie einen neuen `GetDepartments` Methode, eine `sortExpression` Parameter, der verwendet wird, um die Liste von Abteilungen zu sortieren, die zurückgegeben wird:</span><span class="sxs-lookup"><span data-stu-id="954eb-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="954eb-129">Die `sortExpression` -Parameter wird die Spalte, nach denen sortiert und die Sortierreihenfolge angeben.</span><span class="sxs-lookup"><span data-stu-id="954eb-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="954eb-130">Fügen Sie Code für die neue Methode, um die *SchoolRepository.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="954eb-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="954eb-131">Ändern Sie die vorhandene parameterlosen `GetDepartments` Methode, die die neue Methode aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="954eb-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="954eb-132">Fügen Sie in das Testprojekt, und die folgende neue Methode zum *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="954eb-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="954eb-133">Wenn Sie möchten alle Komponententests zu erstellen, die von dieser Methode zurückgeben einer sortierten Liste abhing, müssen Sie würde die Liste zu sortieren, bevor sie zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="954eb-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="954eb-134">Tests wird nicht wie in diesem Tutorial erstellen Sie damit die Methode nur die unsortierte Liste von Abteilungen zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="954eb-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="954eb-135">In der *SchoolBL.cs* Datei, die der Business Logic-Klasse die folgende neue Methode hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="954eb-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="954eb-136">Dieser Code übergibt der Sortierparameter an die Repository-Methode.</span><span class="sxs-lookup"><span data-stu-id="954eb-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="954eb-137">Führen Sie die *Departments.aspx* Seite.</span><span class="sxs-lookup"><span data-stu-id="954eb-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="954eb-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="954eb-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="954eb-139">Sie können jetzt eine beliebige Spaltenüberschrift, um nach dieser Spalte sortieren klicken.</span><span class="sxs-lookup"><span data-stu-id="954eb-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="954eb-140">Wenn die Spalte bereits sortiert ist, kehrt das Klicken auf die Überschrift die Sortierrichtung.</span><span class="sxs-lookup"><span data-stu-id="954eb-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="954eb-141">Hinzufügen eines Suchfelds</span><span class="sxs-lookup"><span data-stu-id="954eb-141">Adding a Search Box</span></span>

<span data-ttu-id="954eb-142">In diesem Abschnitt werden Sie ein Textfeld Suche hinzufügen, verknüpfen Sie es mit der `ObjectDataSource` Steuerelement, mit einem Steuerelementparameter, und fügen Sie eine Methode zur Business Logic Klasse, um die Filterung von versionsbereichen.</span><span class="sxs-lookup"><span data-stu-id="954eb-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="954eb-143">Öffnen der *Departments.aspx* Seite, und fügen Sie das folgende Markup zwischen die Überschrift und die erste `ObjectDataSource` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="954eb-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="954eb-144">In der `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="954eb-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="954eb-145">Hinzufügen einer `SelectParameters` -Element für einen Parameter namens `nameSearchString` abruft, die in eingegebenen Wert die `SearchTextBox` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="954eb-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="954eb-146">Ändern der `SelectMethod` -Attributwert auf `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="954eb-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="954eb-147">(Sie müssen diese Methode später erstellen.)</span><span class="sxs-lookup"><span data-stu-id="954eb-147">(You'll create this method later.)</span></span>

<span data-ttu-id="954eb-148">Das Markup für die `ObjectDataSource` Steuerelement wird jetzt im folgende Beispiel ähnelt:</span><span class="sxs-lookup"><span data-stu-id="954eb-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="954eb-149">In *ISchoolRepository.cs*, Hinzufügen einer `GetDepartmentsByName` Methode, die beide akzeptiert `sortExpression` und `nameSearchString` Parameter:</span><span class="sxs-lookup"><span data-stu-id="954eb-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="954eb-150">In *SchoolRepository.cs*, fügen Sie die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="954eb-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="954eb-151">Dieser Code verwendet ein `Where` Methode zum Auswählen von Elementen, die die Suchzeichenfolge enthalten.</span><span class="sxs-lookup"><span data-stu-id="954eb-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="954eb-152">Wenn die Suchzeichenfolge leer ist, werden alle Datensätze ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="954eb-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="954eb-153">Beachten Sie, dass, wenn Sie die Methode ruft zusammen in einer Anweisung wie folgt angeben (`Include`, klicken Sie dann `OrderBy`, klicken Sie dann `Where`), wird die `Where` Methode muss immer in der letzten.</span><span class="sxs-lookup"><span data-stu-id="954eb-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="954eb-154">Ändern Sie die vorhandene `GetDepartments` Methode, eine `sortExpression` Parameter, um die neue Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="954eb-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="954eb-155">In *MockSchoolRepository.cs* in das Testprojekt, und fügen Sie die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="954eb-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="954eb-156">In *SchoolBL.cs*, fügen Sie die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="954eb-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="954eb-157">Führen Sie die *Departments.aspx* Seite, und geben Sie eine Suchzeichenfolge, um sicherzustellen, dass die Auswahllogik funktioniert.</span><span class="sxs-lookup"><span data-stu-id="954eb-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="954eb-158">Lassen Sie das Textfeld leer, und versuchen Sie es einer Suche, um sicherzustellen, dass alle Datensätze zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="954eb-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="954eb-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="954eb-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="954eb-160">Hinzufügen einer Detailspalte für jede Zeile des Datenblatts</span><span class="sxs-lookup"><span data-stu-id="954eb-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="954eb-161">Als Nächstes möchten Sie alle Kurse für jede Abteilung in der rechten Zelle des Rasters angezeigt.</span><span class="sxs-lookup"><span data-stu-id="954eb-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="954eb-162">Zu diesem Zweck verwenden Sie eine geschachtelte `GridView` Kontrolle und Databind damit Daten aus der `Courses` Navigationseigenschaft der `Department` Entität.</span><span class="sxs-lookup"><span data-stu-id="954eb-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="954eb-163">Open *Departments.aspx* und im Markup für die `GridView` zu steuern, geben Sie einen Handler für die `RowDataBound` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="954eb-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="954eb-164">Das Markup für das Anfangstag des Steuerelements ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="954eb-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="954eb-165">Fügen Sie einen neuen `TemplateField` Element an, nach der `Administrator` Vorlagenfeld:</span><span class="sxs-lookup"><span data-stu-id="954eb-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="954eb-166">Dieses Markup erstellt eine geschachtelte `GridView` Steuerelement, das der Kursnummer und Titel der Liste der Kurse anzeigt.</span><span class="sxs-lookup"><span data-stu-id="954eb-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="954eb-167">Es wird eine Datenquelle nicht angegeben, da Databind wird es im Code in die `RowDataBound` Handler.</span><span class="sxs-lookup"><span data-stu-id="954eb-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="954eb-168">Open *Departments.aspx.cs* und fügen Sie den folgenden Ereignishandler für die `RowDataBound` Ereignis:</span><span class="sxs-lookup"><span data-stu-id="954eb-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="954eb-169">Dieser Code Ruft die `Department` Entität aus den Ereignisargumenten, konvertiert die `Courses` Navigationseigenschaft für eine `List` Sammlung und stellt die geschachtelte `GridView` der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="954eb-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="954eb-170">Öffnen der *SchoolRepository.cs* Datei, und geben Sie eager Loading für die `Courses` Navigationseigenschaft durch Aufrufen der `Include` -Methode in der die Objektabfrage, die Sie, in Erstellen der `GetDepartmentsByName` Methode.</span><span class="sxs-lookup"><span data-stu-id="954eb-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="954eb-171">Die `return` -Anweisung in der `GetDepartmentsByName` Methode ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="954eb-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="954eb-172">Führen Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="954eb-172">Run the page.</span></span> <span data-ttu-id="954eb-173">Neben den sortieren und Filtern die Funktion, die Sie zuvor hinzugefügt haben, zeigt das GridView-Steuerelement jetzt geschachtelte Kursdetails für jede Abteilung.</span><span class="sxs-lookup"><span data-stu-id="954eb-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="954eb-174">[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="954eb-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="954eb-175">Dies schließt die Einführung zu sortieren, Filtern und Master / Detail-Szenarien ab.</span><span class="sxs-lookup"><span data-stu-id="954eb-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="954eb-176">Im nächsten Tutorial sehen Sie, wie Parallelität behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="954eb-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="954eb-177">[Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="954eb-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
