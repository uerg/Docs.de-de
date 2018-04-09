---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Datenbank zuerst mit ASP.NET MVC: Ändern der Datenbank | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="2d72f-104">EF Datenbank zuerst mit ASP.NET MVC: Ändern der Datenbank</span><span class="sxs-lookup"><span data-stu-id="2d72f-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="2d72f-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2d72f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2d72f-106">MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2d72f-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="2d72f-107">Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="2d72f-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="2d72f-108">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="2d72f-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="2d72f-109">In diesem Teil der Reihe konzentriert sich auf ein Update auf die Struktur der Datenbank herstellen und das Weitergeben von dieser Änderung in der gesamten Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2d72f-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="2d72f-110">Hinzufügen einer Spalte</span><span class="sxs-lookup"><span data-stu-id="2d72f-110">Add a column</span></span>

<span data-ttu-id="2d72f-111">Wenn Sie die Struktur einer Tabelle in der Datenbank aktualisieren, müssen Sie sicherstellen, dass die Änderung an das Datenmodell, Ansichten und Controller weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2d72f-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="2d72f-112">Für dieses Lernprogramm fügen Sie eine neue Spalte der Tabelle Student zum Aufzeichnen der zweite Vorname des Studenten hinzu.</span><span class="sxs-lookup"><span data-stu-id="2d72f-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="2d72f-113">Um diese Spalte hinzuzufügen, öffnen Sie das Datenbankprojekt, und öffnen Sie die Datei Student.sql.</span><span class="sxs-lookup"><span data-stu-id="2d72f-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="2d72f-114">Über den Designer oder den T-SQL-Code, fügen Sie eine Spalte mit dem Namen **MiddleName** , ist ein NVARCHAR(50) und NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="2d72f-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![Zweiter Vorname hinzufügen](changing-the-database/_static/image1.png)

<span data-ttu-id="2d72f-116">Bereitstellen Sie diese Änderung können Sie auf die lokale Datenbank, indem Sie Ihr Datenbankprojekt (oder F5) starten.</span><span class="sxs-lookup"><span data-stu-id="2d72f-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="2d72f-117">Die Tabelle wird das neue Feld hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="2d72f-117">The new field is added to the table.</span></span> <span data-ttu-id="2d72f-118">Wenn Sie sie im Objekt-Explorer von SQL Server nicht angezeigt werden, klicken Sie auf die Schaltfläche "Aktualisieren" im Bereich.</span><span class="sxs-lookup"><span data-stu-id="2d72f-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![neue Spalte anzeigen](changing-the-database/_static/image2.png)

<span data-ttu-id="2d72f-120">Die neue Spalte in der Datenbanktabelle vorhanden ist, aber derzeit nicht in der Datenmodellklasse vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2d72f-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="2d72f-121">Sie müssen das Modell, um die neue Spalte berücksichtigt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2d72f-121">You must update the model to include your new column.</span></span> <span data-ttu-id="2d72f-122">In der **Modelle** Ordner die **ContosoModel.edmx** Datei, um das Modelldiagramm anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2d72f-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="2d72f-123">Beachten Sie, dass das Modell Student nicht die MiddleName-Eigenschaft enthält.</span><span class="sxs-lookup"><span data-stu-id="2d72f-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="2d72f-124">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen Sie **Modell aus der Datenbank aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="2d72f-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![Modell aktualisieren](changing-the-database/_static/image3.png)

<span data-ttu-id="2d72f-126">Wählen Sie in der Update-Assistent die **aktualisieren** Registerkarte und der **Student** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="2d72f-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Assistent zum Aktualisieren](changing-the-database/_static/image4.png)

<span data-ttu-id="2d72f-128">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="2d72f-128">Click **Finish**.</span></span>

<span data-ttu-id="2d72f-129">Nach Abschluss des Updatevorgangs Datenbankdiagramm enthält das neue **MiddleName** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2d72f-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="2d72f-130">Speichern Sie die **ContosoModel.edmx** Datei.</span><span class="sxs-lookup"><span data-stu-id="2d72f-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="2d72f-131">Sie speichern müssen Sie diese Datei für die neue Eigenschaft an die **Student.cs** Klasse.</span><span class="sxs-lookup"><span data-stu-id="2d72f-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="2d72f-132">Sie haben jetzt die Datenbank und das Modell aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="2d72f-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="2d72f-133">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="2d72f-133">Build the solution.</span></span>

<span data-ttu-id="2d72f-134">Die Ansichten enthalten noch leider nicht die neue Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2d72f-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="2d72f-135">Aktualisiert die Ansichten stehen Ihnen zwei Optionen zur Verfügung: Sie können entweder erneut generieren die Ansichten des Gerüstbaus für die Klasse Student erneut hinzufügen, oder Sie können die neue Eigenschaft manuell hinzufügen, um Ihre vorhandenen Ansichten.</span><span class="sxs-lookup"><span data-stu-id="2d72f-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="2d72f-136">In diesem Lernprogramm fügen Sie das Gerüst erneut aus, da Sie keine benutzerdefinierten Änderungen an den automatisch generierten Sichten vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="2d72f-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="2d72f-137">Sie sollten erwägen, die Eigenschaft manuell hinzufügen, wenn Sie Änderungen, auf die Ansichten vorgenommen haben und nicht diese Änderungen verlieren möchten.</span><span class="sxs-lookup"><span data-stu-id="2d72f-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="2d72f-138">Um sicherzustellen, dass die Ansichten neu erstellt wurde, löschen Sie die **Studenten** Unterordner **Ansichten**, und löschen die **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="2d72f-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="2d72f-139">Klicken Sie dann mit der rechten Maustaste die **Controller** Ordner und Hinzufügen des Gerüstbaus für die **Student** Modell.</span><span class="sxs-lookup"><span data-stu-id="2d72f-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="2d72f-140">Nennen Sie den Controller wieder **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="2d72f-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="2d72f-141">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d72f-141">Select **OK**.</span></span>

<span data-ttu-id="2d72f-142">Die Ansichten enthalten jetzt die MiddleName-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2d72f-142">The views now contain the MiddleName property.</span></span>

![Zweiter Vorname anzeigen](changing-the-database/_static/image5.png)

<span data-ttu-id="2d72f-144">Im nächsten Abschnitt fügen Sie Code aus, um die Ansicht zum Anzeigen von Details zu einem Datensatz Student anzupassen.</span><span class="sxs-lookup"><span data-stu-id="2d72f-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d72f-145">[Zurück](generating-views.md)
> [Weiter](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="2d72f-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
