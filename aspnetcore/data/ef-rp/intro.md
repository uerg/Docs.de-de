---
title: 'Razor Pages mit Entity Framework Core: Tutorial 1 von 8'
author: rick-anderson
description: Informationen zum Erstellen einer Razor Pages-App mit Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="ec1bb-103">Erste Schritte mit Razor Pages und Entity Framework Core unter Verwendung von Visual Studio (1 von 8)</span><span class="sxs-lookup"><span data-stu-id="ec1bb-103">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="ec1bb-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec1bb-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ec1bb-105">Die Beispiel-Web-App der Contoso University veranschaulicht, wie mit Entity Framework Core 2.0 und Visual Studio 2017 ASP.NET Core 2.0 MVC-Webanwendungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="ec1bb-106">Bei der Beispiel-App handelt es sich um eine Website für die fiktive Contoso University.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="ec1bb-107">Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="ec1bb-108">Dies ist die erste Seite eines mehrseitigen Tutorials, in dem die Erstellung der Beispiel-App der Contoso University erläutert wird.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="ec1bb-109">Download or view the completed app (Herunterladen oder anzeigen der vollständigen App).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="ec1bb-110">[Anweisungen zum Download.](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ec1bb-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec1bb-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ec1bb-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="ec1bb-112">Kenntnisse über [Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="ec1bb-113">Anfänger sollten den Artikel [Erste Schritte mit Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) lesen, bevor sie mit diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ec1bb-114">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="ec1bb-114">Troubleshooting</span></span>

<span data-ttu-id="ec1bb-115">Wenn Sie auf ein Problem stoßen, das Sie nicht lösen können, sollten Sie versuchen, Ihren Code mit der [abgeschlossenen Phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) oder dem [abgeschlossenem Projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final) zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="ec1bb-116">Eine Liste mit häufig auftretenden Fehlern und den jeweiligen Lösungen finden Sie im Abschnitt [zur Fehlerbehebung auf im letzten Tutorial dieser Tutorialreihe](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="ec1bb-117">Wenn Sie dort nicht die gewünschten Informationen finden, können Sie unter [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) für [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) oder [Entity Framework Core](https://stackoverflow.com/questions/tagged/entity-framework-core) eine Frage posten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="ec1bb-118">Dieses Tutorial basiert auf anderen älteren Tutorials.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="ec1bb-119">Sie sollten jedes Mal, wenn Sie erfolgreich ein Tutorial abgeschlossen haben, eine Kopie des Projekts erstellen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="ec1bb-120">Wenn Sie dann auf Probleme stoßen, können Sie zurück zum vorherigen Tutorial gehen und müssen nicht wieder ganz von vorne beginnen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="ec1bb-121">Stattdessen können Sie aber auch eine [abgeschlossene Phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) herunterladen und diese zum Starten verwenden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="ec1bb-122">Die Web-App der Contoso University</span><span class="sxs-lookup"><span data-stu-id="ec1bb-122">The Contoso University web app</span></span>

<span data-ttu-id="ec1bb-123">Bei der App, die mithilfe dieser Tutorials erstellt werden soll, handelt es sich um eine einfache Website einer Universität.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="ec1bb-124">Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="ec1bb-125">Im Folgenden sind einige Anzeigen dargestellt, die mithilfe dieses Tutorials erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-125">Here are a few of the screens created in the tutorial.</span></span>

![Indexseite „Studenten“](intro/_static/students-index.png)

![Bearbeitungsseite für Studenten](intro/_static/student-edit.png)

<span data-ttu-id="ec1bb-128">Der Benutzeroberflächenstil dieser Website ähnelt den durch die integrierten Vorlagen generierten Seiten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="ec1bb-129">In diesem Tutorial wird Entity Framework Core mit Razor Pages thematisiert. Die Benutzeroberfläche wird nicht erläutert.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ec1bb-130">Erstellen einer Razor Pages-Web-App</span><span class="sxs-lookup"><span data-stu-id="ec1bb-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="ec1bb-131">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ec1bb-132">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ec1bb-133">Geben Sie dem Projekt den Namen **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="ec1bb-134">Es ist wichtig, dass Sie dem Projekt exakt diesen Namen geben, sodass die Namespaces übereinstimmen, wenn der Code kopiert und eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="ec1bb-135">![neue ASP.NET Core-Webanwendung](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="ec1bb-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="ec1bb-136">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="ec1bb-137">![Webanwendung (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="ec1bb-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="ec1bb-138">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="ec1bb-139">Einrichten des Websitestils</span><span class="sxs-lookup"><span data-stu-id="ec1bb-139">Set up the site style</span></span>

<span data-ttu-id="ec1bb-140">Sie können das Websitemenü, das Layout und die Startseite über einige Änderungen einrichten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="ec1bb-141">Öffnen Sie *Pages/_Layout.cshtml*, und nehmen Sie die folgenden Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="ec1bb-142">Ändern Sie jedes „ContosoUniversity“ in „Contoso University“.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="ec1bb-143">Diese Begriffskombination kommt dreimal vor.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-143">There are three occurrences.</span></span>

* <span data-ttu-id="ec1bb-144">Fügen Sie Menüeinträge für **Studenten**, **Kurse**, **Dozenten** und **Abteilungen** hinzu, und löschen Sie den Menüeintrag **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="ec1bb-145">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-145">The changes are highlighted.</span></span> <span data-ttu-id="ec1bb-146">(Es wird *nicht* das gesamte Markup angezeigt.)</span><span class="sxs-lookup"><span data-stu-id="ec1bb-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="ec1bb-147">Ersetzen Sie in *Pages/Index.cshtml* die Inhalte der Datei durch den folgenden Code. Dadurch ersetzen Sie den Text zu ASP.NET und MVC durch Text zu dieser App:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="ec1bb-148">Drücken Sie STRG+F5, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="ec1bb-149">Im Folgenden wird die Startseite mit den Registerkarten angezeigt, die in den folgenden Tutorials erstellt wurden:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso University-Startseite](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="ec1bb-151">Erstellen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="ec1bb-151">Create the data model</span></span>

<span data-ttu-id="ec1bb-152">Erstellen Sie Entitätsklassen für die Contoso University-App.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="ec1bb-153">Beginnen Sie mit dem folgenden drei Entitäten:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-153">Start with the following three entities:</span></span>

![Datenmodelldiagramm zur Kursanmeldung für Studenten](intro/_static/data-model-diagram.png)

<span data-ttu-id="ec1bb-155">Es besteht eine 1:n-Beziehung zwischen der `Student`- und der `Enrollment`-Entität.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="ec1bb-156">Es besteht eine 1:n-Beziehung zwischen der `Course`- und der `Enrollment`-Entität.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="ec1bb-157">Jeder Student kann sich für eine beliebige Anzahl von Kursen anmelden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="ec1bb-158">Für jeden Kurs kann sich eine beliebige Anzahl von Studenten anmelden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="ec1bb-159">In den folgenden Abschnitten wird für jede dieser Entitäten eine Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="ec1bb-160">Die Entität „Student“</span><span class="sxs-lookup"><span data-stu-id="ec1bb-160">The Student entity</span></span>

![Entitätsdiagramm „Student“](intro/_static/student-entity.png)

<span data-ttu-id="ec1bb-162">Erstellen Sie einen Ordner *Models* (Modelle).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-162">Create a *Models* folder.</span></span> <span data-ttu-id="ec1bb-163">Erstellen Sie mit dem folgenden Code im Ordner *Models* (Modelle) eine Klassendatei mit dem Namen *Student.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="ec1bb-164">Die `ID`-Eigenschaft fungiert als Primärschlüsselspalte der Datenbanktabelle, die dieser Klasse entspricht.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="ec1bb-165">Standardmäßig interpretiert Entity Framework Core eine Eigenschaft mit dem Namen `ID` oder `classnameID` als Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="ec1bb-166">Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="ec1bb-167">Navigationseigenschaften werden mit anderen Entitäten verknüpft, die dieser Entität zugehörig sind.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="ec1bb-168">In diesem Fall enthält die `Enrollments`-Eigenschaft einer `Student entity` all diese `Enrollment`-Entitäten, die mit diesem `Student` in Zusammenhang stehen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="ec1bb-169">Wenn es z.B. für eine „Student“-Zeile in der Datenbank zwei zugehörige „Enrollment“-Zeilen gibt, enthält die Navigationseigenschaft `Enrollments` zwei `Enrollment`-Entitäten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="ec1bb-170">Bei einer zugehörigen `Enrollment`-Zeile handelt es sich um eine Zeile, die den Wert des Primärschlüssels des Studenten in der `StudentID`-Spalte enthält.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="ec1bb-171">Nehmen Sie z.B. an, dass es für den Studenten mit ID=1 zwei Zeilen in der `Enrollment`-Tabelle gibt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="ec1bb-172">Die `Enrollment`-Tabelle verfügt über zwei Zeilen mit `StudentID`=1.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="ec1bb-173">Bei `StudentID` handelt es sich um einen Fremdschlüssel in der `Enrollment`-Tabelle, der den Studenten in der `Student`-Tabelle angibt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="ec1bb-174">Wenn in einer Navigationseigenschaft mehrere Entitäten gespeichert werden können, muss es sich um einen Listentyp wie `ICollection<T>` handeln.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="ec1bb-175">`ICollection<T>` oder ein Typ wie `List<T>` oder `HashSet<T>` können angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="ec1bb-176">Wenn `ICollection<T>` verwendet wird, erstellt Entity Framework Core standardmäßig eine `HashSet<T>`-Auflistung.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="ec1bb-177">Navigationseigenschaften, die mehrere Entitäten enthalten, entstehen auf der Grundlage von m:n- oder 1:n-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="ec1bb-178">Die Entität „Enrollment“</span><span class="sxs-lookup"><span data-stu-id="ec1bb-178">The Enrollment entity</span></span>

![Entitätsdiagramm „Enrollment“](intro/_static/enrollment-entity.png)

<span data-ttu-id="ec1bb-180">Erstellen Sie mit dem folgenden Code im Ordner *Models* (Modelle) eine *Enrollment.cs*-Datei:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="ec1bb-181">Bei der `EnrollmentID`-Eigenschaft handelt es sich um den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="ec1bb-182">Diese Entität verwendet genau wie die `Student`-Entität das `classnameID`-Muster anstelle von `ID`.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="ec1bb-183">In der Regel wählen Entwickler ein Muster aus und verwenden dieses im gesamten Datenmodell.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="ec1bb-184">In einem der nächsten Tutorials wird erläutert, wie Sie eine ID ohne Klassennamen verwenden, um die Vererbung einfacher in das Datenmodell zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="ec1bb-185">Bei der `Grade`-Eigenschaft handelt es sich um eine `enum`.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="ec1bb-186">Das Fragezeichen nach der `Grade`-Typdeklaration gibt an, dass die `Grade`-Eigenschaft NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="ec1bb-187">Eine Grade-Eigenschaft mit dem Wert NULL unterscheidet sich von einer Grade-Eigenschaft mit dem Wert 0 (null). Der Wert NULL bedeutet, dass keine Grade-Eigenschaft bekannt ist oder noch keine zugewiesen wurde.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="ec1bb-188">Bei der `StudentID`-Eigenschaft handelt es sich um einen Fremdschlüssel und `Student` ist die entsprechende Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="ec1bb-189">Die `Enrollment`-Entität wird einer `Student`-Entität zugewiesen. Das bedeutet, dass die Eigenschaft genau eine `Student`-Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="ec1bb-190">Die `Student`-Entität unterscheidet sich von der `Student.Enrollments`-Navigationseigenschaft, die mehrere `Enrollment`-Entitäten enthält.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="ec1bb-191">Bei der `CourseID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und `Course` ist die entsprechende Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="ec1bb-192">Die `Enrollment`-Entität wird einer `Course`-Entität zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="ec1bb-193">Entity Framework Core interpretiert Eigenschaften als Fremdschlüssel, wenn diese den Namen `<navigation property name><primary key property name>` haben.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="ec1bb-194">Beispielsweise `StudentID` für die `Student`-Navigationseigenschaft, da `ID` der Primärschlüssel der `Student`-Entität ist.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="ec1bb-195">Fremdschlüsseleigenschaften können ebenfalls den Namen `<primary key property name>` haben.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="ec1bb-196">Beispielsweise `CourseID`, da `CourseID` der Primärschlüssel der `Course`-Entität ist.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="ec1bb-197">Die Entität „Course“</span><span class="sxs-lookup"><span data-stu-id="ec1bb-197">The Course entity</span></span>

![Entitätsdiagramm „Course“](intro/_static/course-entity.png)

<span data-ttu-id="ec1bb-199">Erstellen Sie mit dem folgenden Code im Ordner *Models* (Modelle) eine *Course.cs*-Datei:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="ec1bb-200">Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="ec1bb-201">`Course`-Entitäten können sich auf jede beliebige Anzahl von `Enrollment`-Entitäten beziehen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="ec1bb-202">Das `DatabaseGenerated`-Attribut lässt es zu, dass die App den Primärschlüssel angibt, sodass die Datenbank diesen nicht generieren muss.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="ec1bb-203">Erstellen des Datenbankkontexts „SchoolContext“</span><span class="sxs-lookup"><span data-stu-id="ec1bb-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="ec1bb-204">Bei der Datenbankkontextklasse handelt es sich um die Hauptklasse, die die Entity Framework Core-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="ec1bb-205">Der Datenkontext wird von `Microsoft.EntityFrameworkCore.DbContext` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="ec1bb-206">Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="ec1bb-207">In diesem Projekt heißt die Klasse `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="ec1bb-208">Erstellen Sie im Projektordner einen Ordner mit dem Namen *Data* (Daten).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="ec1bb-209">Erstellen Sie im Ordner *Data* (Daten) mit dem folgenden Code die *SchoolContext.cs*-Datei:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="ec1bb-210">Dieser Code erstellt eine `DbSet`-Eigenschaft für jede Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="ec1bb-211">Das heißt für Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-211">In EF Core terminology:</span></span>

* <span data-ttu-id="ec1bb-212">Entitätenmengen entsprechen in der Regel einer Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="ec1bb-213">Entitäten entsprechen Zeilen in Tabellen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="ec1bb-214">`DbSet<Enrollment>` und `DbSet<Course>` können ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="ec1bb-215">Diese sind implizit in Entity Framework Core enthalten, da die `Student`-Entität auf die `Enrollment`-Entität verweist, und die `Enrollment`-Entität auf die `Course`-Entität verweist.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="ec1bb-216">Behalten Sie für dieses Tutorial `DbSet<Enrollment>` und `DbSet<Course>` im `SchoolContext`-Kontext.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="ec1bb-217">Wenn die Datenbank erstellt wird, erstellt Entity Framework Core Tabellen mit Namen, die den `DbSet`-Eigenschaftennamen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="ec1bb-218">Eigenschaftennamen für Auflistungen stehen in der Regel im Plural (Students anstelle von Student).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="ec1bb-219">Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="ec1bb-220">In diesen Tutorials wird das Standardverhalten außer Kraft gesetzt, indem im DbContext Tabellennamen im Singular angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="ec1bb-221">Fügen Sie den folgenden hervorgehobenen Code hinzu, um Tabellennamen im Singular anzugeben:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="ec1bb-222">Registrieren des Kontexts durch Dependency Injection</span><span class="sxs-lookup"><span data-stu-id="ec1bb-222">Register the context with dependency injection</span></span>

<span data-ttu-id="ec1bb-223">[Dependency Injection](xref:fundamentals/dependency-injection) ist in ASP.NET Core enthalten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ec1bb-224">Dienste (z.B. der Datenbankkontext Entity Framework Core) werden über Dependency Injection beim Anwendungsstart registriert.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ec1bb-225">Komponenten, die diese Dienste erfordern (z.B. Razor Pages), werden von diesen Diensten über Konstruktorparameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="ec1bb-226">Der Konstruktorcode, der eine Datenbankkontextinstanz abruft, wird später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="ec1bb-227">Öffnen Sie die *Startup.cs-Datei*, und fügen Sie der `ConfigureServices`-Methode die hervorgehobenen Zeilen hinzu, um `SchoolContext` als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="ec1bb-228">Der Name der Verbindungszeichenfolge wird an den Kontext übergeben, indem Sie eine Methode auf einem `DbContextOptionsBuilder`-Objekt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="ec1bb-229">Für die lokale Entwicklung liest das [ASP.NET Core-Konfigurationssystem](xref:fundamentals/configuration/index) die Verbindungszeichenfolge aus der *appsettings.json*-Datei.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="ec1bb-230">Fügen Sie für die Namespaces `ContosoUniversity.Data` und `Microsoft.EntityFrameworkCore` die `using`-Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="ec1bb-231">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="ec1bb-232">Öffnen Sie die *appsettings.json*-Datei, und fügen Sie wie im folgenden Code dargestellt eine Verbindungszeichenfolge hinzu:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="ec1bb-233">Die darauf folgende Verbindungszeichenfolge verwendet `ConnectRetryCount=0`, um zu vermeiden, dass [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) nicht mehr reagiert.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="ec1bb-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="ec1bb-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="ec1bb-235">Die Verbindungszeichenfolge gibt eine SQL Server-LocalDB-Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="ec1bb-236">LocalDB ist eine Basisversion der SQL Server Express-Datenbank-Engine, die zwar für die Anwendungsentwicklung, aber nicht für den Produktionseinsatz bestimmt ist.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="ec1bb-237">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="ec1bb-238">Standardmäßig erstellt LocalDB *.mdf*-Datenbankdateien im `C:/Users/<user>`-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="ec1bb-239">Hinzufügen von Code zum Initialisieren der Datenbank mithilfe von Testdaten</span><span class="sxs-lookup"><span data-stu-id="ec1bb-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="ec1bb-240">Entity Framework Core erstellt eine leere Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="ec1bb-241">In diesem Abschnitt wird eine *Seed*-Methode geschrieben, um diese mit Testdaten aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="ec1bb-242">Erstellen Sie im Ordner *Data* eine neuen Klassendatei mit dem Namen *DbInitializer.cs*, und fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="ec1bb-243">Der Code überprüft, ob Studenten in der Datenbank enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="ec1bb-244">Wenn keine Studenten in der Datenbank enthalten sind, wird für diese ein Seeding mit Testdaten ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="ec1bb-245">Testdaten werden in Arrays anstelle von `List<T>`-Auflistungen geladen, um die Leistung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="ec1bb-246">Die `EnsureCreated`-Methode erstellt automatisch die Datenbank für den Datenbankkontext.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="ec1bb-247">Wenn die Datenbank vorhanden ist, wird `EnsureCreated` zurückgegeben, ohne Änderungen an der Datenbank vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="ec1bb-248">Ändern Sie in der *Program.cs*-Datei die `Main`-Methode, um die folgenden Vorgänge auszuführen:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="ec1bb-249">Rufen Sie eine Datenbankkontextinstanz aus dem Dependency Injection-Container ab.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="ec1bb-250">Rufen Sie die Seedmethode auf, indem Sie den Kontext an diese übergeben.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="ec1bb-251">Löschen Sie den Kontext, wenn die Seedmethode abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="ec1bb-252">Der folgende Code zeigt die aktualisierte *Program.cs*-Datei.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="ec1bb-253">Wenn die App das erste Mal ausgeführt wird, wird die Datenbank erstellt, und es wird ein Seeding mit Testdaten ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="ec1bb-254">Gehen Sie wie folgt vor, wenn ein Update für das Datenmodell ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-254">When the data model is updated:</span></span>
* <span data-ttu-id="ec1bb-255">Löschen Sie die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-255">Delete the DB.</span></span>
* <span data-ttu-id="ec1bb-256">Führen Sie ein Update für die Seedmethode aus.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-256">Update the seed method.</span></span>
* <span data-ttu-id="ec1bb-257">Führen Sie die App aus. Dann wird eine neue Datenbank erstellt, für die ein Seeding ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="ec1bb-258">In späteren Tutorials wird ein Update für die Datenbank ausgeführt, wenn das Datenmodell geändert wird, ohne die Datenbank zu löschen und neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="ec1bb-259">Hinzufügen von Gerüsttools</span><span class="sxs-lookup"><span data-stu-id="ec1bb-259">Add scaffold tooling</span></span>

<span data-ttu-id="ec1bb-260">In diesem Abschnitt wird die Paket-Manager-Konsole verwendet, um das Visual Studio-Paket zur Webcodegenerierung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="ec1bb-261">Dieses Paket ist für die Ausführung des Gerüstbaumoduls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="ec1bb-262">Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="ec1bb-263">Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="ec1bb-264">Über den obenstehenden Befehl werden die NuGet-Pakete zu der \*.csproj-Datei hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-264">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="ec1bb-265">Aufbauen des Modells</span><span class="sxs-lookup"><span data-stu-id="ec1bb-265">Scaffold the model</span></span>

* <span data-ttu-id="ec1bb-266">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ec1bb-267">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="ec1bb-268">Wenn der folgende Fehler generiert wird:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="ec1bb-269">Führen Sie den Befehl erneut aus, und hinterlassen Sie einen Kommentar unten auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="ec1bb-270">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="ec1bb-271">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="ec1bb-272">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-272">Build the project.</span></span> <span data-ttu-id="ec1bb-273">Der Build generiert z.B. die folgenden Fehler:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="ec1bb-274">Ändern Sie `_context.Student` allgemein in `_context.Students` (d.h., fügen Sie ein „s“ zu `Student` hinzu).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="ec1bb-275">Der Begriff wurde siebenmal gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="ec1bb-276">[Dieser Fehler](https://github.com/aspnet/Scaffolding/issues/633) sollte im nächsten Release behoben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="ec1bb-277">Testen der App</span><span class="sxs-lookup"><span data-stu-id="ec1bb-277">Test the app</span></span>

<span data-ttu-id="ec1bb-278">Führen Sie die App aus, und klicken Sie auf den Link **Students**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="ec1bb-279">Abhängig von der Breite Ihres Browsers wird der Link **Students** im oberen Bereich der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="ec1bb-280">Wenn der Link **Students** nicht angezeigt wird, klicken Sie auf das Navigationssymbol in der oberen rechten Ecke.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-280">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Contoso University-Startseite verkleinert](intro/_static/home-page-narrow.png)

<span data-ttu-id="ec1bb-282">Testen Sie die Links **Erstellen**, **Bearbeiten** und **Details**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="ec1bb-283">Abrufen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="ec1bb-283">View the DB</span></span>

<span data-ttu-id="ec1bb-284">Wenn die App gestartet wird, ruft `DbInitializer.Initialize` `EnsureCreated` auf.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="ec1bb-285">`EnsureCreated` ermittelt, ob die Datenbank vorhanden ist, und erstellt ggf. eine.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="ec1bb-286">Wenn keine Studenten in der Datenbank enthalten sind, werden über die `Initialize`-Methode Studenten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="ec1bb-287">Öffnen Sie über das Menü **Ansicht** im Visual Studio **SQL Server-Objekt-Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="ec1bb-288">Klicken Sie im SSOX auf **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="ec1bb-289">Erweitern Sie den Knoten **Tabellen**.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="ec1bb-290">Klicken Sie mit der rechten Maustaste auf die Tabelle **Student**, und klicken Sie auf **Daten anzeigen**, um die erstellten Spalten und die in die Tabelle eingefügten Zeilen aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="ec1bb-291">Die Datenbankdateien *.mdf* und *.ldf* befinden sich im Ordner *C:\Users\\<yourusername>*.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="ec1bb-292">`EnsureCreated` wird beim Starten der App aufgerufen, wodurch der folgende Workflow ermöglicht wird:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="ec1bb-293">Löschen Sie die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-293">Delete the DB.</span></span>
* <span data-ttu-id="ec1bb-294">Ändern Sie das Datenbankschema, also fügen Sie z.B. ein `EmailAddress`-Feld hinzu.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="ec1bb-295">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-295">Run the app.</span></span>

<span data-ttu-id="ec1bb-296">Über `EnsureCreated` wird eine Datenbank mit der `EmailAddress`-Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="ec1bb-297">Konventionen</span><span class="sxs-lookup"><span data-stu-id="ec1bb-297">Conventions</span></span>

<span data-ttu-id="ec1bb-298">Es wurde nur wenig Code in die Reihenfolge für Entity Framework Core geschrieben, um eine vollständige Datenbank zu erstellen, da Konventionen oder Annahmen von Entity Framework verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="ec1bb-299">Die Namen der `DbSet`-Eigenschaften werden als Tabellennamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="ec1bb-300">Für Entitäten, auf die nicht über eine `DbSet`-Eigenschaft verwiesen wird, werden Entitätsklassennamen als Tabellennamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="ec1bb-301">Eigenschaftennamen von Entitäten werden als Spaltennamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="ec1bb-302">Entitätseigenschaften mit dem Namen „ID“ oder „classnameID“ werden als Primärschlüsseleigenschaften erkannt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="ec1bb-303">Eigenschaften werden als Fremdschlüsseleigenschaften interpretiert, wenn Sie den Namen *<navigation property name><primary key property name>* haben – z.B. `StudentID` für die `Student`-Navigationseigenschaft, da der Primärschlüssel der `Student`-Entität `ID` lautet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="ec1bb-304">Fremdschlüsseleigenschaften können den Namen *<primary key property name>* haben – z.B. `EnrollmentID`, da der Primärschlüssel der `Enrollment`-Entität `EnrollmentID` lautet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="ec1bb-305">Konventionelles Verhalten kann überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="ec1bb-306">Beispielsweise können die Tabellennamen wie bereits in diesem Tutorial gezeigt explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="ec1bb-307">Die Spaltennamen können explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-307">The column names can be explicitly set.</span></span> <span data-ttu-id="ec1bb-308">Primärschlüssel und Fremdschlüssel können explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="ec1bb-309">Asynchroner Code</span><span class="sxs-lookup"><span data-stu-id="ec1bb-309">Asynchronous code</span></span>

<span data-ttu-id="ec1bb-310">Die asynchrone Programmierung ist der Standardmodus für ASP.NET Core und EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="ec1bb-311">Der Webserver verfügt nur über eine begrenzte Anzahl von Threads. Daher werden bei hoher Auslastung möglicherweise alle verfügbaren Threads gleichzeitig verwendet.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="ec1bb-312">Wenn dies der Fall ist, kann der Server keine neuen Anforderungen verarbeiten, bis die Threads wieder freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="ec1bb-313">Wenn synchroner Code verwendet wird, kann es sein, dass zwar viele Threads belegt sind, diese aber keine Vorgänge ausführen, da sie auf den Abschluss der E/A-Vorgänge warten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="ec1bb-314">Wenn asynchroner Code verwendet wird, werden Threads für den Server freigegeben, wenn diese nur auf den Abschluss der E/A-Vorgänge warten, damit andere Anforderungen verarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="ec1bb-315">Das bedeutet, dass es durch asynchronen Code ermöglicht wird, Serverressourcen effizienter zu nutzen, und der Server kann ohne Verzögerungen eine größere Menge von Datenverkehr verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="ec1bb-316">Zur Laufzeit bedeutet die Verwendung von asynchronem Code allerdings, dass ein wenig mehr Aufwand entsteht.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="ec1bb-317">Für Situationen mit wenig Datenverkehr haben diese Leistungseinbußen keine negativen Folgen. Wenn es jedoch eine große Menge an Datenverkehr gibt, ist eine potentielle Verbesserung der Leistung von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="ec1bb-318">Im folgenden Code haben das `async`-Schlüsselwort, der `Task<T>`-Rückgabewert, das `await`-Schlüsselwort und die `ToListAsync`-Methode zur Folge, dass der Code auf asynchrone Weise ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="ec1bb-319">Das `async`-Schlüsselwort gibt dem Compiler folgende Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="ec1bb-320">Er soll Rückrufe für Teile des Methodentexts generieren.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="ec1bb-321">Er soll automatisch das [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7)-Objekt erstellen, das zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="ec1bb-322">Weitere Informationen finden Sie unter [Aufgabenrückgabetyp](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="ec1bb-323">Der implizite Rückgabetyp `Task` steht für die derzeit ausgeführte Arbeit.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="ec1bb-324">Das `await`-Schlüsselwort hat zur Folge, dass der Compiler die Methode in zwei Teile unterteilt.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="ec1bb-325">Der erste Teil endet mit dem Vorgang, der auf asynchrone Weise gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-325">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="ec1bb-326">Der zweite Teil wird in eine Rückrufmethode übertragen, die aufgerufen wird, wenn der Vorgang abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-326">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="ec1bb-327">Bei `ToListAsync` handelt es sich um die asynchrone Version der `ToList`-Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="ec1bb-328">Behalten Sie Folgendes im Hinterkopf, wenn Sie asynchronen Code schreiben, der Entity Framework Core verwendet:</span><span class="sxs-lookup"><span data-stu-id="ec1bb-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="ec1bb-329">Es werden nur Anweisungen auf asynchrone Weise ausgeführt, die Abfragen oder Befehle auslösen, die an die Datenbank gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="ec1bb-330">Dazu gehören `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` und `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="ec1bb-331">Anweisungen wie `var students = context.Students.Where(s => s.LastName == "Davolio")`, die nur eine `IQueryable`-Instanz ändern, sind davon ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-331">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="ec1bb-332">Entity Framework Core-Kontexte sind nicht threadsicher. Versuchen Sie daher nicht, mehrere Vorgänge gleichzeitig auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-332">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="ec1bb-333">Wenn Sie von den Leistungsvorteilen durch asynchronen Code profitieren möchten, überprüfen Sie, ob Bibliothekspakete (z.B. zum Paging) asynchronen Code verwenden, wenn sie Entity Framework Core-Methoden aufrufen, die Abfragen an die Datenbank senden.</span><span class="sxs-lookup"><span data-stu-id="ec1bb-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="ec1bb-334">Weitere Informationen zur asynchronen Programmierung in .NET finden Sie unter [Async (Übersicht)](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="ec1bb-335">Im nächsten Tutorial erfahren Sie mehr über die CRUD-Vorgänge (Create, Read, Update, Delete = Erstellen, Lesen, Aktualisieren, Löschen).</span><span class="sxs-lookup"><span data-stu-id="ec1bb-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ec1bb-336">Nächste</span><span class="sxs-lookup"><span data-stu-id="ec1bb-336">Next</span></span>](xref:data/ef-rp/crud)
