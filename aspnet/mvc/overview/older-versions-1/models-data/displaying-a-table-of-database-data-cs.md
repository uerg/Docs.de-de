---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Anzeigen einer Tabelle von Datenbankdaten (c#) | Microsoft Docs
author: microsoft
description: In diesem Lernprogramm werden ich zwei Methoden zum Anzeigen von einer Gruppe von Datenbankdatensätzen gezeigt. Ich anzeigen zwei Methoden für einen Satz von Datenbank-Datensätze in einem HTML-ta Formatierung
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d5dc9dd4a82e4577c6c1a3b124d45fef0b0f67c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872883"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="aa180-104">Anzeigen einer Tabelle von Datenbankdaten (c#)</span><span class="sxs-lookup"><span data-stu-id="aa180-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="aa180-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="aa180-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="aa180-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="aa180-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="aa180-107">In diesem Lernprogramm werden ich zwei Methoden zum Anzeigen von einer Gruppe von Datenbankdatensätzen gezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa180-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="aa180-108">Ich zeigen zwei Methoden zur Formatierung der eines Satz von Datenbankdatensätze in einer HTML-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="aa180-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="aa180-109">Zunächst wird aufgezeigt, wie Sie Datensätze aus der Datenbank direkt in einer Ansicht formatieren können.</span><span class="sxs-lookup"><span data-stu-id="aa180-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="aa180-110">Als Nächstes veranschaulichen ich, wie Sie Replikatsgruppenelemente nutzen können beim Formatieren von Datenbank-Datensätze.</span><span class="sxs-lookup"><span data-stu-id="aa180-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="aa180-111">Ziel dieses Lernprogramms wird erläutert, wie Sie eine HTML-Tabelle der Datenbankdaten in einer ASP.NET MVC-Anwendung anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="aa180-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="aa180-112">Zunächst erfahren Sie, wie mit der Gerüstbau-Tools in Visual Studio enthalten um eine Ansicht zu generieren, in dem eine Gruppe von Datensätzen automatisch angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa180-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="aa180-113">Als Nächstes erfahren Sie, wie eine partielle als Vorlage verwenden, bei der Formatierung von Datenbank-Datensätze.</span><span class="sxs-lookup"><span data-stu-id="aa180-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="aa180-114">Erstellen Sie die Modellklassen</span><span class="sxs-lookup"><span data-stu-id="aa180-114">Create the Model Classes</span></span>

<span data-ttu-id="aa180-115">Wir sind den Satz von Datensätzen aus der Datenbanktabelle Filme anzeigen möchten.</span><span class="sxs-lookup"><span data-stu-id="aa180-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="aa180-116">Die Datenbanktabelle Filme enthält die folgenden Spalten:</span><span class="sxs-lookup"><span data-stu-id="aa180-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="aa180-117">**Spaltenname**</span><span class="sxs-lookup"><span data-stu-id="aa180-117">**Column Name**</span></span> | <span data-ttu-id="aa180-118">**Datentyp**</span><span class="sxs-lookup"><span data-stu-id="aa180-118">**Data Type**</span></span> | <span data-ttu-id="aa180-119">**NULL-Werte zulassen**</span><span class="sxs-lookup"><span data-stu-id="aa180-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="aa180-120">Id</span><span class="sxs-lookup"><span data-stu-id="aa180-120">Id</span></span> | <span data-ttu-id="aa180-121">Int</span><span class="sxs-lookup"><span data-stu-id="aa180-121">Int</span></span> | <span data-ttu-id="aa180-122">False</span><span class="sxs-lookup"><span data-stu-id="aa180-122">False</span></span> |
| <span data-ttu-id="aa180-123">Titel</span><span class="sxs-lookup"><span data-stu-id="aa180-123">Title</span></span> | <span data-ttu-id="aa180-124">Nvarchar(200)-Datentyp gepackt ist</span><span class="sxs-lookup"><span data-stu-id="aa180-124">Nvarchar(200)</span></span> | <span data-ttu-id="aa180-125">False</span><span class="sxs-lookup"><span data-stu-id="aa180-125">False</span></span> |
| <span data-ttu-id="aa180-126">Director</span><span class="sxs-lookup"><span data-stu-id="aa180-126">Director</span></span> | <span data-ttu-id="aa180-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="aa180-127">NVarchar(50)</span></span> | <span data-ttu-id="aa180-128">False</span><span class="sxs-lookup"><span data-stu-id="aa180-128">False</span></span> |
| <span data-ttu-id="aa180-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="aa180-129">DateReleased</span></span> | <span data-ttu-id="aa180-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="aa180-130">DateTime</span></span> | <span data-ttu-id="aa180-131">False</span><span class="sxs-lookup"><span data-stu-id="aa180-131">False</span></span> |


<span data-ttu-id="aa180-132">Um die Filme-Tabelle in unseren ASP.NET MVC-Anwendung darstellen zu können, muss eine Modellklasse erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="aa180-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="aa180-133">In diesem Lernprogramm verwenden wir das Entity Framework von Microsoft, um unsere Modellklassen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="aa180-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="aa180-134">In diesem Lernprogramm verwenden wir die Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa180-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="aa180-135">Allerdings ist es wichtig zu verstehen, dass Sie eine Vielzahl von verschiedenen Technologien für die Interaktion mit einer Datenbank von einer ASP.NET MVC-Anwendung, einschließlich LINQ to SQL, NHibernate oder ADO.NET verwenden können.</span><span class="sxs-lookup"><span data-stu-id="aa180-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="aa180-136">Führen Sie folgende Schritte des Assistenten für Entity Data Model zu starten:</span><span class="sxs-lookup"><span data-stu-id="aa180-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="aa180-137">Mit der rechten Maustaste im Projektmappen-Explorer, und wählen Sie die Menüoption Ordner Models **hinzufügen, neue Element**.</span><span class="sxs-lookup"><span data-stu-id="aa180-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="aa180-138">Wählen Sie die **Daten** Kategorie, und wählen die **ADO.NET Entity Data Model** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="aa180-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="aa180-139">Geben Sie den Namen Ihres Datenmodells *MoviesDBModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="aa180-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="aa180-140">Nachdem Sie die Schaltfläche "hinzufügen" klicken, wird der Assistent für Entity Data Model (siehe Abbildung 1) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa180-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="aa180-141">Gehen Sie zum Abschließen des Assistenten wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="aa180-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="aa180-142">In der **Modellinhalte** Schritt wählen Sie die **aus Datenbank generieren** Option.</span><span class="sxs-lookup"><span data-stu-id="aa180-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="aa180-143">In der **wählen Sie Ihre Datenverbindung** Schritt: Verwenden der *MoviesDB.mdf* Datenverbindung und den Namen *MoviesDBEntities* für die Verbindungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="aa180-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="aa180-144">Klicken Sie auf die **Weiter** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="aa180-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="aa180-145">In der **Datenbankobjekte auswählen** Schritt, erweitern Sie den Knoten "Tabellen", wählen Sie die Tabelle für Filme.</span><span class="sxs-lookup"><span data-stu-id="aa180-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="aa180-146">Geben Sie den Namespace *Modelle* , und klicken Sie auf die **Fertig stellen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="aa180-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="aa180-147">[![Erstellen von LINQ to SQL-Klassen](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="aa180-148">**Abbildung 01**: Erstellen von LINQ to SQL-Klassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="aa180-149">Nach Abschluss des Assistenten für Entity Data Model wird im Entity Data Model-Designer geöffnet.</span><span class="sxs-lookup"><span data-stu-id="aa180-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="aa180-150">Anzeigen des Designers die Filme Entität (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="aa180-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="aa180-151">[![Im Entity Data Model-Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="aa180-152">**Abbildung 02**: The Entity Data Model-Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="aa180-153">Wir müssen eine Änderung vornehmen, bevor der Vorgang fortgesetzt werden kann.</span><span class="sxs-lookup"><span data-stu-id="aa180-153">We need to make one change before we continue.</span></span> <span data-ttu-id="aa180-154">Der Assistent für Entity Data generiert eine Modellklasse namens *Filme* , die die Datenbanktabelle Filme darstellt.</span><span class="sxs-lookup"><span data-stu-id="aa180-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="aa180-155">Da wir die Filme-Klasse verwenden werden, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse zu ändern *Film* anstelle von *Filme* (singular statt im plural).</span><span class="sxs-lookup"><span data-stu-id="aa180-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="aa180-156">Doppelklicken Sie auf den Namen der Klasse auf der Designeroberfläche aus, und ändern Sie den Namen der Klasse von Filmen Film.</span><span class="sxs-lookup"><span data-stu-id="aa180-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="aa180-157">Nachdem Sie diese Änderung vornehmen, klicken Sie auf die **speichern** (das Symbol für die Diskette)-Schaltfläche, um die Film-Klasse zu generieren.</span><span class="sxs-lookup"><span data-stu-id="aa180-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="aa180-158">Erstellen Sie den Controller Filme</span><span class="sxs-lookup"><span data-stu-id="aa180-158">Create the Movies Controller</span></span>

<span data-ttu-id="aa180-159">Nun mit dem eine Möglichkeit, die registrierten Datenbank Daten darstellen, können wir einen Controller erstellen, der die Auflistung von Filmen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="aa180-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="aa180-160">Klicken Sie im Projektmappen-Explorer von Visual Studio-Fenster mit der rechten Maustaste in des Ordners Controller, und wählen Sie die Menüoption **hinzufügen, Controller** (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="aa180-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="aa180-161">[![Die Controller-Menü "hinzufügen"](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="aa180-162">**Abbildung 03**: die Controller-Menü hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="aa180-163">Wenn die **Controller hinzufügen** Dialogfeld angezeigt wird, geben Sie den Namen des Controllers MovieController (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="aa180-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="aa180-164">Klicken Sie auf die **hinzufügen** Schaltfläche, um den neuen Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="aa180-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="aa180-165">[![Das Dialogfeld "Controller hinzufügen"](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="aa180-166">**Abbildung 04**: der Controller der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="aa180-167">Wir müssen so ändern Sie die Index()-Aktion, die vom Controller Film verfügbar gemacht, sodass er den Satz von Datenbankdatensätzen zurück.</span><span class="sxs-lookup"><span data-stu-id="aa180-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="aa180-168">Ändern Sie den Controller, sodass es den Controller im Codebeispiel 1 aussieht.</span><span class="sxs-lookup"><span data-stu-id="aa180-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="aa180-169">**1 – Controllers\MovieController.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="aa180-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="aa180-170">Im Codebeispiel 1 dient die Klasse MoviesDBEntities MoviesDB darstellen.</span><span class="sxs-lookup"><span data-stu-id="aa180-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="aa180-171">Um diese Klasse verwenden zu können, müssen Sie wie folgt den MvcApplication1.Models-Namespace zu importieren:</span><span class="sxs-lookup"><span data-stu-id="aa180-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="aa180-172">Verwenden MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="aa180-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="aa180-173">Der Ausdruck *Entitäten. MovieSet.ToList()* gibt den Satz aller Filme aus der Datenbanktabelle Filme.</span><span class="sxs-lookup"><span data-stu-id="aa180-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="aa180-174">Erstellen Sie die Sicht</span><span class="sxs-lookup"><span data-stu-id="aa180-174">Create the View</span></span>

<span data-ttu-id="aa180-175">Die einfachste Möglichkeit zum Anzeigen eines Satzes von Datenbankdatensätzen in einer HTML-Tabelle ist das Gerüst von Visual Studio bereitgestellten nutzen.</span><span class="sxs-lookup"><span data-stu-id="aa180-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="aa180-176">Die Anwendung erstellen, indem Sie im Menü die Option **erstellen, Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="aa180-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="aa180-177">Bauen Sie Ihre Anwendung vor dem Öffnen der **Ansicht hinzufügen** Dialog- oder Ihre Datenklassen wird nicht im Dialogfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa180-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="aa180-178">Mit der rechten Maustaste der Index()-Aktion, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 5).</span><span class="sxs-lookup"><span data-stu-id="aa180-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="aa180-179">[![Hinzufügen einer Ansicht](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="aa180-180">**Abbildung 05**: Hinzufügen einer Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="aa180-181">In der **Ansicht hinzufügen** Dialogfeld, aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.</span><span class="sxs-lookup"><span data-stu-id="aa180-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="aa180-182">Wählen Sie die Film-Klasse als die **Datenklasse anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="aa180-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="aa180-183">Wählen Sie *Liste* als die **Inhalt anzeigen** (siehe Abbildung 6).</span><span class="sxs-lookup"><span data-stu-id="aa180-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="aa180-184">Bei Auswahl dieser Optionen wird eine stark typisierte Ansicht generiert, in dem eine Liste von Filmen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa180-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="aa180-185">[![Das Dialogfeld "Ansicht hinzufügen"](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="aa180-186">**Abbildung 06**: die Ansicht der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="aa180-187">Nachdem Sie auf die **hinzufügen** Schaltfläche, die Ansicht im Codebeispiel 2 wird automatisch generiert.</span><span class="sxs-lookup"><span data-stu-id="aa180-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="aa180-188">Diese Ansicht enthält den Code, der zum Durchlaufen der Auflistung von Filmen und Anzeigen der Eigenschaften eines Films erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="aa180-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="aa180-189">**Auflisten von 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="aa180-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="aa180-190">Sie können die Anwendung ausführen, indem Sie im Menü die Option **Debuggen, Debugging starten** (oder drücken die F5-Taste).</span><span class="sxs-lookup"><span data-stu-id="aa180-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="aa180-191">Ausführen der Anwendung startet Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="aa180-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="aa180-192">Wenn Sie /Movie URL navigieren, klicken Sie dann sehen die Seite in Abbildung 7 Sie.</span><span class="sxs-lookup"><span data-stu-id="aa180-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="aa180-193">[![Eine Tabelle von Filmen](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="aa180-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="aa180-194">**Abbildung 07**: eine Tabelle von Filmen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="aa180-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="aa180-195">Wenn Sie etwas über die Darstellung des Rasters der Datenbankdatensätze in Abbildung 7 nicht gefallen können Sie einfach die Indexansicht ändern.</span><span class="sxs-lookup"><span data-stu-id="aa180-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="aa180-196">Sie können z. B. Ändern der *DateReleased* Header *Veröffentlichungsdatum* durch die Indexansicht ändern.</span><span class="sxs-lookup"><span data-stu-id="aa180-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="aa180-197">Erstellen Sie eine Vorlage mit einer partiellen</span><span class="sxs-lookup"><span data-stu-id="aa180-197">Create a Template with a Partial</span></span>

<span data-ttu-id="aa180-198">Wenn eine Sicht zu kompliziert wird, ist es eine gute Idee, starten Sie die Ansicht in Replikatsgruppenelemente unterteilt.</span><span class="sxs-lookup"><span data-stu-id="aa180-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="aa180-199">Replikatsgruppenelemente können Ihre Ansichten einfacher verstehen und zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="aa180-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="aa180-200">Wir erstellen eine partielle, die wir können als Vorlage verwenden, um die Datenbankdatensätze Film zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="aa180-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="aa180-201">Führen Sie diese Schritte aus, um die partielle erstellen:</span><span class="sxs-lookup"><span data-stu-id="aa180-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="aa180-202">Mit der rechten Maustaste in des Ordners Views\Movie, und wählen Sie die Menüoption **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="aa180-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="aa180-203">Aktivieren Sie das Kontrollkästchen mit der Bezeichnung *erstellen Sie eine Teilansicht (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="aa180-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="aa180-204">Name der partiellen *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="aa180-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="aa180-205">Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.</span><span class="sxs-lookup"><span data-stu-id="aa180-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="aa180-206">Wählen Sie Film als die *Datenklasse anzeigen*.</span><span class="sxs-lookup"><span data-stu-id="aa180-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="aa180-207">Wählen Sie als leere der *Inhalt anzeigen*.</span><span class="sxs-lookup"><span data-stu-id="aa180-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="aa180-208">Klicken Sie auf die **hinzufügen** Schaltfläche, um die partielle zu Ihrem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="aa180-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="aa180-209">Nachdem Sie diese Schritte abgeschlossen haben, ändern Sie die teilweise MovieTemplate auflisten 3 aussehen.</span><span class="sxs-lookup"><span data-stu-id="aa180-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="aa180-210">**3 – Views\Movie\MovieTemplate.ascx auflisten**</span><span class="sxs-lookup"><span data-stu-id="aa180-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="aa180-211">Der partiellen auflisten 3 enthält eine Vorlage für eine einzelne Zeile von Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="aa180-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="aa180-212">Die geänderte Indexansicht auflisten 4 verwendet die partielle MovieTemplate.</span><span class="sxs-lookup"><span data-stu-id="aa180-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="aa180-213">**4 – Views\Movie\Index.aspx auflisten**</span><span class="sxs-lookup"><span data-stu-id="aa180-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="aa180-214">Die Ansicht im Codebeispiel 4 enthält eine foreach-Schleife, die alle der Filme durchläuft.</span><span class="sxs-lookup"><span data-stu-id="aa180-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="aa180-215">Für jeden Film wird die partielle MovieTemplate verwendet, um den Film formatieren.</span><span class="sxs-lookup"><span data-stu-id="aa180-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="aa180-216">Die MovieTemplate wird durch Aufrufen der Hilfsmethode RenderPartial() gerendert.</span><span class="sxs-lookup"><span data-stu-id="aa180-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="aa180-217">Die geänderte Indexansicht rendert die selbe HTML-Tabelle der Datenbankdatensätze.</span><span class="sxs-lookup"><span data-stu-id="aa180-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="aa180-218">Allerdings wurde die Sicht erheblich vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="aa180-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="aa180-219">Die RenderPartial()-Methode ist anders als die meisten anderen Hilfsmethoden, da dies eine Zeichenfolge zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="aa180-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="aa180-220">Aus diesem Grund müssen Sie die Methode mithilfe RenderPartial() aufrufen &lt;"" Html.RenderPartial();&gt; anstelle von &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="aa180-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="aa180-221">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="aa180-221">Summary</span></span>

<span data-ttu-id="aa180-222">Das Ziel dieses Lernprogramms wurde zur Erläuterung, wie Sie einen Satz von Datenbank-Datensätze in einer HTML-Tabelle anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="aa180-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="aa180-223">Zunächst haben Sie gelernt, wie einen Satz von Datenbank-Datensätze in eine Controlleraktion zurückgegeben, durch die Nutzung von Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa180-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="aa180-224">Als Nächstes haben Sie gelernt, wie Visual Studio Gerüstbau zu verwenden, um eine Ansicht zu generieren, die eine Auflistung von Elementen werden automatisch angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa180-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="aa180-225">Schließlich haben Sie gelernt, wie Sie die Sicht zu vereinfachen, indem Sie eine partielle nutzen.</span><span class="sxs-lookup"><span data-stu-id="aa180-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="aa180-226">Sie haben gelernt, wie eine partielle als Vorlage verwenden, damit Sie jede Datenbankeintrag formatieren können.</span><span class="sxs-lookup"><span data-stu-id="aa180-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aa180-227">[Zurück](creating-model-classes-with-linq-to-sql-cs.md)
> [Weiter](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="aa180-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
