---
uid: signalr/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in SignalR | Microsoft Docs
author: MikeWasson
description: Versionen der Software verwendete in diesem Thema Visual Studio 2013 .NET 4.5 SignalR Version 2-Vorgängerversionen des in diesem Thema Informationen zu früheren Versionen von...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504179"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="41bed-103">Abhängigkeitsinjektion in SignalR</span><span class="sxs-lookup"><span data-stu-id="41bed-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="41bed-104">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="41bed-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="41bed-105">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="41bed-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="41bed-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="41bed-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="41bed-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="41bed-107">.NET 4.5</span></span>
> - <span data-ttu-id="41bed-108">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="41bed-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="41bed-109">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="41bed-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="41bed-110">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="41bed-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="41bed-111">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="41bed-111">Questions and comments</span></span>
> 
> <span data-ttu-id="41bed-112">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="41bed-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="41bed-113">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="41bed-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="41bed-114">Abhängigkeitsinjektion ist eine Möglichkeit zum Entfernen von hartcodierten Abhängigkeiten zwischen Objekten, die leichter auf ein Objekt Abhängigkeiten, entweder für das Testen (unter Verwendung von Pseudoobjekten) zu ersetzen oder Laufzeitverhalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="41bed-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="41bed-115">In diesem Lernprogramm wird gezeigt, wie Abhängigkeitsinjektion für SignalR-Hubs ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="41bed-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="41bed-116">Es wird gezeigt, wie mit SignalR IoC-Container verwenden.</span><span class="sxs-lookup"><span data-stu-id="41bed-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="41bed-117">Ein IoC-Container ist ein allgemeines Framework für zielabhängigkeit.</span><span class="sxs-lookup"><span data-stu-id="41bed-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="41bed-118">Was ist die Abhängigkeitsinjektion?</span><span class="sxs-lookup"><span data-stu-id="41bed-118">What is Dependency Injection?</span></span>

<span data-ttu-id="41bed-119">Überspringen Sie diesen Abschnitt, wenn Sie bereits mit Abhängigkeitsinjektion vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="41bed-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="41bed-120">*Abhängigkeitsinjektion* (DI) ist ein Muster, in denen sind Objekte nicht verantwortlich für das Erstellen ihrer eigenen Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="41bed-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="41bed-121">Hier ist ein einfaches Beispiel zum DI angeregt.</span><span class="sxs-lookup"><span data-stu-id="41bed-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="41bed-122">Nehmen Sie an, dass Sie ein Objekt haben, die benötigt werden, um Nachrichten zu protokollieren.</span><span class="sxs-lookup"><span data-stu-id="41bed-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="41bed-123">Sie können eine protokollierungsschnittstelle definieren:</span><span class="sxs-lookup"><span data-stu-id="41bed-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="41bed-124">In Ihr Objekt erstellen Sie eine `ILogger` um Nachrichten zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="41bed-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="41bed-125">Dies funktioniert, aber es ist nicht zum optimalen Entwurf.</span><span class="sxs-lookup"><span data-stu-id="41bed-125">This works, but it's not the best design.</span></span> <span data-ttu-id="41bed-126">Wenn Sie ersetzen möchten `FileLogger` mit einem anderen `ILogger` Implementierung müssen Sie ändern `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="41bed-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="41bed-127">Kommunale, die Verwendung auf viele andere Objekte `FileLogger`, müssen Sie alle von ihnen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="41bed-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="41bed-128">Oder wenn Sie sich entscheiden, stellen `FileLogger` ein Singleton, Sie müssen auch Änderungen in der gesamten Anwendung vornehmen.</span><span class="sxs-lookup"><span data-stu-id="41bed-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="41bed-129">Ein besserer Ansatz "Einfügen" ist ein `ILogger` in das Objekt – z. B. durch einen Konstruktor mit dem Argument:</span><span class="sxs-lookup"><span data-stu-id="41bed-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="41bed-130">Nachdem das Objekt nicht für die Auswahl der zuständig ist `ILogger` verwenden.</span><span class="sxs-lookup"><span data-stu-id="41bed-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="41bed-131">Sie können Swich `ILogger` Implementierungen ohne Änderung der Objekte, die von ihm abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="41bed-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="41bed-132">Dieses Muster wird aufgerufen, [Konstruktoreinfügung](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="41bed-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="41bed-133">Ein anderes Muster ist Setter-Injection, in dem Sie die Abhängigkeit über einen Setter-Methode oder Eigenschaft festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="41bed-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="41bed-134">Einfache Abhängigkeitsinjektion in SignalR</span><span class="sxs-lookup"><span data-stu-id="41bed-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="41bed-135">Betrachten Sie die Chat-Anwendung aus dem Lernprogramm [erste Schritte mit SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="41bed-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="41bed-136">So sieht die hubklasse über diese Anwendung aus:</span><span class="sxs-lookup"><span data-stu-id="41bed-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="41bed-137">Nehmen Sie an, dass Sie Chat-Nachrichten auf dem Server speichern, vor dem senden möchten.</span><span class="sxs-lookup"><span data-stu-id="41bed-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="41bed-138">Sie definieren eine Schnittstelle, die diese Funktionalität abstrahiert und DI zum Einfügen von der Schnittstelle für verwenden möglicherweise die `ChatHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="41bed-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="41bed-139">Das einzige Problem besteht darin, dass eine Anwendung SignalR Hubs nicht direkt erstellt; SignalR erstellt für Sie.</span><span class="sxs-lookup"><span data-stu-id="41bed-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="41bed-140">Standardmäßig erwartet SignalR eine hubklasse über einen parameterlosen Konstruktor verfügt.</span><span class="sxs-lookup"><span data-stu-id="41bed-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="41bed-141">Allerdings kann problemlos eine Funktion zur Erstellung der hubinstanzen registrieren, und mit dieser Funktion können DI ausführen.</span><span class="sxs-lookup"><span data-stu-id="41bed-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="41bed-142">Registrieren Sie die Funktion durch Aufrufen von **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="41bed-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="41bed-143">Jetzt SignalR dieser anonymen Funktion immer dann wird wird benötigt aufgerufen, um erstellen eine `ChatHub` Instanz.</span><span class="sxs-lookup"><span data-stu-id="41bed-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="41bed-144">IoC-Container</span><span class="sxs-lookup"><span data-stu-id="41bed-144">IoC Containers</span></span>

<span data-ttu-id="41bed-145">Der vorhergehende Code ist in einfachen Fällen gut.</span><span class="sxs-lookup"><span data-stu-id="41bed-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="41bed-146">Aber weiterhin mussten Sie Folgendes schreiben:</span><span class="sxs-lookup"><span data-stu-id="41bed-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="41bed-147">In einer komplexen Anwendung mit vielen Abhängigkeiten müssen Sie möglicherweise ein Großteil dieser "verknüpfen" Code schreiben.</span><span class="sxs-lookup"><span data-stu-id="41bed-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="41bed-148">Dieser Code kann schwierig zu verwalten sein, insbesondere dann, wenn Abhängigkeiten geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="41bed-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="41bed-149">Es ist auch schwer zu Komponententest.</span><span class="sxs-lookup"><span data-stu-id="41bed-149">It is also hard to unit test.</span></span>

<span data-ttu-id="41bed-150">Eine Lösung besteht darin einen IoC-Container zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="41bed-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="41bed-151">Ein IoC-Container ist eine Softwarekomponente, die zum Verwalten von Abhängigkeiten zuständig ist. Registrieren von Typen mit dem Container, und klicken Sie dann den Container verwenden, um Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="41bed-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="41bed-152">Der Container ermittelt automatisch die Abhängigkeit Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="41bed-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="41bed-153">Viele IoC-Container können Sie steuern, z. B. Objektlebensdauer und Bereich.</span><span class="sxs-lookup"><span data-stu-id="41bed-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="41bed-154">"IoC" steht für "Inversion des Steuerelements" Dies ist ein allgemeines Muster, in denen ein Framework Anwendungscode aufruft.</span><span class="sxs-lookup"><span data-stu-id="41bed-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="41bed-155">Ein IoC-Container erstellt die Objekte, die "die übliche ablaufsteuerung kehrt".</span><span class="sxs-lookup"><span data-stu-id="41bed-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="41bed-156">Mithilfe von IoC-Container in SignalR</span><span class="sxs-lookup"><span data-stu-id="41bed-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="41bed-157">Chat-Anwendung ist wahrscheinlich zu einfach einen IoC-Container profitieren.</span><span class="sxs-lookup"><span data-stu-id="41bed-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="41bed-158">Stattdessen sehen wir uns die [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) Beispiel.</span><span class="sxs-lookup"><span data-stu-id="41bed-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="41bed-159">Im Beispiel StockTicker definiert die beiden Hauptklassen:</span><span class="sxs-lookup"><span data-stu-id="41bed-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="41bed-160">`StockTickerHub`: Der hubklasse, die Clientverbindungen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="41bed-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="41bed-161">`StockTicker`: Ein Singleton, der Aktienkurse enthält und in regelmäßigen Abständen aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="41bed-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="41bed-162">`StockTickerHub`enthält einen Verweis auf die `StockTicker` Singleton, während `StockTicker` enthält einen Verweis auf die **IHubConnectionContext** für die `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="41bed-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="41bed-163">Er verwendet diese Schnittstelle für die Kommunikation mit `StockTickerHub` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="41bed-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="41bed-164">(Weitere Informationen finden Sie unter [Broadcast-Server mit ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="41bed-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="41bed-165">Wir können einen IoC-Container verwenden, um diese Abhängigkeiten etwas beurteilt.</span><span class="sxs-lookup"><span data-stu-id="41bed-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="41bed-166">Zunächst sehen wir vereinfachen die `StockTickerHub` und `StockTicker` Klassen.</span><span class="sxs-lookup"><span data-stu-id="41bed-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="41bed-167">Im folgenden Code habe ich die Teile auskommentiert, dass wir nicht brauchen.</span><span class="sxs-lookup"><span data-stu-id="41bed-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="41bed-168">Entfernen des parameterlosen Konstruktors von `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="41bed-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="41bed-169">Es wird stattdessen immer DI verwenden, um den Hub zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="41bed-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="41bed-170">StockTicker entfernen Sie die Singleton-Instanz.</span><span class="sxs-lookup"><span data-stu-id="41bed-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="41bed-171">Später verwenden wir den IoC-Container zur Steuerung der Lebensdauer StockTicker.</span><span class="sxs-lookup"><span data-stu-id="41bed-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="41bed-172">Darüber hinaus öffentlich des Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="41bed-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="41bed-173">Als Nächstes können wir den Code Umgestalten, erstellen Sie eine Schnittstelle für `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="41bed-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="41bed-174">Wir verwenden diese Schnittstelle zum Entkoppeln der `StockTickerHub` aus der `StockTicker` Klasse.</span><span class="sxs-lookup"><span data-stu-id="41bed-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="41bed-175">Visual Studio legt diese Art von Umgestaltung einfach.</span><span class="sxs-lookup"><span data-stu-id="41bed-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="41bed-176">Öffnen Sie die Datei StockTicker.cs der rechten Maustaste auf die `StockTicker` Klassendeklaration, und wählen Sie **Umgestalten** ... **Schnittstelle extrahieren**.</span><span class="sxs-lookup"><span data-stu-id="41bed-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="41bed-177">In der **Schnittstelle extrahieren** Dialogfeld klicken Sie auf **Alles markieren**.</span><span class="sxs-lookup"><span data-stu-id="41bed-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="41bed-178">Lassen Sie die anderen Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="41bed-178">Leave the other defaults.</span></span> <span data-ttu-id="41bed-179">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="41bed-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="41bed-180">Visual Studio erstellt eine neue Schnittstelle namens `IStockTicker`, und ändert sich auch `StockTicker` Ableitung `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="41bed-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="41bed-181">Öffnen Sie die Datei IStockTicker.cs und ändern Sie die Schnittstelle zum **öffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="41bed-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="41bed-182">In der `StockTickerHub` Klasse, ändern Sie die zwei Instanzen des `StockTicker` auf `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="41bed-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="41bed-183">Erstellen einer `IStockTicker` Schnittstelle nicht unbedingt erforderlich ist, aber ich soll demonstriert werden, wie DI helfen kann, um Kopplung zwischen Komponenten in Ihrer Anwendung zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="41bed-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="41bed-184">Fügen Sie der Bibliothek Ninject hinzu</span><span class="sxs-lookup"><span data-stu-id="41bed-184">Add the Ninject Library</span></span>

<span data-ttu-id="41bed-185">Es gibt viele Open Source-IoC-Container für .NET.</span><span class="sxs-lookup"><span data-stu-id="41bed-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="41bed-186">Für dieses Lernprogramm verwende ich [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="41bed-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="41bed-187">(Andere beliebten Bibliotheken enthalten [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), und [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="41bed-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="41bed-188">Verwenden Sie NuGet Package Manager zum Installieren der [Ninject Bibliothek](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="41bed-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="41bed-189">In Visual Studio aus der **Tools** Menü die Option **Bibliothekspaket-Manager** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="41bed-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="41bed-190">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="41bed-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="41bed-191">Ersetzen Sie den Abhängigkeitskonfliktlöser für SignalR</span><span class="sxs-lookup"><span data-stu-id="41bed-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="41bed-192">Um Ninject in SignalR verwenden zu können, erstellen Sie eine Klasse, die abgeleitet **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="41bed-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="41bed-193">Diese Klasse überschreibt die **GetService** und **"GetServices" auf** Methoden der **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="41bed-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="41bed-194">SignalR ruft diese Methoden zum Erstellen von verschiedenen Objekten zur Laufzeit, einschließlich hubinstanzen sowie verschiedene Dienste, die intern vom SignalR verwendet.</span><span class="sxs-lookup"><span data-stu-id="41bed-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="41bed-195">Die **GetService** Methode erstellt eine einzelne Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="41bed-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="41bed-196">Überschreiben Sie diese Methode zum Aufrufen des Ninject Kernels **TryGet** Methode.</span><span class="sxs-lookup"><span data-stu-id="41bed-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="41bed-197">Wenn diese Methode null zurückgibt, ein Fallback auf den Standardkonfliktlöser.</span><span class="sxs-lookup"><span data-stu-id="41bed-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="41bed-198">Die **"GetServices" auf** Methode erstellt eine Auflistung von Objekten eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="41bed-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="41bed-199">Überschreiben Sie diese Methode, um die Ergebnisse von Ninject mit den Ergebnissen aus den Standardkonfliktlöser verketten.</span><span class="sxs-lookup"><span data-stu-id="41bed-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="41bed-200">Ninject Bindungen konfigurieren</span><span class="sxs-lookup"><span data-stu-id="41bed-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="41bed-201">Jetzt müssen wir Ninject verwenden, um die Bindungen des Typs deklarieren.</span><span class="sxs-lookup"><span data-stu-id="41bed-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="41bed-202">Öffnen Sie die Anwendung Startup.cs-Klasse (, dass Sie entweder manuell gemäß den Paket-Anweisungen in erstellt `readme.txt`, oder, Hinzufügen von Authentifizierung zu Ihrem Projekt erstellt wurde).</span><span class="sxs-lookup"><span data-stu-id="41bed-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="41bed-203">In der `Startup.Configuration` -Methode, erstellen Sie den Ninject-Container, der aufgerufen wird, Ninject der *Kernel*.</span><span class="sxs-lookup"><span data-stu-id="41bed-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="41bed-204">Erstellen Sie eine Instanz des unsere benutzerdefinierte Abhängigkeitskonfliktlöser:</span><span class="sxs-lookup"><span data-stu-id="41bed-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="41bed-205">Erstellen Sie eine Bindung für `IStockTicker` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="41bed-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="41bed-206">Dieser Code ist zwei Dinge darauf hingewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="41bed-206">This code is saying two things.</span></span> <span data-ttu-id="41bed-207">Erste, wenn die Anwendung muss eine `IStockTicker`, Kernel sollten erstellen Sie eine Instanz des `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="41bed-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="41bed-208">Zweitens stellt die `StockTicker` sollten Klasse als Singleton-Objekt erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="41bed-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="41bed-209">Ninject erstellt eine Instanz des Objekts, und die gleiche Instanz für jede Anforderung zurück.</span><span class="sxs-lookup"><span data-stu-id="41bed-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="41bed-210">Erstellen Sie eine Bindung für **IHubConnectionContext** wie folgt:</span><span class="sxs-lookup"><span data-stu-id="41bed-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="41bed-211">Dieser Code Creatres einer anonymen Funktion, die gibt eine **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="41bed-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="41bed-212">Die **WhenInjectedInto** Methode teilt Ninject diese Funktion verwenden, nur beim Erstellen `IStockTicker` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="41bed-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="41bed-213">Der Grund ist, dass SignalR erstellt **IHubConnectionContext** Instanzen intern, und wir möchten nicht überschreiben, wie Sie SignalR erstellt.</span><span class="sxs-lookup"><span data-stu-id="41bed-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="41bed-214">Diese Funktion gilt nur für unsere `StockTicker` Klasse.</span><span class="sxs-lookup"><span data-stu-id="41bed-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="41bed-215">Übergeben Sie den Abhängigkeitskonfliktlöser in der **MapSignalR** Methode durch Hinzufügen einer Hub-Konfigurations:</span><span class="sxs-lookup"><span data-stu-id="41bed-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="41bed-216">Aktualisieren Sie die Startup.ConfigureSignalR-Methode in der Beispiel-Startklasse, mit der neue Parameter:</span><span class="sxs-lookup"><span data-stu-id="41bed-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="41bed-217">Nun SignalR verwendet die im angegebenen Konfliktlösers **MapSignalR**, anstatt des Standardkonfliktlösers.</span><span class="sxs-lookup"><span data-stu-id="41bed-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="41bed-218">Hier wird die vollständige codeauflistung für `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="41bed-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="41bed-219">Drücken Sie F5, um die StockTicker-Anwendung in Visual Studio auszuführen.</span><span class="sxs-lookup"><span data-stu-id="41bed-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="41bed-220">Wechseln Sie in das Browserfenster zu `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="41bed-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="41bed-221">Die Anwendung muss genau die gleiche Funktionalität wie vor.</span><span class="sxs-lookup"><span data-stu-id="41bed-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="41bed-222">(Eine Beschreibung finden Sie unter [Broadcast-Server mit ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Wir noch nicht das Verhalten geändert werden. nur vereinfacht den Code testen, verwalten und entwickeln.</span><span class="sxs-lookup"><span data-stu-id="41bed-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
