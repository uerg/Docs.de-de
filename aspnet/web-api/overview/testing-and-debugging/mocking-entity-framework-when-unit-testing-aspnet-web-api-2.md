---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulieren des Entitätsframework bei Komponententests für ASP.NET Web API 2 | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Leitfaden und die Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung zu erstellen, das Entity Framework verwendet. Es veranschaulicht das Ändern der...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8945f913abe8fb8397d07a5994000fff2348f1f7
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795378"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="8f779-104">Simulieren des Entitätsframework bei Komponententests für ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8f779-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8f779-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8f779-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="8f779-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="8f779-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="8f779-107">Dieser Leitfaden und die Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung zu erstellen, das Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f779-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="8f779-108">Vorgehensweise: Ändern Sie den erstellten Controller aus, um die ermöglicht das Übergeben einer Context-Objekt, für das Testen und das Testobjekte zu erstellen, die mit Entity Framework verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f779-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="8f779-109">Eine Einführung in Komponententests mit ASP.NET Web-API, finden Sie unter [Komponententests mit ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8f779-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="8f779-110">In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.NET Web-API vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="8f779-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="8f779-111">Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8f779-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8f779-112">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f779-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="8f779-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8f779-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="8f779-114">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="8f779-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="8f779-115">In diesem Thema</span><span class="sxs-lookup"><span data-stu-id="8f779-115">In this topic</span></span>

<span data-ttu-id="8f779-116">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="8f779-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="8f779-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="8f779-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="8f779-118">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="8f779-118">Download code</span></span>](#download)
- [<span data-ttu-id="8f779-119">Erstellen Sie Anwendung mit Komponententestprojekt</span><span class="sxs-lookup"><span data-stu-id="8f779-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="8f779-120">Erstellen Sie die Model-Klasse</span><span class="sxs-lookup"><span data-stu-id="8f779-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="8f779-121">Hinzufügen des Controllers</span><span class="sxs-lookup"><span data-stu-id="8f779-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="8f779-122">Hinzufügen von Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="8f779-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="8f779-123">Installieren von NuGet-Pakete im Test-Projekt</span><span class="sxs-lookup"><span data-stu-id="8f779-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="8f779-124">Test erstellen</span><span class="sxs-lookup"><span data-stu-id="8f779-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="8f779-125">Erstellen von tests</span><span class="sxs-lookup"><span data-stu-id="8f779-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="8f779-126">Ausführen von tests</span><span class="sxs-lookup"><span data-stu-id="8f779-126">Run tests</span></span>](#runtests)

<span data-ttu-id="8f779-127">Wenn Sie bereits, dass die Schritte im haben [Komponententests mit ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), können Sie mit Abschnitt fortfahren [Hinzufügen des Controllers](#controller).</span><span class="sxs-lookup"><span data-stu-id="8f779-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="8f779-128">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="8f779-128">Prerequisites</span></span>

<span data-ttu-id="8f779-129">Visual Studio 2017 Community, Professional oder Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="8f779-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="8f779-130">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="8f779-130">Download code</span></span>

<span data-ttu-id="8f779-131">Herunterladen der [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="8f779-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="8f779-132">Herunterladbare Projekt enthält Komponententests Codes für in diesem Thema und die [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) Thema.</span><span class="sxs-lookup"><span data-stu-id="8f779-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="8f779-133">Erstellen Sie Anwendung mit Komponententestprojekt</span><span class="sxs-lookup"><span data-stu-id="8f779-133">Create application with unit test project</span></span>

<span data-ttu-id="8f779-134">Sie können entweder ein Komponententestprojekt erstellen, wenn Sie Ihre Anwendung zu erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8f779-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="8f779-135">Dieses Tutorial veranschaulicht das Erstellen eines Komponententestprojekts, beim Erstellen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8f779-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="8f779-136">Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="8f779-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="8f779-137">Wählen Sie in den Fenstern neues ASP.NET-Projekt die **leere** Vorlage und fügen Sie Ordner und kernreferenzen für Web-API.</span><span class="sxs-lookup"><span data-stu-id="8f779-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="8f779-138">Wählen Sie die **Komponententests hinzufügen,** Option.</span><span class="sxs-lookup"><span data-stu-id="8f779-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="8f779-139">Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="8f779-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="8f779-140">Sie können diesen Namen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="8f779-140">You can keep this name.</span></span>

![Komponententestprojekt erstellen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="8f779-142">Nach dem Erstellen der Anwendung werden Sie sehen, sie enthält zwei Projekte - **StoreApp** und **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="8f779-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="8f779-143">Erstellen Sie die Model-Klasse</span><span class="sxs-lookup"><span data-stu-id="8f779-143">Create the model class</span></span>

<span data-ttu-id="8f779-144">Fügen Sie in Ihrem StoreApp-Projekt eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="8f779-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="8f779-145">Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="8f779-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="8f779-146">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="8f779-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="8f779-147">Hinzufügen des Controllers</span><span class="sxs-lookup"><span data-stu-id="8f779-147">Add the controller</span></span>

<span data-ttu-id="8f779-148">Mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie **hinzufügen** und **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="8f779-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="8f779-149">Wählen Sie die Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8f779-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Hinzufügen eines neuen Controllers](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="8f779-151">Geben Sie die folgenden Werte an:</span><span class="sxs-lookup"><span data-stu-id="8f779-151">Set the following values:</span></span>

- <span data-ttu-id="8f779-152">Controllername: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="8f779-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="8f779-153">Modellklasse: **Produkt**</span><span class="sxs-lookup"><span data-stu-id="8f779-153">Model class: **Product**</span></span>
- <span data-ttu-id="8f779-154">Datenkontextklasse: [auswählen **neuer Datenkontext** Schaltfläche, die die Werte, siehe Abbildung unten ausfüllt]</span><span class="sxs-lookup"><span data-stu-id="8f779-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Geben Sie controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="8f779-156">Klicken Sie auf **hinzufügen** den Controller mit automatisch generiertem Code zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8f779-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="8f779-157">Der Code enthält Methoden zum Erstellen, abrufen, aktualisieren und Löschen von Instanzen der Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="8f779-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="8f779-158">Der folgende Code zeigt die Methode für ein Produkt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8f779-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="8f779-159">Beachten Sie, dass die Methode eine Instanz der zurückgibt **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="8f779-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="8f779-160">IHttpActionResult ist eines der neuen Features in Web-API 2, und vereinfacht dadurch das Unit Testentwicklung.</span><span class="sxs-lookup"><span data-stu-id="8f779-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="8f779-161">Im nächsten Abschnitt wird den generierten Code zu vereinfachen angepasst Testobjekte an den Controller übergeben.</span><span class="sxs-lookup"><span data-stu-id="8f779-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="8f779-162">Hinzufügen von Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="8f779-162">Add dependency injection</span></span>

<span data-ttu-id="8f779-163">Derzeit ist die ProductController-Klasse eine Instanz der Klasse StoreAppContext verwenden hartcodiert.</span><span class="sxs-lookup"><span data-stu-id="8f779-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="8f779-164">Sie werden nach dem Schema der Abhängigkeitsinjektion verwenden, ändern Sie die Anwendung, und entfernen diese Abhängigkeit hartcodiert.</span><span class="sxs-lookup"><span data-stu-id="8f779-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="8f779-165">Diese Abhängigkeit wird aufteilen, können Sie beim Testen in ein mock-Objekt übergeben.</span><span class="sxs-lookup"><span data-stu-id="8f779-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="8f779-166">Mit der rechten Maustaste die **Modelle** Ordner, und fügen Sie eine neue Schnittstelle namens **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="8f779-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="8f779-167">Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="8f779-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="8f779-168">Öffnen Sie die StoreAppContext.cs-Datei, und stellen Sie die folgenden hervorgehobenen Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="8f779-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="8f779-169">Die wichtigen Änderungen zu beachten sind:</span><span class="sxs-lookup"><span data-stu-id="8f779-169">The important changes to note are:</span></span>

- <span data-ttu-id="8f779-170">StoreAppContext-Klasse implementiert IStoreAppContext-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="8f779-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="8f779-171">MarkAsModified-Methode wird implementiert.</span><span class="sxs-lookup"><span data-stu-id="8f779-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="8f779-172">Öffnen Sie die ProductController.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="8f779-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="8f779-173">Ändern Sie den vorhandenen Code den hervorgehobenen Code entsprechend.</span><span class="sxs-lookup"><span data-stu-id="8f779-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="8f779-174">Diese Änderungen die Abhängigkeit auf StoreAppContext unterbrechen und aktivieren Sie andere Klassen in einem anderen Objekt der Context-Klasse übergeben.</span><span class="sxs-lookup"><span data-stu-id="8f779-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="8f779-175">Diese Änderung können Sie während der Komponententests in einem Testkontext übergeben.</span><span class="sxs-lookup"><span data-stu-id="8f779-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="8f779-176">Es gibt eine weitere Änderung, die Sie ProductController vornehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="8f779-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="8f779-177">In der **PutProduct** -Methode, ersetzen die Zeile, die den Entitätszustand wird auf mit einem Aufruf der Methode MarkAsModified geändert.</span><span class="sxs-lookup"><span data-stu-id="8f779-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="8f779-178">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="8f779-178">Build the solution.</span></span>

<span data-ttu-id="8f779-179">Sie können nun können Sie das Testprojekt einrichten.</span><span class="sxs-lookup"><span data-stu-id="8f779-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="8f779-180">Installieren von NuGet-Pakete im Test-Projekt</span><span class="sxs-lookup"><span data-stu-id="8f779-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="8f779-181">Wenn Sie die leere Vorlage verwenden, um eine Anwendung zu erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="8f779-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="8f779-182">Weitere Vorlagen, wie z. B. die Web-API-Vorlage, schließen Sie einige NuGet-Pakete in das Komponententestprojekt.</span><span class="sxs-lookup"><span data-stu-id="8f779-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="8f779-183">Für dieses Tutorial müssen Sie die Entity Framework-Paket und das Microsoft ASP.NET Web API 2-Core-Paket für das Testprojekt einschließen.</span><span class="sxs-lookup"><span data-stu-id="8f779-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="8f779-184">Mit der rechten Maustaste in des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="8f779-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="8f779-185">Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.</span><span class="sxs-lookup"><span data-stu-id="8f779-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Verwalten von Paketen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="8f779-187">Suchen Sie die Online-Pakete und installieren Sie das EntityFramework-Paket (Version 6.0 oder höher).</span><span class="sxs-lookup"><span data-stu-id="8f779-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="8f779-188">Wenn es angezeigt wird, dass das EntityFramework-Paket bereits installiert ist, haben Sie möglicherweise das StoreApp Projekt statt dem StoreApp.Tests-Projekt ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="8f779-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Entity Framework hinzufügen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="8f779-190">Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.</span><span class="sxs-lookup"><span data-stu-id="8f779-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Installieren Sie Web-api-Core-Paket](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="8f779-192">Schließen Sie das Fenster "NuGet-Pakete verwalten".</span><span class="sxs-lookup"><span data-stu-id="8f779-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="8f779-193">Test erstellen</span><span class="sxs-lookup"><span data-stu-id="8f779-193">Create test context</span></span>

<span data-ttu-id="8f779-194">Fügen Sie eine Klasse, die mit dem Namen **TestDbSet** für das Testprojekt.</span><span class="sxs-lookup"><span data-stu-id="8f779-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="8f779-195">Diese Klasse dient als Basisklasse für das Test-DataSet.</span><span class="sxs-lookup"><span data-stu-id="8f779-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="8f779-196">Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="8f779-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="8f779-197">Fügen Sie eine Klasse, die mit dem Namen **TestProductDbSet** für das Testprojekt, die den folgenden Code enthält.</span><span class="sxs-lookup"><span data-stu-id="8f779-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="8f779-198">Fügen Sie eine Klasse, die mit dem Namen **TestStoreAppContext** , und Ersetzen Sie den vorhandenen Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="8f779-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="8f779-199">Tests erstellen</span><span class="sxs-lookup"><span data-stu-id="8f779-199">Create tests</span></span>

<span data-ttu-id="8f779-200">Das Testprojekt enthält standardmäßig eine leeren Test-Datei mit dem Namen **"UnitTest1.cs"**.</span><span class="sxs-lookup"><span data-stu-id="8f779-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="8f779-201">Diese Datei werden die Attribute, dass Sie zum Erstellen der Testmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="8f779-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="8f779-202">In diesem Tutorial können Sie diese Datei löschen, da Sie eine neue Testklasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8f779-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="8f779-203">Fügen Sie eine Klasse, die mit dem Namen **TestProductController** für das Testprojekt.</span><span class="sxs-lookup"><span data-stu-id="8f779-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="8f779-204">Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="8f779-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="8f779-205">Tests durchführen</span><span class="sxs-lookup"><span data-stu-id="8f779-205">Run tests</span></span>

<span data-ttu-id="8f779-206">Sie können nun zum Ausführen der Tests.</span><span class="sxs-lookup"><span data-stu-id="8f779-206">You are now ready to run the tests.</span></span> <span data-ttu-id="8f779-207">Alle der Methode, die mit markierten Felder der **TestMethod** Attribut getestet.</span><span class="sxs-lookup"><span data-stu-id="8f779-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="8f779-208">Von der **Test** Menüelement, führen Sie die Tests.</span><span class="sxs-lookup"><span data-stu-id="8f779-208">From the **Test** menu item, run the tests.</span></span>

![Tests durchführen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="8f779-210">Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.</span><span class="sxs-lookup"><span data-stu-id="8f779-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![Testergebnisse](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
