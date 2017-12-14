---
title: Razor-Seiten mit Entity Framework Core - 8-Lernprogramm 1
author: rick-anderson
description: Veranschaulicht das Erstellen einer Razor-Seiten-app, die mit Entity Framework Core
keywords: ASP.NET Core, Entity Framework Core-Lernprogramm
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: 98fe1b0c2dcf2e133d921b2cc8695bd2056c5ec0
ms.sourcegitcommit: a33737ea24e1ea9642e461d1bc90d6701f889436
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="d6633-104">Erste Schritte mit Razor-Seiten und Entity Framework Core mithilfe von Visual Studio (1 von 8)</span><span class="sxs-lookup"><span data-stu-id="d6633-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="d6633-105">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d6633-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d6633-106">Die Web-app von Contoso University Beispiel veranschaulicht, wie ASP.NET Core 2.0 MVC-Webanwendungen, die mit Entity Framework (EF) Core 2.0 und Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d6633-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="d6633-107">Die Beispiel-app ist eine Website für eine fiktive Contoso-Universität.</span><span class="sxs-lookup"><span data-stu-id="d6633-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="d6633-108">Es umfasst Funktionen wie Student Zulassung, Kurs Erstellung und Instructor-Zuweisungen.</span><span class="sxs-lookup"><span data-stu-id="d6633-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="d6633-109">Diese Seite ist die erste Aufgabe in eine Reihe von Lernprogrammen, die erläutern, wie die Universität von Contoso-Beispiel-app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6633-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="d6633-110">Herunterladen Sie, oder zeigen Sie abgeschlossene Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="d6633-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="d6633-111">[Anweisungen zum Herunterladen](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d6633-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6633-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="d6633-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="d6633-113">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="d6633-113">Troubleshooting</span></span>

<span data-ttu-id="d6633-114">Wenn Sie ein Problem Sie können nicht aufgelöst werden auftreten, im Allgemeinen finden Sie die Projektmappe durch Vergleichen des Codes: auf die [Phase abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) oder [abgeschlossenes Projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="d6633-114">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="d6633-115">Eine Übersicht über häufige Fehler und deren Lösung finden Sie unter [im Abschnitt Problembehandlung der im letzten Lernprogramm in der Reihe](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="d6633-115">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="d6633-116">Wenn Sie nicht finden, was Sie es benötigen, können Sie selbst eine Frage veröffentlichen, um StackOverflow.com für [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) oder [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="d6633-116">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="d6633-117">Diese Reihe von Lernprogrammen baut auf was in früheren Lernprogramme erfolgt.</span><span class="sxs-lookup"><span data-stu-id="d6633-117">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="d6633-118">Denken Sie speichern eine Kopie des Projekts nach jeder erfolgreichen Abschluss des Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="d6633-118">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="d6633-119">Wenn Probleme auftreten, können Sie über aus dem vorherigen Lernprogramm nicht zurück auf den Anfang starten.</span><span class="sxs-lookup"><span data-stu-id="d6633-119">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="d6633-120">Alternativ können Sie Herunterladen einer [Phase abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) und beginnen Sie erneut mit der abgeschlossenen Phase.</span><span class="sxs-lookup"><span data-stu-id="d6633-120">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="d6633-121">Die Contoso-University Web-app</span><span class="sxs-lookup"><span data-stu-id="d6633-121">The Contoso University web app</span></span>

<span data-ttu-id="d6633-122">Die in diesen Lernprogrammen erstellte app ist eine grundlegende University-Website.</span><span class="sxs-lookup"><span data-stu-id="d6633-122">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="d6633-123">Benutzer können anzeigen und Studenten, Kurs und Instructor-Informationen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d6633-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="d6633-124">Hier sind einige der Bildschirme, die im Lernprogramm erstellt.</span><span class="sxs-lookup"><span data-stu-id="d6633-124">Here are a few of the screens created in the tutorial.</span></span>

![Indexseite für Studenten](intro/_static/students-index.png)

![Studenten bearbeiten (Seite)](intro/_static/student-edit.png)

<span data-ttu-id="d6633-127">Den Stil der Benutzeroberfläche von dieser Website liegt nahe bei was von den integrierten Vorlagen generiert wird.</span><span class="sxs-lookup"><span data-stu-id="d6633-127">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="d6633-128">Das Lernprogramm Schwerpunkt liegt auf dem EF-Core mit Razor-Seiten, nicht die Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="d6633-128">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d6633-129">Erstellen Sie eine Web-app mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="d6633-129">Create a Razor Pages web app</span></span>

* <span data-ttu-id="d6633-130">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d6633-130">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d6633-131">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="d6633-131">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d6633-132">Nennen Sie das Projekt **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="d6633-132">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="d6633-133">Es ist wichtig, den Projektnamen *ContosoUniversity* daher die Namespaces überein, wenn Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="d6633-133">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="d6633-134">![neue ASP.NET Core-Webanwendung](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="d6633-134">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="d6633-135">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="d6633-135">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="d6633-136">![Webanwendung (Razor-Seiten)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="d6633-136">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="d6633-137">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.</span><span class="sxs-lookup"><span data-stu-id="d6633-137">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="d6633-138">Richten Sie den Website-Stil</span><span class="sxs-lookup"><span data-stu-id="d6633-138">Set up the site style</span></span>

<span data-ttu-id="d6633-139">Einige Änderungen richten Sie das Menü "Vorschauwebsite", die Layout und die Startseite.</span><span class="sxs-lookup"><span data-stu-id="d6633-139">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="d6633-140">Open *Pages/_Layout.cshtml* und die folgenden Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="d6633-140">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="d6633-141">Ändern von jedem Vorkommen von "ContosoUniversity" in "Contoso-Universität."</span><span class="sxs-lookup"><span data-stu-id="d6633-141">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="d6633-142">Es gibt drei Vorkommen.</span><span class="sxs-lookup"><span data-stu-id="d6633-142">There are three occurrences.</span></span>

* <span data-ttu-id="d6633-143">Fügen Sie im Menüeinträge für **Studenten**, **Kurse**, **Lehrkräfte**, und **Abteilungen**, und löschen Sie die **Kontakt** Menüeintrag.</span><span class="sxs-lookup"><span data-stu-id="d6633-143">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="d6633-144">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="d6633-144">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="d6633-145">In *Pages/Index.cshtml*, ersetzen Sie den Inhalt der Datei mit den folgenden Code hinzu, ersetzt wechseln den Text zu ASP.NET und MVC mit Text über diese app:</span><span class="sxs-lookup"><span data-stu-id="d6633-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="d6633-146">Drücken Sie STRG+F5, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6633-146">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="d6633-147">Die Startseite ist mit Registerkarten erstellt, die in den folgenden Lernprogrammen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="d6633-147">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso University-Startseite](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="d6633-149">Erstellen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="d6633-149">Create the data model</span></span>

<span data-ttu-id="d6633-150">Erstellen Sie Entitätsklassen für die Contoso-University-app.</span><span class="sxs-lookup"><span data-stu-id="d6633-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="d6633-151">Beginnen Sie mit den folgenden drei Entitäten:</span><span class="sxs-lookup"><span data-stu-id="d6633-151">Start with the following three entities:</span></span>

![Modelldiagramm Kurs-Registrierung-Student-Daten](intro/_static/data-model-diagram.png)

<span data-ttu-id="d6633-153">Es wird eine 1: n-Beziehung zwischen `Student` und `Enrollment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="d6633-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="d6633-154">Es wird eine 1: n-Beziehung zwischen `Course` und `Enrollment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="d6633-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="d6633-155">Eine Student kann in eine beliebige Anzahl von Kurse registrieren.</span><span class="sxs-lookup"><span data-stu-id="d6633-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="d6633-156">Ein Kurs kann eine beliebige Anzahl der Schüler in es registriert haben.</span><span class="sxs-lookup"><span data-stu-id="d6633-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="d6633-157">In den folgenden Abschnitten wird eine Klasse für jede dieser Entitäten erstellt.</span><span class="sxs-lookup"><span data-stu-id="d6633-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="d6633-158">Die Student-Entität</span><span class="sxs-lookup"><span data-stu-id="d6633-158">The Student entity</span></span>

![Diagramm der Student-Entität](intro/_static/student-entity.png)

<span data-ttu-id="d6633-160">Erstellen einer *Modelle* Ordner.</span><span class="sxs-lookup"><span data-stu-id="d6633-160">Create a *Models* folder.</span></span> <span data-ttu-id="d6633-161">In der *Modelle* Ordner, erstellen Sie eine Klassendatei namens *Student.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d6633-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="d6633-162">Die `ID` Eigenschaft wird die Primärschlüsselspalte der Datenbanktabelle (DB), die diese Klasse entspricht.</span><span class="sxs-lookup"><span data-stu-id="d6633-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="d6633-163">Standardmäßig EF Core interpretiert eine Eigenschaft mit dem Namen `ID` oder `classnameID` als Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="d6633-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="d6633-164">Die `Enrollments` Eigenschaft ist eine Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d6633-164">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d6633-165">Navigationseigenschaften Verknüpfen mit anderen Entitäten, die zu dieser Entität verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="d6633-165">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="d6633-166">In diesem Fall die `Enrollments` Eigenschaft eine `Student entity` enthält alle der `Enrollment` , die verbundenen, Entitäten `Student`.</span><span class="sxs-lookup"><span data-stu-id="d6633-166">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="d6633-167">Z. B. wenn eine Student-Zeile in der Datenbank verfügt über zwei verknüpfte Zeilen, die Registrierung der `Enrollments` Navigationseigenschaft enthält diese zwei `Enrollment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="d6633-167">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="d6633-168">Eine verknüpfte `Enrollment` Zeile ist eine Zeile, diese Student Primärschlüsselwert in enthält, der `StudentID` Spalte.</span><span class="sxs-lookup"><span data-stu-id="d6633-168">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="d6633-169">Nehmen wir beispielsweise an den Studenten mit ID = 1 verfügt über zwei Zeilen der `Enrollment` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="d6633-169">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="d6633-170">Die `Enrollment` Tabelle verfügt über zwei Zeilen mit `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="d6633-170">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="d6633-171">`StudentID`ist ein Fremdschlüssel in der `Enrollment` Tabelle, der angibt, die Studenten in die `Student` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="d6633-171">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="d6633-172">Wenn eine Navigationseigenschaft über mehrere Entitäten enthalten kann, die Navigationseigenschaft muss vom Listentyp sein, z. B. `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="d6633-172">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="d6633-173">`ICollection<T>`kann angegeben werden, oder ein Typ sein wie z. B. `List<T>` oder `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="d6633-173">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="d6633-174">Wenn `ICollection<T>` ist EF Core verwendet, erstellt werden. eine `HashSet<T>` Auflistung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="d6633-174">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="d6633-175">Navigationseigenschaften, die mehrere Entitäten enthalten, stammen von m: n- und 1: n-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="d6633-175">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="d6633-176">Die Registrierung-Entität</span><span class="sxs-lookup"><span data-stu-id="d6633-176">The Enrollment entity</span></span>

![Diagramm der Enrollment-Entität](intro/_static/enrollment-entity.png)

<span data-ttu-id="d6633-178">In der *Modelle* Ordner erstellen *Enrollment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d6633-178">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="d6633-179">Die `EnrollmentID` Eigenschaft ist der Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="d6633-179">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="d6633-180">Diese Entität verwendet die `classnameID` Muster anstelle von `ID` wie die `Student` Entität.</span><span class="sxs-lookup"><span data-stu-id="d6633-180">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="d6633-181">In der Regel Entwickler wählen Sie ein Muster und verwenden Sie es in das Datenmodell.</span><span class="sxs-lookup"><span data-stu-id="d6633-181">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="d6633-182">In einem späteren Lernprogramm wird mit der ID ohne Classname angezeigt, die zum Implementieren der Vererbung im Datenmodell zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="d6633-182">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="d6633-183">Die `Grade` Eigenschaft ist ein `enum`.</span><span class="sxs-lookup"><span data-stu-id="d6633-183">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="d6633-184">Das Fragezeichen nach der `Grade` Typdeklaration gibt an, dass die `Grade` Eigenschaft NULL ist zulässig.</span><span class="sxs-lookup"><span data-stu-id="d6633-184">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="d6633-185">Eine Klasse, die null ist unterscheidet sich von einer NULL Grade – null bedeutet, dass eine Klasse ist nicht bekannt ist oder noch nicht zugewiesen wurden, noch.</span><span class="sxs-lookup"><span data-stu-id="d6633-185">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="d6633-186">Die `StudentID` Eigenschaft ist ein Fremdschlüssel, und die entsprechende Navigationseigenschaft ist `Student`.</span><span class="sxs-lookup"><span data-stu-id="d6633-186">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="d6633-187">Ein `Enrollment` Entität ist mit einem zugeordneten `Student` Entität, damit die Eigenschaft eine einzelnen enthält `Student` Entität.</span><span class="sxs-lookup"><span data-stu-id="d6633-187">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="d6633-188">Die `Student` Entität unterscheidet sich von der `Student.Enrollments` Navigationseigenschaft, die enthält mehrere `Enrollment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="d6633-188">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="d6633-189">Die `CourseID` Eigenschaft ist ein Fremdschlüssel, und die entsprechende Navigationseigenschaft ist `Course`.</span><span class="sxs-lookup"><span data-stu-id="d6633-189">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="d6633-190">Ein `Enrollment` Entität ist mit einem zugeordneten `Course` Entität.</span><span class="sxs-lookup"><span data-stu-id="d6633-190">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="d6633-191">EF Core interpretiert eine Eigenschaft als Fremdschlüssel, wenn es heißt `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d6633-191">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="d6633-192">Beispielsweise`StudentID` für die `Student` Navigationseigenschaft, da die `Student` primären Schlüssel der Entität ist `ID`.</span><span class="sxs-lookup"><span data-stu-id="d6633-192">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="d6633-193">Fremdschlüsseleigenschaften können auch benannte `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d6633-193">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="d6633-194">Beispielsweise `CourseID` seit der `Course` primären Schlüssel der Entität ist `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d6633-194">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="d6633-195">Die Kurs-Entität</span><span class="sxs-lookup"><span data-stu-id="d6633-195">The Course entity</span></span>

![Diagramm der Kurs-Entität](intro/_static/course-entity.png)

<span data-ttu-id="d6633-197">In der *Modelle* Ordner erstellen *Course.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d6633-197">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="d6633-198">Die `Enrollments` Eigenschaft ist eine Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d6633-198">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d6633-199">Ein `Course` Entität kann sich auf eine beliebige Anzahl von beziehen `Enrollment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="d6633-199">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="d6633-200">Die `DatabaseGenerated` Attribut ermöglicht der app, geben Sie den Primärschlüssel, sondern mit der Datenbank generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="d6633-200">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="d6633-201">Erstellen Sie den Datenbankkontext SchoolContext</span><span class="sxs-lookup"><span data-stu-id="d6633-201">Create the SchoolContext DB context</span></span>

<span data-ttu-id="d6633-202">Die Hauptklasse, die EF-Kernfunktionalität für ein Datenmodell für die angegebenen Koordinaten wird der DB-Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d6633-202">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="d6633-203">Der Datenkontext abgeleitet `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d6633-203">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="d6633-204">Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="d6633-204">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="d6633-205">In diesem Projekt die Klasse heißt `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d6633-205">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="d6633-206">Im Projektordner, erstellen Sie einen Ordner namens *Daten*.</span><span class="sxs-lookup"><span data-stu-id="d6633-206">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="d6633-207">In der *Daten* Ordner erstellen *SchoolContext.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d6633-207">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="d6633-208">Dieser Code erstellt eine `DbSet` -Eigenschaft für jede Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="d6633-208">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="d6633-209">In der EF-Core-Terminologie:</span><span class="sxs-lookup"><span data-stu-id="d6633-209">In EF Core terminology:</span></span>

* <span data-ttu-id="d6633-210">In der Regel eine Entitätenmenge entspricht einem DB-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="d6633-210">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="d6633-211">Eine Entität entspricht einer Zeile in der Tabelle.</span><span class="sxs-lookup"><span data-stu-id="d6633-211">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d6633-212">`DbSet<Enrollment>`und `DbSet<Course>` kann ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="d6633-212">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="d6633-213">EF Core nimmt diese implizit da die `Student` Entitätsverweise der `Enrollment` Entität, und die `Enrollment` Entitätsverweise der `Course` Entität.</span><span class="sxs-lookup"><span data-stu-id="d6633-213">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="d6633-214">Behalten Sie für dieses Lernprogramm `DbSet<Enrollment>` und `DbSet<Course>` in der `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d6633-214">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="d6633-215">Wenn die Datenbank erstellt wird, EF Core Tabellen erstellt, deren Namen identisch mit der `DbSet` Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="d6633-215">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="d6633-216">Eigenschaftennamen für Sammlungen sind in der Regel im plural (Studenten statt Student).</span><span class="sxs-lookup"><span data-stu-id="d6633-216">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="d6633-217">Entwickler streiten, ob Tabellennamen im plural sein soll.</span><span class="sxs-lookup"><span data-stu-id="d6633-217">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="d6633-218">Für diese Lernprogramme ist das Standardverhalten durch Angabe von singular Tabellennamen in der DbContext überschrieben.</span><span class="sxs-lookup"><span data-stu-id="d6633-218">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="d6633-219">Um im singular Tabellennamen anzugeben, fügen Sie den folgenden hervorgehobenen Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6633-219">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="d6633-220">Registrieren Sie den Kontext mit Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="d6633-220">Register the context with dependency injection</span></span>

<span data-ttu-id="d6633-221">ASP.NET Core enthält [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6633-221">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d6633-222">Dienste (z. B. EF Core Datenbankkontext) werden während des Anwendungsstarts Abhängigkeitsinjektion registriert.</span><span class="sxs-lookup"><span data-stu-id="d6633-222">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d6633-223">Komponenten, die diese Dienste (z. B. Razor-Seiten) erfordern, werden diese Dienste über Konstruktorparameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="d6633-223">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d6633-224">Der Konstruktorcode, die eine Instanz des DB-Kontext abruft, wird später in diesem Lernprogramm gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d6633-224">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d6633-225">Zum Registrieren `SchoolContext` als Dienst öffnen *Startup.cs*, und fügen Sie die markierten Zeilen, die die `ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="d6633-225">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="d6633-226">Der Name der Verbindungszeichenfolge übergeben an den Kontext durch Aufrufen einer Methode für eine `DbContextOptionsBuilder` Objekt.</span><span class="sxs-lookup"><span data-stu-id="d6633-226">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="d6633-227">Für die lokale Entwicklung der [ASP.NET Core Konfigurationssystem](xref:fundamentals/configuration/index) liest die Verbindungszeichenfolge aus der *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="d6633-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="d6633-228">Hinzufügen `using` -Anweisungen für `ContosoUniversity.Data` und `Microsoft.EntityFrameworkCore` Namespaces.</span><span class="sxs-lookup"><span data-stu-id="d6633-228">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="d6633-229">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="d6633-229">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="d6633-230">Öffnen der *appsettings.json* Datei, und fügen Sie eine Verbindungszeichenfolge ein, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="d6633-230">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="d6633-231">Verwendet die vorherigen Verbindungszeichenfolge `ConnectRetryCount=0` verhindern [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) reagiert.</span><span class="sxs-lookup"><span data-stu-id="d6633-231">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="d6633-232">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d6633-232">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d6633-233">Die Verbindungszeichenfolge gibt eine SQL Server-LocalDB-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d6633-233">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="d6633-234">LocalDB ist eine vereinfachte Version von SQL Server Express-Datenbankmoduls und ist für die Entwicklung von Apps, nicht für Produktionszwecke vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="d6633-234">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="d6633-235">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="d6633-235">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="d6633-236">Erstellt standardmäßig LocalDB *mdf* DB Dateien in den `C:/Users/<user>` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d6633-236">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="d6633-237">Fügen Sie Code zum Initialisieren der Datenbank mit der Testdaten</span><span class="sxs-lookup"><span data-stu-id="d6633-237">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="d6633-238">EF Core erstellt eine leere DB.</span><span class="sxs-lookup"><span data-stu-id="d6633-238">EF Core creates an empty DB.</span></span> <span data-ttu-id="d6633-239">In diesem Abschnitt eine *Ausgangswert* Methode geschrieben wird, um sie mit Testdaten auszustatten.</span><span class="sxs-lookup"><span data-stu-id="d6633-239">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="d6633-240">In der *Daten* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *DbInitializer.cs* und fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6633-240">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="d6633-241">Der Code überprüft, ob alle Studenten in der Datenbank vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="d6633-241">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="d6633-242">Wenn keine Schüler in der Datenbank vorhanden sind, wird die Datenbank mit Testdaten mit Anfangsdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="d6633-242">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="d6633-243">Es lädt Testdaten in Arrays statt `List<T>` Sammlungen, um die Leistung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="d6633-243">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="d6633-244">Die `EnsureCreated` -Methode erstellt automatisch die Datenbank für den DB-Kontext.</span><span class="sxs-lookup"><span data-stu-id="d6633-244">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="d6633-245">Wenn die Datenbank vorhanden ist, `EnsureCreated` zurück, ohne die Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="d6633-245">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="d6633-246">In *"Program.cs"*, ändern Sie die `Main` Methode, um die folgenden Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="d6633-246">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="d6633-247">Eine Instanz des DB-Kontext aus der abhängigkeitseinschleusungscontainer abrufen.</span><span class="sxs-lookup"><span data-stu-id="d6633-247">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="d6633-248">Rufen Sie die Seed-Methode, übergibt Sie an den Kontext.</span><span class="sxs-lookup"><span data-stu-id="d6633-248">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="d6633-249">Löschen Sie den Kontext aus, wenn Seed-Methode abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="d6633-249">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="d6633-250">Der folgende Code zeigt die aktualisierte *"Program.cs"* Datei.</span><span class="sxs-lookup"><span data-stu-id="d6633-250">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="d6633-251">Beim ersten der app ausführen wird die Datenbank erstellt und mit Anfangsdaten gefüllt mit Testdaten.</span><span class="sxs-lookup"><span data-stu-id="d6633-251">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="d6633-252">Wenn das Datenmodell aktualisiert wird:</span><span class="sxs-lookup"><span data-stu-id="d6633-252">When the data model is updated:</span></span>
* <span data-ttu-id="d6633-253">Löschen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="d6633-253">Delete the DB.</span></span>
* <span data-ttu-id="d6633-254">Aktualisieren der Seed-Methode.</span><span class="sxs-lookup"><span data-stu-id="d6633-254">Update the seed method.</span></span>
* <span data-ttu-id="d6633-255">Führen Sie die app und eine neue per Seeding hinzugefügte Datenbank wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="d6633-255">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="d6633-256">In späteren Lernprogrammen wird die Datenbank die Daten ändert, modellieren, ohne zu löschen und Neuerstellen der Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="d6633-256">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="d6633-257">Die Tools sind Gerüst hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d6633-257">Add scaffold tooling</span></span>

<span data-ttu-id="d6633-258">In diesem Abschnitt wird die Paket-Manager-Konsole (PMC) verwendet, die Visual Studio Code-Generierung Webpaket hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d6633-258">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="d6633-259">Dieses Paket ist für die Ausführung des Gerüstbaumoduls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d6633-259">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="d6633-260">Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="d6633-260">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="d6633-261">Geben Sie in der Paket-Manager-Konsole (PMC) die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="d6633-261">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="d6633-262">Mit dem vorherige Befehl fügt das NuGet-Pakete in der Datei *.csproj:</span><span class="sxs-lookup"><span data-stu-id="d6633-262">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="d6633-263">Das Gerüst für das Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="d6633-263">Scaffold the model</span></span>

* <span data-ttu-id="d6633-264">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="d6633-264">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d6633-265">Führen Sie die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="d6633-265">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="d6633-266">Wenn der folgende Fehler generiert wird:</span><span class="sxs-lookup"><span data-stu-id="d6633-266">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="d6633-267">Führen Sie den Befehl erneut aus, und einen Kommentar am unteren Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="d6633-267">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="d6633-268">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="d6633-268">Build the project.</span></span> <span data-ttu-id="d6633-269">Der Build generiert Fehler wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d6633-269">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="d6633-270">Globales ändern `_context.Student` auf `_context.Students` (d. h. hinzufügen ein "s" `Student`).</span><span class="sxs-lookup"><span data-stu-id="d6633-270">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="d6633-271">7 Vorkommen gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="d6633-271">7 occurrences are found and updated.</span></span> <span data-ttu-id="d6633-272">Wir hoffen, beheben Sie [dieses Fehlers](https://github.com/aspnet/Scaffolding/issues/633) in der nächsten Version.</span><span class="sxs-lookup"><span data-stu-id="d6633-272">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="d6633-273">Testen der App</span><span class="sxs-lookup"><span data-stu-id="d6633-273">Test the app</span></span>

<span data-ttu-id="d6633-274">Führen Sie die app, und wählen Sie die **Studenten** Link.</span><span class="sxs-lookup"><span data-stu-id="d6633-274">Run the app and select the **Students** link.</span></span> <span data-ttu-id="d6633-275">Abhängig von der Breite des Browserfensters die **Studenten** Link am oberen Rand der Seite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d6633-275">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="d6633-276">Wenn die **Studenten** Link ist nicht sichtbar, klicken Sie auf das Symbol "Navigation" in der oberen rechten Ecke.</span><span class="sxs-lookup"><span data-stu-id="d6633-276">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Contoso University-Startseite schmalen](intro/_static/home-page-narrow.png)

<span data-ttu-id="d6633-278">Testen der **erstellen**, **bearbeiten**, und **Details** Links.</span><span class="sxs-lookup"><span data-stu-id="d6633-278">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="d6633-279">Zeigen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="d6633-279">View the DB</span></span>

<span data-ttu-id="d6633-280">Wenn die app gestartet wird, `DbInitializer.Initialize` Aufrufe `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="d6633-280">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="d6633-281">`EnsureCreated`erkennt, ob die Datenbank vorhanden ist, und bei Bedarf erstellt.</span><span class="sxs-lookup"><span data-stu-id="d6633-281">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="d6633-282">Wenn es befinden sich keine Schüler in der Datenbank zu den `Initialize` Methode fügt den Studenten.</span><span class="sxs-lookup"><span data-stu-id="d6633-282">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="d6633-283">Open **Objekt-Explorer von SQL Server** (SSOX) aus der **Ansicht** Menü in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6633-283">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="d6633-284">Klicken Sie im SSOX, auf **(Localdb) \MSSQLLocalDB > Datenbanken > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="d6633-284">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="d6633-285">Erweitern Sie die **Tabellen** Knoten.</span><span class="sxs-lookup"><span data-stu-id="d6633-285">Expand the **Tables** node.</span></span>

<span data-ttu-id="d6633-286">Mit der rechten Maustaste die **Student** Tabelle, und klicken Sie auf **Ansichtsdaten** an die erstellten Spalten und Zeilen in die Tabelle eingefügt.</span><span class="sxs-lookup"><span data-stu-id="d6633-286">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="d6633-287">Die *mdf* und *ldf* Datenbankdateien befinden sich in der *C:\Users\\ <yourusername>*  Ordner.</span><span class="sxs-lookup"><span data-stu-id="d6633-287">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="d6633-288">`EnsureCreated`wird auf app-Start aufgerufen, die den folgenden Workflow ermöglicht:</span><span class="sxs-lookup"><span data-stu-id="d6633-288">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="d6633-289">Löschen Sie die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="d6633-289">Delete the DB.</span></span>
* <span data-ttu-id="d6633-290">Ändern des DB-Schemas (z. B. Hinzufügen einer `EmailAddress` Feld).</span><span class="sxs-lookup"><span data-stu-id="d6633-290">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="d6633-291">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="d6633-291">Run the app.</span></span>

<span data-ttu-id="d6633-292">`EnsureCreated`erstellt eine Datenbank mit der`EmailAddress` Spalte.</span><span class="sxs-lookup"><span data-stu-id="d6633-292">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="d6633-293">Konventionen</span><span class="sxs-lookup"><span data-stu-id="d6633-293">Conventions</span></span>

<span data-ttu-id="d6633-294">Der Anteil am Code in der Reihenfolge für EF Core zum Erstellen einer vollständigen Datenbank ist aufgrund der Verwendung der Konventionen oder die Annahmen, die EF Core gerichteten minimal.</span><span class="sxs-lookup"><span data-stu-id="d6633-294">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="d6633-295">Die Namen der `DbSet` Eigenschaften dienen als Tabellennamen auf Richtigkeit.</span><span class="sxs-lookup"><span data-stu-id="d6633-295">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="d6633-296">Für Entitäten, die keinen Verweis durch einen `DbSet` Eigenschaft Entitätsklasse Namen als Tabellennamen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d6633-296">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="d6633-297">Entitätsnamen-Eigenschaft werden für Spaltennamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6633-297">Entity property names are used for column names.</span></span>

* <span data-ttu-id="d6633-298">Eigenschaften der Entität, die ID oder ClassnameID benannt sind, werden als Eigenschaften des Primärschlüssels erkannt.</span><span class="sxs-lookup"><span data-stu-id="d6633-298">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="d6633-299">Eine Eigenschaft wird als eine Fremdschlüsseleigenschaft interpretiert, wenn es heißt  *<navigation property name> <primary key property name>*  (z. B. `StudentID` für die `Student` Navigationseigenschaft seit der `Student` primären Schlüssel der Entität ist `ID`).</span><span class="sxs-lookup"><span data-stu-id="d6633-299">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="d6633-300">Können den Namen von Fremdschlüsseleigenschaften  *<primary key property name>*  (z. B. `EnrollmentID` seit der `Enrollment` primären Schlüssel der Entität ist `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="d6633-300">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="d6633-301">Üblichem Verhalten kann überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="d6633-301">Conventional behavior can be overridden.</span></span> <span data-ttu-id="d6633-302">Beispielsweise können die Tabellennamen explizit angegeben werden wie zuvor in diesem Lernprogramm gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d6633-302">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="d6633-303">Die Spaltennamen können explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="d6633-303">The column names can be explicitly set.</span></span> <span data-ttu-id="d6633-304">Primärschlüssel und Fremdschlüssel können explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="d6633-304">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="d6633-305">Asynchronen code</span><span class="sxs-lookup"><span data-stu-id="d6633-305">Asynchronous code</span></span>

<span data-ttu-id="d6633-306">Asynchrone Programmierung ist der Standardmodus für ASP.NET Core und EF Core.</span><span class="sxs-lookup"><span data-stu-id="d6633-306">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="d6633-307">Ein Webserver verfügt über eine begrenzte Anzahl von Threads zur Verfügung stehen, und in Situationen mit hoher Last möglicherweise alle verfügbaren Threads verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6633-307">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="d6633-308">In diesem Fall kann der Server neue Anforderungen nicht verarbeiten, bis die Threads freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="d6633-308">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="d6633-309">Mit synchroner Code möglicherweise viele Threads einrichten gebunden werden, während sie tatsächlich alle Aufgaben durchführt werden nicht, da für e/a auf Abschluss warten, können sie.</span><span class="sxs-lookup"><span data-stu-id="d6633-309">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="d6633-310">Wenn ein Prozess für e/a auf den Abschluss wartet, wird der Thread mit asynchronen Code für den Server für die Verarbeitung anderer Anforderungen verwenden freigegeben.</span><span class="sxs-lookup"><span data-stu-id="d6633-310">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="d6633-311">Folglich asynchronen Code ermöglicht es, Server-Ressourcen effizienter genutzt werden, und der Server ist aktiviert, um weitere ohne Verzögerungen-Datenverkehr zu bewältigen.</span><span class="sxs-lookup"><span data-stu-id="d6633-311">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="d6633-312">Asynchronen Code zur Laufzeit eine kleine Menge an Mehraufwand führen.</span><span class="sxs-lookup"><span data-stu-id="d6633-312">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="d6633-313">Mit niedrigem Datenverkehr in Situationen die Leistungseinbußen sind zu vernachlässigen, während für Situationen mit hohem Datenverkehr, der möglicher Leistungssteigerungen erhebliche.</span><span class="sxs-lookup"><span data-stu-id="d6633-313">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="d6633-314">Im folgenden Code wird die `async` -Schlüsselwort, `Task<T>` Rückgabewert, `await` -Schlüsselwort, und `ToListAsync` Methode stellen den Code, der asynchron ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="d6633-314">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="d6633-315">Die `async` Schlüsselwort weist den Compiler an:</span><span class="sxs-lookup"><span data-stu-id="d6633-315">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="d6633-316">Generieren Sie Rückrufe für Teile des Methodentextes.</span><span class="sxs-lookup"><span data-stu-id="d6633-316">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="d6633-317">Erstellen Sie automatisch die [Aufgabe](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) -Objekt, das zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="d6633-317">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="d6633-318">Weitere Informationen finden Sie unter [Rückgabetyp Task](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="d6633-318">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="d6633-319">Die implizite Rückgabetyp `Task` stellt derzeit ausgeführte Arbeit dar.</span><span class="sxs-lookup"><span data-stu-id="d6633-319">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="d6633-320">Die `await` Schlüsselwort weist den Compiler an die Methode in zwei Teile geteilt.</span><span class="sxs-lookup"><span data-stu-id="d6633-320">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="d6633-321">Der erste Teil endet mit dem Vorgang, der asynchron gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="d6633-321">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="d6633-322">Der zweite Teil ist eine Rückrufmethode abgelegt, die aufgerufen wird, wenn der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="d6633-322">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="d6633-323">`ToListAsync`ist die asynchrone Version der `ToList` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="d6633-323">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="d6633-324">Einige Aspekte, die beim asynchronen Code schreiben, die mithilfe von EF Core geachtet werden:</span><span class="sxs-lookup"><span data-stu-id="d6633-324">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="d6633-325">Nur die Anweisungen, die dazu führen, dass Abfragen oder Befehle an die Datenbank gesendet werden, werden asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d6633-325">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="d6633-326">Umfasst, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, und `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="d6633-326">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="d6633-327">Es enthält keine Anweisungen auszuführen, ändern nur, eine `IQueryable`, wie z. B. `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="d6633-327">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="d6633-328">Ein EF-Core-Kontext ist nicht sicher threaded: nicht Versuch, das mehrere Vorgänge parallel auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6633-328">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="d6633-329">Um die Leistungsvorteile von asynchronem Code nutzen zu können, stellen Sie sicher, dass die Bibliothek Pakete (z. B. Paging) Async verwenden, wenn sie EF-Core-Methoden aufrufen, die Abfragen mit der Datenbank zu senden.</span><span class="sxs-lookup"><span data-stu-id="d6633-329">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="d6633-330">Weitere Informationen zur asynchronen Programmierung in .NET finden Sie unter [Übersicht über die asynchrone](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="d6633-330">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="d6633-331">In den nächsten Lernprogrammen, grundlegende CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge werden überprüft.</span><span class="sxs-lookup"><span data-stu-id="d6633-331">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d6633-332">Nächste</span><span class="sxs-lookup"><span data-stu-id="d6633-332">Next</span></span>](xref:data/ef-rp/crud)
