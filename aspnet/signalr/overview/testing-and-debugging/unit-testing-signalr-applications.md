---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "Komponententests für SignalR-Anwendungen | Microsoft Docs"
author: pfletcher
description: Dieser Artikel beschreibt, wie die Komponententest-Features von SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="0c610-103">Einheit Testen von SignalR-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="0c610-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="0c610-104">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0c610-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="0c610-105">Dieser Artikel beschreibt die Verwendung der Funktionen UnitTests von SignalR-2.</span><span class="sxs-lookup"><span data-stu-id="0c610-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0c610-106">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="0c610-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="0c610-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0c610-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0c610-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0c610-108">.NET 4.5</span></span>
> - <span data-ttu-id="0c610-109">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="0c610-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="0c610-110">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="0c610-110">Questions and comments</span></span>
> 
> <span data-ttu-id="0c610-111">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="0c610-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0c610-112">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0c610-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="0c610-113">Komponententests für SignalR-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="0c610-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="0c610-114">Die Einheit Test-Funktionen können in SignalR 2 Sie um Komponententests für die SignalR-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0c610-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="0c610-115">SignalR 2 umfasst die [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) -Schnittstelle, die zum Erstellen einer simulierten-Objekt, um Ihre hubmethoden zu Testzwecken simulieren verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0c610-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="0c610-116">In diesem Abschnitt fügen Sie Komponententests für die Anwendung in der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) mit [XUnit.net](https://github.com/xunit/xunit) und [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="0c610-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="0c610-117">XUnit.net wird verwendet werden, um den Test zu steuern. Moq verwendet werden, erstellen Sie eine [modellieren](http://en.wikipedia.org/wiki/Mock_object) Objekt zum Testen.</span><span class="sxs-lookup"><span data-stu-id="0c610-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="0c610-118">Andere Mockframeworks können verwendet werden, falls gewünscht. [NSubstitute](http://nsubstitute.github.io/) ist auch eine gute Wahl.</span><span class="sxs-lookup"><span data-stu-id="0c610-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="0c610-119">Dieses Lernprogramm veranschaulicht, wie die simulierte Objekt auf zwei Arten eingerichtet: zunächst mithilfe einer `dynamic` Objekt (eingeführt in .NET Framework 4) und der Sekunde, mit einer Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0c610-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="0c610-120">Inhalt</span><span class="sxs-lookup"><span data-stu-id="0c610-120">Contents</span></span>

<span data-ttu-id="0c610-121">Dieses Lernprogramm enthält die folgenden Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="0c610-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="0c610-122">Komponententests mit dynamischen</span><span class="sxs-lookup"><span data-stu-id="0c610-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="0c610-123">Komponententests, die nach Typ</span><span class="sxs-lookup"><span data-stu-id="0c610-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="0c610-124">Komponententests mit dynamischen</span><span class="sxs-lookup"><span data-stu-id="0c610-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="0c610-125">In diesem Abschnitt fügen Sie einen Komponententest für die Anwendung in der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) ein dynamisches Objekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="0c610-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="0c610-126">Installieren der [XUnit Runner Erweiterung](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) für Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0c610-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="0c610-127">Entweder ist zum Abschließen der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), oder Herunterladen die fertigen Anwendung von [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="0c610-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="0c610-128">Öffnen Sie bei Verwendung der Downloadversion der Anwendung Einstieg **Package Manager Console** , und klicken Sie auf **wiederherstellen** des SignalR-Pakets zum Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0c610-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Stellen Sie die Pakete wieder her.](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="0c610-130">Fügen Sie ein Projekt der Projektmappe für den Komponententest aus.</span><span class="sxs-lookup"><span data-stu-id="0c610-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="0c610-131">Mit der rechten Maustaste in der Projektmappe in **Projektmappen-Explorer** , und wählen Sie **hinzufügen**, **neues Projekt...** . Klicken Sie unter der **c#** Knoten, wählen die **Windows** Knoten.</span><span class="sxs-lookup"><span data-stu-id="0c610-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="0c610-132">Wählen Sie **-Klassenbibliothek**.</span><span class="sxs-lookup"><span data-stu-id="0c610-132">Select **Class Library**.</span></span> <span data-ttu-id="0c610-133">Nennen Sie das neue Projekt **TestLibrary** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c610-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Erstellen Sie die Bibliothek für das Testen](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="0c610-135">Fügen Sie einen Verweis auf das Projekt SignalRChat im Test-Klassenbibliotheksprojekt.</span><span class="sxs-lookup"><span data-stu-id="0c610-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="0c610-136">Mit der rechten Maustaste die **TestLibrary** Projekt, und wählen Sie **hinzufügen**, **Verweis...** . Wählen Sie die **Projekte** unter Knoten die **Lösung** Knoten, und aktivieren Sie **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="0c610-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="0c610-137">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c610-137">Click **OK**.</span></span>

    ![Fügen Sie Projektverweis hinzu](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="0c610-139">Hinzufügen der Pakete SignalR Moq und XUnit, um die **TestLibrary** Projekt.</span><span class="sxs-lookup"><span data-stu-id="0c610-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="0c610-140">In der **Package Manager Console**legen die **Projekt standardmäßig** Dropdownliste zu **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="0c610-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="0c610-141">Führen Sie die folgenden Befehle im Konsolenfenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="0c610-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Installieren von Paketen](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="0c610-143">Erstellen Sie die Testdatei.</span><span class="sxs-lookup"><span data-stu-id="0c610-143">Create the test file.</span></span> <span data-ttu-id="0c610-144">Mit der rechten Maustaste die **TestLibrary** Projekt, und klicken Sie auf **hinzufügen...** , **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="0c610-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="0c610-145">Benennen Sie die neue Klasse **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="0c610-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="0c610-146">Ersetzen Sie den Inhalt der Tests.cs durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="0c610-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="0c610-147">Im obigen Code wird ein Testclient erstellt, mit der `Mock` -Objekt aus der [Moq](https://github.com/Moq/moq4) Bibliothek des Typs [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (weisen Sie in SignalR-2.1 `dynamic` für den Typ Parameter.) Die `IHubCallerConnectionContext` Schnittstelle ist das Proxyobjekt, mit denen Sie die Methoden auf dem Client aufrufen.</span><span class="sxs-lookup"><span data-stu-id="0c610-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="0c610-148">Die `broadcastMessage` Funktion klicken Sie dann für den simulierten Client definiert, dass sie aufgerufen werden kann, indem Sie die `ChatHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="0c610-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="0c610-149">Das Modul ruft dann die `Send` Methode der `ChatHub` -Klasse, die ihrerseits die mocked `broadcastMessage` Funktion.</span><span class="sxs-lookup"><span data-stu-id="0c610-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="0c610-150">Erstellen Sie die Projektmappe durch Drücken von **F6**.</span><span class="sxs-lookup"><span data-stu-id="0c610-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="0c610-151">Führen Sie den Komponententest aus.</span><span class="sxs-lookup"><span data-stu-id="0c610-151">Run the unit test.</span></span> <span data-ttu-id="0c610-152">Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0c610-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="0c610-153">Im Test-Explorer-Fenster mit der Maustaste **HubsAreMockableViaDynamic** , und wählen Sie **ausgewählte Tests ausführen**.</span><span class="sxs-lookup"><span data-stu-id="0c610-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test-Explorer](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="0c610-155">Stellen Sie sicher, dass der Test erfolgreich war, überprüfen Sie den unteren Bereich im Test-Explorer-Fenster.</span><span class="sxs-lookup"><span data-stu-id="0c610-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="0c610-156">Das Fenster zeigt, dass der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="0c610-156">The window will show that the test passed.</span></span>

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="0c610-158">Komponententests, die nach Typ</span><span class="sxs-lookup"><span data-stu-id="0c610-158">Unit testing by type</span></span>

<span data-ttu-id="0c610-159">In diesem Abschnitt fügen Sie einen Test für die Anwendung erstellt, der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) über eine Schnittstelle, die die Methode, die zu testende enthält.</span><span class="sxs-lookup"><span data-stu-id="0c610-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="0c610-160">Führen Sie die Schritte 1 bis 7 in die [Komponententests mit dynamischen](#dynamic) Lernprogramm oben.</span><span class="sxs-lookup"><span data-stu-id="0c610-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="0c610-161">Ersetzen Sie den Inhalt der Tests.cs durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="0c610-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="0c610-162">Im obigen Code wird eine Schnittstelle erstellt, die Signatur der Definition der `broadcastMessage` Methode für die vom Testmodul ein simulierten Clients erstellen.</span><span class="sxs-lookup"><span data-stu-id="0c610-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="0c610-163">Ein simulierten Client wird dann mit erstellt die `Mock` Objekt des Typs [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (weisen Sie in SignalR-2.1 `dynamic` für den Typparameter.) Die `IHubCallerConnectionContext` Schnittstelle ist das Proxyobjekt, mit denen Sie die Methoden auf dem Client aufrufen.</span><span class="sxs-lookup"><span data-stu-id="0c610-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="0c610-164">Der Test erstellt dann eine Instanz des `ChatHub`, und erstellt dann auf eine Pseudoversion des der `broadcastMessage` -Methode, die wiederum, durch Aufrufen aufgerufen wird der `Send` Methode auf dem Hub.</span><span class="sxs-lookup"><span data-stu-id="0c610-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="0c610-165">Erstellen Sie die Projektmappe durch Drücken von **F6**.</span><span class="sxs-lookup"><span data-stu-id="0c610-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="0c610-166">Führen Sie den Komponententest aus.</span><span class="sxs-lookup"><span data-stu-id="0c610-166">Run the unit test.</span></span> <span data-ttu-id="0c610-167">Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0c610-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="0c610-168">Im Test-Explorer-Fenster mit der Maustaste **HubsAreMockableViaDynamic** , und wählen Sie **ausgewählte Tests ausführen**.</span><span class="sxs-lookup"><span data-stu-id="0c610-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test-Explorer](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="0c610-170">Stellen Sie sicher, dass der Test erfolgreich war, überprüfen Sie den unteren Bereich im Test-Explorer-Fenster.</span><span class="sxs-lookup"><span data-stu-id="0c610-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="0c610-171">Das Fenster zeigt, dass der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="0c610-171">The window will show that the test passed.</span></span>

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image8.png)
