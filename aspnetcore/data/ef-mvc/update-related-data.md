---
title: 'ASP.NET Core MVC mit EF Core: Lesen verwandter Daten (7 von 10)'
author: rick-anderson
description: Mithilfe dieses Tutorials führen Sie Updates für verwandte Daten aus, indem Sie Felder mit Fremdschlüsseln sowie Navigationseigenschaften aktualisieren.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: e8375cfdef9c149efdc722df499744be71923664
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a><span data-ttu-id="97e82-103">ASP.NET Core MVC mit EF Core: Lesen verwandter Daten (7 von 10)</span><span class="sxs-lookup"><span data-stu-id="97e82-103">ASP.NET Core MVC with EF Core - Update Related Data - 7 of 10</span></span>

<span data-ttu-id="97e82-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="97e82-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="97e82-105">Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="97e82-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="97e82-106">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="97e82-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="97e82-107">Mithilfe des letzten Tutorials haben Sie zugehörige Daten abgerufen. Mithilfe dieses Tutorials führen Sie hingegen Updates für verwandte Daten aus, indem Sie Felder mit Fremdschlüsseln sowie Navigationseigenschaften aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="97e82-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="97e82-108">In den folgenden Abbildungen werden die Seiten dargestellt, mit denen Sie arbeiten werden.</span><span class="sxs-lookup"><span data-stu-id="97e82-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Bearbeitungsseite „Kurs“](update-related-data/_static/course-edit.png)

![Bearbeitungsseite „Dozent“](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="97e82-111">Anpassen der Seiten „Erstellen“ und „Bearbeiten“ für Kurse</span><span class="sxs-lookup"><span data-stu-id="97e82-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="97e82-112">Wenn eine neue Kursentität erstellt wird, muss diese in Beziehung zu einer vorhandenen Abteilung stehen.</span><span class="sxs-lookup"><span data-stu-id="97e82-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="97e82-113">Um dies zu vereinfachen, enthält der Gerüstcode Controllermethoden und Ansichten zum „Erstellen“ und „Bearbeiten“, die eine Dropdownliste enthalten, aus denen der Fachbereich ausgewählt werden kann.</span><span class="sxs-lookup"><span data-stu-id="97e82-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="97e82-114">Die Dropdownliste legt die `Course.DepartmentID`-Fremdschlüsseleigenschaft fest. Mehr benötigt Entity Framework nicht, um die `Department`-Navigationseigenschaft mit der passenden Department-Entität zu laden.</span><span class="sxs-lookup"><span data-stu-id="97e82-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="97e82-115">Verwenden Sie den Gerüstcode, aber nehmen Sie kleine Änderungen vor, um die Fehlerbehandlung hinzuzufügen und die Dropdownliste zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="97e82-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="97e82-116">Löschen Sie aus der *CoursesController.cs*-Datei die vier Methoden zum „Erstellen“ und „Bearbeiten“, und ersetzen Sie diese durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="97e82-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="97e82-117">Erstellen Sie nach der HttpPost-Methode `Edit` eine neue Methode, die für die Dropdownliste Informationen zum Fachbereich lädt.</span><span class="sxs-lookup"><span data-stu-id="97e82-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="97e82-118">Die `PopulateDepartmentsDropDownList`-Methode ruft eine nach Namen sortierte Liste aller Abteilungen ab, erstellt eine `SelectList`-Auflistung für die Dropdownliste und übergibt diese Auflistung an die Ansicht in `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="97e82-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="97e82-119">Die Methode akzeptiert den optionalen `selectedDepartment`-Parameter, über den der Code das Element angeben kann, das ausgewählt wird, wenn die Dropdownliste gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="97e82-120">Die Ansicht übergibt den Namen „DepartmentID“ an das Taghilfsprogramm `<select>`. Dann weiß das Hilfsprogramm, dass es nach einem `ViewBag`-Objekt für eine `SelectList` mit dem Namen „DepartmentID“ suchen soll.</span><span class="sxs-lookup"><span data-stu-id="97e82-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="97e82-121">Die HttpGet-Methode `Create` ruft die `PopulateDepartmentsDropDownList`-Methode auf, ohne das ausgewählte Element festzulegen, da der Fachbereich für einen neuen Code noch nicht festgelegt wurde:</span><span class="sxs-lookup"><span data-stu-id="97e82-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="97e82-122">Die HttpGet-Methode `Edit` legt anhand der ID des Fachbereichs, die bereits dem Kurs zugewiesen wurde, der bearbeitet wird, das ausgewählte Element fest:</span><span class="sxs-lookup"><span data-stu-id="97e82-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="97e82-123">Die HttpPost-Methoden für `Create` und `Edit` enthalten außerdem Code, der das ausgewählte Element festlegt, wenn die Seite nach einem Fehler erneut angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="97e82-124">Dadurch wird gewährleistet, dass der Fachbereich nicht geändert wird, wenn eine Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="97e82-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="97e82-125">Hinzufügen von „.AsNoTracking“ zu den Methoden „Details“ und „Delete“</span><span class="sxs-lookup"><span data-stu-id="97e82-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="97e82-126">Fügen Sie `AsNoTracking`-Aufrufe in die `Details` und zu den HttpGet-Methoden `Delete` hinzu, um die Leistung der Kursdetails und Seiten zum „Löschen“ zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="97e82-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="97e82-127">Ändern der Kursansichten</span><span class="sxs-lookup"><span data-stu-id="97e82-127">Modify the Course views</span></span>

<span data-ttu-id="97e82-128">Fügen Sie in der *Views/Courses/Create.cshtml*-Datei die Option „Select Department“ (Fachbereich auswählen) zu der Dropdownliste **Department** (Fachbereich) hinzu, ändern Sie den Titel von **DepartmentID** in **Department**, und fügen Sie eine Validierungsmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="97e82-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="97e82-129">Nehmen Sie in der Datei *Views/Courses/Edit.cshtml* dieselben Änderungen für das Feld „Department“ (Fachbereich) vor wie in der Datei *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="97e82-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="97e82-130">Fügen Sie in der *Views/Courses/Edit.cshtml*-Datei außerdem vor dem Feld **Titel** ein Feld für die Kursnummer hinzu.</span><span class="sxs-lookup"><span data-stu-id="97e82-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="97e82-131">Da es sich bei der Kursnummer um einen Primärschlüssel handelt, wird dieser zwar angezeigt, kann aber nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="97e82-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="97e82-132">In der Ansicht „Bearbeiten“ ist bereits eine ausgeblendetes Feld (`<input type="hidden">`) für die Kursnummer vorhanden.</span><span class="sxs-lookup"><span data-stu-id="97e82-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="97e82-133">Wenn Sie ein `<label>`-Taghilfsprogramm hinzufügen, wird trotzdem ein ausgeblendetes Feld benötigt, da dieses nicht dazu beiträgt, dass die Kursnummer in den bereitgestellten Daten vorhanden ist, wenn der Benutzer auf der Seite **Bearbeiten** auf **Speichern** klickt.</span><span class="sxs-lookup"><span data-stu-id="97e82-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="97e82-134">Fügen Sie oben in der *Views/Courses/Delete.cshtml*-Datei ein Feld für die Kursnummer hinzu, und ändern Sie „Department ID“ (Fachbereichs-ID) in „Department Name“ (Fachbereichsname).</span><span class="sxs-lookup"><span data-stu-id="97e82-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="97e82-135">Nehmen Sie in der *Views/Courses/Details.cshtml*-Datei dieselben Änderungen wie in der *Delete.cshtml*-Datei vor.</span><span class="sxs-lookup"><span data-stu-id="97e82-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="97e82-136">Testen der Kursseiten</span><span class="sxs-lookup"><span data-stu-id="97e82-136">Test the Course pages</span></span>

<span data-ttu-id="97e82-137">Führen Sie die App aus, wählen Sie die Registerkarte **Courses** (Kurse) aus, klicken Sie auf **Neu erstellen**, und geben Sie die Daten für einen neuen Kurs ein:</span><span class="sxs-lookup"><span data-stu-id="97e82-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Seite „Kurs erstellen“](update-related-data/_static/course-create.png)

<span data-ttu-id="97e82-139">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="97e82-139">Click **Create**.</span></span> <span data-ttu-id="97e82-140">Die Indexseite des Kurses wird mit dem Kurs angezeigt, der neu zu der Liste hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="97e82-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="97e82-141">Der Fachbereichsname in der Indexseitenliste wurde der Navigationseigenschaft entnommen und deutet darauf hin, dass die Beziehung ordnungsgemäß festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="97e82-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="97e82-142">Klicken Sie auf der Indexseite eines Kurses auf **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="97e82-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Bearbeitungsseite „Kurs“](update-related-data/_static/course-edit.png)

<span data-ttu-id="97e82-144">Ändern Sie die Daten auf der Seite, und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="97e82-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="97e82-145">Die Indexseite des Kurses wird mit den aktualisierten Kursdaten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="97e82-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="97e82-146">Hinzufügen einer Seite zum „Bearbeiten“ für Dozenten</span><span class="sxs-lookup"><span data-stu-id="97e82-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="97e82-147">Bei der Bearbeitung eines Dozentendatensatzes sollten Sie auch die Bürozuweisung des Dozenten aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="97e82-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="97e82-148">Die Entität „Instructor“ verfügt über eine 1:0..1-Beziehung mit der Entität „OfficeAssignment“. Das bedeutet, dass Ihr Code den folgenden Situationen standhalten muss:</span><span class="sxs-lookup"><span data-stu-id="97e82-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="97e82-149">Wenn der Benutzer die Bürozuweisung löscht, es für diese aber zuvor einen Wert gab, löschen Sie die Entität „OfficeAssignment“.</span><span class="sxs-lookup"><span data-stu-id="97e82-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="97e82-150">Wenn der Benutzer eine Bürozuweisung eingibt, diese aber zuvor leer war, erstellen Sie eine neue OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="97e82-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="97e82-151">Wenn der Benutzer den Wert einer Bürozuweisung verändert, ändern Sie den Wert in eine bereits vorhandene OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="97e82-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="97e82-152">Aktualisieren des Controllers des Dozenten</span><span class="sxs-lookup"><span data-stu-id="97e82-152">Update the Instructors controller</span></span>

<span data-ttu-id="97e82-153">Ändern Sie in der *InstructorsController.cs*-Datei den Code in der HttpGet-Methode `Edit`, sodass diese die `OfficeAssignment`-Eigenschaft der Instructor-Entität lädt und `AsNoTracking` aufruft:</span><span class="sxs-lookup"><span data-stu-id="97e82-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="97e82-154">Ersetzen Sie die HttpPost-Methode `Edit` durch den folgenden Code, damit diese Updates für die Bürozuweisungen verarbeiten kann:</span><span class="sxs-lookup"><span data-stu-id="97e82-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="97e82-155">Der Code führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="97e82-155">The code does the following:</span></span>

-  <span data-ttu-id="97e82-156">Er ändert den Methodennamen auf `EditPost`, da die Signatur jetzt der Signatur der HttpGet-Methode `Edit` entspricht. Das `ActionName`-Attribut gibt an, dass die `/Edit/`-URL immer noch verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="97e82-157">Ruft für die Navigationseigenschaft `OfficeAssignment` die aktuelle Dozentenentität von der Datenbank über Eager Loading ab.</span><span class="sxs-lookup"><span data-stu-id="97e82-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="97e82-158">Dies entspricht dem Vorgang in der HttpGet-Methode `Edit`.</span><span class="sxs-lookup"><span data-stu-id="97e82-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="97e82-159">Führt ein Update für die abgerufene Dozentenentität mit Werten aus der Modellbindung aus.</span><span class="sxs-lookup"><span data-stu-id="97e82-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="97e82-160">Mithilfe der `TryUpdateModel`-Überladung können Sie die Eigenschaften auf die Whitelist setzen, die Sie hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="97e82-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="97e82-161">Dadurch wird, wie im [zweiten Tutorial](crud.md) beschrieben, vermieden, dass zu viele Angaben gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="97e82-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="97e82-162">Wenn kein Standort für das Büro angegeben wird, wird die Instructor.OfficeAssignment-Eigenschaft auf NULL festgelegt, damit die zugehörige Zeile aus der OfficeAssignment-Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="97e82-163">Speichert die Änderungen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="97e82-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="97e82-164">Aktualisieren der Dozentenansicht „Bearbeiten“</span><span class="sxs-lookup"><span data-stu-id="97e82-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="97e82-165">Fügen Sie am Ende vor der Schaltfläche **Speichern** in der *Views/Instructors/Edit.cshtml*-Datei ein neues Feld hinzu, um den Standort des Büros zu bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="97e82-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="97e82-166">Führen Sie die App aus, klicken Sie erst auf die Registerkarte **Dozenten** und dann für einen Dozenten auf **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="97e82-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="97e82-167">Ändern Sie den **Standort des Büros**, und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="97e82-167">Change the **Office Location** and click **Save**.</span></span>

![Bearbeitungsseite „Dozent“](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="97e82-169">Hinzufügen von Kurszuweisungen zur Dozentenseite „Bearbeiten“</span><span class="sxs-lookup"><span data-stu-id="97e82-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="97e82-170">Dozenten können eine beliebige Anzahl von Kursen unterrichten.</span><span class="sxs-lookup"><span data-stu-id="97e82-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="97e82-171">Jetzt soll die Dozentenseite „Bearbeiten“ verbessert werden, indem es ermöglicht wird, Kurszuweisungen über eine Reihe von Kontrollkästchen zu verändern. Dies wird im folgenden Screenshot dargestellt:</span><span class="sxs-lookup"><span data-stu-id="97e82-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Dozentenseite „Bearbeiten“ mit Kursen](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="97e82-173">Zwischen den Entitäten „Course“ und „Instructor“ besteht eine m:n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="97e82-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="97e82-174">Wenn Sie Beziehungen hinzufügen und entfernen möchten, können Sie Entitäten aus der verknüpften CourseAssignments-Entitätenmenge hinzufügen und entfernen.</span><span class="sxs-lookup"><span data-stu-id="97e82-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="97e82-175">Die Benutzeroberfläche, über die Sie ändern können, welchen Kursen ein Dozent zugewiesen ist, besteht aus einer Reihe von Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="97e82-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="97e82-176">Für jeden Kurs in der Datenbank wird ein Kontrollkästchen angezeigt. Die Kontrollkästchen, denen der Dozent zu diesem Zeitpunkt zugewiesen ist, sind aktiviert.</span><span class="sxs-lookup"><span data-stu-id="97e82-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="97e82-177">Der Benutzer kann Kontrollkästchen aktivieren oder deaktivieren, um Kurszuweisungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="97e82-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="97e82-178">Bei einer deutlich höheren Anzahl von Kursen sollten Sie besser eine andere Methode verwenden, um Daten in der Ansicht darzustellen. Sie sollten aber auch in diesem Fall dieselbe Bearbeitungsmethode verwenden, um eine verknüpfte Entität zum Erstellen und Löschen von Beziehungen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="97e82-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="97e82-179">Aktualisieren des Controllers des Dozenten</span><span class="sxs-lookup"><span data-stu-id="97e82-179">Update the Instructors controller</span></span>

<span data-ttu-id="97e82-180">Verwenden Sie eine Ansichtsmodellklasse, um Daten für die Ansicht bereitzustellen, um eine Liste mit Kontrollkästchen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="97e82-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="97e82-181">Erstellen Sie im Ordner *SchoolViewModels* die Datei *AssignedCourseData.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="97e82-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="97e82-182">Ersetzen Sie in der Datei *InstructorsController.cs* die HttpGet-Methode `Edit` durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="97e82-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="97e82-183">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="97e82-183">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="97e82-184">Über den Code wird für die `Courses`-Navigationseigenschaft Eager Loading hinzugefügt und die neue `PopulateAssignedCourseData`-Methode aufgerufen, um über die Ansichtsmodellklasse `AssignedCourseData` Informationen für das Kontrollkästchenarray zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="97e82-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="97e82-185">Der Code in der `PopulateAssignedCourseData`-Methode liest alle Course-Entitäten, um mithilfe der Ansichtsmodellklasse eine Liste von Kursen zu laden.</span><span class="sxs-lookup"><span data-stu-id="97e82-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="97e82-186">Für jeden Kurs überprüft der Code, ob dieser in der `Courses`-Navigationseigenschaft des Dozenten vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="97e82-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="97e82-187">Die Kurse, die dem Dozenten zugewiesen werden, werden in einer `HashSet`-Auflistung dargestellt, um die Suche effizienter zu machen, wenn Sie überprüfen, ob ein Kurs dem Dozenten zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="97e82-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="97e82-188">Die `Assigned`-Eigenschaft ist für Kurse, denen der Dozent zugewiesen ist, auf TRUE festgelegt.</span><span class="sxs-lookup"><span data-stu-id="97e82-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="97e82-189">Die Ansicht verwendet diese Eigenschaft, um zu bestimmen, welche Kontrollkästchen als aktiviert angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="97e82-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="97e82-190">Zuletzt wird die Liste in `ViewData` an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="97e82-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="97e82-191">Fügen Sie als nächstes den Code hinzu, der ausgeführt wird, wenn der Benutzer auf **Speichern** klickt.</span><span class="sxs-lookup"><span data-stu-id="97e82-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="97e82-192">Ersetzen Sie die `EditPost`-Methode durch den folgenden Code, und fügen Sie eine neue Methode hinzu, die ein Update für `Courses`-Navigationseigenschaft der Instructor-Entität ausführt.</span><span class="sxs-lookup"><span data-stu-id="97e82-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="97e82-193">Die Methodensignatur unterscheidet sich jetzt von der HttpGet-Methode `Edit`, wodurch der Methodenname von `EditPost` auf `Edit` geändert wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="97e82-194">Da es in der Ansicht keine Auflistung der Course-Entitäten gibt, kann durch die Modellbindung kein automatisches Update für die `CourseAssignments`-Navigationseigenschaft ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="97e82-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="97e82-195">Verwenden Sie dafür die neue `CourseAssignments`-Methode anstatt die Modellbindung zu verwenden, um ein Update für die `UpdateInstructorCourses`-Navigationseigenschaft auszuführen.</span><span class="sxs-lookup"><span data-stu-id="97e82-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="97e82-196">Aus diesem Grund müssen Sie die `CourseAssignments`-Eigenschaft von der Modellbindung ausschließen.</span><span class="sxs-lookup"><span data-stu-id="97e82-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="97e82-197">Dafür muss der Code, der `TryUpdateModel` aufruft, nicht verändert werden, da Sie die Whitelist-Überladung verwenden und `CourseAssignments` nicht in der Liste enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="97e82-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="97e82-198">Wenn keins der Kontrollkästchen aktiviert ist, initialisiert der Code in `UpdateInstructorCourses` die `CourseAssignments`-Navigationseigenschaft mit einer leeren Auflistung und gibt Folgendes zurück:</span><span class="sxs-lookup"><span data-stu-id="97e82-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="97e82-199">Der Code führt dann eine Schleife durch alle Kurse in der Datenbank aus und überprüft jeden Kurs im Hinblick auf die Kurse, die zu diesem Zeitpunkt dem Dozenten zugewiesen sind, und denen, die in der Ansicht aktiviert wurden.</span><span class="sxs-lookup"><span data-stu-id="97e82-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="97e82-200">Die beiden letzten Auflistungen werden in `HashSet`-Objekten gespeichert, um Suchvorgänge effizienter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="97e82-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="97e82-201">Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.CourseAssignments`-Navigationseigenschaft vorhanden ist, wird dieser der Auflistung in der Navigationseigenschaft hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="97e82-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="97e82-202">Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.CourseAssignments`-Navigationseigenschaft vorhanden ist, wird dieser aus der Auflistung in der Navigationseigenschaft gelöscht.</span><span class="sxs-lookup"><span data-stu-id="97e82-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="97e82-203">Ausführen eines Updates für die Dozentenseiten</span><span class="sxs-lookup"><span data-stu-id="97e82-203">Update the Instructor views</span></span>

<span data-ttu-id="97e82-204">Fügen Sie in der *Views/Instructors/Edit.cshtml*-Datei ein **Kurse**-Feld mit einem Array mit Kontrollkästchen hinzu, indem Sie den folgenden Code direkt nach den `div`-Elementen für das **Büro**-Feld und vor dem `div`-Element für die Schaltfläche **Speichern** einfügen.</span><span class="sxs-lookup"><span data-stu-id="97e82-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="97e82-205">Wenn Sie den Code in Visual Studio einfügen, werden Zeilenumbrüche so geändert, dass der Code unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="97e82-206">Drücken Sie einmal Strg+Z, um die automatische Formatierung rückgängig zu machen.</span><span class="sxs-lookup"><span data-stu-id="97e82-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="97e82-207">Damit werden die Zeilenumbrüche korrigiert, damit sie dem entsprechen, was Sie hier sehen.</span><span class="sxs-lookup"><span data-stu-id="97e82-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="97e82-208">Der Einzug muss nicht perfekt sein, die Zeilen `@</tr><tr>`, `@:<td>`, `@:</td>` und `@:</tr>` müssen jedoch, wie dargestellt, jeweils in einer einzelnen Zeile stehen. Ansonsten wird ein Laufzeitfehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="97e82-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="97e82-209">Drücken Sie, nachdem Sie den Block mit dem neuen Code ausgewählt haben, dreimal auf die TAB-Taste, um den neuen Code am vorhandenen Code auszurichten.</span><span class="sxs-lookup"><span data-stu-id="97e82-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="97e82-210">Sie können [hier](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html) den Status für dieses Problem überprüfen.</span><span class="sxs-lookup"><span data-stu-id="97e82-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="97e82-211">Dieser Code erstellt eine HTML-Tabelle mit drei Spalten.</span><span class="sxs-lookup"><span data-stu-id="97e82-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="97e82-212">Jede Spalte enthält ein Kontrollkästchen gefolgt von einem Titel, der aus der Kursnummer und dem Kurstitel besteht.</span><span class="sxs-lookup"><span data-stu-id="97e82-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="97e82-213">Die Kontrollkästchen haben denselben Namen („selectedCourses“), was bei der Modellbindung deutlich macht, dass diese als Gruppe behandelt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="97e82-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="97e82-214">Das Wertattribut der einzelnen Kontrollkästchen ist auf den Wert von `CourseID` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="97e82-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="97e82-215">Wenn die Seite zurückgesendet wird, übergibt die Modellbindung ein Array an den Controller, das lediglich die `CourseID`-Werte der aktivierten Kontrollkästchen enthält.</span><span class="sxs-lookup"><span data-stu-id="97e82-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="97e82-216">Wenn die Kontrollkästchen zu Beginn gerendert werden, verfügen die Kästchen, die für Kurse stehen, die dem Dozenten zugewiesen sind, über aktivierte Attribute, die diese aktivieren (also als aktiviert anzeigen).</span><span class="sxs-lookup"><span data-stu-id="97e82-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="97e82-217">Führen Sie die App aus, klicken Sie erst auf die Registerkarte **Dozenten** und dann für einen Dozenten auf **Bearbeiten**, um die Seite **Bearbeiten** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="97e82-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Dozentenseite „Bearbeiten“ mit Kursen](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="97e82-219">Ändern Sie einige Kurszuweisungen, und klicken Sie auf „Speichern“.</span><span class="sxs-lookup"><span data-stu-id="97e82-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="97e82-220">Die Änderungen werden auf der Indexseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="97e82-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="97e82-221">Der hier gewählte Ansatz für die Bearbeitung der Kursdaten von Dozenten wird empfohlen, wenn eine begrenzte Anzahl von Kursen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-221">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="97e82-222">Bei umfangreicheren Auflistungen wären eine andere Benutzeroberfläche und eine andere Aktualisierungsmethode erforderlich.</span><span class="sxs-lookup"><span data-stu-id="97e82-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="97e82-223">Aktualisieren der Seite „Delete“ (Löschen)</span><span class="sxs-lookup"><span data-stu-id="97e82-223">Update the Delete page</span></span>

<span data-ttu-id="97e82-224">Löschen Sie aus der Datei *InstructorsController.cs* die `DeleteConfirmed`-Methode, und fügen Sie stattdessen den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="97e82-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="97e82-225">Durch diesen Code werden folgende Änderungen vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="97e82-225">This code makes the following changes:</span></span>

* <span data-ttu-id="97e82-226">Er verwendet Eager Loading für die Navigationseigenschaft `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="97e82-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="97e82-227">Sie müssen diese Eigenschaft hinzufügen. Ansonsten weiß EF nicht, dass es zugehörige `CourseAssignment`-Entitäten gibt und löscht diese nicht.</span><span class="sxs-lookup"><span data-stu-id="97e82-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="97e82-228">Wenn Sie vermeiden möchten, diese lesen zu müssen, können Sie in der Datenbank eine Löschweitergabe konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="97e82-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="97e82-229">Wenn der zu löschende Dozent als Administrator einer beliebigen Abteilung zugewiesen ist, wird die Dozentenzuweisung aus diesen Abteilungen entfernt.</span><span class="sxs-lookup"><span data-stu-id="97e82-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="97e82-230">Hinzufügen von einem Bürostandort und von Kursen zu der Seite „Erstellen“</span><span class="sxs-lookup"><span data-stu-id="97e82-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="97e82-231">Löschen Sie in der *InstructorsController.cs*-Datei die Methoden HttpGet und HttpPost-`Create`, und fügen Sie anschließend stattdessen den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="97e82-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="97e82-232">Dieser Code ähnelt dem Code, der für die `Edit`-Methode verwendet wurde. Allerdings sind nicht von Beginn an Kurse ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="97e82-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="97e82-233">Die HttpGet-Methode `Create` ruft die `PopulateAssignedCourseData`-Methode nicht auf, weil möglicherweise Kurse ausgewählt sind, sondern um eine leere Auflistung für die `foreach`-Schleife in der Ansicht bereitzustellen. Andernfalls gibt der Ansichtscode eine NULL-Verweisausnahme zurück.</span><span class="sxs-lookup"><span data-stu-id="97e82-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="97e82-234">Die HttpPost-Methode `Create` fügt jeden der ausgewählten Kurse zu der `CourseAssignments`-Navigationseigenschaft hinzu, bevor sie nach Validierungsfehlern sucht und den neuen Dozenten zu der Datenbank hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="97e82-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="97e82-235">Kurse werden auch hinzugefügt, wenn es Modellfehler gibt, sodass alle zuvor ausgewählten Kurse automatisch wieder ausgewählt werden, wenn es zu Modellfehlern kommt (weil z.B. der Benutzer ein ungültiges Datum verschlüsselt hat) und die Seite mit Fehlermeldungen erneut dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="97e82-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="97e82-236">Beachten Sie, dass Sie die `CourseAssignments`-Navigationseigenschaft als leere Auflistung initialisieren müssen, wenn Sie dieser Kurse hinzufügen möchten:</span><span class="sxs-lookup"><span data-stu-id="97e82-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="97e82-237">Wenn Sie dies nicht im Controllercode durchführen möchten, können Sie dies auch im Dozentenmodell tun, indem Sie den Eigenschaftengetter ändern, um falls nötig automatisch die Auflistung zu erstellen. Dies wird im folgenden Code dargestellt:</span><span class="sxs-lookup"><span data-stu-id="97e82-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="97e82-238">Wenn Sie die `CourseAssignments`-Eigenschaft auf diese Weise ändern, können Sie den expliziten Code zum Initialisieren der Eigenschaft aus dem Controller entfernen.</span><span class="sxs-lookup"><span data-stu-id="97e82-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="97e82-239">Fügen die in der *Views/Instructor/Create.cshtml*-Datei ein Textfeld für den Bürostandort hinzu, und aktivieren Sie Felder für Kurse, bevor Sie auf die Schaltfläche „Übermitteln“ klicken.</span><span class="sxs-lookup"><span data-stu-id="97e82-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="97e82-240">[Wenn Visual Studio den Code neu formatiert, wenn Sie diesen einfügen, beheben Sie die Formatierung](#notepad) auf dieselbe Weise wie auf der Seite „Bearbeiten“.</span><span class="sxs-lookup"><span data-stu-id="97e82-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="97e82-241">Führen Sie einen Test durch, indem Sie die App ausführen und einen Dozenten erstellen.</span><span class="sxs-lookup"><span data-stu-id="97e82-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="97e82-242">Verarbeiten von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="97e82-242">Handling Transactions</span></span>

<span data-ttu-id="97e82-243">Wie bereits im [CRUD-Tutorial](crud.md) erläutert, implementiert Entity Framework implizit Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="97e82-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="97e82-244">Informationen zu Szenarios, die Sie genauer kontrollieren müssen (z.B. wenn Sie Vorgänge einfügen möchten, die außerhalb von Entity Framework in einer Transaktion ausgeführt werden), finden Sie unter [Transaktionen](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="97e82-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="97e82-245">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="97e82-245">Summary</span></span>

<span data-ttu-id="97e82-246">Damit ist sind die ersten Schritte zum Arbeiten mit zugehörigen Daten abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="97e82-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="97e82-247">Im nächsten Tutorial wird erläutert, wie Nebenläufigkeitskonflikte verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="97e82-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97e82-248">[Zurück](read-related-data.md)
> [Weiter](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="97e82-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
