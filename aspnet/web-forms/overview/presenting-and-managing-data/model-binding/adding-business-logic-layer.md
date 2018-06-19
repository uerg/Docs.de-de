---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Hinzufügen von Geschäftslogikschicht auf ein Projekt, wurden die modellbindung und WebForms verwendet. | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892750"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="3fbbe-104">Hinzufügen von Geschäftslogikschicht auf ein Projekt, wurden die modellbindung und WebForms verwendet</span><span class="sxs-lookup"><span data-stu-id="3fbbe-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="3fbbe-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3fbbe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3fbbe-106">Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="3fbbe-107">Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="3fbbe-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="3fbbe-108">Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="3fbbe-109">Dieses Lernprogramm zeigt, wie mit eine Geschäftslogikschicht wurden die modellbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="3fbbe-110">Sie legen das OnCallingDataMethods-Element, um anzugeben, dass ein anderes Objekt als die aktuelle Seite verwendet wird, die Methoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="3fbbe-111">Dieses Lernprogramm baut auf das Projekt erstellt, der [früheren](retrieving-data.md) Teile der Reihe.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="3fbbe-112">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="3fbbe-113">Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="3fbbe-114">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="3fbbe-115">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="3fbbe-115">What you'll build</span></span>

<span data-ttu-id="3fbbe-116">Wurden die modellbindung können Sie Ihre Daten Interaktion-Code in der CodeBehind-Datei für eine Webseite oder in einer separaten Business Logic-Klasse platziert.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="3fbbe-117">In den vorherigen Lernprogrammen wurden zum Verwenden von Code-Behind-Dateien für die Interaktion Datencode gezeigt.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="3fbbe-118">Dieser Ansatz funktioniert bei kleinen Standorten kann es jedoch führen, um Wiederholung und erschwerte zu codieren, wenn eine große Website verwalten.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="3fbbe-119">Es kann auch sehr schwierig sein, Code programmgesteuert zu testen, die in CodeBehind-Dateien gespeichert ist, da keine Abstraktionsschicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="3fbbe-120">Um die Interaktion Datencode zu zentralisieren, können Sie eine Geschäftslogikschicht erstellen, die alle von der Logik für die Interaktion mit Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="3fbbe-121">Dann rufen Sie die Geschäftslogikschicht von Webseiten.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="3fbbe-122">Dieses Lernprogramm zeigt, wie verschieben Sie alle des Codes, den Sie in den vorherigen Lernprogrammen in eine Geschäftslogikschicht geschrieben haben, und verwenden Sie diesen Code auf den Seiten.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="3fbbe-123">In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="3fbbe-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="3fbbe-124">Verschieben Sie den Code vom Code-Behind-Dateien, eine Geschäftslogikschicht</span><span class="sxs-lookup"><span data-stu-id="3fbbe-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="3fbbe-125">Ändern Sie die datengebundene Steuerelemente zum Aufrufen von Methoden in der Geschäftslogikebene</span><span class="sxs-lookup"><span data-stu-id="3fbbe-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="3fbbe-126">Erstellen Sie die Logik der Geschäftsebene</span><span class="sxs-lookup"><span data-stu-id="3fbbe-126">Create business logic layer</span></span>

<span data-ttu-id="3fbbe-127">Nun erstellen Sie die Klasse, die die Webseiten aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="3fbbe-128">Die Methoden in dieser Klasse ähnelt die Methoden, die Sie in den vorherigen Lernprogrammen verwendet und der Anbieter Wertattribute enthalten.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="3fbbe-129">Fügen Sie zunächst einen neuen Ordner namens **BLL**.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-129">First, add a new folder called **BLL**.</span></span>

![Ordner hinzufügen](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="3fbbe-131">Erstellen Sie eine neue Klasse mit dem Namen im Ordner "BLL" **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="3fbbe-132">Es wird alle Datenvorgänge enthalten, die im Code-Behind-Dateien ursprünglich gespeichert war.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="3fbbe-133">Die Methoden sind nahezu identisch mit den Methoden in der Code-Behind-Datei, aber einige Änderungen enthält.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="3fbbe-134">Die wichtigste Änderung zu beachten ist, dass Sie den Code von innerhalb einer Instanz von nicht mehr ausgeführt werden **Seite** Klasse.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="3fbbe-135">Die Page-Klasse enthält die **TryUpdateModel** Methode und die **ModelState** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="3fbbe-136">Wenn dieser Code in eine Geschäftslogikschicht verschoben wird, müssen Sie nicht mehr eine Instanz der Page-Klasse, um diese Member aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="3fbbe-137">Um dieses Problem zu umgehen, fügen Sie eine **ModelMethodContext** Parameter für jede Methode, die TryUpdateModel oder ModelState zugreift.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="3fbbe-138">Zum Aufrufen von TryUpdateModel ModelState abzurufen, verwenden Sie diesen ModelMethodContext-Parameter.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="3fbbe-139">Sie müssen nicht ändert nichts in die Webseite für diesen neuen Parameter zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="3fbbe-140">Ersetzen Sie den Code in SchoolBL.cs durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="3fbbe-141">Überarbeiten Sie die vorhandene Seiten zum Abrufen von Daten aus der Geschäftslogikebene</span><span class="sxs-lookup"><span data-stu-id="3fbbe-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="3fbbe-142">Zum Schluss konvertieren Sie Students.aspx, AddStudent.aspx und Courses.aspx Seiten aus der Verwendung von Abfragen in der Code-Behind-Datei mit der Geschäftslogikebene.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="3fbbe-143">Klicken Sie in den Code-Behind-Dateien für Studenten, Bildungseinrichtungen AddStudent und Kurse löschen Sie, oder kommentieren Sie die folgenden Abfragemethoden:</span><span class="sxs-lookup"><span data-stu-id="3fbbe-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="3fbbe-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="3fbbe-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="3fbbe-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="3fbbe-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="3fbbe-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="3fbbe-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="3fbbe-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="3fbbe-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="3fbbe-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="3fbbe-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="3fbbe-149">Sie verfügen jetzt über keinen Code in der CodeBehind-Datei, die zum Datenvorgänge gehört.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="3fbbe-150">Die **OnCallingDataMethods** Ereignishandler ermöglicht es Ihnen, geben Sie ein Objekt, das für die Data-Methoden verwendet.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="3fbbe-151">Fügen Sie in Students.aspx einen Wert für diesen Ereignishandler hinzu, und ändern Sie die Namen der Datenmethoden den Namen der Methoden in der Business Logic-Klasse.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="3fbbe-152">Definieren Sie in der CodeBehind-Datei für Students.aspx den Ereignishandler für das CallingDataMethods-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="3fbbe-153">In diesem Ereignishandler geben Sie die Business Logic-Klasse für Datenvorgänge.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="3fbbe-154">Stellen Sie im AddStudent.aspx ähnliche Änderungen.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="3fbbe-155">Stellen Sie im Courses.aspx ähnliche Änderungen.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="3fbbe-156">Führen Sie die Anwendung, und beachten Sie, dass alle Seiten funktionieren wie zuvor mussten.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="3fbbe-157">Die Validierungslogik auch funktioniert ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="3fbbe-158">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="3fbbe-158">Conclusion</span></span>

<span data-ttu-id="3fbbe-159">In diesem Lernprogramm strukturiert neu Sie die Anwendung eine Datenzugriffsschicht und Geschäftslogikschicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="3fbbe-160">Sie angegeben, dass die Data-Steuerelemente ein Objekts verwenden, das nicht die aktuelle Seite Datenvorgänge ist.</span><span class="sxs-lookup"><span data-stu-id="3fbbe-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3fbbe-161">Vorherige</span><span class="sxs-lookup"><span data-stu-id="3fbbe-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
