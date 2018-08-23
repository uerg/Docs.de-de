---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Komponententests für ASP.NET Web API 2 | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Leitfaden und die Anwendung veranschaulichen, wie einfacher Komponententests für Ihre Web-API 2-Anwendung zu erstellen. Dieses Tutorial zeigt, wie eine Unit Test Proj enthalten...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a3f9ddde725995359de85c5e0783106f5de02bd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837179"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="b3fe9-104">Komponententests für ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="b3fe9-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="b3fe9-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b3fe9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="b3fe9-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="b3fe9-107">Dieser Leitfaden und die Anwendung veranschaulichen, wie einfacher Komponententests für Ihre Web-API 2-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="b3fe9-108">In diesem Tutorial veranschaulicht einen Datenbankkomponententest-Projekt in der Projektmappe, und Schreiben von Testmethoden, die die zurückgegebenen Werte von einer Controllermethode aus zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="b3fe9-109">In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.NET Web-API vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="b3fe9-110">Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b3fe9-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="b3fe9-111">Die Komponententests in diesem Thema sind absichtlich auf einfachen Szenarien beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="b3fe9-112">Komponententests erweiterte Data-Szenarien finden Sie unter [Mocking Entity Framework bei Komponententests Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="b3fe9-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b3fe9-113">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b3fe9-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b3fe9-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="b3fe9-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="b3fe9-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="b3fe9-116">In diesem Thema</span><span class="sxs-lookup"><span data-stu-id="b3fe9-116">In this topic</span></span>

<span data-ttu-id="b3fe9-117">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="b3fe9-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b3fe9-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b3fe9-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="b3fe9-119">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-119">Download code</span></span>](#download)
- [<span data-ttu-id="b3fe9-120">Erstellen Sie Anwendung mit Komponententestprojekt</span><span class="sxs-lookup"><span data-stu-id="b3fe9-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="b3fe9-121">Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu</span><span class="sxs-lookup"><span data-stu-id="b3fe9-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="b3fe9-122">Datenbankkomponententest-Projekt zu einer vorhandenen Anwendung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="b3fe9-123">Einrichten der Web-API 2-Anwendung</span><span class="sxs-lookup"><span data-stu-id="b3fe9-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="b3fe9-124">Installieren von NuGet-Pakete im Test-Projekt</span><span class="sxs-lookup"><span data-stu-id="b3fe9-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="b3fe9-125">Erstellen von tests</span><span class="sxs-lookup"><span data-stu-id="b3fe9-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="b3fe9-126">Ausführen von tests</span><span class="sxs-lookup"><span data-stu-id="b3fe9-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="b3fe9-127">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b3fe9-127">Prerequisites</span></span>

<span data-ttu-id="b3fe9-128">Visual Studio 2017 Community, Professional oder Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="b3fe9-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="b3fe9-129">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-129">Download code</span></span>

<span data-ttu-id="b3fe9-130">Herunterladen der [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="b3fe9-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="b3fe9-131">Herunterladbare Projekt enthält Komponententests Codes für in diesem Thema und die [Mocking Entity Framework beim Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) Thema.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="b3fe9-132">Erstellen Sie Anwendung mit Komponententestprojekt</span><span class="sxs-lookup"><span data-stu-id="b3fe9-132">Create application with unit test project</span></span>

<span data-ttu-id="b3fe9-133">Sie können entweder ein Komponententestprojekt erstellen, wenn Sie Ihre Anwendung zu erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="b3fe9-134">Dieses Tutorial zeigt beide Methoden zum Erstellen eines Komponententestprojekts.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="b3fe9-135">Um dieses Tutorial folgen zu können, können Sie beide Ansätze verwenden.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="b3fe9-136">Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu</span><span class="sxs-lookup"><span data-stu-id="b3fe9-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="b3fe9-137">Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Projekt erstellen](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="b3fe9-139">Wählen Sie in den Fenstern neues ASP.NET-Projekt die **leere** Vorlage und fügen Sie Ordner und kernreferenzen für Web-API.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="b3fe9-140">Wählen Sie die **Komponententests hinzufügen,** Option.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="b3fe9-141">Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="b3fe9-142">Sie können diesen Namen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-142">You can keep this name.</span></span>

![Komponententestprojekt erstellen](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="b3fe9-144">Nach dem Erstellen der Anwendung sehen Sie sich, dass sie zwei Projekte enthält.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-144">After creating the application, you will see it contains two projects.</span></span>

![zwei Projekte](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="b3fe9-146">Datenbankkomponententest-Projekt zu einer vorhandenen Anwendung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="b3fe9-147">Wenn Sie das Komponententestprojekt nicht bei der Erstellung Ihrer Anwendung erstellt haben, können Sie eine zu einem beliebigen Zeitpunkt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="b3fe9-148">Nehmen wir beispielsweise an, dass Sie bereits über eine Anwendung namens StoreApp verfügen, und Komponententests hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="b3fe9-149">Klicken Sie zum Hinzufügen eines Komponententestprojekts mit der rechten Maustaste in der Projektmappe, und wählen Sie **hinzufügen** und **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Neues Projekt zu Projektmappe hinzufügen](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="b3fe9-151">Wählen Sie **Test** im linken Bereich, und wählen **Komponententestprojekt** für den Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="b3fe9-152">Nennen Sie das Projekt **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-152">Name the project **StoreApp.Tests**.</span></span>

![Datenbankkomponententest-Projekt hinzufügen](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="b3fe9-154">Sie sehen das Komponententestprojekt in Ihrer Lösung.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="b3fe9-155">Fügen Sie in der Datenbankkomponententest-Projekt einen Projektverweis auf das ursprüngliche Projekt ein.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="b3fe9-156">Einrichten der Web-API 2-Anwendung</span><span class="sxs-lookup"><span data-stu-id="b3fe9-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="b3fe9-157">Fügen Sie in Ihrem StoreApp-Projekt eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="b3fe9-158">Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="b3fe9-159">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-159">Build the solution.</span></span>

<span data-ttu-id="b3fe9-160">Mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie **hinzufügen** und **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="b3fe9-161">Wählen Sie **Web-API 2-Controller - leer**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-161">Select **Web API 2 Controller - Empty**.</span></span>

![Hinzufügen eines neuen Controllers](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="b3fe9-163">Legen Sie den Controllernamen **SimpleProductController**, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Geben Sie controller](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="b3fe9-165">Ersetzen Sie den vorhandenen Code durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="b3fe9-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="b3fe9-166">Zur Vereinfachung dieses Beispiels werden die Daten in einer Liste anstelle einer Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="b3fe9-167">Die in dieser Klasse definierten Liste stellt die Produktionsdaten dar.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="b3fe9-168">Beachten Sie, dass der Controller enthält einen Konstruktor, der als Parameter eine Liste der Product-Objekte akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="b3fe9-169">Dieser Konstruktor können Sie Testdaten zu übergeben wenn Komponententests ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="b3fe9-170">Der Controller enthält auch zwei **Async** Methoden, um asynchrone Methoden für die Komponententests zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="b3fe9-171">Diese asynchronen Methoden wurden implementiert, durch den Aufruf **Task.FromResult** Minimieren von überflüssigen Code, aber normalerweise die Methoden würde ressourcenintensive Vorgänge enthalten.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="b3fe9-172">Die GetProduct-Methode gibt eine Instanz des der **IHttpActionResult** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="b3fe9-173">IHttpActionResult ist eines der neuen Features in Web-API 2, und vereinfacht dadurch das Unit Testentwicklung.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="b3fe9-174">Klassen, die die IHttpActionResult-Schnittstelle implementieren finden Sie in der [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) Namespace.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="b3fe9-175">Diese Klassen stellen die mögliche Antworten aus einer aktionsanforderung dar, und sie entsprechen den HTTP-Statuscodes.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="b3fe9-176">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-176">Build the solution.</span></span>

<span data-ttu-id="b3fe9-177">Sie können nun können Sie das Testprojekt einrichten.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="b3fe9-178">Installieren von NuGet-Pakete im Test-Projekt</span><span class="sxs-lookup"><span data-stu-id="b3fe9-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="b3fe9-179">Wenn Sie die leere Vorlage verwenden, um eine Anwendung zu erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="b3fe9-180">Weitere Vorlagen, wie z. B. die Web-API-Vorlage, schließen Sie einige NuGet-Pakete in das Komponententestprojekt.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="b3fe9-181">Für dieses Tutorial müssen Sie das Microsoft ASP.NET Web API 2-Core-Paket für das Testprojekt einschließen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="b3fe9-182">Mit der rechten Maustaste in des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="b3fe9-183">Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Verwalten von Paketen](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="b3fe9-185">Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Installieren Sie Web-api-Core-Paket](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="b3fe9-187">Schließen Sie das Fenster "NuGet-Pakete verwalten".</span><span class="sxs-lookup"><span data-stu-id="b3fe9-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="b3fe9-188">Tests erstellen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-188">Create tests</span></span>

<span data-ttu-id="b3fe9-189">Standardmäßig enthält das Testprojekt eine leeren Test-Datei, die mit dem Namen "UnitTest1.cs".</span><span class="sxs-lookup"><span data-stu-id="b3fe9-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="b3fe9-190">Diese Datei werden die Attribute, dass Sie zum Erstellen der Testmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="b3fe9-191">Für die Komponententests können Sie diese Datei verwenden oder eine eigene Datei erstellen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="b3fe9-193">In diesem Tutorial erstellen Sie eine eigene Testklasse.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="b3fe9-194">Sie können die Datei "UnitTest1.cs" löschen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="b3fe9-195">Fügen Sie eine Klasse, die mit dem Namen **TestSimpleProductController.cs**, und Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="b3fe9-196">Tests durchführen</span><span class="sxs-lookup"><span data-stu-id="b3fe9-196">Run tests</span></span>

<span data-ttu-id="b3fe9-197">Sie können nun zum Ausführen der Tests.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-197">You are now ready to run the tests.</span></span> <span data-ttu-id="b3fe9-198">Alle der Methode, die mit markierten Felder der **TestMethod** Attribut getestet.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="b3fe9-199">Von der **Test** Menüelement, führen Sie die Tests.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-199">From the **Test** menu item, run the tests.</span></span>

![Tests durchführen](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="b3fe9-201">Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![Testergebnisse](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="b3fe9-203">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b3fe9-203">Summary</span></span>

<span data-ttu-id="b3fe9-204">Sie haben dieses Lernprogramm abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-204">You have completed this tutorial.</span></span> <span data-ttu-id="b3fe9-205">Die Daten in diesem Tutorial wurde absichtlich Schwerpunkte Komponententests Bedingungen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="b3fe9-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="b3fe9-206">Komponententests erweiterte Data-Szenarien finden Sie unter [Mocking Entity Framework bei Komponententests Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="b3fe9-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
