---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Anzeigen einer Tabelle von Datenbankdaten (c#) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial zeige ich zwei Methoden zum Anzeigen eines Satzes von Datenbank-Datensätzen. Aufgezeigt werden zwei Methoden einen Satz von Datenbank-Datensätzen in einem HTML-ta formatieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 06dc5c9398adb45d5a5ff8f57ff42816c983ee04
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395979"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="5e3bf-104">Anzeigen einer Tabelle von Datenbankdaten (c#)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="5e3bf-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="5e3bf-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="5e3bf-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="5e3bf-107">In diesem Tutorial zeige ich zwei Methoden zum Anzeigen eines Satzes von Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="5e3bf-108">Ich zeigen, zwei Methoden einen Satz von Datenbank-Datensätzen in einer HTML-Tabelle zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="5e3bf-109">Zunächst zeige ich, wie Sie Datensätze aus der Datenbank direkt in einer Ansicht formatieren können.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="5e3bf-110">Als Nächstes zeige ich, wie Sie von Teilansichten profitieren bei der Formatierung von Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="5e3bf-111">Das Ziel in diesem Tutorial wird beschrieben, wie Sie eine HTML-Tabelle von Datenbankdaten in ASP.NET MVC-Anwendungen anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="5e3bf-112">Zunächst erfahren Sie, wie Sie mit, dass die Gerüstbau-Tools in Visual Studio enthalten um eine Sicht zu generieren, die eine Gruppe von Datensätzen automatisch anzeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="5e3bf-113">Als Nächstes erfahren Sie, wie Sie eine partielle als Vorlage verwenden, bei der Formatierung von Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="5e3bf-114">Erstellen Sie die Modell-Klasse</span><span class="sxs-lookup"><span data-stu-id="5e3bf-114">Create the Model Classes</span></span>

<span data-ttu-id="5e3bf-115">Wir werden eine Reihe von Datensätzen aus der Tabelle der Datenbank Filme angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="5e3bf-116">Die Filme-Datenbank-Tabelle enthält die folgenden Spalten:</span><span class="sxs-lookup"><span data-stu-id="5e3bf-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="5e3bf-117">**Name der Spalte**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-117">**Column Name**</span></span> | <span data-ttu-id="5e3bf-118">**Datentyp**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-118">**Data Type**</span></span> | <span data-ttu-id="5e3bf-119">**NULL-Werte zulassen**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e3bf-120">Id</span><span class="sxs-lookup"><span data-stu-id="5e3bf-120">Id</span></span> | <span data-ttu-id="5e3bf-121">Int</span><span class="sxs-lookup"><span data-stu-id="5e3bf-121">Int</span></span> | <span data-ttu-id="5e3bf-122">False</span><span class="sxs-lookup"><span data-stu-id="5e3bf-122">False</span></span> |
| <span data-ttu-id="5e3bf-123">Titel</span><span class="sxs-lookup"><span data-stu-id="5e3bf-123">Title</span></span> | <span data-ttu-id="5e3bf-124">Nvarchar(200)-Datentyp gepackt ist</span><span class="sxs-lookup"><span data-stu-id="5e3bf-124">Nvarchar(200)</span></span> | <span data-ttu-id="5e3bf-125">False</span><span class="sxs-lookup"><span data-stu-id="5e3bf-125">False</span></span> |
| <span data-ttu-id="5e3bf-126">Director</span><span class="sxs-lookup"><span data-stu-id="5e3bf-126">Director</span></span> | <span data-ttu-id="5e3bf-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-127">NVarchar(50)</span></span> | <span data-ttu-id="5e3bf-128">False</span><span class="sxs-lookup"><span data-stu-id="5e3bf-128">False</span></span> |
| <span data-ttu-id="5e3bf-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="5e3bf-129">DateReleased</span></span> | <span data-ttu-id="5e3bf-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="5e3bf-130">DateTime</span></span> | <span data-ttu-id="5e3bf-131">False</span><span class="sxs-lookup"><span data-stu-id="5e3bf-131">False</span></span> |


<span data-ttu-id="5e3bf-132">Um die Filme-Tabelle in der ASP.NET MVC-Anwendung darstellen zu können, müssen wir eine Model-Klasse zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="5e3bf-133">In diesem Tutorial verwenden wir das Microsoft Entity Framework zum Erstellen von unserem Modellklassen aus.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5e3bf-134">In diesem Tutorial verwenden wir das Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="5e3bf-135">Allerdings ist es wichtig, zu verstehen, dass Sie eine Vielzahl verschiedener Technologien für die Interaktion mit einer Datenbank aus einer ASP.NET MVC-Anwendung, einschließlich LINQ to SQL, NHibernate oder ADO.NET verwenden können.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="5e3bf-136">Um den Entity Data Model-Assistenten zu starten, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="5e3bf-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="5e3bf-137">Mit der rechten Maustaste in den Ordner "Models" im Projektmappen-Explorer und Auswählen der Menüoption **hinzufügen, neue Element**.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="5e3bf-138">Wählen Sie die **Daten** Kategorie, und wählen die **ADO.NET Entity Data Model** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="5e3bf-139">Geben Sie den Namen Ihres Datenmodells *MoviesDBModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="5e3bf-140">Nachdem Sie die Schaltfläche "hinzufügen", klicken Sie auf Assistent für Entity Data Model wird angezeigt (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="5e3bf-141">Um den Assistenten abgeschlossen haben, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="5e3bf-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="5e3bf-142">In der **auswählen des Modellinhalts** Schritt wählen die **aus Datenbank generieren** Option.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="5e3bf-143">In der **wählen Sie Ihre Datenverbindung** Schritt, verwenden Sie die *MoviesDB.mdf* Datenverbindung und dem Namen *MoviesDBEntities* für die Verbindungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="5e3bf-144">Klicken Sie auf die **Weiter** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="5e3bf-145">In der **Datenbankobjekte auswählen** Schritt, erweitern Sie den Knoten "Tabellen", wählen Sie die Tabelle für Filme.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="5e3bf-146">Geben Sie den Namespace *Modelle* , und klicken Sie auf die **Fertig stellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="5e3bf-147">[![Erstellen von LINQ to SQL-Klassen](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="5e3bf-148">**Abbildung 01**: Erstellen von LINQ to SQL-Klassen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="5e3bf-149">Nach Abschluss des Assistenten für Entity Data Model wird dem Entity Data Model-Designer geöffnet.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="5e3bf-150">Der Designer sollte die Filme-Entität angezeigt (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="5e3bf-151">[![Der Entity Data Model-Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="5e3bf-152">**Abbildung 02**: das Entity Data Model Designer ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="5e3bf-153">Wir müssen eine Änderung vornehmen, bevor der Vorgang fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-153">We need to make one change before we continue.</span></span> <span data-ttu-id="5e3bf-154">Assistent für Entity Data generiert eine Modellklasse namens *Filme* , die die Tabelle der Datenbank Filme darstellt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="5e3bf-155">Da wir die Filme-Klasse verwenden müssen, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse ändern *Film* anstelle von *Filme* (im singular statt im plural).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="5e3bf-156">Doppelklicken Sie auf den Namen der Klasse auf der Designeroberfläche, und ändern Sie den Namen der Klasse von Filmen, Film.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="5e3bf-157">Nachdem Sie diese Änderung vornehmen, klicken Sie auf die **speichern** (das Symbol der Diskette) Schaltfläche, um die Movie-Klasse zu generieren.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="5e3bf-158">Erstellen des "Movies"-Controllers</span><span class="sxs-lookup"><span data-stu-id="5e3bf-158">Create the Movies Controller</span></span>

<span data-ttu-id="5e3bf-159">Nun, da wir eine Methode zur Darstellung von unseren Unterlagen Datenbank haben, können wir einen Controller erstellen, der die Auflistung von Filmen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="5e3bf-160">Klicken Sie im Fenster Projektmappen-Explorer von Visual Studio mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie die Menüoption **hinzufügen, Controller** (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="5e3bf-161">[![Die Controller-Menü "hinzufügen"](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="5e3bf-162">**Abbildung 03**: die Controller-Menü hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="5e3bf-163">Wenn die **Controller hinzufügen** Dialogfeld angezeigt wird, geben Sie den Namen des Controllers MovieController (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="5e3bf-164">Klicken Sie auf die **hinzufügen** , um den neuen Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="5e3bf-165">[![Das Dialogfeld "Controller hinzufügen"](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="5e3bf-166">**Abbildung 04**: die Controller der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="5e3bf-167">Wir müssen so ändern Sie die Index()-Aktion, die von der Movie-Controller verfügbar gemacht werden, sodass sie den Satz von Datenbank-Datensätzen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="5e3bf-168">Ändern Sie den Controller, sodass es den Controller in der Liste 1 aussieht.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="5e3bf-169">**Codebeispiel 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="5e3bf-170">In Codebeispiel 1 ist die MoviesDBEntities-Klasse verwendet, um die Datenbank MoviesDB darzustellen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="5e3bf-171">Um diese Klasse verwenden zu können, müssen Sie wie folgt den MvcApplication1.Models-Namespace zu importieren:</span><span class="sxs-lookup"><span data-stu-id="5e3bf-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="5e3bf-172">Verwenden von MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="5e3bf-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="5e3bf-173">Der Ausdruck *Entitäten. MovieSet.ToList()* gibt den Satz aller Filme aus der Tabelle der Datenbank Filme.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="5e3bf-174">Erstellen Sie die Sicht</span><span class="sxs-lookup"><span data-stu-id="5e3bf-174">Create the View</span></span>

<span data-ttu-id="5e3bf-175">Die einfachste Möglichkeit zum Anzeigen eines Satzes von Datenbank-Datensätzen in einer HTML-Tabelle ist das Gerüst, das von Visual Studio bereitgestellten nutzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="5e3bf-176">Erstellen Sie Ihre Anwendung durch Auswählen der Menüoption **erstellen "," Projektmappe erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="5e3bf-177">Bauen Sie Ihre Anwendung vor dem Öffnen der **Ansicht hinzufügen** Dialogfeld oder die Datenklassen nicht in das Dialogfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="5e3bf-178">Mit der rechten Maustaste der Index()-Aktion, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 5).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="5e3bf-179">[![Hinzufügen einer Ansicht](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="5e3bf-180">**Abbildung 05**: Hinzufügen einer Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="5e3bf-181">In der **Ansicht hinzufügen** Dialogfeld aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="5e3bf-182">Wählen Sie die Movie-Klasse als die **Datenklasse anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="5e3bf-183">Wählen Sie *Liste* als die **Inhalt anzeigen** (siehe Abbildung 6).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="5e3bf-184">Wählen diese Optionen generiert eine stark typisierte Ansicht, in dem eine Liste von Filmen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="5e3bf-185">[![Das Dialogfeld "Ansicht hinzufügen"](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="5e3bf-186">**Abbildung 06**: die Ansicht der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="5e3bf-187">Nachdem Sie auf die **hinzufügen** Schaltfläche, die Ansicht im Codebeispiel 2 wird automatisch generiert.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="5e3bf-188">Diese Ansicht enthält den Code, der zum Durchlaufen der Auflistung von Filmen und Anzeigen aller Eigenschaften eines Films erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="5e3bf-189">**Codebeispiel 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="5e3bf-190">Sie können die Anwendung ausführen, indem Sie durch Auswählen der Menüoption **Debuggen, Debugging starten** (oder F5 drücken).</span><span class="sxs-lookup"><span data-stu-id="5e3bf-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="5e3bf-191">Internet Explorer wird gestartet, wenn Sie die Anwendung ausführen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="5e3bf-192">Wenn Sie an die /Movie URL navigieren, und klicken Sie dann die Seite in Abbildung 7 angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="5e3bf-193">[![Eine Tabelle mit den Filmen](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="5e3bf-194">**Abbildung 07**: eine Tabelle mit den Filmen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="5e3bf-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="5e3bf-195">Wenn Sie etwas über das Erscheinungsbild des Rasters der Datenbank-Datensätzen in Abbildung 7 nicht zufrieden sind können Sie einfach die Ansicht "Index" ändern.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="5e3bf-196">Sie können z. B. Ändern der *DateReleased* Header *Veröffentlichungsdatum* durch Ändern der Ansicht "Index".</span><span class="sxs-lookup"><span data-stu-id="5e3bf-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="5e3bf-197">Erstellen Sie eine Vorlage mit einer partiellen</span><span class="sxs-lookup"><span data-stu-id="5e3bf-197">Create a Template with a Partial</span></span>

<span data-ttu-id="5e3bf-198">Wenn eine Ansicht zu kompliziert wird, ist es eine gute Idee, starten Sie die Ansicht in Teilansichten aufteilen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="5e3bf-199">Verwenden von Teilansichten erleichtert Ihre Ansichten verstehen und zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="5e3bf-200">Wir erstellen eine partielle, wir können jedes von der Movie-Datenbank-Datensätzen formatiert als Vorlage verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="5e3bf-201">Um die partielle erstellen, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="5e3bf-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="5e3bf-202">Mit der rechten Maustaste in des Ordners Views\Movie, und wählen Sie die Menüoption **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="5e3bf-203">Aktivieren Sie das Kontrollkästchen mit der Bezeichnung *erstellen Sie eine Teilansicht (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="5e3bf-204">Benennen Sie die partielle *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="5e3bf-205">Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="5e3bf-206">Wählen Sie Film als die *Datenklasse anzeigen*.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="5e3bf-207">Wählen Sie die leere als die *Inhalt anzeigen*.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="5e3bf-208">Klicken Sie auf die **hinzufügen** , um die partielle zu Ihrem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="5e3bf-209">Nachdem Sie diese Schritte abgeschlossen haben, ändern Sie die partielle MovieTemplate Codebeispiel 3 aussehen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="5e3bf-210">**Codebeispiel 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="5e3bf-211">Die partielle in Programmausdruck 3 enthält eine Vorlage für eine einzelne Zeile von Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="5e3bf-212">Geänderte Ansicht "Index" in Listing 4 wird der partiellen MovieTemplate verwendet.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="5e3bf-213">**Programmausdruck 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="5e3bf-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="5e3bf-214">Die Ansicht in Listing 4 enthält eine Foreach-Schleife, die alle Filme durchläuft.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="5e3bf-215">Für jeden Film wird der partiellen MovieTemplate verwendet, um den Film zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="5e3bf-216">Die MovieTemplate wird durch Aufrufen der Hilfsmethode RenderPartial() gerendert.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="5e3bf-217">Geänderte Ansicht "Index" rendert, die dieselbe HTML-Tabelle der Datenbank-Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="5e3bf-218">Allerdings wurde die Ansicht stark vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="5e3bf-219">Die RenderPartial()-Methode ist anders als die meisten anderen Hilfsmethoden, da sie keine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="5e3bf-220">Aus diesem Grund müssen Sie die Methode mit RenderPartial() aufrufen &lt;% Html.RenderPartial(); %&gt; anstelle von &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="5e3bf-221">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="5e3bf-221">Summary</span></span>

<span data-ttu-id="5e3bf-222">Das Ziel in diesem Tutorial wurde erläutert, wie Sie einen Satz von Datenbank-Datensätzen in einer HTML-Tabelle anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="5e3bf-223">Zuerst, wie Sie einen Satz von Datenbank-Datensätzen in eine Controlleraktion zurückgegeben wird, durch die Nutzung von Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="5e3bf-224">Als Nächstes, wie Sie Visual Studio-Gerüst zu verwenden, um eine Sicht zu generieren, die eine Auflistung von Elementen automatisch anzeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="5e3bf-225">Abschließend wie Sie die Ansicht zu vereinfachen, indem Sie eine partielle zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="5e3bf-226">Sie haben gelernt, wie eine partielle als Vorlage zu verwenden, damit Sie jeden Datensatz der Datenbank formatieren können.</span><span class="sxs-lookup"><span data-stu-id="5e3bf-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5e3bf-227">[Zurück](creating-model-classes-with-linq-to-sql-cs.md)
> [Weiter](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5e3bf-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
