---
title: "Razor-Seiten mit EF-Core - Aktualisieren von verknüpften Daten - 7, 8"
author: rick-anderson
description: "In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren, durch die Aktualisierung von foreign Key-Felder und Navigationseigenschaften."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 5c91c91ab938f3aa4abc55049c54f399469f6163
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="07c26-103">Aktualisieren von verknüpften Daten - EF Core Razor-Seiten (7 von 8)</span><span class="sxs-lookup"><span data-stu-id="07c26-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="07c26-104">Durch [Tom Dykstra](https://github.com/tdykstra), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="07c26-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="07c26-105">Dieses Lernprogramm veranschaulicht aktualisieren verknüpfte Daten.</span><span class="sxs-lookup"><span data-stu-id="07c26-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="07c26-106">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="07c26-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="07c26-107">In der folgenden Abbildung werden einige der abgeschlossenen Seiten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="07c26-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="07c26-108">![Bearbeiten (Seite) Kurs](update-related-data/_static/course-edit.png)
![Seite Dozenten bearbeiten](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="07c26-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="07c26-109">Überprüfen Sie und Testen Sie die Kurs-Seiten erstellen und bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="07c26-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="07c26-110">Erstellen Sie einen neuen Kurs.</span><span class="sxs-lookup"><span data-stu-id="07c26-110">Create a new course.</span></span> <span data-ttu-id="07c26-111">Abteilung wird nach dem Primärschlüssel (eine Ganzzahl), nicht seinen Namen ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="07c26-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="07c26-112">Bearbeiten Sie den neuen Kurs.</span><span class="sxs-lookup"><span data-stu-id="07c26-112">Edit the new course.</span></span> <span data-ttu-id="07c26-113">Wenn Sie die Tests abgeschlossen haben, löschen Sie den neuen Kurs.</span><span class="sxs-lookup"><span data-stu-id="07c26-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="07c26-114">Erstellen Sie eine Basisklasse zum gemeinsamen Code nutzen</span><span class="sxs-lookup"><span data-stu-id="07c26-114">Create a base class to share common code</span></span>

<span data-ttu-id="07c26-115">Über die Seiten Kurse/Create "und" Kurse/bearbeiten wird eine Liste der Abteilungsnamen benötigen.</span><span class="sxs-lookup"><span data-stu-id="07c26-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="07c26-116">Erstellen der *Pages/Courses/DepartmentNamePageModel.cshtml.cs* Basisklasse für die Seiten erstellen und bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="07c26-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="07c26-117">Der vorangehende Code erstellt ein [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) enthält die Liste aller Abteilungsnamen.</span><span class="sxs-lookup"><span data-stu-id="07c26-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="07c26-118">Wenn `selectedDepartment` angegeben ist, wird dieser Abteilung ausgewählt ist, der `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="07c26-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="07c26-119">Das Erstellen und Bearbeiten von Seite Modellklassen abgeleitet `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="07c26-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="07c26-120">Anpassen der Courses-Seiten</span><span class="sxs-lookup"><span data-stu-id="07c26-120">Customize the Courses Pages</span></span>

<span data-ttu-id="07c26-121">Wenn eine neue Kurs-Entität erstellt wird, muss sie eine Beziehung zu einer vorhandenen Abteilung verfügen.</span><span class="sxs-lookup"><span data-stu-id="07c26-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="07c26-122">Zum Hinzufügen einer Abteilung beim Erstellen eines Kurs enthält die Basisklasse für erstellen und bearbeiten eine Dropdownliste zum Auswählen der Abteilung.</span><span class="sxs-lookup"><span data-stu-id="07c26-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="07c26-123">Im Dropdown-Liste wird der `Course.DepartmentID` Fremdschlüssel (FS)-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07c26-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="07c26-124">EF Kern verwendet die `Course.DepartmentID` FK beim Laden der `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07c26-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Kurs erstellen](update-related-data/_static/ddl.png)

<span data-ttu-id="07c26-126">Aktualisieren Sie das Modell erstellen, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="07c26-127">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-127">The preceding code:</span></span>

* <span data-ttu-id="07c26-128">Wird von `DepartmentNamePageModel` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="07c26-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="07c26-129">Verwendet `TryUpdateModelAsync` verhindern [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="07c26-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="07c26-130">Ersetzt `ViewData["DepartmentID"]` mit `DepartmentNameSL` (von der Basisklasse).</span><span class="sxs-lookup"><span data-stu-id="07c26-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="07c26-131">`ViewData["DepartmentID"]`ersetzt mit strikter typbindung `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="07c26-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="07c26-132">Stark typisierte Modelle sind über bevorzugte schwach typisiert.</span><span class="sxs-lookup"><span data-stu-id="07c26-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="07c26-133">Weitere Informationen finden Sie unter [schwach typisierte Daten (ViewData und ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="07c26-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="07c26-134">Aktualisieren der Seite "Kurse erstellen"</span><span class="sxs-lookup"><span data-stu-id="07c26-134">Update the Courses Create page</span></span>

<span data-ttu-id="07c26-135">Update *Pages/Courses/Create.cshtml* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="07c26-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="07c26-136">Das vorhergehende Markup nimmt folgende Änderungen:</span><span class="sxs-lookup"><span data-stu-id="07c26-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="07c26-137">Ändert die Beschriftung von **"DepartmentID"** auf **Abteilung**.</span><span class="sxs-lookup"><span data-stu-id="07c26-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="07c26-138">Ersetzt `"ViewBag.DepartmentID"` mit `DepartmentNameSL` (von der Basisklasse).</span><span class="sxs-lookup"><span data-stu-id="07c26-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="07c26-139">Fügt die Option "Select-Abteilung".</span><span class="sxs-lookup"><span data-stu-id="07c26-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="07c26-140">Diese Änderung wird anstelle der ersten Abteilung "Select-Abteilung" gerendert.</span><span class="sxs-lookup"><span data-stu-id="07c26-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="07c26-141">Fügt eine validierungsmeldung angezeigt, wenn die Abteilung nicht ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="07c26-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="07c26-142">Die Razor-Seite verwendet der [wählen Sie Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="07c26-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="07c26-143">Testen Sie die Seite "erstellen".</span><span class="sxs-lookup"><span data-stu-id="07c26-143">Test the Create page.</span></span> <span data-ttu-id="07c26-144">Die Seite "erstellen" zeigt an, den Abteilungsnamen, anstatt die Department-ID.</span><span class="sxs-lookup"><span data-stu-id="07c26-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="07c26-145">Aktualisieren Sie die Seite Kurse zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="07c26-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="07c26-146">Aktualisieren Sie das Modell bearbeiten, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="07c26-147">Die Änderungen sind ähnlich, die in das Modell erstellen.</span><span class="sxs-lookup"><span data-stu-id="07c26-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="07c26-148">Im vorangehenden Code `PopulateDepartmentsDropDownList` übergibt die Department-ID, die die Abteilung, angegeben in der Dropdown-Liste auswählen.</span><span class="sxs-lookup"><span data-stu-id="07c26-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="07c26-149">Update *Pages/Courses/Edit.cshtml* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="07c26-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="07c26-150">Das vorhergehende Markup nimmt folgende Änderungen:</span><span class="sxs-lookup"><span data-stu-id="07c26-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="07c26-151">Zeigt die Kurs-ID an.</span><span class="sxs-lookup"><span data-stu-id="07c26-151">Displays the course ID.</span></span> <span data-ttu-id="07c26-152">Der primäre Schlüssel (PK) einer Entität werden im Allgemeinen nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="07c26-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="07c26-153">PKs sind in der Regel für Benutzer ohne Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="07c26-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="07c26-154">In diesem Fall ist der Primärschlüssel der Kurs Anzahl.</span><span class="sxs-lookup"><span data-stu-id="07c26-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="07c26-155">Ändert die Beschriftung von **"DepartmentID"** auf **Abteilung**.</span><span class="sxs-lookup"><span data-stu-id="07c26-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="07c26-156">Ersetzt `"ViewBag.DepartmentID"` mit `DepartmentNameSL` (von der Basisklasse).</span><span class="sxs-lookup"><span data-stu-id="07c26-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="07c26-157">Fügt die Option "Select-Abteilung".</span><span class="sxs-lookup"><span data-stu-id="07c26-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="07c26-158">Diese Änderung wird anstelle der ersten Abteilung "Select-Abteilung" gerendert.</span><span class="sxs-lookup"><span data-stu-id="07c26-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="07c26-159">Fügt eine validierungsmeldung angezeigt, wenn die Abteilung nicht ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="07c26-159">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="07c26-160">Die Seite enthält ein ausgeblendetes Feld (`<input type="hidden">`) für die Kurs-Anzahl.</span><span class="sxs-lookup"><span data-stu-id="07c26-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="07c26-161">Hinzufügen einer `<label>` tag Hilfsprogramm mit `asp-for="Course.CourseID"` nicht das ausgeblendete Feld überflüssig.</span><span class="sxs-lookup"><span data-stu-id="07c26-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="07c26-162">`<input type="hidden">`ist erforderlich, damit die Anzahl der Kurs in die bereitgestellten Daten aufgenommen werden, wenn der Benutzer klickt **speichern**.</span><span class="sxs-lookup"><span data-stu-id="07c26-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="07c26-163">Testen des aktualisierten Codes.</span><span class="sxs-lookup"><span data-stu-id="07c26-163">Test the updated code.</span></span> <span data-ttu-id="07c26-164">Erstellen, bearbeiten und einen Kurs zu löschen.</span><span class="sxs-lookup"><span data-stu-id="07c26-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="07c26-165">AsNoTracking zu den Details hinzufügen und Löschen von Modellen Seite</span><span class="sxs-lookup"><span data-stu-id="07c26-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="07c26-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) kann die Leistung verbessern, wenn keine Überwachung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="07c26-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="07c26-167">Hinzufügen `AsNoTracking` zum Seitenmodell löschen und Details.</span><span class="sxs-lookup"><span data-stu-id="07c26-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="07c26-168">Der folgende Code zeigt die aktualisierte Seitenmodell löschen:</span><span class="sxs-lookup"><span data-stu-id="07c26-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="07c26-169">Update der `OnGetAsync` Methode in der *Pages/Courses/Details.cshtml.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="07c26-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="07c26-170">Ändern Sie die Seiten löschen und Details</span><span class="sxs-lookup"><span data-stu-id="07c26-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="07c26-171">Aktualisieren Sie das Löschen von Razor-Seite durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="07c26-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="07c26-172">Nehmen Sie die gleichen Änderungen an der Seite "Details" ein.</span><span class="sxs-lookup"><span data-stu-id="07c26-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="07c26-173">Testen Sie die Kurs-Seiten</span><span class="sxs-lookup"><span data-stu-id="07c26-173">Test the Course pages</span></span>

<span data-ttu-id="07c26-174">Test erstellen, bearbeiten, Details, und löschen.</span><span class="sxs-lookup"><span data-stu-id="07c26-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="07c26-175">Aktualisieren Sie die Instructor-Seiten</span><span class="sxs-lookup"><span data-stu-id="07c26-175">Update the instructor pages</span></span>

<span data-ttu-id="07c26-176">In den folgenden Abschnitten aktualisieren die Instructor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="07c26-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="07c26-177">Hinzufügen von Office-Speicherort</span><span class="sxs-lookup"><span data-stu-id="07c26-177">Add office location</span></span>

<span data-ttu-id="07c26-178">Wenn Sie einen Instructor-Eintrag bearbeiten zu können, empfiehlt es sich, den Dozenten bürozuweisung zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="07c26-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="07c26-179">Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="07c26-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="07c26-180">Die Instructor-Code zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="07c26-180">The instructor code must handle:</span></span>

* <span data-ttu-id="07c26-181">Wenn der Benutzer die Office-Zuweisung behoben wird, löschen Sie die `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="07c26-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="07c26-182">Wenn der Benutzer eine bürozuweisung gibt, und es leer war, erstellen Sie ein neues `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="07c26-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="07c26-183">Wenn der Benutzer die Zuweisung von Office ändert, Aktualisieren der `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="07c26-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="07c26-184">Aktualisieren Sie das Modell bearbeiten Lehrkräfte durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="07c26-185">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-185">The preceding code:</span></span>

- <span data-ttu-id="07c26-186">Ruft die aktuellen `Instructor` Entität aus der Datenbank mit unverzüglichem Laden für die `OfficeAssignment` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07c26-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="07c26-187">Aktualisiert die abgerufenen `Instructor` Entität mit Werten aus den Modellbinder.</span><span class="sxs-lookup"><span data-stu-id="07c26-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="07c26-188">`TryUpdateModel`verhindert, dass [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="07c26-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="07c26-189">Wenn der Niederlassung leer ist, legt `Instructor.OfficeAssignment` auf Null.</span><span class="sxs-lookup"><span data-stu-id="07c26-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="07c26-190">Wenn `Instructor.OfficeAssignment` null ist, ist die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="07c26-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="07c26-191">Aktualisieren Sie die Instructor bearbeiten (Seite)</span><span class="sxs-lookup"><span data-stu-id="07c26-191">Update the instructor Edit page</span></span>

<span data-ttu-id="07c26-192">Update *Pages/Instructors/Edit.cshtml* mit den Office-Speicherort:</span><span class="sxs-lookup"><span data-stu-id="07c26-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="07c26-193">Stellen Sie sicher, dass Sie einem Bürostandort Lehrkräfte ändern können.</span><span class="sxs-lookup"><span data-stu-id="07c26-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="07c26-194">Die Instructor-Bearbeitungsseite Kursaufgaben hinzufügen</span><span class="sxs-lookup"><span data-stu-id="07c26-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="07c26-195">Lehrkräfte können eine beliebige Anzahl von Kurse werden folgende Themen behandelt.</span><span class="sxs-lookup"><span data-stu-id="07c26-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="07c26-196">In diesem Abschnitt fügen Sie die Möglichkeit zum Ändern der Kurs Zuordnung hinzu.</span><span class="sxs-lookup"><span data-stu-id="07c26-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="07c26-197">Die folgende Abbildung zeigt den aktualisierten Dozenten bearbeiten (Seite):</span><span class="sxs-lookup"><span data-stu-id="07c26-197">The following image shows the updated instructor Edit page:</span></span>

![Instructor-Bearbeitungsseite mit Kurse](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="07c26-199">`Course`und `Instructor` weist eine m: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="07c26-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="07c26-200">Zum Hinzufügen und Entfernen von Beziehungen, hinzufügen und Entfernen von Entitäten aus der `CourseAssignments` Entitätenmenge verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="07c26-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="07c26-201">Kontrollkästchen aktivieren, Änderungen an der Kurse, denen ein Kursleiter zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="07c26-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="07c26-202">Ein Kontrollkästchen wird für jede Vorgehensweise in der Datenbank angezeigt.</span><span class="sxs-lookup"><span data-stu-id="07c26-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="07c26-203">Kurse, denen der Dozenten zugewiesen ist, werden überprüft.</span><span class="sxs-lookup"><span data-stu-id="07c26-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="07c26-204">Der Benutzer kann aktivieren oder deaktivieren Kontrollkästchen, um den Kurs Zuweisungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="07c26-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="07c26-205">Wenn die Anzahl der Kurse viel größer sind:</span><span class="sxs-lookup"><span data-stu-id="07c26-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="07c26-206">Wahrscheinlich verwenden Sie eine andere Benutzeroberfläche, um die Kurse anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="07c26-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="07c26-207">Die Methode zum Bearbeiten einer Joinentität zum Erstellen oder Löschen von Beziehungen würde nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="07c26-207">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="07c26-208">Hinzufügen von Klassen zur Unterstützung Instructor-Seiten erstellen und bearbeiten</span><span class="sxs-lookup"><span data-stu-id="07c26-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="07c26-209">Erstellen Sie *SchoolViewModels/AssignedCourseData.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="07c26-210">Die `AssignedCourseData` Klasse enthält Daten, um die Kontrollkästchen für zugewiesenen Kurse durch einen Kursleiter zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="07c26-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="07c26-211">Erstellen der *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* Basisklasse:</span><span class="sxs-lookup"><span data-stu-id="07c26-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="07c26-212">Die `InstructorCoursesPageModel` ist die Basisklasse, die Sie für die Bearbeitung und Seite Modelle erstellen.</span><span class="sxs-lookup"><span data-stu-id="07c26-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="07c26-213">`PopulateAssignedCourseData`Liest alle `Course` zum Auffüllen Entitäten `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="07c26-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="07c26-214">Für jeden Kurs der Code legt die `CourseID`, Titel und ob der Kurs Kursleiter zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="07c26-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="07c26-215">Ein [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) dient zum effizienten Suchvorgänge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="07c26-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="07c26-216">Seitenmodell für Lehrkräfte bearbeiten</span><span class="sxs-lookup"><span data-stu-id="07c26-216">Instructors Edit page model</span></span>

<span data-ttu-id="07c26-217">Aktualisieren Sie das Modell bearbeiten Dozenten durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="07c26-218">Der vorangehende Code verarbeitet Änderungen an der sicherheitsbereichszuweisung Office.</span><span class="sxs-lookup"><span data-stu-id="07c26-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="07c26-219">Aktualisieren Sie den Dozenten Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="07c26-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="07c26-220">Wenn Sie den Code in Visual Studio einfügen, werden Zeilenumbrüche in einer Weise geändert, die der Code unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="07c26-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="07c26-221">Drücken Sie einmal STRG + Z, um die automatische Formatierung rückgängig zu machen.</span><span class="sxs-lookup"><span data-stu-id="07c26-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="07c26-222">STRG + Z behebt die Zeilenumbrüche, damit sie sehen, wie Sie hier sehen.</span><span class="sxs-lookup"><span data-stu-id="07c26-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="07c26-223">Der Einzug keinen perfekt, aber die `@</tr><tr>`, `@:<td>`, `@:</td>`, und `@:</tr>` Zeilen jeweils muss in einer einzelnen Zeile wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="07c26-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="07c26-224">Drücken Sie mit dem Block von neuem Code ausgewählt ist Tab dreimal an den neuen Code mit dem vorhandenen Code.</span><span class="sxs-lookup"><span data-stu-id="07c26-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="07c26-225">Stimmen Sie auf, oder Überprüfen Sie den Status dieses Fehlers [mit diesem Link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="07c26-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="07c26-226">Der vorangehende Code erstellt eine HTML-Tabelle, die drei Spalten aufweist.</span><span class="sxs-lookup"><span data-stu-id="07c26-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="07c26-227">Jede Spalte verfügt über ein Kontrollkästchen und eine Beschriftung, die den Titel und Kursnummer enthält.</span><span class="sxs-lookup"><span data-stu-id="07c26-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="07c26-228">Die Kontrollkästchen, die alle haben den gleichen Namen ("SelectedCourses").</span><span class="sxs-lookup"><span data-stu-id="07c26-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="07c26-229">Mit dem gleichen Namen informiert den Modellbinder, um sie als eine Gruppe zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="07c26-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="07c26-230">Das Value-Attribut der einzelnen Kontrollkästchen wird festgelegt, um `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="07c26-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="07c26-231">Wenn die Seite zurückgesendet wird, übergibt der Modellbinder ein Array, aus denen besteht die `CourseID` Werte für nur die Kontrollkästchen, die ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="07c26-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="07c26-232">Wenn Sie die Kontrollkästchen gerendert werden, haben Kurse zu den Dozenten zugewiesen Attribute überprüft.</span><span class="sxs-lookup"><span data-stu-id="07c26-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="07c26-233">Führen Sie die app, und Testen Sie die aktualisierte Lehrkräfte bearbeiten (Seite).</span><span class="sxs-lookup"><span data-stu-id="07c26-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="07c26-234">Einige Kurs Zuweisungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="07c26-234">Change some course assignments.</span></span> <span data-ttu-id="07c26-235">Die Änderungen werden auf der Seite "Index" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="07c26-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="07c26-236">Hinweis: Ansatz wird hier zum Bearbeiten von Daten für Dozenten Kurs funktioniert gut, wenn eine begrenzte Anzahl von Kurse.</span><span class="sxs-lookup"><span data-stu-id="07c26-236">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="07c26-237">Für Sammlungen, die viel größer sind, werden eine andere Benutzeroberfläche und eine andere Methode für die Aktualisierung noch verwendbar und effektiver gebildet.</span><span class="sxs-lookup"><span data-stu-id="07c26-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="07c26-238">Aktualisieren der Lehrkräfte-Seite "erstellen"</span><span class="sxs-lookup"><span data-stu-id="07c26-238">Update the instructors Create page</span></span>

<span data-ttu-id="07c26-239">Aktualisieren Sie das Modell erstellen Dozenten durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="07c26-240">Der vorhergehende Code ist ähnlich wie die *Pages/Instructors/Edit.cshtml.cs* Code.</span><span class="sxs-lookup"><span data-stu-id="07c26-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="07c26-241">Aktualisieren Sie die Instructor-Erstellen von Razor-Seite durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="07c26-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="07c26-242">Testen Sie die Instructor-Seite erstellen.</span><span class="sxs-lookup"><span data-stu-id="07c26-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="07c26-243">Aktualisieren Sie die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="07c26-243">Update the Delete page</span></span>

<span data-ttu-id="07c26-244">Aktualisieren Sie das Modell löschen, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="07c26-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="07c26-245">Der vorangehende Code stellt die folgenden Änderungen:</span><span class="sxs-lookup"><span data-stu-id="07c26-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="07c26-246">Verwendet unverzüglichem Laden für die `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="07c26-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="07c26-247">`CourseAssignments`eingeschlossen werden müssen oder sie werden nicht gelöscht, wenn der Kursleiter gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="07c26-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="07c26-248">Um zu vermeiden, diese zu lesen, konfigurieren Sie kaskadierte Löschung in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="07c26-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="07c26-249">Die Instructor gelöscht werden soll als Administrator, der alle Abteilungen zugewiesen wird, entfernt die Instructor-Zuweisung von die Abteilungen.</span><span class="sxs-lookup"><span data-stu-id="07c26-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="07c26-250">[Zurück](xref:data/ef-rp/read-related-data)
[Weiter](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="07c26-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
