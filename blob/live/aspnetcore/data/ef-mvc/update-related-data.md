---
title: "ASP.NET Core MVC mit EF-Kern - Update bezogene Daten per Push – 7 von 10"
author: tdykstra
description: "In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren, durch die Aktualisierung von foreign Key-Felder und Navigationseigenschaften."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 0e4df407a1ca15aa5baa2b7226be1cf91902a583
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="c4943-103">Aktualisieren von verknüpften Daten - EF-Core mit ASP.NET Core MVC-Lernprogramm (7 von 10)</span><span class="sxs-lookup"><span data-stu-id="c4943-103">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="c4943-104">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4943-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4943-105">Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4943-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="c4943-106">Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="c4943-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="c4943-107">Im vorherigen Lernprogramm angezeigten Sie verwandte Daten; In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren, durch die Aktualisierung von foreign Key-Felder und Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c4943-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="c4943-108">Die folgenden Abbildungen zeigen einige Seiten, denen Sie verwenden müssen.</span><span class="sxs-lookup"><span data-stu-id="c4943-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Kurs bearbeiten (Seite)](update-related-data/_static/course-edit.png)

![Instructor bearbeiten (Seite)](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="c4943-111">Das Erstellen und von Bearbeitungsseiten für Kurse anpassen</span><span class="sxs-lookup"><span data-stu-id="c4943-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="c4943-112">Wenn eine neue Kurs-Entität erstellt wird, muss sie eine Beziehung zu einer vorhandenen Abteilung verfügen.</span><span class="sxs-lookup"><span data-stu-id="c4943-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="c4943-113">Um dies zu ermöglichen, enthält der scaffolded Code Controllermethoden und erstellen und Bearbeiten von Ansichten, die eine Dropdown-Liste für die Auswahl der Abteilung enthalten.</span><span class="sxs-lookup"><span data-stu-id="c4943-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="c4943-114">Im Dropdown-Liste wird die `Course.DepartmentID` Fremdschlüsseleigenschaft, und das ist alles Entity Framework zu benötigt, um das Laden der `Department` Navigationseigenschaft mit der entsprechenden Abteilung-Entität.</span><span class="sxs-lookup"><span data-stu-id="c4943-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="c4943-115">Sie müssen den scaffolded Code, aber ändern, damit Sie etwas hinzufügen Fehlerbehandlung und sortieren Sie die Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="c4943-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="c4943-116">In *CoursesController.cs*, löschen Sie die vier erstellen und Bearbeiten von Methoden, und Ersetzen Sie sie durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c4943-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="c4943-117">Nach der `Edit` HttpPost-Methode erstellen Sie eine neue Methode, die Abteilung Informationen für die Dropdown-Liste geladen.</span><span class="sxs-lookup"><span data-stu-id="c4943-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="c4943-118">Die `PopulateDepartmentsDropDownList` Methode ruft eine Liste aller Abteilungen, sortiert nach dem Namen ab, erstellt eine `SelectList` Auflistung eine Dropdown-Liste, und übergibt die Auflistung an der Sicht `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="c4943-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="c4943-119">Akzeptiert die Methode den optionalen `selectedDepartment` Parameter, der den Aufrufcode das Element an, die beim Rendern der Dropdown Liste ausgewählt werden kann.</span><span class="sxs-lookup"><span data-stu-id="c4943-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="c4943-120">Die Ansicht übergeben den Namen "DepartmentID" die `<select>` Tag-Hilfsobjekt und das Hilfsprogramm zum Suchen in weiß der `ViewBag` -Objekt für eine `SelectList` mit dem Namen "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="c4943-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="c4943-121">Die HttpGet `Create` Methodenaufrufe der `PopulateDepartmentsDropDownList` Methode ohne das ausgewählte Element festlegen, da für einen neuen Kurs die Abteilung noch nicht eingerichtet wurde:</span><span class="sxs-lookup"><span data-stu-id="c4943-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="c4943-122">Die HttpGet `Edit` -Methode legt das ausgewählte Element, das anhand der ID der Abteilung, die den zu bearbeitenden Kurs bereits zugewiesen ist:</span><span class="sxs-lookup"><span data-stu-id="c4943-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="c4943-123">Für beide Methoden HttpPost `Create` und `Edit` auch Code enthalten, der das ausgewählte Element festgelegt werden soll, wenn sie die Seite nach einem Fehler erneut.</span><span class="sxs-lookup"><span data-stu-id="c4943-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="c4943-124">Dadurch wird sichergestellt, dass wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, die Abteilung ausgewählt wurde ausgewählten bleibt.</span><span class="sxs-lookup"><span data-stu-id="c4943-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="c4943-125">Fügen Sie hinzu. AsNoTracking zu den Details und Löschmethoden</span><span class="sxs-lookup"><span data-stu-id="c4943-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="c4943-126">Zum Optimieren der Leistung der Kursdetails und Delete-Seiten hinzufügen `AsNoTracking` ruft in der `Details` und HttpGet `Delete` Methoden.</span><span class="sxs-lookup"><span data-stu-id="c4943-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="c4943-127">Ändern Sie die Kurs-Ansichten</span><span class="sxs-lookup"><span data-stu-id="c4943-127">Modify the Course views</span></span>

<span data-ttu-id="c4943-128">In *Views/Courses/Create.cshtml*, fügen Sie eine Option "Select-Abteilung", um die **Abteilung** Dropdown-Liste, ändern Sie die Beschriftung von **"DepartmentID"** auf  **Abteilung**, und fügen Sie eine validierungsmeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c4943-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="c4943-129">In *Views/Courses/Edit.cshtml*, stellen Sie die gleiche Änderung für das Feld für die Abteilung, die Sie soeben in *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c4943-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="c4943-130">Ebenfalls im *Views/Courses/Edit.cshtml*, ein Zahlenfeld "Kurs" vor dem Hinzufügen der **Titel** Feld.</span><span class="sxs-lookup"><span data-stu-id="c4943-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="c4943-131">Da die Kurs-Anzahl der Primärschlüssel ist, wird angezeigt, jedoch nicht geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="c4943-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="c4943-132">Es ist bereits ein ausgeblendetes Feld (`<input type="hidden">`) für die Kursnummer in der Bearbeitungsansicht.</span><span class="sxs-lookup"><span data-stu-id="c4943-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="c4943-133">Hinzufügen einer `<label>` Tag Hilfsprogramm nicht entfällt der Bedarf für das ausgeblendete Feld, da sie nicht dazu, dass die Anzahl der Kurs in die bereitgestellten Daten aufgenommen werden, wenn der Benutzer klickt **speichern** auf die **bearbeiten** Seite.</span><span class="sxs-lookup"><span data-stu-id="c4943-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="c4943-134">In *Views/Courses/Delete.cshtml*, fügen Sie oben ein Zahlenfeld Kurs hinzu und Department ID, Name der Abteilung zu ändern.</span><span class="sxs-lookup"><span data-stu-id="c4943-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="c4943-135">In *Views/Courses/Details.cshtml*, stellen Sie die gleiche Änderung, die Sie soeben für *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c4943-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="c4943-136">Testen Sie die Kurs-Seiten</span><span class="sxs-lookup"><span data-stu-id="c4943-136">Test the Course pages</span></span>

<span data-ttu-id="c4943-137">Die app auszuführen, wählen Sie die **Kurse** auf **neu erstellen**, und geben Sie Daten für einen neuen Kurs:</span><span class="sxs-lookup"><span data-stu-id="c4943-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Seite "Kurs erstellen"](update-related-data/_static/course-create.png)

<span data-ttu-id="c4943-139">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="c4943-139">Click **Create**.</span></span> <span data-ttu-id="c4943-140">Die Kurse Indexseite wird mit den neuen Kurs zur Liste hinzugefügt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c4943-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="c4943-141">Der Abteilungsname in der Liste der Index-Seite stammt die Navigationseigenschaft, die anzeigt, dass die Beziehung ordnungsgemäß eingerichtet wurde.</span><span class="sxs-lookup"><span data-stu-id="c4943-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="c4943-142">Klicken Sie auf **bearbeiten** für eine Vorgehensweise in der Courses-Indexseite.</span><span class="sxs-lookup"><span data-stu-id="c4943-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Kurs bearbeiten (Seite)](update-related-data/_static/course-edit.png)

<span data-ttu-id="c4943-144">Ändern von Daten auf der Seite, und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="c4943-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="c4943-145">Die Kurse Indexseite wird mit den aktualisierten Kursdaten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c4943-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="c4943-146">Fügen Sie eine Bearbeitungsseite für Dozenten</span><span class="sxs-lookup"><span data-stu-id="c4943-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="c4943-147">Beim Bearbeiten eines Instructor-Datensatzes möchten Sie den Dozenten bürozuweisung aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="c4943-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="c4943-148">Die Instructor-Entität verfügt über eine auf 0 (null) oder eine 1-Beziehung mit der Entität OfficeAssignment, was, dass der Code hat bedeutet, behandeln die folgenden Situationen:</span><span class="sxs-lookup"><span data-stu-id="c4943-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="c4943-149">Wenn der Benutzer die Office-Zuweisung löscht, und er ursprünglich einen Wert hatte, löschen Sie die OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="c4943-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="c4943-150">Erstellen Sie eine neue OfficeAssignment Entität aus, wenn der Benutzer eine Office-Zuweisungswert gibt und er ursprünglich leer war.</span><span class="sxs-lookup"><span data-stu-id="c4943-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="c4943-151">Wenn der Benutzer den Wert einer Zuweisung von Office ändert, ändern Sie den Wert in einer vorhandenen OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="c4943-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="c4943-152">Aktualisieren Sie den Controller für Dozenten</span><span class="sxs-lookup"><span data-stu-id="c4943-152">Update the Instructors controller</span></span>

<span data-ttu-id="c4943-153">In *InstructorsController.cs*, ändern Sie den Code in die HttpGet `Edit` Methode, sodass die It der Instructor-Entität lädt `OfficeAssignment` Navigationseigenschaft und Aufrufe `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="c4943-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="c4943-154">Ersetzen Sie die HttpPost `Edit` Methode durch den folgenden Code aus, um Updates für Office-Zuweisung zu behandeln:</span><span class="sxs-lookup"><span data-stu-id="c4943-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="c4943-155">Der Code führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="c4943-155">The code does the following:</span></span>

-  <span data-ttu-id="c4943-156">Ändert den Namen der Methode, um `EditPost` , da die Signatur jetzt die HttpGet ist `Edit` Methode (die `ActionName` Attribut gibt an, dass die `/Edit/` URL wird weiterhin verwendet).</span><span class="sxs-lookup"><span data-stu-id="c4943-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="c4943-157">Ruft die aktuelle Instructor-Entität aus der Datenbank-eager loading für die `OfficeAssignment` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c4943-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="c4943-158">Dies ist identisch mit was Sie in der HttpGet haben `Edit` Methode.</span><span class="sxs-lookup"><span data-stu-id="c4943-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="c4943-159">Die abgerufene Instructor-Entität aktualisiert mit Werten aus den Modellbinder.</span><span class="sxs-lookup"><span data-stu-id="c4943-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="c4943-160">Die `TryUpdateModel` Überladung können Sie auf die weiße Liste die Eigenschaften, die Sie einschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="c4943-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="c4943-161">Dies verhindert die übermäßige Buchung wie beschrieben in der [zweite Lernprogramm](crud.md).</span><span class="sxs-lookup"><span data-stu-id="c4943-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="c4943-162">Wenn der Niederlassung leer ist, legt die Instructor.OfficeAssignment-Eigenschaft auf null fest, damit die zugehörige Zeile in der Tabelle OfficeAssignment gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="c4943-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="c4943-163">Speichert die Änderungen in der Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="c4943-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="c4943-164">Aktualisieren Sie die Ansicht Dozenten bearbeiten</span><span class="sxs-lookup"><span data-stu-id="c4943-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="c4943-165">In *Views/Instructors/Edit.cshtml*, Hinzufügen eines neuen Felds für die Bearbeitung der Bürostandort am Ende vor der **speichern** Schaltfläche:</span><span class="sxs-lookup"><span data-stu-id="c4943-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="c4943-166">Die app auszuführen, wählen Sie die **Lehrkräfte** Registerkarte, und klicken Sie dann auf **bearbeiten** auf einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="c4943-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="c4943-167">Ändern der **Filiale** , und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="c4943-167">Change the **Office Location** and click **Save**.</span></span>

![Instructor bearbeiten (Seite)](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="c4943-169">Hinzufügen von Kurs Zuweisungen auf der Seite "Dozenten bearbeiten"</span><span class="sxs-lookup"><span data-stu-id="c4943-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="c4943-170">Lehrkräfte können eine beliebige Anzahl von Kurse werden folgende Themen behandelt.</span><span class="sxs-lookup"><span data-stu-id="c4943-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="c4943-171">Jetzt müssen Sie die Seite Dozenten bearbeiten verbessern, durch Hinzufügen der Funktion zum Ändern der Kurs-Zuordnung, die eine Gruppe von Kontrollkästchen, wie im folgenden Screenshot gezeigt:</span><span class="sxs-lookup"><span data-stu-id="c4943-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor-Bearbeitungsseite mit Kurse](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="c4943-173">Die Beziehung zwischen den Entitäten Kurs und Dozenten ist m: n.</span><span class="sxs-lookup"><span data-stu-id="c4943-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="c4943-174">Zum Hinzufügen und Entfernen von Beziehungen, hinzufügen und Entfernen von Entitäten in und aus der CourseAssignments Join-Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="c4943-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="c4943-175">Die Benutzeroberfläche, die Sie ändern, welche Kurse ermöglicht ein Kursleiter ist ist zugewiesen eine Gruppe von Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="c4943-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="c4943-176">Ein Kontrollkästchen für jede Vorgehensweise in der Datenbank angezeigt wird, und diejenigen Argumente, die der Dozenten derzeit zugewiesen ist, werden ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="c4943-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="c4943-177">Der Benutzer kann aktivieren oder deaktivieren Kontrollkästchen, um den Kurs Zuweisungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="c4943-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="c4943-178">Wenn die Anzahl der Kurse viel größer sind, Sie möchten wahrscheinlich verwenden Sie eine andere Methode zur Darstellung der Daten in der Ansicht von, aber verwenden Sie die gleiche Methode zum Bearbeiten einer Joinentität erstellen oder Löschen von Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="c4943-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="c4943-179">Aktualisieren Sie den Controller für Dozenten</span><span class="sxs-lookup"><span data-stu-id="c4943-179">Update the Instructors controller</span></span>

<span data-ttu-id="c4943-180">Um Daten zur Ansicht für die Liste von Kontrollkästchen bereitzustellen, verwenden Sie eine Modellklasse anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c4943-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="c4943-181">Erstellen Sie *AssignedCourseData.cs* in der *SchoolViewModels* Ordner und Ersetzen Sie den vorhandenen Code durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c4943-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="c4943-182">In *InstructorsController.cs*, ersetzen Sie die HttpGet `Edit` -Methode durch folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="c4943-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="c4943-183">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="c4943-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="c4943-184">Der Code fügt unverzüglichem Laden für die `Courses` Navigationseigenschaft und ruft die neue `PopulateAssignedCourseData` -Methode zum Bereitstellen von Informationen für das Kontrollkästchen Array mithilfe der `AssignedCourseData` Modellklasse anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c4943-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="c4943-185">Der Code in der `PopulateAssignedCourseData` Methode liest über alle Kurs Entitäten auf, um eine Liste der Kurse mit Ansichtsklasse Modell zu laden.</span><span class="sxs-lookup"><span data-stu-id="c4943-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="c4943-186">Für jeden Kurs im Code wird überprüft, ob der Kurs in des Dozenten befindet `Courses` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c4943-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="c4943-187">Um effiziente Suche nach erstellen, bei der Überprüfung, ob ein Kurs zu den Dozenten zugewiesen wird, den Dozenten zugewiesene Kurse abgelegt sind eine `HashSet` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="c4943-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="c4943-188">Die `Assigned` -Eigenschaftensatz auf "true" für Kurse Kursleiter zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="c4943-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="c4943-189">Die Ansicht wird diese Eigenschaft verwenden, um zu bestimmen, welche Felder als angezeigt werden, müssen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c4943-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="c4943-190">Schließlich wird die Liste auf die Ansicht übergeben `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="c4943-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="c4943-191">Als Nächstes fügen Sie den Code, der ausgeführt wird, wenn der Benutzer klickt **speichern**.</span><span class="sxs-lookup"><span data-stu-id="c4943-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="c4943-192">Ersetzen Sie die `EditPost` -Methode durch folgenden code, und fügen Sie eine neue Methode, die aktualisiert die `Courses` Navigationseigenschaft die Instructor-Entität.</span><span class="sxs-lookup"><span data-stu-id="c4943-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="c4943-193">Die Methodensignatur unterscheidet sich jetzt von der HttpGet `Edit` Methode, sodass der Name der Methode von ändert `EditPost` an `Edit`.</span><span class="sxs-lookup"><span data-stu-id="c4943-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="c4943-194">Da die Sicht eine Auflistung von Entitäten Kurs besitzt, kann nicht automatisch der Modellbinder aktualisiert die `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c4943-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="c4943-195">Verwenden Sie statt des Modellbinders zum Aktualisieren der `CourseAssignments` Navigationseigenschaft, Sie dies vornehmen, in der neuen `UpdateInstructorCourses` Methode.</span><span class="sxs-lookup"><span data-stu-id="c4943-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="c4943-196">Daher müssen Sie zum Ausschließen der `CourseAssignments` Eigenschaft von der modellbindung.</span><span class="sxs-lookup"><span data-stu-id="c4943-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="c4943-197">Dies erfordert jede Änderung an der aufrufende Code `TryUpdateModel` , da Sie die Überladung Whitelisting verwenden und `CourseAssignments` befindet sich nicht in der Include-Liste.</span><span class="sxs-lookup"><span data-stu-id="c4943-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="c4943-198">Wenn keine Überprüfung aktiviert wurden, den Code in `UpdateInstructorCourses` initialisiert die `CourseAssignments` Navigationseigenschaft mit eine leere Auflistung und gibt:</span><span class="sxs-lookup"><span data-stu-id="c4943-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="c4943-199">Der Code durchläuft alle Kurse in der Datenbank und jeder Kurs gegen diejenigen zugewiesener den Dozenten im Vergleich zu den Vorgängen, die in der Ansicht ausgewählt wurden überprüft.</span><span class="sxs-lookup"><span data-stu-id="c4943-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="c4943-200">Um effiziente Suchvorgänge zu erleichtern, werden die letzten beiden Auflistungen in gespeichert `HashSet` Objekte.</span><span class="sxs-lookup"><span data-stu-id="c4943-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="c4943-201">Wenn das Kontrollkästchen für einen Kurs ausgewählt wurde, aber der Kurs befindet sich nicht in der `Instructor.CourseAssignments` Navigationseigenschaft, die Auflistung in der Navigationseigenschaft des Betriebs hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="c4943-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="c4943-202">Wenn das Kontrollkästchen für einen Kurs wurde nicht aktiviert, aber die Vorgehensweise in ist der `Instructor.CourseAssignments` Navigationseigenschaft, des Betriebs aus der Navigationseigenschaft entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="c4943-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="c4943-203">Aktualisieren Sie die Instructor-Ansichten</span><span class="sxs-lookup"><span data-stu-id="c4943-203">Update the Instructor views</span></span>

<span data-ttu-id="c4943-204">In *Views/Instructors/Edit.cshtml*, Hinzufügen einer **Kurse** Feld mit einem Array von Kontrollkästchen durch das Hinzufügen der folgenden code unmittelbar nach der `div` Elemente für die **Office**  Feld und vor der `div` -Element für die **speichern** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="c4943-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="c4943-205">Wenn Sie den Code in Visual Studio einfügen, werden Zeilenumbrüche auf eine Weise geändert werden, die der Code unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="c4943-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="c4943-206">Drücken Sie einmal STRG + Z, um die automatische Formatierung rückgängig zu machen.</span><span class="sxs-lookup"><span data-stu-id="c4943-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="c4943-207">Die Zeilenumbrüche wird korrigiert werden, damit sie sehen, wie Sie hier sehen.</span><span class="sxs-lookup"><span data-stu-id="c4943-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="c4943-208">Der Einzug keinen perfekt, aber die `@</tr><tr>`, `@:<td>`, `@:</td>`, und `@:</tr>` Zeilen müssen jeweils in einer einzelnen Zeile dargestellten oder erhalten Sie einen Laufzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="c4943-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="c4943-209">Drücken Sie mit dem Block von neuem Code ausgewählt ist Tab dreimal an den neuen Code mit dem vorhandenen Code.</span><span class="sxs-lookup"><span data-stu-id="c4943-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="c4943-210">Sie können den Status des Problems überprüfen [hier](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="c4943-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="c4943-211">Dieser Code erstellt eine HTML-Tabelle, die drei Spalten aufweist.</span><span class="sxs-lookup"><span data-stu-id="c4943-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="c4943-212">In jeder Spalte wird ein Kontrollkästchen, gefolgt von einem Titel, der den Titel und Kursnummer besteht.</span><span class="sxs-lookup"><span data-stu-id="c4943-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="c4943-213">Die Kontrollkästchen, die alle haben den gleichen Namen ("SelectedCourses"), der den Modellbinder informiert, die sie als eine Gruppe behandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="c4943-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="c4943-214">Das Value-Attribut der einzelnen Kontrollkästchen wird festgelegt, auf den Wert des `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c4943-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="c4943-215">Wenn die Seite zurückgesendet wird, übergibt der Modellbinder ein Array mit dem Controller an, aus denen besteht die `CourseID` Werte für nur die Kontrollkästchen ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="c4943-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="c4943-216">Die Kontrollkästchen gerendert werden, können solche, die für Kurse zu den Dozenten zugewiesen werden Attribute, die ausgewählt werden (angezeigt werden, wenn sie aktiviert), überprüft.</span><span class="sxs-lookup"><span data-stu-id="c4943-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="c4943-217">Die app auszuführen, wählen Sie die **Lehrkräfte** Registerkarte, und klicken Sie auf **bearbeiten** auf einen Kursleiter finden Sie unter der **bearbeiten** Seite.</span><span class="sxs-lookup"><span data-stu-id="c4943-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Instructor-Bearbeitungsseite mit Kurse](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="c4943-219">Ändern Sie einige Kurs Zuweisungen, und klicken Sie auf Speichern.</span><span class="sxs-lookup"><span data-stu-id="c4943-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="c4943-220">Die Änderungen, die Sie vornehmen, werden auf der Seite "Index" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c4943-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="c4943-221">Der Ansatz hier Dozenten Kurs Data funktioniert gut, wenn eine begrenzte Anzahl von Kurse zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="c4943-221">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="c4943-222">Bei Auflistungen, die viel größer sind, wäre eine andere Benutzeroberfläche und eine andere Methode für die Aktualisierung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c4943-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="c4943-223">Aktualisieren Sie die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="c4943-223">Update the Delete page</span></span>

<span data-ttu-id="c4943-224">In *InstructorsController.cs*, löschen Sie die `DeleteConfirmed` -Methode und einfügen, die im folgenden an seiner Stelle Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="c4943-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="c4943-225">Dieser Code macht die folgenden Änderungen:</span><span class="sxs-lookup"><span data-stu-id="c4943-225">This code makes the following changes:</span></span>

* <span data-ttu-id="c4943-226">Tut Eager loading für die `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c4943-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="c4943-227">Sie dazu gehören oder EF erfahren Sie nicht zu verwandten `CourseAssignment` Entitäten und wird nicht gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="c4943-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="c4943-228">Um zu vermeiden, diese zu lesen konnte hier kaskadierte Löschung in der Datenbank konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="c4943-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="c4943-229">Die Instructor gelöscht werden soll als Administrator, der alle Abteilungen zugewiesen wird, entfernt die Instructor-Zuweisung von die Abteilungen.</span><span class="sxs-lookup"><span data-stu-id="c4943-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="c4943-230">Hinzufügen von Office-Speicherort und Kurse auf der Seite "erstellen"</span><span class="sxs-lookup"><span data-stu-id="c4943-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="c4943-231">In *InstructorsController.cs*, löschen Sie die HttpGet und HttpPost `Create` Methoden, und fügen Sie dann den folgenden Code an ihrer Stelle:</span><span class="sxs-lookup"><span data-stu-id="c4943-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="c4943-232">Dieser Code ähnelt der für gesehen haben die `Edit` Methoden außer, dass zunächst keine Kurse ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="c4943-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="c4943-233">Die HttpGet `Create` Methodenaufrufe der `PopulateAssignedCourseData` Methode nicht, da aber in ausgewählte Kurse möglicherweise sortieren, geben Sie eine leere Auflistung für die `foreach` Schleife in der Ansicht (andernfalls den Code anzeigen Auslösung ausgeben würde ein null-Verweis).</span><span class="sxs-lookup"><span data-stu-id="c4943-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="c4943-234">Die HttpPost `Create` Methode fügt jeder ausgewählten Kurs zu den `CourseAssignments` Navigationseigenschaft, die vor dem Fehler bei der Validierung überprüft und der Datenbank den neuen Kursleiter hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c4943-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="c4943-235">Kurse werden hinzugefügt, auch wenn Modellfehler, damit beim Modellfehler (beispielsweise ein Benutzer ein ungültiges Datum sortiert) vorliegen, und die Seite wird mit einer Fehlermeldung erneut, Kurs Angaben, die vorgenommen wurden automatisch wiederhergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="c4943-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="c4943-236">Beachten Sie, dass damit Kurse zum Hinzufügen der `CourseAssignments` Navigationseigenschaft Sie die Eigenschaft als eine leere Auflistung zu initialisieren müssen:</span><span class="sxs-lookup"><span data-stu-id="c4943-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="c4943-237">Als Alternative zur controllercode Dadurch konnte Sie dazu im Modell Dozenten ändern den Eigenschaftengetter, um automatisch den Sammlungssatz zu erstellen, wenn er nicht vorhanden ist, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="c4943-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="c4943-238">Wenn Sie ändern die `CourseAssignments` Eigenschaft auf diese Weise können Sie explizite Eigenschaft Initialisierungscode im Controller entfernen.</span><span class="sxs-lookup"><span data-stu-id="c4943-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="c4943-239">In *Views/Instructor/Create.cshtml*, fügen Sie ein Office-Speicherort-Textfeld hinzu und überprüfen Sie die Kontrollkästchen für Kurse, bevor Sie die Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="c4943-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="c4943-240">Im Fall der bearbeiten (Seite), [zu beheben, die Formatierung, wenn Visual Studio den Code neu formatiert, wenn Sie es einfügen](#notepad).</span><span class="sxs-lookup"><span data-stu-id="c4943-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="c4943-241">Durch Ausführen der app, und erstellen einen Kursleiter testen.</span><span class="sxs-lookup"><span data-stu-id="c4943-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="c4943-242">Behandeln von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="c4943-242">Handling Transactions</span></span>

<span data-ttu-id="c4943-243">Wie in beschrieben die [CRUD-Lernprogramm](crud.md), das Entity Framework implizit Transaktionen implementiert.</span><span class="sxs-lookup"><span data-stu-id="c4943-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="c4943-244">Für Szenarien, in denen mehr, – beispielsweise gesteuert müssen wenn Vorgängen, die außerhalb von Entity Framework in einer Transaktion--enthalten sein sollen, finden Sie unter [Transaktionen](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="c4943-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="c4943-245">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c4943-245">Summary</span></span>

<span data-ttu-id="c4943-246">Sie haben jetzt die Einführung in die Arbeit mit verknüpften Daten abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="c4943-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="c4943-247">In den nächsten Lernprogrammen sehen Sie, wie Parallelitätskonflikte behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="c4943-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c4943-248">[Zurück](read-related-data.md)
[Weiter](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="c4943-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
