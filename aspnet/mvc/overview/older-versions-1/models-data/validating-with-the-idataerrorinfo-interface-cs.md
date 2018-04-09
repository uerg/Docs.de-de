---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Überprüfen mit der IDataErrorInfo-Schnittstelle (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wird gezeigt, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="0a2d9-103">Überprüfen mit der IDataErrorInfo-Schnittstelle (c#)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-103">Validating with the IDataErrorInfo Interface (C#)</span></span>
====================
<span data-ttu-id="0a2d9-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="0a2d9-105">Stephen Walther wird gezeigt, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="0a2d9-106">Das Ziel dieses Lernprogramms wird ein Ansatz besteht darin zu erläutern, mit der Ausführung der Überprüfung in einer ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="0a2d9-107">Erfahren Sie, wie Sie verhindern, dass jemand ein HTML-Formular ohne Bereitstellung von Werten für die erforderlichen Felder des Formulars zu senden.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="0a2d9-108">In diesem Lernprogramm erfahren Sie, wie die Validierung mithilfe der IErrorDataInfo-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="0a2d9-109">Annahmen</span><span class="sxs-lookup"><span data-stu-id="0a2d9-109">Assumptions</span></span>

<span data-ttu-id="0a2d9-110">In diesem Lernprogramm wird die MoviesDB-Datenbank und der Datenbanktabelle Filme verwendet.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="0a2d9-111">Diese Tabelle weist die folgenden Spalten:</span><span class="sxs-lookup"><span data-stu-id="0a2d9-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| <span data-ttu-id="0a2d9-112">**Spaltenname**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-112">**Column Name**</span></span> | <span data-ttu-id="0a2d9-113">**Datentyp**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-113">**Data Type**</span></span> | <span data-ttu-id="0a2d9-114">**NULL-Werte zulassen**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0a2d9-115">Id</span><span class="sxs-lookup"><span data-stu-id="0a2d9-115">Id</span></span> | <span data-ttu-id="0a2d9-116">Int</span><span class="sxs-lookup"><span data-stu-id="0a2d9-116">Int</span></span> | <span data-ttu-id="0a2d9-117">False</span><span class="sxs-lookup"><span data-stu-id="0a2d9-117">False</span></span> |
| <span data-ttu-id="0a2d9-118">Titel</span><span class="sxs-lookup"><span data-stu-id="0a2d9-118">Title</span></span> | <span data-ttu-id="0a2d9-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-119">Nvarchar(100)</span></span> | <span data-ttu-id="0a2d9-120">False</span><span class="sxs-lookup"><span data-stu-id="0a2d9-120">False</span></span> |
| <span data-ttu-id="0a2d9-121">Director</span><span class="sxs-lookup"><span data-stu-id="0a2d9-121">Director</span></span> | <span data-ttu-id="0a2d9-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-122">Nvarchar(100)</span></span> | <span data-ttu-id="0a2d9-123">False</span><span class="sxs-lookup"><span data-stu-id="0a2d9-123">False</span></span> |
| <span data-ttu-id="0a2d9-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="0a2d9-124">DateReleased</span></span> | <span data-ttu-id="0a2d9-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="0a2d9-125">DateTime</span></span> | <span data-ttu-id="0a2d9-126">False</span><span class="sxs-lookup"><span data-stu-id="0a2d9-126">False</span></span> |


<span data-ttu-id="0a2d9-127">In diesem Lernprogramm mit der ich Microsoft Entity Framework Meine Datenbank Modellklassen generiert.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="0a2d9-128">In Abbildung 1 wird die Film-Klasse, die vom Entity Framework generierten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="0a2d9-129">[![Die Film-Entität](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="0a2d9-130">**Abbildung 01**: die Film-Entität ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0a2d9-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="0a2d9-131">Weitere Informationen zum Generieren von Ihren Modellklassen Datenbank mithilfe von Entity Framework finden Sie meine Lernprogramm Erstellen von Modellklassen mit dem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="0a2d9-132">Der Controller-Klasse</span><span class="sxs-lookup"><span data-stu-id="0a2d9-132">The Controller Class</span></span>

<span data-ttu-id="0a2d9-133">Wir verwenden den Home-Controller zu Filmen Liste, und erstellen neue Kinofilmen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="0a2d9-134">Der Code für diese Klasse ist im Codebeispiel 1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="0a2d9-135">**1 – Controllers\HomeController. cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="0a2d9-136">Die Home-Controller-Klasse in Codebeispiel 1 enthält zwei Create()-Aktionen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="0a2d9-137">Die erste Aktion wird das HTML-Formular für das Erstellen eines neuen Films angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="0a2d9-138">Die zweite Create()-Aktion führt das eigentliche Einfügen des neuen Films in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="0a2d9-139">Die zweite Create()-Aktion wird aufgerufen, wenn das Formular angezeigt, die von der ersten Create()--Aktion an den Server gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="0a2d9-140">Beachten Sie, dass die zweite Create()--Aktion, die folgenden Codezeilen enthält ein:</span><span class="sxs-lookup"><span data-stu-id="0a2d9-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="0a2d9-141">IsValid-Eigenschaft gibt false zurück, wenn ein Validierungsfehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="0a2d9-142">In diesem Fall wird die Ansicht für das Erstellen, das das HTML-Formular für das Erstellen eines Films enthält erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="0a2d9-143">Erstellen einer partiellen Klasse</span><span class="sxs-lookup"><span data-stu-id="0a2d9-143">Creating a Partial Class</span></span>

<span data-ttu-id="0a2d9-144">Die Film-Klasse wird vom Entity Framework generiert.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="0a2d9-145">Sie können den Code für die Klasse Film festzustellen, ob Sie die MoviesDBModel.edmx-Datei im Projektmappen-Explorer-Fenster erweitern, und öffnen Sie die Datei MoviesDBModel.Designer.cs im Code-Editor (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="0a2d9-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="0a2d9-146">[![Der Code für die Film-Entität](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="0a2d9-147">**Abbildung 02**: der Code für die Entität Film ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="0a2d9-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="0a2d9-148">Die Film-Klasse ist eine partielle Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-148">The Movie class is a partial class.</span></span> <span data-ttu-id="0a2d9-149">Das bedeutet, dass wir eine andere partielle Klasse mit dem gleichen Namen zum Erweitern der Funktionalität der Film-Klasse hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="0a2d9-150">Wir fügen die Überprüfungslogik der neuen partiellen Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="0a2d9-151">Fügen Sie die Klasse 2 Auflisten der Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="0a2d9-152">**Auflisten von 2 – Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="0a2d9-153">Beachten Sie, die die Klasse im Codebeispiel 2 enthält das *partielle* Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="0a2d9-154">Alle Methoden oder Eigenschaften, die diese Klasse hinzugefügt werden Teil der Film-Klasse, die vom Entity Framework generiert.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="0a2d9-155">Hinzufügen von OnChanging und OnChanged partielle Methoden</span><span class="sxs-lookup"><span data-stu-id="0a2d9-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="0a2d9-156">Wenn Entity Framework eine Entitätsklasse generiert, fügt das Entity Framework die partielle Methoden der Klasse automatisch aus.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="0a2d9-157">Das Entity Framework generiert OnChanging und OnChanged partielle Methoden, die jede Eigenschaft der Klasse entsprechen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="0a2d9-158">Im Fall der Film-Klasse erstellt das Entity Framework die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="0a2d9-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="0a2d9-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="0a2d9-159">OnIdChanging</span></span>
- <span data-ttu-id="0a2d9-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="0a2d9-160">OnIdChanged</span></span>
- <span data-ttu-id="0a2d9-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="0a2d9-161">OnTitleChanging</span></span>
- <span data-ttu-id="0a2d9-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="0a2d9-162">OnTitleChanged</span></span>
- <span data-ttu-id="0a2d9-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="0a2d9-163">OnDirectorChanging</span></span>
- <span data-ttu-id="0a2d9-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="0a2d9-164">OnDirectorChanged</span></span>
- <span data-ttu-id="0a2d9-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="0a2d9-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="0a2d9-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="0a2d9-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="0a2d9-167">Die OnChanging-Methode wird rechts aufgerufen, bevor die entsprechende Eigenschaft geändert wird.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="0a2d9-168">Die OnChanged-Methode wird rechts aufgerufen, nach dem Ändern der Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="0a2d9-169">Sie können diese partielle Methoden zum Hinzufügen von Validierungslogik auf die Film-Klasse nutzen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="0a2d9-170">Das Update Film-Klasse in 3 auflisten überprüft, dass der Titel und Director-Eigenschaften nicht leere Werte zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="0a2d9-171">Eine partielle Methode ist eine Methode definiert, die in einer Klasse, die nicht zum Implementieren erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="0a2d9-172">Wenn Sie eine partielle Methode implementieren keine entfernt der Compiler die Signatur der Methode, und alle Aufrufe der Methode bringt werden keine Kosten zur Laufzeit die partielle Methode.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="0a2d9-173">Im Code-Editor von Visual Studio können Sie eine partielle Methode hinzufügen, indem Sie das Schlüsselwort *partielle* gefolgt von einem Leerzeichen zum Anzeigen einer Liste von Replikatsgruppenelemente implementieren.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="0a2d9-174">**3 – Models\Movie.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="0a2d9-175">Z. B., wenn Sie versuchen, die Title-Eigenschaft eine leere Zeichenfolge zuweisen, klicken Sie dann folgende Fehlermeldung wird zugewiesen ein Wörterbuch mit dem Namen \_Fehler.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="0a2d9-176">An diesem Punkt nichts tatsächlich geschieht, wenn Sie die Title-Eigenschaft eine leere Zeichenfolge zuweisen und ein Fehler wird an die Private hinzugefügt \_Fehler Feld.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="0a2d9-177">Wir müssen implementieren die IDataErrorInfo-Schnittstelle, um diese Überprüfungsfehler auf, um das ASP.NET MVC-Framework verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="0a2d9-178">Implementieren der IDataErrorInfo-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="0a2d9-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="0a2d9-179">IDataErrorInfo-Schnittstelle ist Teil von .NET Framework seit der ersten Version.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="0a2d9-180">Diese Schnittstelle ist eine sehr einfache Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="0a2d9-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="0a2d9-181">Wenn eine Klasse die IDataErrorInfo-Schnittstelle implementiert, wird diese Schnittstelle in ASP.NET MVC-Framework verwenden, beim Erstellen einer Instanz der Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="0a2d9-182">Die Home-Controller Create()-Aktion akzeptiert z. B. eine Instanz der Klasse Film:</span><span class="sxs-lookup"><span data-stu-id="0a2d9-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="0a2d9-183">ASP.NET MVC-Framework erstellt die Instanz des Films an die Create()--Aktion übergeben wird, indem Sie einen Modellbinder (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="0a2d9-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="0a2d9-184">Der Modellbinder ist verantwortlich für das Erstellen einer Instanz des Objekts Film durch die HTML-Felder zu einer Instanz des Objekts Film binden.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="0a2d9-185">Die DefaultModelBinder erkennt, und zwar unabhängig davon, ob eine Klasse die IDataErrorInfo-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="0a2d9-186">Wenn eine Klasse diese Schnittstelle implementiert, ruft der Modellbinder IDataErrorInfo.this Indexer für jede Eigenschaft der Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="0a2d9-187">Der Indexer auf eine Fehlermeldung zurück fügt der Modellbinder diese Fehlermeldung Status automatisch zu modellieren.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="0a2d9-188">Die DefaultModelBinder überprüft auch die IDataErrorInfo.Error-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="0a2d9-189">Diese Eigenschaft dient zur Darstellung von bestimmten nicht-eigenschaftenvalidierung Fehler im Zusammenhang mit der Klasse.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="0a2d9-190">Beispielsweise empfiehlt es sich um eine Validierungsregel zu erzwingen, von die die Werte von mehreren Eigenschaften der Klasse Film abhängt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="0a2d9-191">In diesem Fall würden Sie einen Validierungsfehler aus Fehlereigenschaft zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="0a2d9-192">Die aktualisierte Film-Klasse in 4 auflisten implementiert die IDataErrorInfo-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="0a2d9-193">**Auflisten von 4 – Models\Movie.cs (IDataErrorInfo implementiert)**</span><span class="sxs-lookup"><span data-stu-id="0a2d9-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="0a2d9-194">Auflisten von 4 der Indexereigenschaft überprüft die \_Errors-Auflistung, um festzustellen, ob es sich um einen Schlüssel enthält, die den Namen der Eigenschaft entspricht dem Indexer übergeben.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="0a2d9-195">Wenn keine Validierungsfehler, die der Eigenschaft zugeordnet ist, wird eine leere Zeichenfolge zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="0a2d9-196">Sie müssen nicht den Home-Controller in keiner Weise verwenden Sie die geänderte Film-Klasse ändern.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="0a2d9-197">Die Seite wird angezeigt, die in Abbildung 3 wird veranschaulicht, was geschieht, wenn kein Wert für den Titel oder Director Formularfelder eingegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="0a2d9-198">[![Aktionsmethoden erstellen automatisch](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="0a2d9-199">**Abbildung 03**: ein Formular mit fehlenden Werten ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0a2d9-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="0a2d9-200">Beachten Sie, dass der Wert DateReleased automatisch überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="0a2d9-201">Da die DateReleased-Eigenschaft nicht NULL-Werte akzeptiert, generiert der DefaultModelBinder einen Validierungsfehler für diese Eigenschaft automatisch, wenn sie nicht über einen Wert verfügt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="0a2d9-202">Wenn Sie die Fehlermeldung für die Eigenschaft DateReleased ändern möchten, müssen Sie einen benutzerdefinierten Modellbinder zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="0a2d9-203">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="0a2d9-203">Summary</span></span>

<span data-ttu-id="0a2d9-204">In diesem Lernprogramm haben Sie gelernt, wie die IDataErrorInfo-Schnittstelle verwenden, um Überprüfungsfehlermeldungen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="0a2d9-205">Zunächst haben wir eine partielle Film-Klasse, die die Funktionalität der partiellen Film-Klasse, die vom Entity Framework generierten erweitert.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="0a2d9-206">Als Nächstes haben wir die Film Klasse OnTitleChanging() und OnDirectorChanging() partiellen Methoden Validierungslogik hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="0a2d9-207">Abschließend implementiert wir IDataErrorInfo-Schnittstelle, um diese validierungsmeldungen zu ASP.NET MVC-Framework verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="0a2d9-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a2d9-208">[Zurück](performing-simple-validation-cs.md)
> [Weiter](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0a2d9-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
