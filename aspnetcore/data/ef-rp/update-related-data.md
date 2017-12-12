---
title: "Razor-Seiten mit EF-Core - Aktualisieren von verknüpften Daten - 7, 8"
author: rick-anderson
description: "In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren, durch die Aktualisierung von foreign Key-Felder und Navigationseigenschaften."
keywords: "ASP.NET Core, Entity Framework Core, verknüpfte Daten verknüpft."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: f07a33c19ba1be623fae14228f8fbc909d766816
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="0e802-104">Aktualisieren von verknüpften Daten - EF Core Razor-Seiten (7 von 8)</span><span class="sxs-lookup"><span data-stu-id="0e802-104">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="0e802-105">Durch [Tom Dykstra](https://github.com/tdykstra), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0e802-105">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="0e802-106">Dieses Lernprogramm veranschaulicht aktualisieren verknüpfte Daten.</span><span class="sxs-lookup"><span data-stu-id="0e802-106">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="0e802-107">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="0e802-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="0e802-108">In der folgenden Abbildung werden einige der abgeschlossenen Seiten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e802-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="0e802-109">![Bearbeiten (Seite) Kurs](update-related-data/_static/course-edit.png)
![Seite Dozenten bearbeiten](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="0e802-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="0e802-110">Überprüfen Sie und Testen Sie die Kurs-Seiten erstellen und bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0e802-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="0e802-111">Erstellen Sie einen neuen Kurs.</span><span class="sxs-lookup"><span data-stu-id="0e802-111">Create a new course.</span></span> <span data-ttu-id="0e802-112">Abteilung wird nach dem Primärschlüssel (eine Ganzzahl), nicht seinen Namen ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="0e802-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="0e802-113">Bearbeiten Sie den neuen Kurs.</span><span class="sxs-lookup"><span data-stu-id="0e802-113">Edit the new course.</span></span> <span data-ttu-id="0e802-114">Wenn Sie die Tests abgeschlossen haben, löschen Sie den neuen Kurs.</span><span class="sxs-lookup"><span data-stu-id="0e802-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="0e802-115">Erstellen Sie eine Basisklasse zum gemeinsamen Code nutzen</span><span class="sxs-lookup"><span data-stu-id="0e802-115">Create a base class to share common code</span></span>

<span data-ttu-id="0e802-116">Über die Seiten Kurse/Create "und" Kurse/bearbeiten wird eine Liste der Abteilungsnamen benötigen.</span><span class="sxs-lookup"><span data-stu-id="0e802-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="0e802-117">Erstellen der *Pages/Courses/DepartmentNamePageModel.cshtml.cs* Basisklasse für die Seiten erstellen und bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="0e802-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="0e802-118">Der vorangehende Code erstellt ein [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) enthält die Liste aller Abteilungsnamen.</span><span class="sxs-lookup"><span data-stu-id="0e802-118">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="0e802-119">Wenn `selectedDepartment` angegeben ist, wird dieser Abteilung ausgewählt ist, der `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="0e802-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="0e802-120">Das Erstellen und Bearbeiten von Seite Modellklassen abgeleitet `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="0e802-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="0e802-121">Anpassen der Courses-Seiten</span><span class="sxs-lookup"><span data-stu-id="0e802-121">Customize the Courses Pages</span></span>

<span data-ttu-id="0e802-122">Wenn eine neue Kurs-Entität erstellt wird, muss sie eine Beziehung zu einer vorhandenen Abteilung verfügen.</span><span class="sxs-lookup"><span data-stu-id="0e802-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="0e802-123">Zum Hinzufügen einer Abteilung beim Erstellen eines Kurs enthält die Basisklasse für erstellen und bearbeiten eine Dropdownliste zum Auswählen der Abteilung.</span><span class="sxs-lookup"><span data-stu-id="0e802-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="0e802-124">Im Dropdown-Liste wird der `Course.DepartmentID` Fremdschlüssel (FS)-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0e802-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="0e802-125">EF Kern verwendet die `Course.DepartmentID` FK beim Laden der `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0e802-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Kurs erstellen](update-related-data/_static/ddl.png)

<span data-ttu-id="0e802-127">Aktualisieren Sie das Modell erstellen, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-127">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="0e802-128">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-128">The preceding code:</span></span>

* <span data-ttu-id="0e802-129">Wird von `DepartmentNamePageModel` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="0e802-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="0e802-130">Verwendet `TryUpdateModelAsync` verhindern [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="0e802-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="0e802-131">Ersetzt `ViewData["DepartmentID"]` mit `DepartmentNameSL` (von der Basisklasse).</span><span class="sxs-lookup"><span data-stu-id="0e802-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="0e802-132">`ViewData["DepartmentID"]`ersetzt mit strikter typbindung `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="0e802-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="0e802-133">Stark typisierte Modelle sind über bevorzugte schwach typisiert.</span><span class="sxs-lookup"><span data-stu-id="0e802-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="0e802-134">Weitere Informationen finden Sie unter [schwach typisierte Daten (ViewData und ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="0e802-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="0e802-135">Aktualisieren der Seite "Kurse erstellen"</span><span class="sxs-lookup"><span data-stu-id="0e802-135">Update the Courses Create page</span></span>

<span data-ttu-id="0e802-136">Update *Pages/Courses/Create.cshtml* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="0e802-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="0e802-137">Das vorhergehende Markup nimmt folgende Änderungen:</span><span class="sxs-lookup"><span data-stu-id="0e802-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="0e802-138">Ändert die Beschriftung von **"DepartmentID"** auf **Abteilung**.</span><span class="sxs-lookup"><span data-stu-id="0e802-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="0e802-139">Ersetzt `"ViewBag.DepartmentID"` mit `DepartmentNameSL` (von der Basisklasse).</span><span class="sxs-lookup"><span data-stu-id="0e802-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="0e802-140">Fügt die Option "Select-Abteilung".</span><span class="sxs-lookup"><span data-stu-id="0e802-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="0e802-141">Diese Änderung wird anstelle der ersten Abteilung "Select-Abteilung" gerendert.</span><span class="sxs-lookup"><span data-stu-id="0e802-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="0e802-142">Fügt eine validierungsmeldung angezeigt, wenn die Abteilung nicht ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="0e802-142">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="0e802-143">Die Razor-Seite verwendet der [wählen Sie Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="0e802-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="0e802-144">Testen Sie die Seite "erstellen".</span><span class="sxs-lookup"><span data-stu-id="0e802-144">Test the Create page.</span></span> <span data-ttu-id="0e802-145">Die Seite "erstellen" zeigt an, den Abteilungsnamen, anstatt die Department-ID.</span><span class="sxs-lookup"><span data-stu-id="0e802-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="0e802-146">Aktualisieren Sie die Seite Kurse zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0e802-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="0e802-147">Aktualisieren Sie das Modell bearbeiten, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-147">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="0e802-148">Die Änderungen sind ähnlich, die in das Modell erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e802-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="0e802-149">Im vorangehenden Code `PopulateDepartmentsDropDownList` übergibt die Department-ID, die die Abteilung, angegeben in der Dropdown-Liste auswählen.</span><span class="sxs-lookup"><span data-stu-id="0e802-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="0e802-150">Update *Pages/Courses/Edit.cshtml* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="0e802-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="0e802-151">Das vorhergehende Markup nimmt folgende Änderungen:</span><span class="sxs-lookup"><span data-stu-id="0e802-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="0e802-152">Zeigt die Kurs-ID an.</span><span class="sxs-lookup"><span data-stu-id="0e802-152">Displays the course ID.</span></span> <span data-ttu-id="0e802-153">Der primäre Schlüssel (PK) einer Entität wird im Allgemeinen nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e802-153">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="0e802-154">PKs sind in der Regel für Benutzer ohne Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="0e802-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="0e802-155">In diesem Fall ist der Primärschlüssel der Kurs Anzahl.</span><span class="sxs-lookup"><span data-stu-id="0e802-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="0e802-156">Ändert die Beschriftung von **"DepartmentID"** auf **Abteilung**.</span><span class="sxs-lookup"><span data-stu-id="0e802-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="0e802-157">Ersetzt `"ViewBag.DepartmentID"` mit `DepartmentNameSL` (von der Basisklasse).</span><span class="sxs-lookup"><span data-stu-id="0e802-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="0e802-158">Fügt die Option "Select-Abteilung".</span><span class="sxs-lookup"><span data-stu-id="0e802-158">Adds the "Select Department" option.</span></span> <span data-ttu-id="0e802-159">Diese Änderung wird anstelle der ersten Abteilung "Select-Abteilung" gerendert.</span><span class="sxs-lookup"><span data-stu-id="0e802-159">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="0e802-160">Fügt eine validierungsmeldung angezeigt, wenn die Abteilung nicht ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="0e802-160">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="0e802-161">Die Seite enthält ein ausgeblendetes Feld (`<input type="hidden">`) für die Kurs-Anzahl.</span><span class="sxs-lookup"><span data-stu-id="0e802-161">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="0e802-162">Hinzufügen einer `<label>` tag Hilfsprogramm mit `asp-for="Course.CourseID"` nicht das ausgeblendete Feld überflüssig.</span><span class="sxs-lookup"><span data-stu-id="0e802-162">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="0e802-163">`<input type="hidden">`ist erforderlich, damit die Anzahl der Kurs in die bereitgestellten Daten aufgenommen werden, wenn der Benutzer klickt **speichern**.</span><span class="sxs-lookup"><span data-stu-id="0e802-163">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="0e802-164">Testen des aktualisierten Codes.</span><span class="sxs-lookup"><span data-stu-id="0e802-164">Test the updated code.</span></span> <span data-ttu-id="0e802-165">Erstellen, bearbeiten und einen Kurs zu löschen.</span><span class="sxs-lookup"><span data-stu-id="0e802-165">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="0e802-166">AsNoTracking zu den Details hinzufügen und Löschen von Modellen Seite</span><span class="sxs-lookup"><span data-stu-id="0e802-166">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="0e802-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) kann die Leistung verbessern, wenn keine Überwachung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="0e802-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="0e802-168">Hinzufügen `AsNoTracking` zum Seitenmodell löschen und Details.</span><span class="sxs-lookup"><span data-stu-id="0e802-168">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="0e802-169">Der folgende Code zeigt die aktualisierte Seitenmodell löschen:</span><span class="sxs-lookup"><span data-stu-id="0e802-169">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="0e802-170">Update der `OnGetAsync` Methode in der *Pages/Courses/Details.cshtml.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="0e802-170">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="0e802-171">Ändern Sie die Seiten löschen und Details</span><span class="sxs-lookup"><span data-stu-id="0e802-171">Modify the Delete and Details pages</span></span>

<span data-ttu-id="0e802-172">Aktualisieren Sie das Löschen von Razor-Seite durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="0e802-172">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="0e802-173">Nehmen Sie die gleichen Änderungen an der Seite "Details" ein.</span><span class="sxs-lookup"><span data-stu-id="0e802-173">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="0e802-174">Testen Sie die Kurs-Seiten</span><span class="sxs-lookup"><span data-stu-id="0e802-174">Test the Course pages</span></span>

<span data-ttu-id="0e802-175">Test erstellen, bearbeiten, Details, und löschen.</span><span class="sxs-lookup"><span data-stu-id="0e802-175">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="0e802-176">Aktualisieren Sie die Instructor-Seiten</span><span class="sxs-lookup"><span data-stu-id="0e802-176">Update the instructor pages</span></span>

<span data-ttu-id="0e802-177">In den folgenden Abschnitten aktualisieren die Instructor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="0e802-177">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="0e802-178">Hinzufügen von Office-Speicherort</span><span class="sxs-lookup"><span data-stu-id="0e802-178">Add office location</span></span>

<span data-ttu-id="0e802-179">Wenn Sie einen Instructor-Eintrag bearbeiten zu können, empfiehlt es sich, den Dozenten bürozuweisung zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="0e802-179">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="0e802-180">Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="0e802-180">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="0e802-181">Die Instructor-Code zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="0e802-181">The instructor code must handle:</span></span>

* <span data-ttu-id="0e802-182">Wenn der Benutzer die Office-Zuweisung behoben wird, löschen Sie die `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="0e802-182">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="0e802-183">Wenn der Benutzer eine bürozuweisung gibt, und es leer war, erstellen Sie ein neues `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="0e802-183">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="0e802-184">Wenn der Benutzer die Zuweisung von Office ändert, Aktualisieren der `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="0e802-184">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="0e802-185">Aktualisieren Sie das Modell bearbeiten Lehrkräfte durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-185">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="0e802-186">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-186">The preceding code:</span></span>

- <span data-ttu-id="0e802-187">Ruft die aktuellen `Instructor` Entität aus der Datenbank mit unverzüglichem Laden für die `OfficeAssignment` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0e802-187">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="0e802-188">Aktualisiert die abgerufenen `Instructor` Entität mit Werten aus den Modellbinder.</span><span class="sxs-lookup"><span data-stu-id="0e802-188">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="0e802-189">`TryUpdateModel`verhindert, dass [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="0e802-189">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="0e802-190">Wenn der Niederlassung leer ist, legt `Instructor.OfficeAssignment` auf Null.</span><span class="sxs-lookup"><span data-stu-id="0e802-190">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="0e802-191">Wenn `Instructor.OfficeAssignment` null ist, ist die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0e802-191">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="0e802-192">Aktualisieren Sie die Instructor bearbeiten (Seite)</span><span class="sxs-lookup"><span data-stu-id="0e802-192">Update the instructor Edit page</span></span>

<span data-ttu-id="0e802-193">Update *Pages/Instructors/Edit.cshtml* mit den Office-Speicherort:</span><span class="sxs-lookup"><span data-stu-id="0e802-193">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="0e802-194">Stellen Sie sicher, dass Sie einem Bürostandort Lehrkräfte ändern können.</span><span class="sxs-lookup"><span data-stu-id="0e802-194">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="0e802-195">Die Instructor-Bearbeitungsseite Kursaufgaben hinzufügen</span><span class="sxs-lookup"><span data-stu-id="0e802-195">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="0e802-196">Lehrkräfte können eine beliebige Anzahl von Kurse werden folgende Themen behandelt.</span><span class="sxs-lookup"><span data-stu-id="0e802-196">Instructors may teach any number of courses.</span></span> <span data-ttu-id="0e802-197">In diesem Abschnitt fügen Sie die Möglichkeit zum Ändern der Kurs Zuordnung hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e802-197">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="0e802-198">Die folgende Abbildung zeigt den aktualisierten Dozenten bearbeiten (Seite):</span><span class="sxs-lookup"><span data-stu-id="0e802-198">The following image shows the updated instructor Edit page:</span></span>

![Instructor-Bearbeitungsseite mit Kurse](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0e802-200">`Course`und `Instructor` weist eine m: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="0e802-200">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="0e802-201">Zum Hinzufügen und Entfernen von Beziehungen, hinzufügen und Entfernen von Entitäten aus der `CourseAssignments` Entitätenmenge verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="0e802-201">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="0e802-202">Kontrollkästchen aktivieren, Änderungen an der Kurse, denen ein Kursleiter zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="0e802-202">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="0e802-203">Ein Kontrollkästchen wird für jede Vorgehensweise in der Datenbank angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e802-203">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="0e802-204">Kurse, denen der Dozenten zugewiesen ist, werden überprüft.</span><span class="sxs-lookup"><span data-stu-id="0e802-204">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="0e802-205">Der Benutzer kann aktivieren oder deaktivieren Kontrollkästchen, um den Kurs Zuweisungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="0e802-205">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="0e802-206">Wenn die Anzahl der Kurse viel größer sind:</span><span class="sxs-lookup"><span data-stu-id="0e802-206">If the number of courses were much greater:</span></span>

* <span data-ttu-id="0e802-207">Wahrscheinlich verwenden Sie eine andere Benutzeroberfläche, um die Kurse anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e802-207">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="0e802-208">Die Methode zum Bearbeiten einer Joinentität zum Erstellen oder Löschen von Beziehungen werden nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="0e802-208">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="0e802-209">Hinzufügen von Klassen zur Unterstützung Instructor-Seiten erstellen und bearbeiten</span><span class="sxs-lookup"><span data-stu-id="0e802-209">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="0e802-210">Erstellen Sie *SchoolViewModels/AssignedCourseData.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-210">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="0e802-211">Die `AssignedCourseData` Klasse enthält Daten, um die Kontrollkästchen für zugewiesenen Kurse durch einen Kursleiter zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e802-211">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="0e802-212">Erstellen der *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* Basisklasse:</span><span class="sxs-lookup"><span data-stu-id="0e802-212">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="0e802-213">Die `InstructorCoursesPageModel` ist die Basisklasse, die Sie für die Bearbeitung und Seite Modelle erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e802-213">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="0e802-214">`PopulateAssignedCourseData`Liest alle `Course` zum Auffüllen Entitäten `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="0e802-214">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="0e802-215">Für jeden Kurs der Code legt die `CourseID`, Titel und ob der Kurs Kursleiter zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0e802-215">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="0e802-216">Ein [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) dient zum effizienten Suchvorgänge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e802-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="0e802-217">Seitenmodell für Lehrkräfte bearbeiten</span><span class="sxs-lookup"><span data-stu-id="0e802-217">Instructors Edit page model</span></span>

<span data-ttu-id="0e802-218">Aktualisieren Sie das Modell bearbeiten Dozenten durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-218">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="0e802-219">Der vorangehende Code verarbeitet Änderungen an der sicherheitsbereichszuweisung Office.</span><span class="sxs-lookup"><span data-stu-id="0e802-219">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="0e802-220">Aktualisieren Sie den Dozenten Razor-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="0e802-220">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="0e802-221">Wenn Sie den Code in Visual Studio einfügen, werden Zeilenumbrüche in einer Weise geändert, die der Code unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="0e802-221">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="0e802-222">Drücken Sie einmal STRG + Z, um die automatische Formatierung rückgängig zu machen.</span><span class="sxs-lookup"><span data-stu-id="0e802-222">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="0e802-223">STRG + Z behebt die Zeilenumbrüche, damit sie sehen, wie Sie hier sehen.</span><span class="sxs-lookup"><span data-stu-id="0e802-223">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="0e802-224">Der Einzug keinen perfekt, aber die `@</tr><tr>`, `@:<td>`, `@:</td>`, und `@:</tr>` Zeilen jeweils muss in einer einzelnen Zeile wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e802-224">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="0e802-225">Drücken Sie mit dem Block von neuem Code ausgewählt ist Tab dreimal an den neuen Code mit dem vorhandenen Code.</span><span class="sxs-lookup"><span data-stu-id="0e802-225">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="0e802-226">Stimmen Sie auf, oder Überprüfen Sie den Status dieses Fehlers [mit diesem Link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="0e802-226">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="0e802-227">Der vorangehende Code erstellt eine HTML-Tabelle, die drei Spalten aufweist.</span><span class="sxs-lookup"><span data-stu-id="0e802-227">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="0e802-228">Jede Spalte verfügt über ein Kontrollkästchen und eine Beschriftung, die den Titel und Kursnummer enthält.</span><span class="sxs-lookup"><span data-stu-id="0e802-228">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="0e802-229">Die Kontrollkästchen, die alle haben den gleichen Namen ("SelectedCourses").</span><span class="sxs-lookup"><span data-stu-id="0e802-229">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="0e802-230">Mit dem gleichen Namen informiert den Modellbinder, um sie als eine Gruppe zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="0e802-230">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="0e802-231">Das Value-Attribut der einzelnen Kontrollkästchen wird festgelegt, um `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="0e802-231">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="0e802-232">Wenn die Seite zurückgesendet wird, übergibt der Modellbinder ein Array, aus denen besteht die `CourseID` Werte für nur die Kontrollkästchen, die ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="0e802-232">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="0e802-233">Wenn Sie die Kontrollkästchen gerendert werden, haben Kurse zu den Dozenten zugewiesen Attribute überprüft.</span><span class="sxs-lookup"><span data-stu-id="0e802-233">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="0e802-234">Führen Sie die app, und Testen Sie die aktualisierte Lehrkräfte bearbeiten (Seite).</span><span class="sxs-lookup"><span data-stu-id="0e802-234">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="0e802-235">Einige Kurs Zuweisungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="0e802-235">Change some course assignments.</span></span> <span data-ttu-id="0e802-236">Die Änderungen werden auf der Seite "Index" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e802-236">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="0e802-237">Hinweis: Ansatz wird hier zum Bearbeiten von Daten für Dozenten Kurs funktioniert gut, wenn eine begrenzte Anzahl von Kurse.</span><span class="sxs-lookup"><span data-stu-id="0e802-237">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="0e802-238">Für Sammlungen, die viel größer sind, werden eine andere Benutzeroberfläche und eine andere Methode für die Aktualisierung noch verwendbar und effektiver gebildet.</span><span class="sxs-lookup"><span data-stu-id="0e802-238">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="0e802-239">Aktualisieren der Lehrkräfte-Seite "erstellen"</span><span class="sxs-lookup"><span data-stu-id="0e802-239">Update the instructors Create page</span></span>

<span data-ttu-id="0e802-240">Aktualisieren Sie das Modell erstellen Dozenten durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-240">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="0e802-241">Der vorhergehende Code ist ähnlich wie die *Pages/Instructors/Edit.cshtml.cs* Code.</span><span class="sxs-lookup"><span data-stu-id="0e802-241">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="0e802-242">Aktualisieren Sie die Instructor-Erstellen von Razor-Seite durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="0e802-242">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="0e802-243">Testen Sie die Instructor-Seite erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e802-243">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0e802-244">Aktualisieren Sie die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="0e802-244">Update the Delete page</span></span>

<span data-ttu-id="0e802-245">Aktualisieren Sie das Modell löschen, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e802-245">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="0e802-246">Der vorangehende Code stellt die folgenden Änderungen:</span><span class="sxs-lookup"><span data-stu-id="0e802-246">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="0e802-247">Verwendet unverzüglichem Laden für die `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0e802-247">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="0e802-248">`CourseAssignments`eingeschlossen werden müssen oder sie werden nicht gelöscht, wenn der Kursleiter gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0e802-248">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="0e802-249">Um zu vermeiden, diese zu lesen, konfigurieren Sie kaskadierte Löschung in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0e802-249">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="0e802-250">Die Instructor gelöscht werden soll als Administrator, der alle Abteilungen zugewiesen wird, entfernt die Instructor-Zuweisung von die Abteilungen.</span><span class="sxs-lookup"><span data-stu-id="0e802-250">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0e802-251">[Zurück](xref:data/ef-rp/read-related-data)
[Weiter](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="0e802-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>