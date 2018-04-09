---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Erste Schritte mit Entity Framework 6 Database First mit MVC 5 | Microsoft Docs
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="e669a-104">Erste Schritte mit Entity Framework 6 Database First mit MVC 5</span><span class="sxs-lookup"><span data-stu-id="e669a-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="e669a-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e669a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e669a-106">MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="e669a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e669a-107">Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e669a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e669a-108">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="e669a-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="e669a-109">Im letzten Teil der Reihe stellen Sie den Standort- und Datenbankserver in Azure bereit.</span><span class="sxs-lookup"><span data-stu-id="e669a-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="e669a-110">In diesem Teil der Reihe konzentriert sich auf eine Datenbank erstellen und mit Daten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="e669a-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="e669a-111">Diese Reihe Schreibvorgangs mit Beiträgen aus Tom Dykstra und Rick Anderson.</span><span class="sxs-lookup"><span data-stu-id="e669a-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="e669a-112">Es wurde basierend auf Feedback von Benutzern im Abschnitt "Kommentare" verbessert.</span><span class="sxs-lookup"><span data-stu-id="e669a-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="e669a-113">Einführung</span><span class="sxs-lookup"><span data-stu-id="e669a-113">Introduction</span></span>

<span data-ttu-id="e669a-114">In diesem Thema wird gezeigt, wie start mit einer vorhandenen Datenbank und erstellen Sie schnell eine Webanwendung, die Benutzern ermöglicht, die mit den Daten interagieren.</span><span class="sxs-lookup"><span data-stu-id="e669a-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="e669a-115">Es verwendet die Entity Framework 6 und MVC 5, um die Webanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e669a-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="e669a-116">Die ASP.NET Gerüstbau-Funktion können Sie Code zum Anzeigen, aktualisieren, erstellen und Löschen von Daten automatisch zu generieren.</span><span class="sxs-lookup"><span data-stu-id="e669a-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="e669a-117">Verwenden die publishing Tools in Visual Studio, können Sie einfach die Standort- und Datenbankserver in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e669a-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="e669a-118">Dieses Thema behandelt die Situation, in dem Sie mit einer Datenbank und zum Generieren von Code für eine Webanwendung, die auf Basis der Felder in dieser Datenbank werden soll.</span><span class="sxs-lookup"><span data-stu-id="e669a-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="e669a-119">Dieser Ansatz wird Database First-Entwicklung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e669a-119">This approach is called Database First development.</span></span> <span data-ttu-id="e669a-120">Wenn Sie nicht bereits über eine vorhandene Datenbank verfügen, können Sie stattdessen eine Methode wird aufgerufen, was das Datenklassen zu definieren, und generieren die Datenbank in den Klasseneigenschaften umfasst Code First-Entwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="e669a-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="e669a-121">Einführende beispielsweise der Code First-Entwicklung finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="e669a-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="e669a-122">Ein komplexeres Beispiel finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="e669a-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="e669a-123">Anleitung zum Auswählen von Entity Framework Ansatz verwenden, finden Sie unter [Ansätze zur Entity Framework-Entwicklung](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="e669a-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e669a-124">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="e669a-124">Prerequisites</span></span>

<span data-ttu-id="e669a-125">Visual Studio 2013 oder Visual Studio Express 2013 für Web</span><span class="sxs-lookup"><span data-stu-id="e669a-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="e669a-126">Einrichten der Datenbank</span><span class="sxs-lookup"><span data-stu-id="e669a-126">Set up the database</span></span>

<span data-ttu-id="e669a-127">Zum Imitieren der Umgebung von einer vorhandenen Datenbank müssen Sie zuerst erstellen Sie eine Datenbank mit Daten vorab mit Daten aufgefüllt, und erstellen Sie Ihre Webanwendung, die mit der Datenbank verbunden.</span><span class="sxs-lookup"><span data-stu-id="e669a-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="e669a-128">Dieses Lernprogramm wurde mit LocalDB mit Visual Studio 2013 oder Visual Studio Express 2013 für Web entwickelt.</span><span class="sxs-lookup"><span data-stu-id="e669a-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="e669a-129">Sie können einen vorhandenen Datenbankserver anstelle von LocalDB verwenden, aber abhängig von Ihrer Version von Visual Studio und den Typ der Datenbank, aller Data Tools in Visual Studio möglicherweise nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e669a-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="e669a-130">Wenn die Tools nicht für die Datenbank verfügbar sind, müssen Sie möglicherweise einige der Schritte in der Management Suite datenbankspezifischen für Ihre Datenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="e669a-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="e669a-131">Wenn Sie ein Problem mit der Datenbanktools in Ihrer Version von Visual Studio verfügen, stellen Sie sicher, dass Sie die neueste Version der Datenbanktools installiert haben.</span><span class="sxs-lookup"><span data-stu-id="e669a-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="e669a-132">Informationen zum Aktualisieren oder installieren die Datenbanktools finden Sie unter [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="e669a-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="e669a-133">Starten Sie Visual Studio und erstellen Sie eine **SQL Server-Datenbankprojekt**.</span><span class="sxs-lookup"><span data-stu-id="e669a-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="e669a-134">Nennen Sie das Projekt **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="e669a-134">Name the project **ContosoUniversityData**.</span></span>

![Datenbankprojekt erstellen](setting-up-database/_static/image1.png)

<span data-ttu-id="e669a-136">Sie haben jetzt ein leeres Datenbankprojekt.</span><span class="sxs-lookup"><span data-stu-id="e669a-136">You now have an empty database project.</span></span> <span data-ttu-id="e669a-137">Sie werden diese Datenbank in Azure weiter unten in diesem Lernprogramm bereitstellen, daher Sie Azure SQL-Datenbank als Zielplattform für das Projekt festlegen müssen.</span><span class="sxs-lookup"><span data-stu-id="e669a-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="e669a-138">Festlegen der Zielplattform wird die Datenbank nicht tatsächlich bereitgestellt; Es bedeutet lediglich, dass das Datenbankprojekt prüfen, ob der Datenbankentwurf mit der Zielplattform kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="e669a-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="e669a-139">Um die Zielplattform festzulegen, öffnen Sie die **Eigenschaften** für das Projekt und wählen Sie **Microsoft Azure SQL-Datenbank** für die Zielplattform.</span><span class="sxs-lookup"><span data-stu-id="e669a-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![Set-Zielplattform](setting-up-database/_static/image2.png)

<span data-ttu-id="e669a-141">Sie können die Tabellen, die für dieses Lernprogramm erforderlich sind, durch Hinzufügen von SQL-Skripts, die definieren, die Tabellen erstellen.</span><span class="sxs-lookup"><span data-stu-id="e669a-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="e669a-142">Mit der rechten Maustaste des Projekts, und fügen Sie ein neues Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="e669a-142">Right-click your project and add a new item.</span></span>

![Neues Element hinzufügen](setting-up-database/_static/image3.png)

<span data-ttu-id="e669a-144">Fügen Sie eine neue Tabelle namens Student hinzu.</span><span class="sxs-lookup"><span data-stu-id="e669a-144">Add a new table named Student.</span></span>

![Studententabelle hinzufügen](setting-up-database/_static/image4.png)

<span data-ttu-id="e669a-146">Ersetzen Sie den T-SQL-Befehl in der Tabelle durch den folgenden Code zum Erstellen der Tabelle.</span><span class="sxs-lookup"><span data-stu-id="e669a-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="e669a-147">Beachten Sie, dass das Entwurfsfenster automatisch durch den Code synchronisiert wird.</span><span class="sxs-lookup"><span data-stu-id="e669a-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="e669a-148">Sie können mit dem Code oder -Designer arbeiten.</span><span class="sxs-lookup"><span data-stu-id="e669a-148">You can work with either the code or designer.</span></span>

![Anzeigen von Code und Entwurf](setting-up-database/_static/image5.png)

<span data-ttu-id="e669a-150">Fügen Sie einer anderen Tabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="e669a-150">Add another table.</span></span> <span data-ttu-id="e669a-151">Dieses Mal nennen Sie sie Kurs und verwenden Sie den folgenden T-SQL-Befehl.</span><span class="sxs-lookup"><span data-stu-id="e669a-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="e669a-152">Und wiederholen Sie noch einmal, eine Tabelle mit dem Namen Registrierung erstellen.</span><span class="sxs-lookup"><span data-stu-id="e669a-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="e669a-153">Sie können die Datenbank mit Daten über ein Skript auffüllen, die ausgeführt wird, nachdem die Datenbank bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e669a-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="e669a-154">Fügen Sie ein Skript nach der Bereitstellung zum Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="e669a-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="e669a-155">Sie können den Standardnamen verwenden.</span><span class="sxs-lookup"><span data-stu-id="e669a-155">You can use the default name.</span></span>

![Hinzufügen des Skripts nach der Bereitstellung](setting-up-database/_static/image6.png)

<span data-ttu-id="e669a-157">Fügen Sie den folgenden T-SQL-Code, um das Skript nach der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="e669a-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="e669a-158">Dieses Skript fügt Daten einfach mit der Datenbank, wenn keine übereinstimmenden Datensatz gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="e669a-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="e669a-159">Es werden keine überschreiben oder löschen alle Daten, die Sie in die Datenbank eingegeben haben.</span><span class="sxs-lookup"><span data-stu-id="e669a-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="e669a-160">Es ist wichtig zu beachten, dass das Skript nach der Bereitstellung ausgeführt wird, jedes Mal, wenn Sie das Datenbankprojekt bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e669a-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="e669a-161">Aus diesem Grund müssen Sie Ihren Anforderungen sorgfältig zu überlegen, wenn Sie dieses Skript schreiben.</span><span class="sxs-lookup"><span data-stu-id="e669a-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="e669a-162">In einigen Fällen möchten Sie möglicherweise über Starten von einem bekannten Satz von Daten jedes Mal, wenn das Projekt bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e669a-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="e669a-163">In anderen Fällen möchten Sie möglicherweise nicht die vorhandenen Daten in irgendeiner Weise ändern.</span><span class="sxs-lookup"><span data-stu-id="e669a-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="e669a-164">Je nach Ihren Anforderungen, können Sie entscheiden, ob Sie benötigen ein Skript nach der Bereitstellung oder in das Skript eingefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e669a-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="e669a-165">Weitere Informationen zum Auffüllen der Datenbank mit einem Skript nach der Bereitstellung finden Sie unter [einschließlich Daten in einem SQL Server-Datenbankprojekt](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="e669a-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="e669a-166">Sie verfügen jetzt über 4 SQL-Skriptdateien, aber keine tatsächlichen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="e669a-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="e669a-167">Sie sind bereit für die Bereitstellung des Datenbankprojekts auf "Localdb".</span><span class="sxs-lookup"><span data-stu-id="e669a-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="e669a-168">Klicken Sie in Visual Studio auf die Schaltfläche "Start" (oder F5) zum Erstellen und Bereitstellen des Datenbankprojekts.</span><span class="sxs-lookup"><span data-stu-id="e669a-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="e669a-169">Überprüfen Sie die Registerkarte "Ausgabe", um sicherzustellen, dass der Build- und Bereitstellungsprozess wurde erfolgreich abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e669a-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Ausgabe anzeigen](setting-up-database/_static/image7.png)

<span data-ttu-id="e669a-171">Um zu sehen, dass die neue Datenbank erstellt wurde, öffnen Sie **Objekt-Explorer von SQL Server** und suchen Sie nach den Namen des Projekts in der richtigen lokalen Datenbankserver (in diesem Fall **(Localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="e669a-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Anzeigen der neuen Datenbank](setting-up-database/_static/image8.png)

<span data-ttu-id="e669a-173">Um zu sehen, dass die Tabellen mit Daten aufgefüllt werden, mit der rechten Maustaste in einer Tabellenstatus, und wählen **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="e669a-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Anzeigen von Tabellendaten](setting-up-database/_static/image9.png)

<span data-ttu-id="e669a-175">Eine bearbeitbare Ansicht der Tabellendaten wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e669a-175">An editable view of the table data is displayed.</span></span>

![Anzeigen der Ergebnisse der Tabelle](setting-up-database/_static/image10.png)

<span data-ttu-id="e669a-177">Ihre Datenbank weist nun eingerichtet und mit Daten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="e669a-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="e669a-178">In den nächsten Lernprogrammen erstellen Sie eine Webanwendung für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e669a-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e669a-179">Nächste</span><span class="sxs-lookup"><span data-stu-id="e669a-179">Next</span></span>](creating-the-web-application.md)
