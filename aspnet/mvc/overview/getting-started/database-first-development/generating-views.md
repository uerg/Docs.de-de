---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First mit ASP.NET MVC: Generieren von Sichten | Microsoft-Dokumentation'
author: Rick-Anderson
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7d925573dd4cdf5c1a36e51f312e18093bd35043
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021087"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="46a6a-104">EF Database First mit ASP.NET MVC: Generieren von Sichten</span><span class="sxs-lookup"><span data-stu-id="46a6a-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="46a6a-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="46a6a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="46a6a-106">Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="46a6a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="46a6a-107">Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="46a6a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="46a6a-108">Der generierte Code entspricht die Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="46a6a-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="46a6a-109">Dieser Teil der Serie konzentriert sich auf ASP.NET-Gerüstbau verwenden, um die Controller und Ansichten zu generieren.</span><span class="sxs-lookup"><span data-stu-id="46a6a-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="46a6a-110">Gerüst hinzufügen</span><span class="sxs-lookup"><span data-stu-id="46a6a-110">Add scaffold</span></span>

<span data-ttu-id="46a6a-111">Sie können zum Generieren von Code, die standard-Vorgänge für die Modellklassen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="46a6a-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="46a6a-112">Sie können den Code durch Hinzufügen eines Elements Gerüst hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="46a6a-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="46a6a-113">Es gibt viele Optionen für den Typ der Gerüstbau, die Sie hinzufügen können. In diesem Tutorial enthält das Gerüst, einen Controller und Ansichten, die die Modelle "Student" und der Registrierung entsprechen, die Sie im vorherigen Abschnitt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="46a6a-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="46a6a-114">Um Konsistenz in Ihrem Projekt zu gewährleisten, fügen Sie den neuen Controller hinzu, die vorhandene **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="46a6a-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="46a6a-115">Mit der rechten Maustaste die **Controller** Ordner, und wählen **hinzufügen** – **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="46a6a-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Gerüst hinzufügen](generating-views/_static/image1.png)

<span data-ttu-id="46a6a-117">Wählen Sie die **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** Option.</span><span class="sxs-lookup"><span data-stu-id="46a6a-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="46a6a-118">Diese Option wird die Controller und Ansichten zum Aktualisieren, löschen, erstellen und Anzeigen der Daten in Ihrem Modell generiert.</span><span class="sxs-lookup"><span data-stu-id="46a6a-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Hinzufügen der Mvc-controller](generating-views/_static/image2.png)

<span data-ttu-id="46a6a-120">Wählen Sie **für Schüler und Studenten** für die Model-Klasse, und wählen die **ContosoUniversityEntities** für der Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="46a6a-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="46a6a-121">Behalten Sie den Namen der Controller als **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="46a6a-121">Keep the controller name as **StudentsController**,</span></span>

![Geben Sie controller](generating-views/_static/image3.png)

<span data-ttu-id="46a6a-123">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="46a6a-123">Click **Add**.</span></span>

<span data-ttu-id="46a6a-124">Wenn Sie eine Fehlermeldung erhalten, kann es sein, da Sie nicht auf das Projekt im vorherigen Abschnitt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="46a6a-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="46a6a-125">Wenn dies der Fall ist, versuchen Sie es beim Erstellen des Projekts, und klicken Sie dann das erstellte Element erneut hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="46a6a-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="46a6a-126">Nachdem der Prozess der codegenerierung abgeschlossen ist, werden Sie einen neuen Controller und Ansichten in Ihrem Projekt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="46a6a-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Ansichten anzeigen](generating-views/_static/image4.png)

<span data-ttu-id="46a6a-128">Führen Sie die gleichen Schritte erneut aus, aber fügen Sie ein Gerüst für die Registrierung-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="46a6a-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="46a6a-129">Wenn Sie fertig sind, müssen Sie eine **EnrollmentsController.cs** Datei sowie einen Ordner unter **Ansichten** mit dem Namen **Registrierungen** mit der Create, Delete, Details, bearbeiten und Index Ansichten.</span><span class="sxs-lookup"><span data-stu-id="46a6a-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Ansichten anzeigen](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="46a6a-131">Hinzufügen von Links zu den neuen Ansichten</span><span class="sxs-lookup"><span data-stu-id="46a6a-131">Add links to new views</span></span>

<span data-ttu-id="46a6a-132">Um für die Navigation zu Ihrer neuen Ansichten vereinfachen, können Sie eine Reihe von Links für Schüler/Studenten und Registrierungen, die den Index Ansichten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="46a6a-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="46a6a-133">Öffnen Sie die Datei unter **Views/Home/Index.cshtml**, dies ist die Startseite für Ihre Website.</span><span class="sxs-lookup"><span data-stu-id="46a6a-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="46a6a-134">Fügen Sie den folgenden Code unter den Jumbotron hinzu.</span><span class="sxs-lookup"><span data-stu-id="46a6a-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="46a6a-135">Für die ActionLink-Methode ist der erste Parameter der im Link anzuzeigende Text.</span><span class="sxs-lookup"><span data-stu-id="46a6a-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="46a6a-136">Der zweite Parameter ist die Aktion aus, und der dritte Parameter ist der Name des Controllers.</span><span class="sxs-lookup"><span data-stu-id="46a6a-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="46a6a-137">Zeigt beispielsweise der erste Link zu der Aktion Index StudentsController an.</span><span class="sxs-lookup"><span data-stu-id="46a6a-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="46a6a-138">Der tatsächlichen Link wird unter folgenden Werten erstellt.</span><span class="sxs-lookup"><span data-stu-id="46a6a-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="46a6a-139">Die erste Verknüpfung letztendlich gelangen Benutzer zum die **"Index.cshtml"** Datei innerhalb der **Ansichten/Schüler/Studenten** Ordner.</span><span class="sxs-lookup"><span data-stu-id="46a6a-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="46a6a-140">Anzeigen von für Schüler und Studenten-Ansichten</span><span class="sxs-lookup"><span data-stu-id="46a6a-140">Display student views</span></span>

<span data-ttu-id="46a6a-141">Sie werden überprüfen, dass der Code ordnungsgemäß zu Ihrem Projekt hinzugefügt wird eine Liste der Studenten angezeigt, und Benutzern ermöglicht das Bearbeiten, erstellen oder löschen Sie die Studentendatensätze für Schüler und in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="46a6a-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="46a6a-142">Mit der rechten Maustaste die **Views/Home/Index.cshtml** , und wählen Sie **in Browser anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="46a6a-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="46a6a-143">Klicken Sie auf dieser Seite auf den Link, um die Liste der Schüler/Studenten.</span><span class="sxs-lookup"><span data-stu-id="46a6a-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="46a6a-144">Beachten Sie auf dieser Seite die Liste der Schüler/Studenten und Links zum Ändern dieser Daten ein.</span><span class="sxs-lookup"><span data-stu-id="46a6a-144">On this page, notice the list of the students and links to modify this data.</span></span>

![Liste der Schüler/Studenten](generating-views/_static/image7.png)

<span data-ttu-id="46a6a-146">Klicken Sie auf die **neu erstellen** verknüpfen, und geben Sie einige Werte für einen neuen Studenten.</span><span class="sxs-lookup"><span data-stu-id="46a6a-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Erstellen Sie neue für Schüler und Studenten](generating-views/_static/image8.png)

<span data-ttu-id="46a6a-148">Klicken Sie auf **erstellen**, und beachten Sie, dass die neue Studenten zur Liste hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="46a6a-148">Click **Create**, and notice the new student is added to your list.</span></span>

![Liste mit den neuen Studenten](generating-views/_static/image9.png)

<span data-ttu-id="46a6a-150">Wählen Sie die **bearbeiten** verknüpfen, und einige der Werte für Schüler/Student ändern.</span><span class="sxs-lookup"><span data-stu-id="46a6a-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Kursteilnehmer bearbeiten](generating-views/_static/image10.png)

<span data-ttu-id="46a6a-152">Klicken Sie auf **speichern**, und beachten Sie, dass der Student-Datensatz geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="46a6a-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="46a6a-153">Wählen Sie abschließend die **löschen** verknüpfen, und bestätigen Sie den Datensatz zu löschen, indem Sie auf die **löschen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="46a6a-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Löschen Sie "Student"](generating-views/_static/image11.png)

<span data-ttu-id="46a6a-155">Ohne Code schreiben zu müssen, haben Sie die Sichten hinzugefügt, die allgemeine Vorgänge auf die Daten in der Tabelle "Student" ausführen.</span><span class="sxs-lookup"><span data-stu-id="46a6a-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="46a6a-156">Ihnen möglicherweise aufgefallen, dass das die textbezeichnung für ein Feld auf die Datenbankeigenschaft (z. B. **"LastName"**) dem ist nicht unbedingt auf der Webseite anzeigen möchten.</span><span class="sxs-lookup"><span data-stu-id="46a6a-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="46a6a-157">Z. B. möglicherweise bevorzugen Sie die Bezeichnung sein **Nachname**.</span><span class="sxs-lookup"><span data-stu-id="46a6a-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="46a6a-158">Sie werden später im Tutorial dieses Anzeigeproblems behoben.</span><span class="sxs-lookup"><span data-stu-id="46a6a-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="46a6a-159">Anzeigen von Ansichten der Registrierung</span><span class="sxs-lookup"><span data-stu-id="46a6a-159">Display enrollment views</span></span>

<span data-ttu-id="46a6a-160">Ihre Datenbank enthält eine 1: n Beziehung zwischen den Tabellen "Student" und Registrierung, und eine 1: n Beziehung zwischen den Tabellen Kurs- und Registrierungsentitäten.</span><span class="sxs-lookup"><span data-stu-id="46a6a-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="46a6a-161">Die Ansichten für die Registrierung zu diese Beziehungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="46a6a-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="46a6a-162">Navigieren Sie zur Startseite für Ihre Website, und wählen die **Liste der Registrierungen** Link und klicken Sie dann die **neu erstellen** Link.</span><span class="sxs-lookup"><span data-stu-id="46a6a-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="46a6a-163">Die Ansicht zeigt ein Formular zum Erstellen eines neuen Eintrags für die Registrierung.</span><span class="sxs-lookup"><span data-stu-id="46a6a-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="46a6a-164">Beachten Sie insbesondere, dass das Formular mit zwei Dropdownlisten enthält, die mit Werten aus den verknüpften Tabellen aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="46a6a-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Erstellen Sie die Registrierung](generating-views/_static/image12.png)

<span data-ttu-id="46a6a-166">Darüber hinaus wird die Überprüfung der bereitgestellten Werte automatisch basierend auf dem Datentyp des Felds angewendet.</span><span class="sxs-lookup"><span data-stu-id="46a6a-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="46a6a-167">Professionelle erfordert eine Zahl ist, damit eine Fehlermeldung angezeigt wird, wenn Sie versuchen, einen nicht kompatiblen Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="46a6a-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![validierungsmeldung](generating-views/_static/image13.png)

<span data-ttu-id="46a6a-169">Sie haben sichergestellt, dass die automatisch generierte Ansichten für die Arbeit mit den Daten in der Datenbank ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="46a6a-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="46a6a-170">Im nächsten Tutorial dieser Reihe Sie die Datenbank aktualisieren, und nehmen die entsprechenden Änderungen in der Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="46a6a-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="46a6a-171">[Zurück](creating-the-web-application.md)
> [Weiter](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="46a6a-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
