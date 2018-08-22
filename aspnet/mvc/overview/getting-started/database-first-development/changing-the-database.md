---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First mit ASP.NET MVC: Ändern der Datenbank | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 4ee73dc066a56944dd2e5600460628656ae87e37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826742"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="534b1-104">EF Database First mit ASP.NET MVC: Ändern der Datenbank</span><span class="sxs-lookup"><span data-stu-id="534b1-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="534b1-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="534b1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="534b1-106">Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="534b1-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="534b1-107">Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="534b1-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="534b1-108">Der generierte Code entspricht die Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="534b1-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="534b1-109">Dieser Teil der Serie konzentriert sich auf eine Aktualisierung vornehmen, um die Struktur der Datenbank und das Weitergeben von dieser Änderung in der Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="534b1-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="534b1-110">Hinzufügen einer Spalte</span><span class="sxs-lookup"><span data-stu-id="534b1-110">Add a column</span></span>

<span data-ttu-id="534b1-111">Wenn Sie die Struktur einer Tabelle in der Datenbank aktualisieren, müssen Sie sicherstellen, dass die Änderung an das Datenmodell, Ansichten und Controller weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="534b1-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="534b1-112">In diesem Tutorial werden Sie eine neue Spalte der Tabelle "Student", um den zweiten Vornamen der Student aufzuzeichnen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="534b1-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="534b1-113">Um diese Spalte hinzuzufügen, öffnen Sie das Datenbankprojekt, und öffnen Sie die Datei Student.sql.</span><span class="sxs-lookup"><span data-stu-id="534b1-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="534b1-114">Über Designer "oder" T-SQL-Code, fügen Sie eine Spalte, die mit dem Namen **MiddleName** , ein NVARCHAR(50) und NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="534b1-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![Zweiter Vorname hinzufügen](changing-the-database/_static/image1.png)

<span data-ttu-id="534b1-116">Ihr Datenbankprojekt (oder F5) starten, um bereitstellen Sie diese Änderung in Ihre lokale Datenbank.</span><span class="sxs-lookup"><span data-stu-id="534b1-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="534b1-117">Das neue Feld wird in der Tabelle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="534b1-117">The new field is added to the table.</span></span> <span data-ttu-id="534b1-118">Wenn Sie es in der Objekt-Explorer von SQL Server nicht angezeigt werden, klicken Sie auf die Schaltfläche "Aktualisieren", klicken Sie im Bereich.</span><span class="sxs-lookup"><span data-stu-id="534b1-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![neue Spalte anzeigen](changing-the-database/_static/image2.png)

<span data-ttu-id="534b1-120">Die neue Spalte, die in der Tabelle der Datenbank vorhanden ist, aber derzeit nicht in der Modellklasse für Daten vorhanden.</span><span class="sxs-lookup"><span data-stu-id="534b1-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="534b1-121">Sie müssen das Modell, um Ihre neue Spalte enthalten aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="534b1-121">You must update the model to include your new column.</span></span> <span data-ttu-id="534b1-122">In der **Modelle** Ordner die **ContosoModel.edmx** Datei in das Modelldiagramm anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="534b1-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="534b1-123">Beachten Sie, dass das Modell "Student" nicht die MiddleName-Eigenschaft enthält.</span><span class="sxs-lookup"><span data-stu-id="534b1-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="534b1-124">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="534b1-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![Aktualisierungsmodell](changing-the-database/_static/image3.png)

<span data-ttu-id="534b1-126">Wählen Sie in der Update-Assistenten die **aktualisieren** Registerkarte und der **für Schüler und Studenten** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="534b1-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Update-Assistenten](changing-the-database/_static/image4.png)

<span data-ttu-id="534b1-128">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="534b1-128">Click **Finish**.</span></span>

<span data-ttu-id="534b1-129">Nachdem der Aktualisierungsvorgang abgeschlossen ist, enthält das Datenbankdiagramm neuen **MiddleName** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="534b1-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="534b1-130">Speichern Sie die **ContosoModel.edmx** Datei.</span><span class="sxs-lookup"><span data-stu-id="534b1-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="534b1-131">Sie müssen diese Datei für die neue Eigenschaft an weitergegeben speichern die **Student.cs** Klasse.</span><span class="sxs-lookup"><span data-stu-id="534b1-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="534b1-132">Sie haben jetzt die Datenbank und das Modell aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="534b1-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="534b1-133">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="534b1-133">Build the solution.</span></span>

<span data-ttu-id="534b1-134">Die Ansichten enthalten noch leider nicht die neue Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="534b1-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="534b1-135">Aktualisieren Sie die Ansichten haben Sie zwei Optionen: können Sie entweder erneut generieren die Ansichten von wieder hinzufügen Gerüst für die Klasse "Student", oder Sie können die neue Eigenschaft manuell hinzufügen, um Ihre vorhandenen Ansichten.</span><span class="sxs-lookup"><span data-stu-id="534b1-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="534b1-136">In diesem Tutorial fügen Sie das Gerüst erneut aus, da Sie nicht benutzerdefinierten Änderungen an den automatisch generierten Ansichten vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="534b1-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="534b1-137">Sie sollten erwägen, die Eigenschaft manuell hinzufügen, wenn Sie Änderungen, in den Ansichten vorgenommen haben und nicht, diese Änderungen verloren möchten.</span><span class="sxs-lookup"><span data-stu-id="534b1-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="534b1-138">Um sicherzustellen, die Ansichten werden neu erstellt, löschen Sie die **Schüler/Studenten** Ordner unter **Ansichten**, und Löschen der **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="534b1-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="534b1-139">Klicken Sie dann mit der rechten Maustaste die **Controller** Ordner und Hinzufügen der Gerüstbau für die **für Schüler und Studenten** Modell.</span><span class="sxs-lookup"><span data-stu-id="534b1-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="534b1-140">Nennen Sie den Controller wieder **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="534b1-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="534b1-141">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="534b1-141">Select **OK**.</span></span>

<span data-ttu-id="534b1-142">Die Ansichten enthalten jetzt die MiddleName-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="534b1-142">The views now contain the MiddleName property.</span></span>

![Zweiter Vorname anzeigen](changing-the-database/_static/image5.png)

<span data-ttu-id="534b1-144">Im nächsten Abschnitt fügen Sie Code zum Anpassen der Ansicht zum Anzeigen von Details zu einem Datensatz für Schüler und Studenten.</span><span class="sxs-lookup"><span data-stu-id="534b1-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="534b1-145">[Zurück](generating-views.md)
> [Weiter](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="534b1-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
