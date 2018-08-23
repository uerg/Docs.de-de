---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Überprüfen mit der IDataErrorInfo-Schnittstelle (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther erfahren Sie, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b80535db32c4567135407aeb99967bb40c279ddb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835321"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="3b640-103">Überprüfen mit der IDataErrorInfo-Schnittstelle (c#)</span><span class="sxs-lookup"><span data-stu-id="3b640-103">Validating with the IDataErrorInfo Interface (C#)</span></span>
====================
<span data-ttu-id="3b640-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="3b640-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="3b640-105">Stephen Walther erfahren Sie, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3b640-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="3b640-106">Das Ziel in diesem Tutorial wird ein Ansatz zum Ausführen der Validierung in ASP.NET MVC-Anwendungen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="3b640-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="3b640-107">Erfahren Sie, wie Sie verhindern, dass eine Person ein HTML-Formular ohne Angabe von Werten für die erforderlichen Felder des Formulars zu senden.</span><span class="sxs-lookup"><span data-stu-id="3b640-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="3b640-108">In diesem Tutorial erfahren Sie, wie die Validierung mithilfe der IErrorDataInfo-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="3b640-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="3b640-109">Annahmen</span><span class="sxs-lookup"><span data-stu-id="3b640-109">Assumptions</span></span>

<span data-ttu-id="3b640-110">In diesem Tutorial verwende ich die MoviesDB-Datenbank und der Tabelle der Datenbank Filme.</span><span class="sxs-lookup"><span data-stu-id="3b640-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="3b640-111">Diese Tabelle weist die folgenden Spalten:</span><span class="sxs-lookup"><span data-stu-id="3b640-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| <span data-ttu-id="3b640-112">**Name der Spalte**</span><span class="sxs-lookup"><span data-stu-id="3b640-112">**Column Name**</span></span> | <span data-ttu-id="3b640-113">**Datentyp**</span><span class="sxs-lookup"><span data-stu-id="3b640-113">**Data Type**</span></span> | <span data-ttu-id="3b640-114">**NULL-Werte zulassen**</span><span class="sxs-lookup"><span data-stu-id="3b640-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b640-115">Id</span><span class="sxs-lookup"><span data-stu-id="3b640-115">Id</span></span> | <span data-ttu-id="3b640-116">Int</span><span class="sxs-lookup"><span data-stu-id="3b640-116">Int</span></span> | <span data-ttu-id="3b640-117">False</span><span class="sxs-lookup"><span data-stu-id="3b640-117">False</span></span> |
| <span data-ttu-id="3b640-118">Titel</span><span class="sxs-lookup"><span data-stu-id="3b640-118">Title</span></span> | <span data-ttu-id="3b640-119">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="3b640-119">Nvarchar(100)</span></span> | <span data-ttu-id="3b640-120">False</span><span class="sxs-lookup"><span data-stu-id="3b640-120">False</span></span> |
| <span data-ttu-id="3b640-121">Director</span><span class="sxs-lookup"><span data-stu-id="3b640-121">Director</span></span> | <span data-ttu-id="3b640-122">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="3b640-122">Nvarchar(100)</span></span> | <span data-ttu-id="3b640-123">False</span><span class="sxs-lookup"><span data-stu-id="3b640-123">False</span></span> |
| <span data-ttu-id="3b640-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="3b640-124">DateReleased</span></span> | <span data-ttu-id="3b640-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="3b640-125">DateTime</span></span> | <span data-ttu-id="3b640-126">False</span><span class="sxs-lookup"><span data-stu-id="3b640-126">False</span></span> |


<span data-ttu-id="3b640-127">In diesem Tutorial verwende ich das Microsoft Entity Framework, meine Datenbank Modellklassen generiert werden.</span><span class="sxs-lookup"><span data-stu-id="3b640-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="3b640-128">In Abbildung 1 ist die Movie-Klasse, die vom Entity Framework generiert wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="3b640-129">[![Die Movie-Entität](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3b640-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="3b640-130">**Abbildung 01**: der Movie-Entität ([klicken Sie, um das Bild in voller Größe anzeigen](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="3b640-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="3b640-131">Weitere Informationen zum mithilfe von Entity Framework auf um Ihren Modellklassen für die Datenbank zu generieren, finden Sie meinen Tutorial Erstellen von Modellklassen mit dem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3b640-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="3b640-132">Der Controller-Klasse</span><span class="sxs-lookup"><span data-stu-id="3b640-132">The Controller Class</span></span>

<span data-ttu-id="3b640-133">Wir verwenden den Home-Controller auf Liste Filme und Erstellen neuer Filme.</span><span class="sxs-lookup"><span data-stu-id="3b640-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="3b640-134">Der Code für diese Klasse ist in Codebeispiel 1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="3b640-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="3b640-135">**Codebeispiel 1 – Controllers\HomeController. cs**</span><span class="sxs-lookup"><span data-stu-id="3b640-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="3b640-136">Die Home-Controller-Klasse in Codebeispiel 1 enthält zwei Create() Aktionen.</span><span class="sxs-lookup"><span data-stu-id="3b640-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="3b640-137">Die erste Aktion zeigt das HTML-Formular zum Erstellen eines neuen Films.</span><span class="sxs-lookup"><span data-stu-id="3b640-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="3b640-138">Die zweite Create()-Aktion führt das eigentliche Einfügen des neuen Film in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3b640-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="3b640-139">Die zweite Create()-Aktion wird aufgerufen, wenn das Formular die erste Create()-Aktion an den Server gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="3b640-140">Beachten Sie, dass die zweite Create()-Aktion die folgenden Codezeilen enthält:</span><span class="sxs-lookup"><span data-stu-id="3b640-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="3b640-141">Die Eigenschaft "IsValid" gibt false zurück, wenn ein Überprüfungsfehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="3b640-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="3b640-142">In diesem Fall wird die Ansicht erstellen, die das HTML-Formular zum Erstellen eines Films enthält erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3b640-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="3b640-143">Erstellen einer partiellen Klasse</span><span class="sxs-lookup"><span data-stu-id="3b640-143">Creating a Partial Class</span></span>

<span data-ttu-id="3b640-144">Die Movie-Klasse wird vom Entity Framework generiert.</span><span class="sxs-lookup"><span data-stu-id="3b640-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="3b640-145">Sie können den Code für die Movie-Klasse sehen, wenn Sie die MoviesDBModel.edmx-Datei im Projektmappen-Explorer-Fenster zu erweitern, und öffnen Sie die MoviesDBModel.Designer.cs-Datei im Code-Editor (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="3b640-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="3b640-146">[![Der Code für die Movie-Entität](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3b640-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="3b640-147">**Abbildung 02**: der Code für die Movie-Entität ([klicken Sie, um das Bild in voller Größe anzeigen](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="3b640-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="3b640-148">Die Movie-Klasse ist eine partielle Klasse.</span><span class="sxs-lookup"><span data-stu-id="3b640-148">The Movie class is a partial class.</span></span> <span data-ttu-id="3b640-149">Das bedeutet, dass wir eine andere partielle Klasse mit dem gleichen Namen zum Erweitern der Funktionalität der Movie-Klasse hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="3b640-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="3b640-150">Wir fügen unserer Validierungslogik auf die neue partielle Klasse.</span><span class="sxs-lookup"><span data-stu-id="3b640-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="3b640-151">Fügen Sie der Klasse in Liste 2 auf den Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="3b640-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="3b640-152">**Codebeispiel 2 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="3b640-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="3b640-153">Beachten Sie, die die Klasse im Codebeispiel 2 enthält die *teilweise* Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="3b640-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="3b640-154">Alle Methoden oder Eigenschaften, die Sie für diese Klasse hinzufügen, werden die Movie-Klasse, die vom Entity Framework generiert.</span><span class="sxs-lookup"><span data-stu-id="3b640-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="3b640-155">Hinzufügen von OnChanging und OnChanged partielle Methoden</span><span class="sxs-lookup"><span data-stu-id="3b640-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="3b640-156">Wenn Entity Framework eine Entitätsklasse generiert, fügt Entity Framework die partielle Methoden der Klasse automatisch aus.</span><span class="sxs-lookup"><span data-stu-id="3b640-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="3b640-157">Das Entity Framework generiert OnChanging und OnChanged partielle Methoden, die jede Eigenschaft der Klasse entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3b640-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="3b640-158">Im Fall von die Movie-Klasse erstellt Entity Framework die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="3b640-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="3b640-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="3b640-159">OnIdChanging</span></span>
- <span data-ttu-id="3b640-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="3b640-160">OnIdChanged</span></span>
- <span data-ttu-id="3b640-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="3b640-161">OnTitleChanging</span></span>
- <span data-ttu-id="3b640-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="3b640-162">OnTitleChanged</span></span>
- <span data-ttu-id="3b640-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="3b640-163">OnDirectorChanging</span></span>
- <span data-ttu-id="3b640-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="3b640-164">OnDirectorChanged</span></span>
- <span data-ttu-id="3b640-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="3b640-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="3b640-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="3b640-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="3b640-167">Die OnChanging-Methode wird rechts aufgerufen, bevor die entsprechende Eigenschaft geändert wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="3b640-168">Die OnChanged-Methode wird rechts aufgerufen, nachdem die Eigenschaft geändert wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="3b640-169">Sie können diese partiellen Methoden, um die Movie-Klasse Validierungslogik hinzufügen nutzen.</span><span class="sxs-lookup"><span data-stu-id="3b640-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="3b640-170">Das Update Movie-Klasse in Programmausdruck 3 stellt sicher, dass die Eigenschaften für Titel und Director nicht leeren Werte zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="3b640-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="3b640-171">Eine partielle Methode ist eine Methode, die in einer Klasse, die Sie nicht erforderlich, um die Implementierung definiert.</span><span class="sxs-lookup"><span data-stu-id="3b640-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="3b640-172">Wenn Sie eine partielle Methode implementieren, nicht der Compiler entfernt die Signatur der Methode, und alle Aufrufe an die Methode also es werden keine Laufzeit-Kosten im Zusammenhang mit der partiellen Methode.</span><span class="sxs-lookup"><span data-stu-id="3b640-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="3b640-173">In Visual Studio Code-Editor können Sie eine partielle Methode hinzufügen, durch das Schlüsselwort *teilweise* gefolgt von einem Leerzeichen zum Anzeigen einer Liste von Teilansichten implementieren.</span><span class="sxs-lookup"><span data-stu-id="3b640-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="3b640-174">**Codebeispiel 3 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="3b640-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="3b640-175">Z. B. Wenn Sie versuchen, eine leere Zeichenfolge an die Title-Eigenschaft zuweisen, klicken Sie dann eine Fehlermeldung erhält in ein Wörterbuch mit dem Namen \_Fehler.</span><span class="sxs-lookup"><span data-stu-id="3b640-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="3b640-176">An diesem Punkt nichts tatsächlich geschieht, wenn Sie eine leere Zeichenfolge an die Title-Eigenschaft zuweisen, und ein Fehler, an die Private hinzugefügt wird \_Fehler Feld.</span><span class="sxs-lookup"><span data-stu-id="3b640-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="3b640-177">Wir müssen zum Implementieren der IDataErrorInfo-Schnittstelle, um diese Überprüfungsfehler auf, um das ASP.NET MVC-Framework verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="3b640-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="3b640-178">Implementieren der IDataErrorInfo-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="3b640-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="3b640-179">Die IDataErrorInfo-Schnittstelle ist Teil von .NET Framework seit der ersten Version.</span><span class="sxs-lookup"><span data-stu-id="3b640-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="3b640-180">Diese Schnittstelle ist eine sehr einfache Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="3b640-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="3b640-181">Wenn eine Klasse die IDataErrorInfo-Schnittstelle implementiert, wird diese Schnittstelle in ASP.NET MVC-Framework verwenden, beim Erstellen einer Instanz der Klasse.</span><span class="sxs-lookup"><span data-stu-id="3b640-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="3b640-182">Die Home-Controller Create() Aktion akzeptiert z. B. eine Instanz der Movie-Klasse:</span><span class="sxs-lookup"><span data-stu-id="3b640-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="3b640-183">ASP.NET MVC-Framework erstellt, die Instanz des Films an die Create()-Aktion mithilfe eines Modellbinders (der DefaultModelBinder) übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="3b640-184">Die modellbindung ist dafür verantwortlich, erstellen eine Instanz von der Movie-Objekt, durch die Bindung der HTML-Formularfelder mit einer Instanz von der Movie-Objekt.</span><span class="sxs-lookup"><span data-stu-id="3b640-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="3b640-185">Der DefaultModelBinder erkennt, und zwar unabhängig davon, ob eine Klasse die IDataErrorInfo-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="3b640-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="3b640-186">Wenn eine Klasse diese Schnittstelle implementiert, ruft die modellbindung IDataErrorInfo.this Indexer für jede Eigenschaft der Klasse.</span><span class="sxs-lookup"><span data-stu-id="3b640-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="3b640-187">Wenn der Indexer eine Fehlermeldung zurückgegeben, fügt die modellbindung diese Fehlermeldung Zustand automatisch zu modellieren.</span><span class="sxs-lookup"><span data-stu-id="3b640-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="3b640-188">Der DefaultModelBinder überprüft auch die IDataErrorInfo.Error-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="3b640-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="3b640-189">Diese Eigenschaft sollte keine Eigenschaften spezifische Validierungsfehler im Zusammenhang mit der Klasse darstellen.</span><span class="sxs-lookup"><span data-stu-id="3b640-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="3b640-190">Beispielsweise empfiehlt es sich um eine Validierungsregel zu erzwingen, von die die Werte aus mehreren Eigenschaften der Movie-Klasse abhängt.</span><span class="sxs-lookup"><span data-stu-id="3b640-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="3b640-191">In diesem Fall müsste einen Validierungsfehler aus die Error-Eigenschaft zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="3b640-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="3b640-192">In Listing 4 die aktualisierte Movie-Klasse implementiert die IDataErrorInfo-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="3b640-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="3b640-193">**Codebeispiel 4: Models\Movie.cs (IDataErrorInfo implementiert)**</span><span class="sxs-lookup"><span data-stu-id="3b640-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="3b640-194">In Listing 4, die Indexereigenschaft überprüft die \_Errors-Auflistung, um festzustellen, ob es sich um einen Schlüssel enthält, die Namen der Eigenschaft entspricht, dem Indexer übergeben.</span><span class="sxs-lookup"><span data-stu-id="3b640-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="3b640-195">Wenn kein Validierungsfehler, die der Eigenschaft zugeordnet ist, wird eine leere Zeichenfolge zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="3b640-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="3b640-196">Sie müssen nicht den Home-Controller in keiner Weise verwenden Sie die geänderte Movie-Klasse zu ändern.</span><span class="sxs-lookup"><span data-stu-id="3b640-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="3b640-197">Die Seite angezeigt, die in Abbildung 3 wird veranschaulicht, was geschieht, wenn kein Wert für die Felder für Titel oder Director Formular eingegeben wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="3b640-198">[![Aktionsmethoden erstellen automatisch](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="3b640-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="3b640-199">**Abbildung 03**: ein Formular mit fehlenden Werten ([klicken Sie, um das Bild in voller Größe anzeigen](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3b640-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="3b640-200">Beachten Sie, dass der Wert DateReleased automatisch überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="3b640-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="3b640-201">Da die DateReleased-Eigenschaft keine NULL-Werte akzeptiert, generiert der DefaultModelBinder ein Überprüfungsfehler für diese Eigenschaft automatisch, wenn sie nicht über einen Wert verfügt.</span><span class="sxs-lookup"><span data-stu-id="3b640-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="3b640-202">Wenn Sie die Fehlermeldung für die DateReleased-Eigenschaft zu ändern, müssen Sie eine benutzerdefinierte modellbindung erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="3b640-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="3b640-203">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="3b640-203">Summary</span></span>

<span data-ttu-id="3b640-204">In diesem Tutorial haben Sie gelernt, wie die IDataErrorInfo-Schnittstelle verwenden, um Meldungen für Validierungsfehler zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3b640-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="3b640-205">Zunächst haben wir eine partielle Movie-Klasse, die die Funktionalität von der partiellen Movie-Klasse, die von Entity Framework generierten erweitert.</span><span class="sxs-lookup"><span data-stu-id="3b640-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="3b640-206">Als Nächstes haben wir die Movie-Klasse OnTitleChanging() und OnDirectorChanging() partiellen Methoden Validierungslogik hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3b640-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="3b640-207">Schließlich implementiert haben wir die IDataErrorInfo-Schnittstelle um diese Nachrichten zur inhaltsprüfung zu ASP.NET MVC-Framework verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="3b640-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3b640-208">[Zurück](performing-simple-validation-cs.md)
> [Weiter](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3b640-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
