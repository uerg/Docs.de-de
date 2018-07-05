---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Hinzufügen der Geschäftslogikebene auf ein Projekt, modellbindung und Web Forms verwendet. | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: be69bf56c6a1c5bf601a47e90e4e1c67c48a760f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386499"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="ad33c-104">Hinzufügen der Geschäftslogikebene auf ein Projekt, modellbindung und Web Forms verwendet</span><span class="sxs-lookup"><span data-stu-id="ad33c-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="ad33c-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ad33c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ad33c-106">Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ad33c-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ad33c-107">Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement).</span><span class="sxs-lookup"><span data-stu-id="ad33c-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ad33c-108">Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ad33c-109">In diesem Tutorial wird gezeigt, wie Sie mit einer Geschäftslogikebene mit modellbindung.</span><span class="sxs-lookup"><span data-stu-id="ad33c-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="ad33c-110">Sie legen das OnCallingDataMethods-Element, um anzugeben, dass ein anderes Objekt als die aktuelle Seite mit der die Datenmethoden aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="ad33c-111">Dieses Tutorial baut auf das Projekt erstellt haben, der [früheren](retrieving-data.md) Teilen der Reihe.</span><span class="sxs-lookup"><span data-stu-id="ad33c-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="ad33c-112">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB.</span><span class="sxs-lookup"><span data-stu-id="ad33c-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ad33c-113">Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ad33c-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ad33c-114">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ad33c-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ad33c-115">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="ad33c-115">What you'll build</span></span>

<span data-ttu-id="ad33c-116">Modellbindung ermöglicht Ihnen, Ihre Interaktion Datencode in der CodeBehind-Datei für eine Webseite oder in einer separaten Business Logic-Klasse zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="ad33c-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="ad33c-117">Den vorherigen Tutorials haben gezeigt, wie der Code-Behind-Dateien für die Interaktion Datencode zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ad33c-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="ad33c-118">Dieser Ansatz funktioniert bei kleinen Standorten, jedoch kann dies zu Wiederholung und schwierigere code, wenn eine große Site verwaltet.</span><span class="sxs-lookup"><span data-stu-id="ad33c-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="ad33c-119">Sie können auch sehr schwierig sein, Code programmgesteuert zu testen, die in der CodeBehind-Dateien befindet, da keine Abstraktionsschicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="ad33c-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="ad33c-120">Um die Interaktion Datenzugriffscode zu zentralisieren, können Sie eine Geschäftslogikebene erstellen, die die gesamte Logik für die Interaktion mit Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="ad33c-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="ad33c-121">Anschließend rufen Sie die Geschäftslogikebene von Ihren Webseiten.</span><span class="sxs-lookup"><span data-stu-id="ad33c-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="ad33c-122">Dieses Tutorial veranschaulicht, wie verschieben sämtlichen Code, die, den Sie in den vorherigen Tutorials in einer Geschäftslogikebene geschrieben haben, und klicken Sie dann diesen Code auf den Seiten verwenden.</span><span class="sxs-lookup"><span data-stu-id="ad33c-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="ad33c-123">In diesem Tutorial müssen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="ad33c-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ad33c-124">Verschieben Sie den Code vom Code-Behind-Dateien auf einer Geschäftslogikebene</span><span class="sxs-lookup"><span data-stu-id="ad33c-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="ad33c-125">Ändern Sie die datengebundene Steuerelemente zum Aufrufen von Methoden in der Geschäftslogikebene</span><span class="sxs-lookup"><span data-stu-id="ad33c-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="ad33c-126">Erstellen der Geschäftslogikebene</span><span class="sxs-lookup"><span data-stu-id="ad33c-126">Create business logic layer</span></span>

<span data-ttu-id="ad33c-127">Erstellen Sie nun die Klasse, die von der Webseiten aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ad33c-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="ad33c-128">Die Methoden in dieser Klasse ähnelt die Methoden, die Sie in den vorherigen Tutorials verwendet und enthalten die Attribute des Wert-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="ad33c-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="ad33c-129">Fügen Sie zunächst einen neuen Ordner namens **BLL**.</span><span class="sxs-lookup"><span data-stu-id="ad33c-129">First, add a new folder called **BLL**.</span></span>

![Ordner hinzufügen](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="ad33c-131">In den BLL-Ordner, erstellen Sie eine neue Klasse namens **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="ad33c-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="ad33c-132">Alle Datenvorgänge, enthält, die im Code-Behind-Dateien ursprünglich gespeichert war.</span><span class="sxs-lookup"><span data-stu-id="ad33c-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="ad33c-133">Die Methoden sind nahezu identisch mit den Methoden in der CodeBehind-Datei, aber einige Änderungen enthält.</span><span class="sxs-lookup"><span data-stu-id="ad33c-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="ad33c-134">Die wichtigste Änderung zu beachten ist, dass Sie den Code aus innerhalb einer Instanz von nicht mehr ausgeführt werden **Seite** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ad33c-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="ad33c-135">Die Page-Klasse enthält die **TryUpdateModel** Methode und die **ModelState** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ad33c-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="ad33c-136">Wenn dieser Code in einer Geschäftslogikebene verschoben wird, müssen Sie nicht mehr eine Instanz der Page-Klasse diese Member aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="ad33c-137">Um dieses Problem zu umgehen, fügen Sie eine **ModelMethodContext** Parameter, um eine Methode, die TryUpdateModel oder ModelState zugreift.</span><span class="sxs-lookup"><span data-stu-id="ad33c-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="ad33c-138">Sie können diesen ModelMethodContext-Parameter verwenden, rufen TryUpdateModel oder ModelState abzurufen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="ad33c-139">Sie müssen keine Änderungen vornehmen, auf der Webseite für diesen neuen Parameter zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="ad33c-140">Ersetzen Sie den Code in SchoolBL.cs durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="ad33c-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="ad33c-141">Überarbeiten Sie die vorhandene Seiten zum Abrufen von Daten aus der Geschäftslogikebene</span><span class="sxs-lookup"><span data-stu-id="ad33c-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="ad33c-142">Zum Schluss konvertieren Sie die Seiten Students.aspx AddStudent.aspx und Courses.aspx aus der Verwendung von Abfragen in der CodeBehind-Datei mit der Geschäftslogikebene.</span><span class="sxs-lookup"><span data-stu-id="ad33c-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="ad33c-143">Klicken Sie in den Code-Behind-Dateien für Schüler/Studenten, Kurse und AddStudent löschen Sie, oder kommentieren Sie die folgenden Abfragemethoden:</span><span class="sxs-lookup"><span data-stu-id="ad33c-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="ad33c-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="ad33c-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="ad33c-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="ad33c-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="ad33c-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="ad33c-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="ad33c-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="ad33c-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="ad33c-148">CoursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="ad33c-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="ad33c-149">Sie verfügen nun über keinen Code im Code-Behind-Datei, die Datenvorgänge betrifft.</span><span class="sxs-lookup"><span data-stu-id="ad33c-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="ad33c-150">Die **OnCallingDataMethods** Ereignishandler können Sie zur Angabe eines Objekts für die Methoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="ad33c-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="ad33c-151">Students.aspx fügen Sie einen Wert für diesen Ereignishandler hinzu und ändern Sie den Namen der Datenmethoden in den Namen der Methoden in der Business Logic-Klasse.</span><span class="sxs-lookup"><span data-stu-id="ad33c-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="ad33c-152">Definieren Sie in der CodeBehind-Datei für Students.aspx den Ereignishandler für das CallingDataMethods-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="ad33c-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="ad33c-153">In diesem Ereignishandler geben Sie die Business Logic-Klasse für Datenvorgänge an.</span><span class="sxs-lookup"><span data-stu-id="ad33c-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="ad33c-154">Stellen Sie in AddStudent.aspx ähnliche Änderungen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="ad33c-155">Stellen Sie in Courses.aspx ähnliche Änderungen.</span><span class="sxs-lookup"><span data-stu-id="ad33c-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="ad33c-156">Führen Sie die Anwendung, und beachten Sie, dass alle Seiten funktionieren wie zuvor hatten.</span><span class="sxs-lookup"><span data-stu-id="ad33c-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="ad33c-157">Die Validierungslogik auch funktioniert ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="ad33c-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ad33c-158">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="ad33c-158">Conclusion</span></span>

<span data-ttu-id="ad33c-159">In diesem Tutorial strukturiert neu Sie Ihrer Anwendung zur Verwendung einer Datenzugriffsebene und den Geschäftslogikebene.</span><span class="sxs-lookup"><span data-stu-id="ad33c-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="ad33c-160">Sie angegeben haben, dass die Datensteuerelemente ein Objekt verwenden, die nicht die aktuelle Seite für Datenvorgänge ist.</span><span class="sxs-lookup"><span data-stu-id="ad33c-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ad33c-161">Vorherige</span><span class="sxs-lookup"><span data-stu-id="ad33c-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
