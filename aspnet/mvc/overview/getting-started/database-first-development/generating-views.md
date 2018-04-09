---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Datenbank zuerst mit ASP.NET MVC: Generieren von Sichten | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="1a46a-104">EF Datenbank zuerst mit ASP.NET MVC: Generieren von Sichten</span><span class="sxs-lookup"><span data-stu-id="1a46a-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="1a46a-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1a46a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1a46a-106">MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="1a46a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="1a46a-107">Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1a46a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="1a46a-108">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="1a46a-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="1a46a-109">In diesem Teil der Reihe konzentriert sich auf die mithilfe von ASP.NET Gerüstbau der Controller und Ansichten generieren.</span><span class="sxs-lookup"><span data-stu-id="1a46a-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="1a46a-110">Gerüst hinzufügen</span><span class="sxs-lookup"><span data-stu-id="1a46a-110">Add scaffold</span></span>

<span data-ttu-id="1a46a-111">Sie sind bereit, um Code zu generieren, die Standarddaten Vorgänge für das Modellklassen bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="1a46a-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="1a46a-112">Sie fügen Code hinzu, indem Sie ein Gerüst-Element.</span><span class="sxs-lookup"><span data-stu-id="1a46a-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="1a46a-113">Es gibt viele Optionen für den Typ des Gerüstbau, die Sie hinzufügen können. In diesem Lernprogramm umfasst das Gerüst einen Controller und Ansichten, die den Modellen Student "und" Registrierung entsprechen, die Sie im vorherigen Abschnitt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="1a46a-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="1a46a-114">Um die Konsistenz in Ihrem Projekt zu wahren, werden Sie Hinzufügen eines neuen Controllers zur vorhandenen **Controller** Ordner.</span><span class="sxs-lookup"><span data-stu-id="1a46a-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="1a46a-115">Mit der rechten Maustaste die **Controller** Ordner, und wählen **hinzufügen** – **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="1a46a-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Gerüst hinzufügen](generating-views/_static/image1.png)

<span data-ttu-id="1a46a-117">Wählen Sie die **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** Option.</span><span class="sxs-lookup"><span data-stu-id="1a46a-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="1a46a-118">Diese Option wird der Controller und Ansichten zum Aktualisieren, löschen, erstellen und Anzeigen der Daten in Ihrem Modell generiert.</span><span class="sxs-lookup"><span data-stu-id="1a46a-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Hinzufügen von Mvc-controller](generating-views/_static/image2.png)

<span data-ttu-id="1a46a-120">Wählen Sie **Student** der Skriptobjektmodell-Klasse, und wählen die **ContosoUniversityEntities** für der Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="1a46a-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="1a46a-121">Behalten Sie den Namen des Controllers als **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="1a46a-121">Keep the controller name as **StudentsController**,</span></span>

![Geben Sie die controller](generating-views/_static/image3.png)

<span data-ttu-id="1a46a-123">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1a46a-123">Click **Add**.</span></span>

<span data-ttu-id="1a46a-124">Wenn Sie eine Fehlermeldung erhalten, möglicherweise darauf zurückzuführen, bis Sie nicht auf das Projekt im vorherigen Abschnitt erstellt.</span><span class="sxs-lookup"><span data-stu-id="1a46a-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="1a46a-125">Wenn dies der Fall ist, versuchen Sie es beim Erstellen des Projekts, und fügen Sie das Gerüst erneut hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a46a-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="1a46a-126">Nachdem der Code-Generierungsprozess abgeschlossen ist, werden Sie einen neuen Controller und Ansichten in Ihrem Projekt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1a46a-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Ansichten anzeigen](generating-views/_static/image4.png)

<span data-ttu-id="1a46a-128">Führen Sie dieselben Schritte erneut aus, aber fügen Sie ein Gerüst für die Registrierung Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a46a-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="1a46a-129">Wenn Sie fertig sind, müssen Sie eine **EnrollmentsController.cs** Datei sowie einen Ordner unter **Ansichten** mit dem Namen **Registrierung** mit der Create, Delete, Details, bearbeiten und Index Ansichten.</span><span class="sxs-lookup"><span data-stu-id="1a46a-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Ansichten anzeigen](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="1a46a-131">Hinzufügen von Links mit neuen Ansichten</span><span class="sxs-lookup"><span data-stu-id="1a46a-131">Add links to new views</span></span>

<span data-ttu-id="1a46a-132">Um Sie zu Ihrer neuen Ansichten navigieren erleichtern, können Sie eine Reihe von Hyperlinks, die den Index Ansichten für Studenten und Registrierung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1a46a-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="1a46a-133">Öffnen Sie die Datei am **Views/Home/Index.cshtml**, also auf der Startseite für die Website.</span><span class="sxs-lookup"><span data-stu-id="1a46a-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="1a46a-134">Fügen Sie den folgenden Code unter der Jumbotron hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a46a-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="1a46a-135">Für die Methode ActionLink ist der erste Parameter im Bereich anzuzeigende Text.</span><span class="sxs-lookup"><span data-stu-id="1a46a-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="1a46a-136">Der zweite Parameter ist die Aktion, und der dritte Parameter ist der Name des Controllers.</span><span class="sxs-lookup"><span data-stu-id="1a46a-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="1a46a-137">Zeigt beispielsweise der erste Link, um die Index-Aktion in StudentsController.</span><span class="sxs-lookup"><span data-stu-id="1a46a-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="1a46a-138">Die tatsächliche Hyperlink wird aus dieser Werte erstellt.</span><span class="sxs-lookup"><span data-stu-id="1a46a-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="1a46a-139">Der erste Link gelangen letztlich Benutzer auf die **Index.cshtml** innerhalb der **Ansichten/Studenten** Ordner.</span><span class="sxs-lookup"><span data-stu-id="1a46a-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="1a46a-140">Student-Ansichten angezeigt</span><span class="sxs-lookup"><span data-stu-id="1a46a-140">Display student views</span></span>

<span data-ttu-id="1a46a-141">Sie überprüft, ob der Code ordnungsgemäß zu Ihrem Projekt hinzugefügt wird eine Liste der Studenten angezeigt, und ermöglicht Benutzern das Bearbeiten, erstellen oder löschen Sie die Studentendatensätze in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1a46a-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="1a46a-142">Mit der rechten Maustaste die **Views/Home/Index.cshtml** Datei, und wählen Sie **in Browser anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="1a46a-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="1a46a-143">Klicken Sie auf dieser Seite auf den Link, um die Liste der Schüler.</span><span class="sxs-lookup"><span data-stu-id="1a46a-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="1a46a-144">Beachten Sie auf dieser Seite die Liste der Schüler und Links zum Ändern dieser Daten ein.</span><span class="sxs-lookup"><span data-stu-id="1a46a-144">On this page, notice the list of the students and links to modify this data.</span></span>

![Liste der Schüler](generating-views/_static/image7.png)

<span data-ttu-id="1a46a-146">Klicken Sie auf die **neu erstellen** verknüpfen, und geben Sie einige Werte für einen neuen Studenten.</span><span class="sxs-lookup"><span data-stu-id="1a46a-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Erstellen von neuen Studenten](generating-views/_static/image8.png)

<span data-ttu-id="1a46a-148">Klicken Sie auf **erstellen**, und beachten Sie die neue Studenten wird zur Liste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1a46a-148">Click **Create**, and notice the new student is added to your list.</span></span>

![Liste mit neuen Studenten](generating-views/_static/image9.png)

<span data-ttu-id="1a46a-150">Wählen Sie die **bearbeiten** verknüpfen, und einige der Werte für ein Student ändern.</span><span class="sxs-lookup"><span data-stu-id="1a46a-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Student bearbeiten](generating-views/_static/image10.png)

<span data-ttu-id="1a46a-152">Klicken Sie auf **speichern**, und beachten Sie die Student-Datensatz wurde geändert.</span><span class="sxs-lookup"><span data-stu-id="1a46a-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="1a46a-153">Wählen Sie abschließend die **löschen** verknüpfen, und vergewissern Sie sich, dass den Datensatz gelöscht werden sollen die **löschen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="1a46a-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Schüler löschen](generating-views/_static/image11.png)

<span data-ttu-id="1a46a-155">Ohne Code schreiben zu müssen, haben Sie Sichten hinzugefügt, die allgemeine Vorgänge auf die Daten in der Tabelle Student ausführen.</span><span class="sxs-lookup"><span data-stu-id="1a46a-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="1a46a-156">Ihnen möglicherweise aufgefallen, dass die Beschriftung für ein Feld auf die Datenbankeigenschaft basiert (z. B. **LastName**) die ist nicht notwendigerweise was auf der Webseite angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1a46a-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="1a46a-157">Z. B. möglicherweise bevorzugen Sie die Bezeichnung sein **Nachname**.</span><span class="sxs-lookup"><span data-stu-id="1a46a-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="1a46a-158">Dieses Problem korrigieren Sie später in diesem Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="1a46a-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="1a46a-159">Registrierung Ansichten angezeigt</span><span class="sxs-lookup"><span data-stu-id="1a46a-159">Display enrollment views</span></span>

<span data-ttu-id="1a46a-160">Ihre Datenbank enthält eine 1: n-Beziehung zwischen den Tabellen Student "und" Registrierung, und eine 1: n-Beziehung zwischen den Tabellen Kurs und Registrierung.</span><span class="sxs-lookup"><span data-stu-id="1a46a-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="1a46a-161">Die Ansichten für die Registrierung zu verarbeiten diese Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="1a46a-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="1a46a-162">Navigieren Sie zu der Homepage Ihrer Website, und wählen die **Liste der Bereitstellungen** Verknüpfung und dann die **neu erstellen** Link.</span><span class="sxs-lookup"><span data-stu-id="1a46a-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="1a46a-163">Die Ansicht zeigt ein Formular für das Erstellen eines neuen Datensatzes für die Registrierung.</span><span class="sxs-lookup"><span data-stu-id="1a46a-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="1a46a-164">Beachten Sie insbesondere, dass das Formular zwei Dropdownlisten enthält, die mit Werten aus den verknüpften Tabellen aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="1a46a-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Erstellen Sie die Registrierung](generating-views/_static/image12.png)

<span data-ttu-id="1a46a-166">Darüber hinaus wird die Überprüfung der bereitgestellten Werte automatisch basierend auf den Datentyp des Felds angewendet.</span><span class="sxs-lookup"><span data-stu-id="1a46a-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="1a46a-167">Grade fordert eine Nummer an, damit eine Fehlermeldung angezeigt wird, wenn Sie versuchen, einen inkompatiblen Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="1a46a-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![überprüfungsmeldung](generating-views/_static/image13.png)

<span data-ttu-id="1a46a-169">Sie haben sichergestellt, dass die automatisch generierten Sichten ermöglichen einen Benutzer mit den Daten in der Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="1a46a-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="1a46a-170">In der nächsten Lernprogramm dieser Reihe Sie die Datenbank aktualisieren, und nehmen Sie die entsprechenden Änderungen in der Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="1a46a-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1a46a-171">[Zurück](creating-the-web-application.md)
> [Weiter](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="1a46a-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
