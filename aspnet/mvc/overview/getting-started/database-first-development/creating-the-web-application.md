---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First mit ASP.NET MVC: Erstellen der Webanwendung und Datenmodelle | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 343131d45ed0b2442f1b0b557c5b63f3877e5d0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823800"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="3e98c-104">EF Database First mit ASP.NET MVC: Erstellen der Webanwendung und Datenmodelle</span><span class="sxs-lookup"><span data-stu-id="3e98c-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="3e98c-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3e98c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3e98c-106">Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="3e98c-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="3e98c-107">Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3e98c-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="3e98c-108">Der generierte Code entspricht die Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="3e98c-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="3e98c-109">Dieser Teil der Serie konzentriert sich auf die Webanwendung erstellen und das Generieren von Datenmodelle auf Grundlage von Datenbanktabellen.</span><span class="sxs-lookup"><span data-stu-id="3e98c-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="3e98c-110">Erstellen Sie eine neue ASP.NET-Webanwendung</span><span class="sxs-lookup"><span data-stu-id="3e98c-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="3e98c-111">Erstellen Sie ein neues Projekt in Visual Studio in einer neuen Projektmappe oder der gleichen Projektmappe wie das Projekt, und wählen Sie die **ASP.NET-Webanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="3e98c-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="3e98c-112">Nennen Sie das Projekt **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-112">Name the project **ContosoSite**.</span></span>

![Projekt erstellen](creating-the-web-application/_static/image1.png)

<span data-ttu-id="3e98c-114">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-114">Click **OK**.</span></span>

<span data-ttu-id="3e98c-115">Wählen Sie im Fenster Neues ASP.NET-Projekt die **MVC** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="3e98c-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="3e98c-116">Sie können löschen, die **in der Cloud hosten** option jetzt, da Sie später die Anwendung in der Cloud bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="3e98c-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="3e98c-117">Klicken Sie auf **OK** zum Erstellen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3e98c-117">Click **OK** to create the application.</span></span>

![Mvc-Vorlage auswählen](creating-the-web-application/_static/image2.png)

<span data-ttu-id="3e98c-119">Das Projekt wird mit der Standarddateien und Ordner erstellt.</span><span class="sxs-lookup"><span data-stu-id="3e98c-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="3e98c-120">In diesem Tutorial verwenden Sie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="3e98c-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="3e98c-121">Sie können die Version von Entity Framework Vergewissern Sie sich in Ihrem Projekt über das Fenster "NuGet-Pakete verwalten".</span><span class="sxs-lookup"><span data-stu-id="3e98c-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="3e98c-122">Aktualisieren Sie Ihre Version von Entity Framework, bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="3e98c-122">If necessary, update your version of Entity Framework.</span></span>

![Version anzeigen](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="3e98c-124">Die Modelle generieren</span><span class="sxs-lookup"><span data-stu-id="3e98c-124">Generate the models</span></span>

<span data-ttu-id="3e98c-125">Erstellen Sie jetzt Entity Framework-Modellen aus den Datenbanktabellen.</span><span class="sxs-lookup"><span data-stu-id="3e98c-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="3e98c-126">Diese Modelle sind Klassen, die Sie verwenden werden, um mit den Daten arbeiten.</span><span class="sxs-lookup"><span data-stu-id="3e98c-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="3e98c-127">Jedes Modell entspricht eine Tabelle in der Datenbank und enthält Eigenschaften, die die Spalten in der Tabelle entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3e98c-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="3e98c-128">Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen** und **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Neues Element hinzufügen](creating-the-web-application/_static/image4.png)

<span data-ttu-id="3e98c-130">Wählen Sie im Fenster "Neues Element hinzufügen", **Daten** im linken Bereich und **ADO.NET Entity Data Model** aus den Optionen im mittleren Bereich.</span><span class="sxs-lookup"><span data-stu-id="3e98c-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="3e98c-131">Nennen Sie die neue Modelldatei **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-131">Name the new model file **ContosoModel**.</span></span>

![Modell erstellen](creating-the-web-application/_static/image5.png)

<span data-ttu-id="3e98c-133">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-133">Click **Add**.</span></span>

<span data-ttu-id="3e98c-134">Wählen Sie in der Assistent für Entity Data Model **EF Designer aus Datenbank**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![aus Datenbank generieren](creating-the-web-application/_static/image6.png)

<span data-ttu-id="3e98c-136">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-136">Click **Next**.</span></span>

<span data-ttu-id="3e98c-137">Wenn Sie Verbindungen mit der Datenbank in Ihrer Entwicklungsumgebung definiert haben, sehen Sie diese Verbindungen vorab ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="3e98c-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="3e98c-138">Sie möchten jedoch eine neue Verbindung mit der Datenbank zu erstellen, die Sie im ersten Teil dieses Tutorials erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="3e98c-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="3e98c-139">Klicken Sie auf die **neue Verbindung** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="3e98c-139">Click the **New Connection** button.</span></span>

![mit Datenbank verbinden](creating-the-web-application/_static/image7.png)

<span data-ttu-id="3e98c-141">Geben Sie im Fenster Eigenschaften der Verbindung den Namen des lokalen Servers, in die Datenbank erstellt wurde (in diesem Fall **(Localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="3e98c-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="3e98c-142">Wählen Sie nachdem Sie den Namen des Servers haben die ContosoUniversityData aus den verfügbaren Datenbanken aus.</span><span class="sxs-lookup"><span data-stu-id="3e98c-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Set-Verbindungseigenschaften](creating-the-web-application/_static/image8.png)

<span data-ttu-id="3e98c-144">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-144">Click **OK**.</span></span>

<span data-ttu-id="3e98c-145">Die richtige Verbindungseigenschaften werden nun angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3e98c-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="3e98c-146">Sie können den Standardnamen für die Verbindung in der Datei "Web.config" verwenden.</span><span class="sxs-lookup"><span data-stu-id="3e98c-146">You can use the default name for connection in the Web.Config file</span></span>

![Verbindungseinstellungen](creating-the-web-application/_static/image9.png)

<span data-ttu-id="3e98c-148">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-148">Click **Next**.</span></span>

<span data-ttu-id="3e98c-149">Wählen Sie **Tabellen** zum Generieren von Modellen für alle drei Tabellen.</span><span class="sxs-lookup"><span data-stu-id="3e98c-149">Select **Tables** to generate models for all three tables.</span></span>

![Tabellen auswählen](creating-the-web-application/_static/image10.png)

<span data-ttu-id="3e98c-151">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="3e98c-151">Click **Finish**.</span></span>

<span data-ttu-id="3e98c-152">Wenn eine sicherheitswarnung angezeigt wird, wählen Sie **OK** zum Fortsetzen der Ausführung der Vorlage.</span><span class="sxs-lookup"><span data-stu-id="3e98c-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="3e98c-153">Die Modelle aus Tabellen der Datenbank generiert wird, und ein Diagramm wird mit den Eigenschaften und Beziehungen zwischen den Tabellen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3e98c-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Diagramm des Modells](creating-the-web-application/_static/image11.png)

<span data-ttu-id="3e98c-155">Der Ordner "Models" enthält jetzt viele neue Dateien, die im Zusammenhang mit der die Modelle, die aus der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="3e98c-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Neues Modelldateien anzeigen](creating-the-web-application/_static/image12.png)

<span data-ttu-id="3e98c-157">Die **ContosoModel.Context.cs** -Datei enthält eine abgeleitete Klasse die **"DbContext"** Klasse, und stellt eine Eigenschaft für jede Modellklasse, die einer Datenbanktabelle entspricht.</span><span class="sxs-lookup"><span data-stu-id="3e98c-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="3e98c-158">Die **Course.cs**, **Enrollment.cs**, und **Student.cs** Dateien enthalten die Modellklassen, die in den Datenbanken Tabellen darstellen.</span><span class="sxs-lookup"><span data-stu-id="3e98c-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="3e98c-159">Sie werden sowohl der Context-Klasse und die Modellklassen verwenden, bei der Arbeit mit Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="3e98c-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="3e98c-160">Erstellen Sie bevor Sie mit diesem Tutorial Fortfahren das Projekt ein.</span><span class="sxs-lookup"><span data-stu-id="3e98c-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="3e98c-161">Im nächsten Abschnitt generieren Sie Code auf Grundlage der Datenmodelle, aber dieser Abschnitt funktioniert nicht, wenn das Projekt noch nicht erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="3e98c-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e98c-162">[Zurück](setting-up-database.md)
> [Weiter](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="3e98c-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
