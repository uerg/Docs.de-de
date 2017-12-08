---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4-Abhängigkeitsinjektion | Microsoft Docs"
author: rick-anderson
description: "Hinweis: Diese praktische Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse der ASP.NET MVC und ASP.NET MVC 4-Filter verfügen. Wenn Sie nicht ASP.NET MVC 4-Filter, bevor wir Rec verwendet haben..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="09cc4-104">ASP.NET MVC 4-Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="09cc4-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="09cc4-105">durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="09cc4-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="09cc4-106">Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC** und **ASP.NET MVC 4 filtert**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="09cc4-107">Wenn Sie nicht verwendet haben **ASP.NET MVC 4 filtert** vorher, empfehlen wir Ihnen, durchlaufen **Aktionsfilter in ASP.NET MVC benutzerdefinierte** praktische Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="09cc4-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="09cc4-108">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="09cc4-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="09cc4-109">In **Objekt Oriented Programming** Paradigma Objekten arbeiten zusammen in einem Modell Zusammenarbeit "Mitwirkende" und Consumer vorliegen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="09cc4-110">Natürlich generiert diese Kommunikationsmodell Abhängigkeiten zwischen Objekten und Komponenten, die schwer zu verwalten, wenn die Komplexität erhöht wird.</span><span class="sxs-lookup"><span data-stu-id="09cc4-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="09cc4-111">![Klasse von Abhängigkeiten und Containerklasse Komplexität](aspnet-mvc-4-dependency-injection/_static/image1.png "-Klasse Abhängigkeiten und Containerklasse Komplexität")</span><span class="sxs-lookup"><span data-stu-id="09cc4-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="09cc4-112">*Klasse Abhängigkeiten und Komplexität des Modells*</span><span class="sxs-lookup"><span data-stu-id="09cc4-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="09cc4-113">Sie haben wahrscheinlich schon die **Factorymuster** und die Trennung zwischen der Benutzeroberfläche und die Implementierung mit Diensten, bei dem die Clientobjekte häufig verantwortlich für die dienstidentifizierung.</span><span class="sxs-lookup"><span data-stu-id="09cc4-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="09cc4-114">Die Abhängigkeitsinjektion Muster ist eine bestimmte Implementierung des Inversion of Control.</span><span class="sxs-lookup"><span data-stu-id="09cc4-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="09cc4-115">**Die Umkehrung des Steuerelements (IoC)** bedeutet, dass Objekte nicht andere Objekte auf dem sie basieren erstellen, um ihre Arbeit zu erledigen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="09cc4-116">Stattdessen erhalten sie die Objekte, die sie aus einer externen Quelle (z. B. eine XML-Konfigurationsdatei) benötigen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="09cc4-117">**(Dependency Injection, DI)** bedeutet, dass dies geschieht ohne Eingreifen Objekt in der Regel durch eine .NET Framework-Komponente, die Konstruktorparameter übergibt und Eigenschaften festlegen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="09cc4-118">Das Entwurfsmuster der Dependency Injection (DI)</span><span class="sxs-lookup"><span data-stu-id="09cc4-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="09cc4-119">Auf hoher Ebene, das Ziel der Abhängigkeitsinjektion besteht darin, die eine Clientklasse (z. B. *der dsmessages*) benötigt, das eine Schnittstelle erfüllt (z. B. *IClub*).</span><span class="sxs-lookup"><span data-stu-id="09cc4-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="09cc4-120">Es ist nicht wichtig, was der konkrete Typ ist (z. B. *WoodClub IronClub, WedgeClub* oder *PutterClub*), Personen, die verarbeitet werden sollen (z. B. eine gute *Caddy*).</span><span class="sxs-lookup"><span data-stu-id="09cc4-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="09cc4-121">Der Abhängigkeitskonfliktlöser in ASP.NET MVC können Sie Ihre Abhängigkeit-Logik, die an anderer Stelle zu registrieren (z. B. einen Container oder ein *Behälter Kreuz*).</span><span class="sxs-lookup"><span data-stu-id="09cc4-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="09cc4-122">![Dependency Injection Diagramm](aspnet-mvc-4-dependency-injection/_static/image2.png "Abhängigkeitsinjektion-Abbildung")</span><span class="sxs-lookup"><span data-stu-id="09cc4-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="09cc4-123">*Abhängigkeitsinjektion - Golf Analogie*</span><span class="sxs-lookup"><span data-stu-id="09cc4-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="09cc4-124">Vorteile der Verwendung der Abhängigkeitseinfügung Muster und Inversion of Control sind folgende:</span><span class="sxs-lookup"><span data-stu-id="09cc4-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="09cc4-125">Reduziert die Klassenkopplungen</span><span class="sxs-lookup"><span data-stu-id="09cc4-125">Reduces class coupling</span></span>
- <span data-ttu-id="09cc4-126">Erhöht die Wiederverwendung von code</span><span class="sxs-lookup"><span data-stu-id="09cc4-126">Increases code reusing</span></span>
- <span data-ttu-id="09cc4-127">Verbessert die Verwaltbarkeit von code</span><span class="sxs-lookup"><span data-stu-id="09cc4-127">Improves code maintainability</span></span>
- <span data-ttu-id="09cc4-128">Verbessert die Anwendung testen</span><span class="sxs-lookup"><span data-stu-id="09cc4-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="09cc4-129">Abhängigkeitsinjektion wird manchmal mit abstrakten Factoryentwurfsmuster verglichen, aber ein geringfügigen Unterschied zwischen beiden Ansätzen vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="09cc4-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="09cc4-130">DI hat ein Framework hinter arbeiten, Abhängigkeiten, die durch Aufrufen von den Factorys und die registrierten Dienste behoben.</span><span class="sxs-lookup"><span data-stu-id="09cc4-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="09cc4-131">Nun, dass Sie das Dependency Injection Muster verstanden haben, lernen während dieser Übung für die anzuwendende in ASP.NET MVC 4 Sie.</span><span class="sxs-lookup"><span data-stu-id="09cc4-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="09cc4-132">Starten Sie mithilfe der Abhängigkeitsinjektion in der **Controller** eine Datenbank-Access-Dienst umfassen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="09cc4-133">Als Nächstes Sie Abhängigkeitsinjektion zum Anwenden der **Ansichten** , nutzen einen Dienst und Anzeigen von Informationen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="09cc4-134">Zum Schluss erweitern die DI zu ASP.NET MVC 4-Filter, Sie Räumen eines benutzerdefinierten Aktionsfilters in der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="09cc4-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="09cc4-135">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="09cc4-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="09cc4-136">Integrieren von ASP.NET MVC 4 mit Unity für Zielabhängigkeit mithilfe von NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="09cc4-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="09cc4-137">Verwenden Sie die Abhängigkeitsinjektion in ASP.NET MVC-Controllers</span><span class="sxs-lookup"><span data-stu-id="09cc4-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="09cc4-138">Verwenden Sie die Abhängigkeitsinjektion in ASP.NET MVC-Ansicht</span><span class="sxs-lookup"><span data-stu-id="09cc4-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="09cc4-139">Verwenden Sie die Abhängigkeitsinjektion innerhalb einer ASP.NET MVC-Action-Filter</span><span class="sxs-lookup"><span data-stu-id="09cc4-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="09cc4-140">Dieser Übung verwendeten Unity.Mvc3 NuGet-Paket für die Auflösung der Abhängigkeit, aber es ist möglich, einem Dependency Injection-Framework zum Arbeiten mit ASP.NET MVC 4 angepasst werden kann.</span><span class="sxs-lookup"><span data-stu-id="09cc4-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="09cc4-141">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="09cc4-141">Prerequisites</span></span>

<span data-ttu-id="09cc4-142">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="09cc4-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="09cc4-143">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="09cc4-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="09cc4-144">Setup</span><span class="sxs-lookup"><span data-stu-id="09cc4-144">Setup</span></span>

<span data-ttu-id="09cc4-145">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="09cc4-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="09cc4-146">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="09cc4-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="09cc4-147">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="09cc4-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="09cc4-148">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang B: mithilfe von Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="09cc4-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="09cc4-149">Übungen</span><span class="sxs-lookup"><span data-stu-id="09cc4-149">Exercises</span></span>

<span data-ttu-id="09cc4-150">Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:</span><span class="sxs-lookup"><span data-stu-id="09cc4-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="09cc4-151">Übung 1: Räumen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="09cc4-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="09cc4-152">Übung 2: Räumen eine Sicht</span><span class="sxs-lookup"><span data-stu-id="09cc4-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="09cc4-153">Übung 3: Räumen Filter</span><span class="sxs-lookup"><span data-stu-id="09cc4-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="09cc4-154">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="09cc4-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="09cc4-155">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="09cc4-156">Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="09cc4-157">Übung 1: Räumen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="09cc4-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="09cc4-158">In dieser Übung erfahren Sie, wie Sie die Abhängigkeitsinjektion in ASP.NET MVC-Controller verwenden, durch die Integration von Unity unter Verwendung eines NuGet-Pakets.</span><span class="sxs-lookup"><span data-stu-id="09cc4-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="09cc4-159">Aus diesem Grund schließt Sie Dienste in Ihre MvcMusicStore-Domänencontroller, um die Logik von der Datenzugriff zu trennen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="09cc4-160">Die Dienste erstellt eine neue Abhängigkeit im Konstruktor Controller, die mithilfe der Abhängigkeitsinjektion mit Hilfe der aufgelöst werden **Unity**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="09cc4-161">Dieser Ansatz wird gezeigt, wie zum Generieren von weniger verbundene Anwendungen sind flexibler und einfacher zu verwalten und zu testen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="09cc4-162">Außerdem erfahren Sie, wie zum Integrieren von ASP.NET MVC mit Unity wird.</span><span class="sxs-lookup"><span data-stu-id="09cc4-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="09cc4-163">Informationen zum StoreManager-Dienst</span><span class="sxs-lookup"><span data-stu-id="09cc4-163">About StoreManager Service</span></span>

<span data-ttu-id="09cc4-164">Das MVC-Music Store jetzt in der Begin-Projektmappe bereitgestellt enthält einen Dienst, der mit dem Namen Store Controller Daten verwaltet **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="09cc4-165">Im folgenden finden Sie die Store-Dienst-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="09cc4-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="09cc4-166">Beachten Sie, dass alle Methoden modellentitäten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="09cc4-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="09cc4-167">**StoreController** von der Begin Projektmappe jetzt nutzt **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="09cc4-168">Wurden alle Datenverweise daraus **StoreController**, jetzt möglich, zum Ändern des aktuellen datenzugriffsanbieters ohne jede Methode, die verbraucht und **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="09cc4-169">Finden Sie nachfolgend die der **StoreController** Implementierung hat eine Abhängigkeit mit **StoreService** im Klassenkonstruktor.</span><span class="sxs-lookup"><span data-stu-id="09cc4-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="09cc4-170">Die Abhängigkeit, die in dieser Übung eingeführt bezieht sich auf **Inversion of Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="09cc4-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="09cc4-171">Die **StoreController** Klassenkonstruktor empfängt eine **IStoreService** Type-Parameter, die zum Ausführen der Dienstaufrufe vom innerhalb der Klasse unbedingt erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="09cc4-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="09cc4-172">Allerdings **StoreController** implementiert nicht die Standardkonstruktor (ohne Parameter), die jeden Controller zum Arbeiten mit ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09cc4-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="09cc4-173">Um die Abhängigkeit zu beheben, muss der Controller erstellt werden, indem eine abstrakte Factory (eine Klasse, die ein Objekt des angegebenen Typs zurückgibt).</span><span class="sxs-lookup"><span data-stu-id="09cc4-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="09cc4-174">Sie erhalten einen Fehler, wenn die Klasse versucht, den StoreController zu erstellen, ohne das Dienstobjekt senden, wie es keinen parameterloser Konstruktor deklariert gibt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="09cc4-175">Aufgabe 1: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="09cc4-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="09cc4-176">In dieser Aufgabe führen Sie die Anwendung beginnen, die den Dienst in den Speicher-Controller enthält, die den Datenzugriff von der Anwendungslogik trennt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="09cc4-177">Wenn die Anwendung ausgeführt wird, erhalten Sie eine Ausnahme, wie der Controller-Dienst wird standardmäßig nicht als Parameter übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="09cc4-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="09cc4-178">Öffnen der **beginnen** Projektmappe befindet sich im **Source\Ex01 Räumen Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="09cc4-179">Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="09cc4-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09cc4-180">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="09cc4-181">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="09cc4-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="09cc4-182">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="09cc4-183">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="09cc4-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09cc4-184">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="09cc4-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09cc4-185">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="09cc4-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="09cc4-186">Drücken Sie **STRG + F5** um die Anwendung ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="09cc4-187">Sie erhalten die Fehlermeldung &quot; **keinen parameterlosen Konstruktor für dieses Objekt definierten**&quot;:</span><span class="sxs-lookup"><span data-stu-id="09cc4-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="09cc4-188">![Fehler beim Ausführen von Begin ASP.NET MVC-Anwendung](aspnet-mvc-4-dependency-injection/_static/image3.png "Fehler während der Ausführung beginnen ASP.NET MVC-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="09cc4-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="09cc4-189">*Fehler beim Ausführen von Begin ASP.NET MVC-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="09cc4-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="09cc4-190">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="09cc4-190">Close the browser.</span></span>

<span data-ttu-id="09cc4-191">Arbeiten Sie in den folgenden Schritten auf Music Store-Lösung, die Abhängigkeit einzufügen, die diesem Controller benötigt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="09cc4-192">Aufgabe 2 – einschließlich Unity in MvcMusicStore-Lösung</span><span class="sxs-lookup"><span data-stu-id="09cc4-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="09cc4-193">In dieser Aufgabe schließt **Unity.Mvc3** NuGet-Paket, um die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="09cc4-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="09cc4-194">ASP.NET MVC 3 Unity.Mvc3 Paket entwickelt wurde, jedoch ist vollständig kompatibel mit ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="09cc4-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="09cc4-195">Unity einen einfachen und erweiterbaren abhängigkeitseinschleusungscontainer mit optionale Unterstützung für die Instanz ist, und geben abfangen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="09cc4-196">Es ist ein allzweckcontainer für die Verwendung in jede Art von Anwendung in .NET.</span><span class="sxs-lookup"><span data-stu-id="09cc4-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="09cc4-197">Es enthält alle gemeinsamen Funktionen, die Dependency Injection Mechanismen, einschließlich gefunden: Erstellen von Objekten, die Abstraktion von Anforderungen durch Angeben von Abhängigkeiten zur Laufzeit und die Flexibilität, indem Sie das Verzögern der Konfigurations der Komponente auf den Container.</span><span class="sxs-lookup"><span data-stu-id="09cc4-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="09cc4-198">Installieren Sie **Unity.Mvc3** NuGet-Paket in der **MvcMusicStore** Projekt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="09cc4-199">Öffnen Sie hierzu die **Package Manager Console** aus **Ansicht** | **Weitere Fenster**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="09cc4-200">Führen Sie den folgenden Befehl ein.</span><span class="sxs-lookup"><span data-stu-id="09cc4-200">Run the following command.</span></span>

    <span data-ttu-id="09cc4-201">PMC</span><span class="sxs-lookup"><span data-stu-id="09cc4-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="09cc4-202">![Installieren von NuGet-Paket Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet-Paket wird installiert.")</span><span class="sxs-lookup"><span data-stu-id="09cc4-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="09cc4-203">*Unity.Mvc3 NuGet-Paket wird installiert.*</span><span class="sxs-lookup"><span data-stu-id="09cc4-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="09cc4-204">Sobald die **Unity.Mvc3** Paket installiert ist, untersuchen Sie die Dateien und Ordner, um die Unity-Konfiguration vereinfachen und somit standardmäßig hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="09cc4-205">![Unity.Mvc3-Paket installiert](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3-Paket installiert")</span><span class="sxs-lookup"><span data-stu-id="09cc4-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="09cc4-206">*Unity.Mvc3-Paket installiert*</span><span class="sxs-lookup"><span data-stu-id="09cc4-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="09cc4-207">Aufgabe 3: Registrieren von Unity im Global.asax.cs Anwendung\_starten</span><span class="sxs-lookup"><span data-stu-id="09cc4-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="09cc4-208">In dieser Aufgabe aktualisieren Sie die **Anwendung\_starten** Methode befindet sich im **Global.asax.cs** den Unity-Bootstrapper-Initialisierer aufrufen und dann aktualisieren die Bootstrapper-Datei registrieren der Dienst und der Controller, die Sie für die Abhängigkeitsinjektion verwenden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="09cc4-209">Sie werden nun der Bootstrapper also die Datei, die den Unity-Container initialisiert und Abhängigkeitskonfliktlöser verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="09cc4-210">Öffnen Sie hierzu **Global.asax.cs** und fügen Sie folgenden hervorgehobenen Code innerhalb der **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="09cc4-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="09cc4-211">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - initialisieren Unity*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="09cc4-212">Open **Bootstrapper.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="09cc4-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="09cc4-213">Fügen Sie die folgenden Namespaces hinzu: **MvcMusicStore.Services** und **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="09cc4-214">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="09cc4-215">Ersetzen Sie **BuildUnityContainer** -Methode den Inhalt durch den folgenden Code, der Speicher-Controller und Store-Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="09cc4-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="09cc4-216">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller und der Dienst*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="09cc4-217">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="09cc4-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="09cc4-218">In dieser Aufgabe führen Sie die Anwendung aus, um sicherzustellen, dass sie nun nach dem einschließen von Unity geladen werden können.</span><span class="sxs-lookup"><span data-stu-id="09cc4-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="09cc4-219">Drücken Sie **F5** zum Ausführen der Anwendung die Anwendung sollte jetzt nicht laden und Fehlermeldungen nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="09cc4-220">![Ausführen der Anwendung mit Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Abhängigkeitsinjektion mit Anwendung")</span><span class="sxs-lookup"><span data-stu-id="09cc4-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="09cc4-221">*Ausführen der Anwendung mit Dependency Injection*</span><span class="sxs-lookup"><span data-stu-id="09cc4-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="09cc4-222">Navigieren Sie zu **/speichern**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-222">Browse to **/Store**.</span></span> <span data-ttu-id="09cc4-223">Dies wird aufgerufen, **StoreController**, er wird nun erstellt, mit **Unity**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="09cc4-224">![MVC-Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="09cc4-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="09cc4-225">*MVC-Music Store*</span><span class="sxs-lookup"><span data-stu-id="09cc4-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="09cc4-226">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="09cc4-226">Close the browser.</span></span>

<span data-ttu-id="09cc4-227">In der folgenden Übung erfahren Sie, wie die Abhängigkeitsinjektion Bereich für die Verwendung in ASP.NET MVC-Ansichten und Aktionsfilter zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="09cc4-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="09cc4-228">Übung 2: Räumen eine Sicht</span><span class="sxs-lookup"><span data-stu-id="09cc4-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="09cc4-229">In dieser Übung erfahren Sie, wie die Abhängigkeitsinjektion in einer Sicht mit der neuen Funktionen von ASP.NET MVC 4 für die Unity-Integration verwenden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="09cc4-230">Zu diesem Zweck rufen Sie einen benutzerdefinierten Dienst in den Speicher durchsuchen Ansicht, die eine Nachricht und eine der folgenden Abbildung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="09cc4-231">Klicken Sie dann, Sie Unity das Projekt integriert und erstellen eine benutzerdefinierte Abhängigkeitskonfliktlöser zum Einfügen von Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="09cc4-232">Aufgabe 1: Erstellen einer Ansicht, die einen Dienst nutzt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="09cc4-233">In dieser Aufgabe erstellen Sie eine Sicht, die einem Webdienstaufruf auf eine neue Abhängigkeit generieren ausführt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="09cc4-234">Der Dienst besteht in einem einfachen messaging-Dienst in dieser Lösung enthalten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="09cc4-235">Öffnen der **beginnen** Projektmappe befindet sich in der **Source\Ex02 Räumen View\Begin** Ordner.</span><span class="sxs-lookup"><span data-stu-id="09cc4-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="09cc4-236">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="09cc4-237">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="09cc4-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09cc4-238">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="09cc4-239">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="09cc4-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="09cc4-240">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="09cc4-241">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="09cc4-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09cc4-242">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="09cc4-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09cc4-243">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="09cc4-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="09cc4-244">Weitere Informationen finden Sie im Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="09cc4-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="09cc4-245">Enthalten die **MessageService.cs** und die **IMessageService.cs** Klassen befinden sich der **Source \Assets** Ordner im **/Dienste**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="09cc4-246">Zu diesem Zweck Maustaste **Services** Ordner, und wählen **vorhandenes Element hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="09cc4-247">Navigieren Sie zum Speicherort der Dateien und enthalten Sie sind.</span><span class="sxs-lookup"><span data-stu-id="09cc4-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="09cc4-248">![Hinzufügen von Nachrichtendienst und Dienstschnittstelle](aspnet-mvc-4-dependency-injection/_static/image8.png "Message Service und Dienstschnittstelle hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="09cc4-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="09cc4-249">*Hinzufügen von Nachrichtendienst und Dienstschnittstelle*</span><span class="sxs-lookup"><span data-stu-id="09cc4-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="09cc4-250">Die **IMessageService** Schnittstelle definiert zwei Eigenschaften, die implementiert werden, indem Sie die **MessageService** Klasse.</span><span class="sxs-lookup"><span data-stu-id="09cc4-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="09cc4-251">Diese Eigenschaften -**Nachricht** und **ImageUrl**-speichern Sie die Nachricht und die URL des Bilds angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="09cc4-252">Erstellen Sie den Ordner **/Seiten** des Projekts root-Ordner, und fügen Sie dann auf die vorhandene Klasse **MyBasePage.cs** aus **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="09cc4-253">Die Basisseite, der Sie erbt, hat die folgende Struktur.</span><span class="sxs-lookup"><span data-stu-id="09cc4-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="09cc4-254">![Ordner Seiten](aspnet-mvc-4-dependency-injection/_static/image9.png "Ordner Seiten")</span><span class="sxs-lookup"><span data-stu-id="09cc4-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="09cc4-255">Open **Browse.cshtml** Anzeigen von **/Ansichten/Store** Ordner, und stellen sie die von erben **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="09cc4-256">In der **Durchsuchen** anzuzeigen, fügen Sie einen Aufruf von **MessageService** anzuzeigenden ein Bild und eine Nachricht vom Dienst abgerufen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="09cc4-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="09cc4-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="09cc4-258">Aufgabe 2: z. B. eine benutzerdefinierte Abhängigkeitskonfliktlöser und eine benutzerdefinierte Ansichtsseite</span><span class="sxs-lookup"><span data-stu-id="09cc4-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="09cc4-259">In der vorherigen Aufgabe eingefügten Sie eine neue Abhängigkeit innerhalb einer Ansicht ein Dienstaufrufs darin ausführen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="09cc4-260">Sie werden nun diese Abhängigkeit beheben, indem die Abhängigkeitsinjektion für ASP.NET MVC-Schnittstellen implementieren **IViewPageActivator** und **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="09cc4-261">Sie enthält in der Projektmappe eine Implementierung von **IDependencyResolver** , die befasst sich mit den Abruf des Diensts mithilfe von Unity.</span><span class="sxs-lookup"><span data-stu-id="09cc4-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="09cc4-262">Anschließend schließt eine andere benutzerdefinierte Implementierung der **IViewPageActivator** -Schnittstelle, die durch die Erstellung der Sichten behoben werden kann.</span><span class="sxs-lookup"><span data-stu-id="09cc4-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="09cc4-263">Seit ASP.NET MVC 3 mussten die Implementierung für Zielabhängigkeit die Schnittstellen zum Registrieren von Diensten vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="09cc4-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="09cc4-264">**IDependencyResolver** und **IViewPageActivator** sind Teil von ASP.NET MVC 3-Funktionen für Zielabhängigkeit.</span><span class="sxs-lookup"><span data-stu-id="09cc4-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="09cc4-265">**-IDependencyResolver** Schnittstelle ersetzt den vorherigen IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="09cc4-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="09cc4-266">Implementierer der IDependencyResolver müssen eine Instanz des Diensts oder einer Dienst-Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="09cc4-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="09cc4-267">**-IViewPageActivator** Schnittstelle bietet eine präzisere Steuerung wie Ansichtsseiten über Abhängigkeitsinjektion instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="09cc4-268">Die Klassen, implementieren **IViewPageActivator** Schnittstelle kann mithilfe von Kontextinformationen zur Ansicht-Instanzen erstellen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="09cc4-269">Erstellen Sie die /**Factorys** -Ordner im Stammordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="09cc4-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="09cc4-270">Umfassen **CustomViewPageActivator.cs** der Projektmappe aus **/Datenquellen/Bestand/** auf **Factorys** Ordner.</span><span class="sxs-lookup"><span data-stu-id="09cc4-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="09cc4-271">Zu diesem Zweck Maustaste die **/Factories** Ordner wählen **hinzufügen | Vorhandenes Element** und wählen Sie dann **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="09cc4-272">Diese Klasse implementiert die **IViewPageActivator** Schnittstelle, um die Unity-Container enthalten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="09cc4-273">**CustomViewPageActivator** ist verantwortlich für die Verwaltung der Erstellung einer Sicht mit einem Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="09cc4-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="09cc4-274">Umfassen **UnityDependencyResolver.cs** aus der Datei **/Quellen/Bestand** auf **/Factories** Ordner.</span><span class="sxs-lookup"><span data-stu-id="09cc4-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="09cc4-275">Zu diesem Zweck Maustaste die **/Factories** Ordner wählen **hinzufügen | Vorhandenes Element** und wählen Sie dann **UnityDependencyResolver.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="09cc4-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="09cc4-276">**UnityDependencyResolver** Klasse ist eine benutzerdefinierte DependencyResolver für Unity.</span><span class="sxs-lookup"><span data-stu-id="09cc4-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="09cc4-277">Wenn ein Dienst im Unity-Container gefunden werden kann, ist die Basis-Resolver invocated.</span><span class="sxs-lookup"><span data-stu-id="09cc4-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="09cc4-278">In der folgenden Aufgabe werden beide Implementierungen des Modells kennen den Speicherort der Dienste und die Ansichten können registriert werden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="09cc4-279">Aufgabe 3 - Registrierung für die Abhängigkeitsinjektion in Unity-Container</span><span class="sxs-lookup"><span data-stu-id="09cc4-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="09cc4-280">In dieser Aufgabe fügen Sie die vorherigen Schritte zusammen, um die Abhängigkeitsinjektion arbeiten stellen ein.</span><span class="sxs-lookup"><span data-stu-id="09cc4-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="09cc4-281">Bis jetzt zu hat Ihre Lösung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="09cc4-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="09cc4-282">Ein **Durchsuchen** Ansicht, erbt **MyBaseClass** und nutzt **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="09cc4-283">Eine Zwischenklasse -**MyBaseClass**-, die Abhängigkeitsinjektion für die Dienstschnittstelle deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="09cc4-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="09cc4-284">Eine Dienst - **MessageService** - und seine Schnittstelle **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="09cc4-285">Eine benutzerdefinierte Abhängigkeitskonfliktlöser für Unity - **UnityDependencyResolver** -, die befasst sich mit den Dienst abrufen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="09cc4-286">Eine Ansichtsseite Activator - **CustomViewPageActivator** -Seite erstellt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="09cc4-287">Zum Einfügen von **Durchsuchen** Ansicht jetzt registrieren Sie den benutzerdefinierten Abhängigkeitskonfliktlöser im Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="09cc4-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="09cc4-288">Open **Bootstrapper.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="09cc4-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="09cc4-289">Registrieren einer Instanz von **MessageService** in den Unity-Container, um den Dienst zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="09cc4-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="09cc4-290">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="09cc4-291">Hinzufügen eines Verweises auf **MvcMusicStore.Factories** Namespace.</span><span class="sxs-lookup"><span data-stu-id="09cc4-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="09cc4-292">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Factorys Namespace*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="09cc4-293">Registrieren Sie **CustomViewPageActivator** als eine Ansichtsseite Activator in den Unity-Container:</span><span class="sxs-lookup"><span data-stu-id="09cc4-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="09cc4-294">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="09cc4-295">Ersetzen Sie ASP.NET MVC 4-Standard-Abhängigkeitskonfliktlöser mit einer Instanz von **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="09cc4-296">Ersetzen Sie hierzu **Initialise** Methode Inhalt durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="09cc4-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="09cc4-297">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Update-Abhängigkeitskonfliktlöser*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="09cc4-298">ASP.NET MVC bietet standardmäßig Dependency Resolver-Klasse.</span><span class="sxs-lookup"><span data-stu-id="09cc4-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="09cc4-299">Um mit benutzerdefinierten Abhängigkeitskonfliktlöser mit dem arbeiten, die wir für Unity erstellt haben, muss dieser Konfliktlöser ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="09cc4-300">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="09cc4-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="09cc4-301">In dieser Aufgabe führen Sie die Anwendung überprüfen, ob der Browser Store nutzt den Dienst und zeigt das Bild und die Nachricht abgerufen:</span><span class="sxs-lookup"><span data-stu-id="09cc4-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="09cc4-302">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="09cc4-303">Klicken Sie auf **Rock** innerhalb der Genres-Menü und finden Sie unter wie die **MessageService** wurde in der Ansicht injiziert und die Begrüßungsnachricht und das Bild geladen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="09cc4-304">In diesem Beispiel werden wir eingeben &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="09cc4-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="09cc4-305">![MVC-Music Store - Ansicht Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - Ansicht-Injection")</span><span class="sxs-lookup"><span data-stu-id="09cc4-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="09cc4-306">*MVC-Music Store - Ansicht-Injection*</span><span class="sxs-lookup"><span data-stu-id="09cc4-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="09cc4-307">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="09cc4-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="09cc4-308">Übung 3: Räumen Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="09cc4-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="09cc4-309">In der vorherigen praktische Übungseinheit **benutzerdefinierte Aktionsfilter** Sie die Filter-Anpassung und Injection gearbeitet haben.</span><span class="sxs-lookup"><span data-stu-id="09cc4-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="09cc4-310">In dieser Übung erfahren Sie, wie Sie Filter mit Abhängigkeitsinjektion einfügen, mit der Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="09cc4-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="09cc4-311">Zu diesem Zweck fügen Sie der Projektmappe Music Store eines benutzerdefinierten Aktionsfilters hinzu, das die Aktivität des Standorts zurückverfolgen wird.</span><span class="sxs-lookup"><span data-stu-id="09cc4-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="09cc4-312">Aufgabe 1 – z. B. den Tracking-Filter in der Projektmappe</span><span class="sxs-lookup"><span data-stu-id="09cc4-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="09cc4-313">In dieser Aufgabe werden Sie in der Music Store ein benutzerdefinierten Aktionsfilters für Ablaufverfolgungsereignisse eingefügt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="09cc4-314">Als benutzerdefinierte Aktionsfilter werden Konzepte bereits in der vorherigen Lektion behandelt &quot;benutzerdefinierte Aktionsfilter&quot;, Sie schließen Sie einfach die Filter-Klasse aus dem Ordner "Assets" in dieser Umgebung, und klicken Sie dann einen Filteranbieter für Unity zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="09cc4-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="09cc4-315">Öffnen der **beginnen** Projektmappe befindet sich in der **Source\Ex03 - Räumen Aktion Filter\Begin** Ordner.</span><span class="sxs-lookup"><span data-stu-id="09cc4-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="09cc4-316">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="09cc4-317">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="09cc4-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="09cc4-318">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="09cc4-319">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="09cc4-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="09cc4-320">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="09cc4-321">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="09cc4-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="09cc4-322">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="09cc4-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="09cc4-323">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="09cc4-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="09cc4-324">Weitere Informationen finden Sie im Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="09cc4-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="09cc4-325">Umfassen **TraceActionFilter.cs** aus der Datei **/Quellen/Bestand** auf **/filtert** Ordner.</span><span class="sxs-lookup"><span data-stu-id="09cc4-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="09cc4-326">Diese benutzerdefinierten Aktionsfilters führt ASP.NET-Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="09cc4-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="09cc4-327">Sehen Sie sich &quot;ASP.NET MVC 4-lokale und dynamische Aktionsfilter&quot; Übung zur weiteren Referenz.</span><span class="sxs-lookup"><span data-stu-id="09cc4-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="09cc4-328">Fügen Sie die leere Klasse **FilterProvider.cs** zum Projekt im Ordner ""   **/filtert.**</span><span class="sxs-lookup"><span data-stu-id="09cc4-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="09cc4-329">Hinzufügen der **System.Web.Mvc** und **Microsoft.Practices.Unity** Namespaces in **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="09cc4-330">(Codeausschnitt - *Dependency Injection Lab - Ex03 - Filter ASP.NET-Anbieter hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="09cc4-331">Legen Sie die Klasse, die von erben **IFilterProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="09cc4-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="09cc4-332">Hinzufügen einer **IUnityContainer** Eigenschaft in der **FilterProvider** Klasse, und erstellen Sie einen Klassenkonstruktor, um den Container zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="09cc4-333">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Filter-Anbieterkonstruktor*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="09cc4-334">Klassenkonstruktor Filter Anbieter ist nicht das Erstellen einer **neue** in Objekt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="09cc4-335">Der Container wird als Parameter übergeben, und die Abhängigkeit von Unity gelöst ist.</span><span class="sxs-lookup"><span data-stu-id="09cc4-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="09cc4-336">In der **FilterProvider** Klasse, implementieren Sie die Methode **GetFilters** aus **IFilterProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="09cc4-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="09cc4-337">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Filter Anbieter GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="09cc4-338">Aufgabe 2: registrieren und aktivieren den Filter</span><span class="sxs-lookup"><span data-stu-id="09cc4-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="09cc4-339">In dieser Aufgabe aktivieren Sie Website-Überwachung.</span><span class="sxs-lookup"><span data-stu-id="09cc4-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="09cc4-340">Registrieren Sie zu diesem Zweck wird der Filter in **Bootstrapper.cs BuildUnityContainer** Methode, um die Ablaufverfolgung starten:</span><span class="sxs-lookup"><span data-stu-id="09cc4-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="09cc4-341">Open **"Web.config"** befindet sich im Projektstammverzeichnis und Trace-Überwachung System.Web-Gruppe aktivieren.</span><span class="sxs-lookup"><span data-stu-id="09cc4-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="09cc4-342">Open **Bootstrapper.cs** auf Stammebene des Projekts.</span><span class="sxs-lookup"><span data-stu-id="09cc4-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="09cc4-343">Hinzufügen eines Verweises auf die **MvcMusicStore.Filters** Namespace.</span><span class="sxs-lookup"><span data-stu-id="09cc4-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="09cc4-344">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="09cc4-345">Wählen Sie die **BuildUnityContainer** -Methode und registrieren Sie den Filter im Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="09cc4-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="09cc4-346">Sie müssen beim Filteranbieter als auch der Aktionsfilter zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="09cc4-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="09cc4-347">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider und Aktionsfilter*)</span><span class="sxs-lookup"><span data-stu-id="09cc4-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="09cc4-348">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="09cc4-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="09cc4-349">In dieser Aufgabe, die Sie die Anwendung ausgeführt werden und testen, ob der benutzerdefinierte Aktionsfilter der aktivitätsablaufverfolgung ist:</span><span class="sxs-lookup"><span data-stu-id="09cc4-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="09cc4-350">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="09cc4-351">Klicken Sie auf **Rock** im Genres-Menü.</span><span class="sxs-lookup"><span data-stu-id="09cc4-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="09cc4-352">Sie können auf mehrere Genres durchsuchen, wenn Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="09cc4-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="09cc4-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="09cc4-354">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="09cc4-354">*Music Store*</span></span>
3. <span data-ttu-id="09cc4-355">Navigieren Sie zu **/Trace.axd** , finden Sie unter der Anwendung-Ablaufverfolgung, und klicken Sie dann auf **"Details anzeigen"**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="09cc4-356">![Anwendung Ablaufverfolgungsprotokoll](aspnet-mvc-4-dependency-injection/_static/image12.png "Anwendung-Ablaufverfolgungsprotokoll")</span><span class="sxs-lookup"><span data-stu-id="09cc4-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="09cc4-357">*Application-Ablaufverfolgungsprotokoll*</span><span class="sxs-lookup"><span data-stu-id="09cc4-357">*Application Trace Log*</span></span>

    <span data-ttu-id="09cc4-358">![Anwendung Trace - Anforderungsdetails](aspnet-mvc-4-dependency-injection/_static/image13.png "Anwendung Trace - Anforderungsdetails")</span><span class="sxs-lookup"><span data-stu-id="09cc4-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="09cc4-359">*Anwendung Trace - Anforderungsdetails*</span><span class="sxs-lookup"><span data-stu-id="09cc4-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="09cc4-360">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="09cc4-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="09cc4-361">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="09cc4-361">Summary</span></span>

<span data-ttu-id="09cc4-362">Durch diese praktische Übungseinheit haben Sie gelernt, durch die Integration von Unity unter Verwendung eines NuGet-Pakets die Abhängigkeitsinjektion in ASP.NET MVC 4 verwenden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="09cc4-363">Um dies zu erreichen, haben Sie die Abhängigkeitsinjektion in Controller, Ansichten und Aktionsfilter verwendet.</span><span class="sxs-lookup"><span data-stu-id="09cc4-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="09cc4-364">Es wurden die folgenden Konzepte behandelt:</span><span class="sxs-lookup"><span data-stu-id="09cc4-364">The following concepts were covered:</span></span>

- <span data-ttu-id="09cc4-365">Abhängigkeitsinjektion in ASP.NET MVC 4-Funktionen</span><span class="sxs-lookup"><span data-stu-id="09cc4-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="09cc4-366">Unity-Integration mit Unity.Mvc3 NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="09cc4-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="09cc4-367">Abhängigkeitsinjektion in Controllern</span><span class="sxs-lookup"><span data-stu-id="09cc4-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="09cc4-368">Abhängigkeitsinjektion in Sichten</span><span class="sxs-lookup"><span data-stu-id="09cc4-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="09cc4-369">Abhängigkeitsinjektion Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="09cc4-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="09cc4-370">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="09cc4-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="09cc4-371">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="09cc4-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="09cc4-372">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="09cc4-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="09cc4-373">Wechseln Sie zu [ [Https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="09cc4-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="09cc4-374">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="09cc4-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="09cc4-375">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-375">Click on **Install Now**.</span></span> <span data-ttu-id="09cc4-376">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="09cc4-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="09cc4-377">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="09cc4-378">![Visual Studio Express installieren](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="09cc4-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="09cc4-379">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="09cc4-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="09cc4-380">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="09cc4-382">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="09cc4-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="09cc4-383">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="09cc4-383">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="09cc4-385">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="09cc4-385">*Installation progress*</span></span>
6. <span data-ttu-id="09cc4-386">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-386">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="09cc4-388">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="09cc4-388">*Installation completed*</span></span>
7. <span data-ttu-id="09cc4-389">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="09cc4-390">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="09cc4-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="09cc4-392">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="09cc4-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="09cc4-393">Anhang B: Verwendung von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="09cc4-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="09cc4-394">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="09cc4-395">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="09cc4-396">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-dependency-injection/_static/image19.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="09cc4-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="09cc4-397">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="09cc4-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="09cc4-398">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="09cc4-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="09cc4-399">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="09cc4-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="09cc4-400">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="09cc4-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="09cc4-401">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="09cc4-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="09cc4-402">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="09cc4-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="09cc4-403">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="09cc4-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="09cc4-404">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-dependency-injection/_static/image20.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="09cc4-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="09cc4-405">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="09cc4-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="09cc4-406">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-dependency-injection/_static/image21.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="09cc4-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="09cc4-407">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="09cc4-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="09cc4-408">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-dependency-injection/_static/image22.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="09cc4-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="09cc4-409">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="09cc4-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="09cc4-410">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="09cc4-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="09cc4-411">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="09cc4-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="09cc4-412">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="09cc4-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="09cc4-413">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="09cc4-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="09cc4-414">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-dependency-injection/_static/image23.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="09cc4-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="09cc4-415">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="09cc4-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="09cc4-416">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-dependency-injection/_static/image24.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="09cc4-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="09cc4-417">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="09cc4-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
