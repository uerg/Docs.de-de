---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 – Abhängigkeitsinjektion | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: Dieser praktischen Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC und ASP.NET MVC 4 haben. Wenn Sie nicht ASP.NET MVC 4-Filter, bevor wir Rec verwendet haben...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 44c8f2055fb62d589e874683cbf43eed87a8c447
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812340"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="9ef4a-104">ASP.NET MVC 4 – Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="9ef4a-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="9ef4a-105">durch [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9ef4a-106">Herunterladen Sie Web Camps Training Kit</span><span class="sxs-lookup"><span data-stu-id="9ef4a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="9ef4a-107">Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse der **ASP.NET MVC** und **ASP.NET MVC 4 filtert**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="9ef4a-108">Wenn Sie nicht verwendet haben **ASP.NET MVC 4 filtert** vor, wir empfehlen Ihnen, sich mit Windows Live ID **Aktionsfilter in ASP.NET MVC benutzerdefinierte** Hands-On Lab.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="9ef4a-109">Alle Beispielcode und Ausschnitte sind im Web Camps Trainingskit, erhältlich über enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="9ef4a-110">Das Projekt, das speziell für dieses Lab finden Sie unter [Abhängigkeitsinjektion in ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="9ef4a-111">In **objektorientierten Programmierung** -Paradigma Objekte zusammenarbeiten in einem Modell für die Zusammenarbeit, in denen es Mitwirkende und Benutzer.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="9ef4a-112">Natürlich generiert dieses Kommunikationsmodell Abhängigkeiten zwischen Objekten und Komponenten, die immer schwierig zu verwalten, wenn die Komplexität erhöht.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="9ef4a-113">![Klasse von Abhängigkeiten und Modellieren von Komplexität](aspnet-mvc-4-dependency-injection/_static/image1.png "Klasse Abhängigkeiten und Modellieren von Komplexität")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="9ef4a-114">*Abhängigkeiten der Klasse und die Modellkomplexität*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="9ef4a-115">Sie haben wahrscheinlich schon die **Factorymuster** und die Trennung zwischen der Schnittstelle und die Implementierung, die mit Diensten, bei dem die Clientobjekte oft verantwortlich für die dienstidentifizierung.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="9ef4a-116">Der Dependency Injection-Muster ist eine bestimmte Implementierung des Inversion of Control.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="9ef4a-117">**Inversion of Control (IoC) steuerungsumkehr** bedeutet, dass Objekte nicht andere Objekte erstellt werden, um ihre Arbeit zu erledigen, auf denen sie basieren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="9ef4a-118">Stattdessen erhalten sie die Objekte, die sie aus einer externen Quelle (z. B. eine XML-Konfigurationsdatei) benötigen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="9ef4a-119">**Dependency Injection (DI)** bedeutet, dass dies geschieht ohne Eingreifen Objekt, in der Regel von einer Framework-Komponente, die Konstruktorparameter übergibt und legen Sie Eigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="9ef4a-120">Das Dependency Injection (DI)-Entwurfsmuster</span><span class="sxs-lookup"><span data-stu-id="9ef4a-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="9ef4a-121">Das Ziel der Abhängigkeitsinjektion auf einer hohen Ebene ist, eine Clientklasse (z. B. *der dsmessages*) benötigt etwas, das eine Schnittstelle erfüllt (z. B. *IClub*).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="9ef4a-122">Ist es unerheblich, wie der konkrete Typ ist (z. B. *WoodClub, IronClub, WedgeClub* oder *PutterClub*), Personen, die behandelt werden sollen (z. B. eine gute *Caddy*).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="9ef4a-123">Der Abhängigkeitskonfliktlöser in ASP.NET MVC können Sie die Abhängigkeit Logik, die an anderer Stelle zu registrieren (z. B. einen Container oder ein *Behälter Kreuz*).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="9ef4a-124">![Dependency Injection Diagramm](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection-Abbildung")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="9ef4a-125">*Dependency Injection - Golf-Beispiel*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="9ef4a-126">Die Vorteile der Verwendung von Dependency Injection-Muster und Inversion of Control, sind die folgenden:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="9ef4a-127">Reduziert die Klassenkopplung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-127">Reduces class coupling</span></span>
- <span data-ttu-id="9ef4a-128">Erhöht die Wiederverwendung von code</span><span class="sxs-lookup"><span data-stu-id="9ef4a-128">Increases code reusing</span></span>
- <span data-ttu-id="9ef4a-129">Verbessert die codeverwaltbarkeit von</span><span class="sxs-lookup"><span data-stu-id="9ef4a-129">Improves code maintainability</span></span>
- <span data-ttu-id="9ef4a-130">Verbessert die Anwendungstests</span><span class="sxs-lookup"><span data-stu-id="9ef4a-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="9ef4a-131">Abhängigkeitsinjektion mit Entwurfsmuster "abstrakte Factory" manchmal verglichen wird, aber es gibt ein geringfügigen Unterschied zwischen den beiden Ansätzen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="9ef4a-132">DI ist ein Framework, hinter arbeiten, um Abhängigkeiten zu lösen, indem die Factorys und die registrierten Dienste aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="9ef4a-133">Nun, da Sie den Dependency Injection-Muster verstanden haben, werden in dieser Übungseinheit erfahren Sie wie für die anzuwendende in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="9ef4a-134">Starten Sie mithilfe der Abhängigkeitsinjektion in die **Controller** eine Datenbank-Access-Dienst umfassen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="9ef4a-135">Als Nächstes Sie Dependency Injection zum Anwenden der **Ansichten** , nutzen einen Dienst und Anzeigen von Informationen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="9ef4a-136">Zum Schluss erweitern die DI auf ASP.NET MVC 4-Filter, Sie Einfügen eines benutzerdefinierten Aktionsfilters in der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="9ef4a-137">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="9ef4a-138">Integrieren von ASP.NET MVC 4 mit Unity für Dependency Injection, die mithilfe von NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="9ef4a-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="9ef4a-139">Dependency Injection in ASP.NET MVC-Controller verwenden</span><span class="sxs-lookup"><span data-stu-id="9ef4a-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="9ef4a-140">Dependency Injection in ASP.NET MVC-Ansicht verwenden</span><span class="sxs-lookup"><span data-stu-id="9ef4a-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="9ef4a-141">Verwenden Sie Dependency Injection in einem ASP.NET MVC-Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="9ef4a-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="9ef4a-142">Diese Übungseinheit Unity.Mvc3 NuGet-Paket für die Auflösung von Abhängigkeiten verwendet, aber es ist möglich, alle Dependency Injection-Framework mit ASP.NET MVC 4 arbeiten anpassen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9ef4a-143">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="9ef4a-143">Prerequisites</span></span>

<span data-ttu-id="9ef4a-144">Sie benötigen Folgendes, um diese testumgebung abzuschließen:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9ef4a-145">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9ef4a-146">Setup</span><span class="sxs-lookup"><span data-stu-id="9ef4a-146">Setup</span></span>

<span data-ttu-id="9ef4a-147">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="9ef4a-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="9ef4a-148">Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="9ef4a-149">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="9ef4a-150">Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang B: Verwenden von Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9ef4a-151">Übungen</span><span class="sxs-lookup"><span data-stu-id="9ef4a-151">Exercises</span></span>

<span data-ttu-id="9ef4a-152">Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="9ef4a-153">Übung 1: Einfügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="9ef4a-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="9ef4a-154">Übung 2: Einfügen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="9ef4a-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="9ef4a-155">Übung 3: Einfügen von Filtern</span><span class="sxs-lookup"><span data-stu-id="9ef4a-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="9ef4a-156">Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9ef4a-157">Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="9ef4a-158">Geschätzte Zeit für diese testumgebung abzuschließen: **30 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="9ef4a-159">Übung 1: Einfügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="9ef4a-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="9ef4a-160">In dieser Übung lernen Sie, wie Dependency Injection in ASP.NET MVC-Controller verwendet werden, durch die Integration von Unity unter Verwendung eines NuGet-Pakets.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="9ef4a-161">Aus diesem Grund umfasst Sie Dienste in Ihre Controller MvcMusicStore, um die Logik von der Datenzugriff zu trennen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="9ef4a-162">Die Dienste erstellt eine neue Abhängigkeit im controllerkonstruktor, der aufgelöst wird mithilfe der Abhängigkeitsinjektion mit Hilfe der **Unity**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="9ef4a-163">Dadurch erfahren Sie, wie, sind flexibler und einfacher zu verwalten und Testen weniger verbundene Anwendungen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="9ef4a-164">Außerdem lernen Sie, wie Sie ASP.NET MVC mit Unity zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="9ef4a-165">Informationen zu StoreManager Service</span><span class="sxs-lookup"><span data-stu-id="9ef4a-165">About StoreManager Service</span></span>

<span data-ttu-id="9ef4a-166">Die MVC Music Store jetzt in der Begin-Lösung bereitgestellt enthält einen Dienst, der die Store-Controller-Daten, die mit dem Namen verwaltet **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="9ef4a-167">Im folgenden finden Sie die Store-dienstimplementierung.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="9ef4a-168">Beachten Sie, dass alle Methoden modellentitäten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="9ef4a-169">**StoreController** von der Begin Projektmappe jetzt nutzt **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="9ef4a-170">Alle Datenverweise wurden aus entfernt **StoreController**, und nun möglich, ändern, den aktuellen Datenzugriffsanbieter ohne jede Methode, die verbraucht **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="9ef4a-171">Finden Sie nachfolgend die **StoreController** Implementierung hat eine Abhängigkeit mit **StoreService** im Klassenkonstruktor.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="9ef4a-172">Die Abhängigkeit eingeführt, die in dieser Übung bezieht sich auf **Inversion of Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="9ef4a-173">Die **StoreController** Klassenkonstruktor empfängt eine **IStoreService** Typparameter, der zum Ausführen von Dienstaufrufe von innerhalb der Klasse erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="9ef4a-174">Allerdings **StoreController** implementiert nicht den Standardkonstruktor (ohne Parameter), die jedem Controller zum Arbeiten mit ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="9ef4a-175">Um die Abhängigkeit zu beheben, muss der Controller erstellt werden, indem eine abstrakte Factory (eine Klasse, die jedes Objekt des angegebenen Typs zurückgibt).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="9ef4a-176">Sie erhalten einen Fehler, wenn die Klasse versucht, die StoreController zu erstellen, ohne das Dienstobjekt senden, wie es keinen parameterloser Konstruktor deklariert gibt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="9ef4a-177">Aufgabe 1: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="9ef4a-178">In dieser Aufgabe führen Sie die Begin-Anwendung, die den Dienst in den Store-Controller enthält, die den Datenzugriff von der Anwendungslogik trennt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="9ef4a-179">Wenn die Anwendung ausgeführt wird, erhalten Sie eine Ausnahme aus, wie der Controller-Dienst nicht als Parameter, wird standardmäßig übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="9ef4a-180">Öffnen der **beginnen** Lösung befindet sich in **Source\Ex01 einfügen Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="9ef4a-181">Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9ef4a-182">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9ef4a-183">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9ef4a-184">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9ef4a-185">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9ef4a-186">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9ef4a-187">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9ef4a-188">Drücken Sie **STRG + F5** um die Anwendung ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="9ef4a-189">Sie erhalten die Fehlermeldung &quot; **kein parameterloser Konstruktor für dieses Objekt definierten**&quot;:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="9ef4a-190">![Fehler beim Ausführen von ASP.NET MVC-Begin-Anwendung](aspnet-mvc-4-dependency-injection/_static/image3.png "Fehler während der Ausführung beginnen ASP.NET MVC-Anwendung")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="9ef4a-191">*Fehler beim Ausführen von ASP.NET MVC-Begin-Anwendung*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="9ef4a-192">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-192">Close the browser.</span></span>

<span data-ttu-id="9ef4a-193">Arbeiten Sie in den folgenden Schritten auf die Music Store-Projektmappe, die Abhängigkeit einzufügen, die diesem Controller benötigt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="9ef4a-194">Aufgabe 2 – einschließlich Unity in MvcMusicStore-Lösung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="9ef4a-195">In dieser Aufgabe enthält **Unity.Mvc3** NuGet-Paket mit der Lösung.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="9ef4a-196">Unity.Mvc3 Paket wurde für ASP.NET MVC 3 entwickelt, aber es ist vollständig kompatibel mit ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="9ef4a-197">Unity für die Instanz wird von einer einfachen und erweiterbaren Container für Abhängigkeitsinjektion mit optionalem Support und Abfangen von Typen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="9ef4a-198">Es ist ein universell einsetzbarer Container für die Verwendung in eine beliebige Art von .NET-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="9ef4a-199">Es enthält alle allgemeinen Funktionen finden Sie in der Dependency Injection Mechanismen, einschließlich: Erstellen von Objekten, die Abstraktion von Anforderungen durch Angeben von Abhängigkeiten auf der Common Language Runtime und Flexibilität durch verzögern der Konfigurations der Komponente auf den Container.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="9ef4a-200">Installieren Sie **Unity.Mvc3** NuGet-Paket in der **MvcMusicStore** Projekt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="9ef4a-201">Zu diesem Zweck öffnen Sie die **-Paket-Manager-Konsole** aus **Ansicht** | **Other Windows**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="9ef4a-202">Führen Sie den folgenden Befehl ein.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-202">Run the following command.</span></span>

    <span data-ttu-id="9ef4a-203">PMC</span><span class="sxs-lookup"><span data-stu-id="9ef4a-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="9ef4a-204">![Installieren von NuGet-Paket Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet-Paket installieren")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="9ef4a-205">*Installieren von NuGet-Paket Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="9ef4a-206">Sobald die **Unity.Mvc3** -Paket installiert ist, untersuchen Sie die Dateien und Ordner, die sie automatisch hinzugefügt, um die Unity-Konfiguration zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="9ef4a-207">![Unity.Mvc3-Paket installiert](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3-Pakets installiert")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="9ef4a-208">*Unity.Mvc3-Pakets installiert*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="9ef4a-209">Aufgabe 3: Registrieren von Unity in "Global.asax.cs" Anwendung\_starten</span><span class="sxs-lookup"><span data-stu-id="9ef4a-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="9ef4a-210">In dieser Aufgabe aktualisieren Sie die **Anwendung\_starten** Methode befindet sich in **"Global.asax.cs"** den Unity-Bootstrapper-Initialisierer aufrufen und aktualisieren Sie dann auf die Bootstrapper-Datei registrieren der Dienst und der Controller, die Sie für die Abhängigkeitsinjektion verwenden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="9ef4a-211">Nun werden Sie von der Bootstrapper die Datei, die den Unity-Container initialisiert und Abhängigkeitskonfliktlöser verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="9ef4a-212">Öffnen Sie zu diesem Zweck **"Global.asax.cs"** und fügen Sie folgenden hervorgehobenen Code innerhalb der **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="9ef4a-213">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - initialisieren Unity*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="9ef4a-214">Open **Bootstrapper.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="9ef4a-215">Die folgenden Namespaces: **MvcMusicStore.Services** und **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="9ef4a-216">(Codeausschnitt - *ASP.NET Dependency Injection, Lab - Ex01 - Bootstrapper, Hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="9ef4a-217">Ersetzen Sie dies **BuildUnityContainer** Methode den Inhalt durch den folgenden Code, der Store-Controller und Store-Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="9ef4a-218">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller und Dienst*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="9ef4a-219">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="9ef4a-220">In dieser Aufgabe führen Sie die Anwendung aus, um sicherzustellen, dass sie nun nach dem einschließen von Unity geladen werden können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="9ef4a-221">Drücken Sie **F5** zum Ausführen der Anwendung, die Anwendung sollte jetzt laden, ohne alle Fehler angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="9ef4a-222">![Ausführen der Anwendung über Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Ausführen der Anwendung über Dependency Injection")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="9ef4a-223">*Ausgeführte Anwendung über Dependency Injection*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="9ef4a-224">Navigieren Sie zu **/Store**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-224">Browse to **/Store**.</span></span> <span data-ttu-id="9ef4a-225">Dies wird aufgerufen, **StoreController**, das wird jetzt erstellt, mit **Unity**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="9ef4a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="9ef4a-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="9ef4a-228">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-228">Close the browser.</span></span>

<span data-ttu-id="9ef4a-229">In den folgenden Übungen lernen Sie, wie Sie den Dependency Injection-Bereich für die Verwendung in ASP.NET MVC-Ansichten und Aktionsfilter zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="9ef4a-230">Übung 2: Einfügen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="9ef4a-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="9ef4a-231">In dieser Übung lernen Sie, wie Sie Dependency Injection in einer Ansicht mit den neuen Features von ASP.NET MVC 4 für Unity-Integration zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="9ef4a-232">Zu diesem Zweck wird einen benutzerdefinierten Dienst in der Store durchsuchen Ansicht aufgerufen, die eine Nachricht und eine der folgenden Abbildung angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="9ef4a-233">Klicken Sie dann Sie das Projekt mit Unity zu integrieren und erstellen einen benutzerdefinierte Abhängigkeitskonfliktlöser zum Einfügen der Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="9ef4a-234">Aufgabe 1: Erstellen einer Ansicht, die einen Dienst nutzt</span><span class="sxs-lookup"><span data-stu-id="9ef4a-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="9ef4a-235">In dieser Aufgabe erstellen Sie eine Ansicht, die führt ein Dienstaufruf aus, um eine neue Abhängigkeit zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="9ef4a-236">Der Dienst besteht in einen einfachen Messagingdienst in dieser Lösung enthalten.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="9ef4a-237">Öffnen der **beginnen** Lösung befindet sich in der **Source\Ex02 einfügen View\Begin** Ordner.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="9ef4a-238">Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9ef4a-239">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9ef4a-240">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9ef4a-241">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9ef4a-242">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9ef4a-243">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9ef4a-244">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9ef4a-245">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="9ef4a-246">Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="9ef4a-247">Enthalten die **MessageService.cs** und die **IMessageService.cs** Klassen befindet sich in der **Source \Assets** Ordner **/Dienste**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="9ef4a-248">Dazu, mit der Maustaste **Services** Ordner, und wählen **vorhandenes Element hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="9ef4a-249">Suchen Sie den Pfad der Dateien, und enthalten Sie sind.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="9ef4a-250">![Hinzufügen von Nachrichtendienst und Dienstschnittstelle](aspnet-mvc-4-dependency-injection/_static/image8.png "Nachrichtendienst und Dienstschnittstelle hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="9ef4a-251">*Hinzufügen von Message-Dienst und Dienstschnittstelle*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9ef4a-252">Die **IMessageService** Schnittstelle definiert zwei Eigenschaften, die von implementiert die **MessageService** Klasse.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="9ef4a-253">Diese Eigenschaften -**Nachricht** und **ImageUrl**– speichern Sie die Nachricht und die URL des Bilds angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="9ef4a-254">Erstellen Sie den Ordner **/Pages** Stammordner des Projekts, und fügen Sie dann die vorhandene Klasse hinzu **MyBasePage.cs** aus **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="9ef4a-255">Die Basisseite, die, der Sie erbt, hat die folgende Struktur.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="9ef4a-256">![Ordner "Pages"](aspnet-mvc-4-dependency-injection/_static/image9.png "Ordner \"Pages\"")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="9ef4a-257">Open **Browse.cshtml** anzeigen **/Views/Store** Ordner, und stellen sie die von erben **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="9ef4a-258">In der **Durchsuchen** anzuzeigen, fügen Sie einen Aufruf von **MessageService** anzuzeigende ein Bild und eine Nachricht vom Dienst abgerufen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="9ef4a-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="9ef4a-260">Aufgabe 2: eine benutzerdefinierte Abhängigkeitskonfliktlöser sowie eine benutzerdefinierte Ansichtsseite</span><span class="sxs-lookup"><span data-stu-id="9ef4a-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="9ef4a-261">In der vorherigen Aufgabe eingefügt Sie eine neue Abhängigkeit in der Ansicht ein Dienstaufrufs darin ausführen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="9ef4a-262">Nun werden Sie diese Abhängigkeit aufgelöst, durch die Implementierung der Abhängigkeitsinjektion für ASP.NET MVC-Schnittstellen **IViewPageActivator** und **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="9ef4a-263">Sie umfasst in der Projektmappe eine Implementierung von **IDependencyResolver** , die mit dem Dienst abrufen mithilfe von Unity behandeln werden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="9ef4a-264">Klicken Sie dann, enthält Sie eine andere benutzerdefinierte Implementierung der **IViewPageActivator** -Schnittstelle, die die Erstellung der Sichten lösen werden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="9ef4a-265">Da ASP.NET MVC 3 mussten die Implementierung für Dependency Injection die Schnittstellen zum Registrieren von Diensten vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="9ef4a-266">**IDependencyResolver** und **IViewPageActivator** sind Bestandteil von ASP.NET MVC 3-Funktionen für Dependency Injection.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="9ef4a-267">**-Der IDependencyResolver** Interface ersetzt die vorherige IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="9ef4a-268">Implementierer der IDependencyResolver müssen es sich um eine Instanz des Dienstes oder eine Sammlung von Diensten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="9ef4a-269">**-IViewPageActivator** Schnittstelle bietet eine präzisere Steuerung wie Ansichtsseiten über Dependency Injection instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="9ef4a-270">Die Klassen, implementieren **IViewPageActivator** Schnittstelle kann Instanzen anzeigen, die mit Kontextinformationen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="9ef4a-271">Erstellen Sie die /**Factorys** -Ordner im Stammordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="9ef4a-272">Umfassen **CustomViewPageActivator.cs** zu Ihrer Lösung von **/Sources/Assets/** zu **Factorys** Ordner.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="9ef4a-273">Zu diesem Zweck Maustaste der **/Factories** Ordner **hinzufügen | Vorhandenes Element** und wählen Sie dann **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="9ef4a-274">Diese Klasse implementiert die **IViewPageActivator** Schnittstelle, die den Unity-Container enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="9ef4a-275">**CustomViewPageActivator** ist verantwortlich für die Verwaltung von die Erstellung einer Ansicht mithilfe eines Unity-Containers.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="9ef4a-276">Umfassen **UnityDependencyResolver.cs** Datei **/Quellen/Assets** zu **/Factories** Ordner.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="9ef4a-277">Zu diesem Zweck Maustaste der **/Factories** Ordner **hinzufügen | Vorhandenes Element** und wählen Sie dann **UnityDependencyResolver.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="9ef4a-278">**UnityDependencyResolver** -Klasse ist eine benutzerdefinierte DependencyResolver für Unity.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="9ef4a-279">Wenn ein Dienst in der Unity-Container nicht gefunden wird, ist die Basis-Resolver invocated.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="9ef4a-280">In der folgenden Aufgabe werden beide Implementierungen registriert werden, damit das Modell, das den Speicherort der Dienste und die Ansichten kennen, können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="9ef4a-281">Aufgabe 3: Registrieren für Dependency Injection in Unity-Container</span><span class="sxs-lookup"><span data-stu-id="9ef4a-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="9ef4a-282">In dieser Aufgabe fügen Sie die vorherigen Schritte zusammen, um die Stellen Dependency Injection funktioniert ein.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="9ef4a-283">Bis jetzt hat Ihre Lösung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="9ef4a-284">Ein **Durchsuchen** Ansicht, erbt **MyBaseClass** und nutzt **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="9ef4a-285">Eine Zwischenklasse -**MyBaseClass**-Listenfeldsteuerelement mit Abhängigkeitsinjektion, die für die Dienstschnittstelle deklariert.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="9ef4a-286">Ein Dienst - **MessageService** - und die Schnittstelle **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="9ef4a-287">Eine benutzerdefinierte Abhängigkeitskonfliktlöser für Unity - **UnityDependencyResolver** -behandelt, die die Service abrufen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="9ef4a-288">Eine Ansichtsseite Activator - **CustomViewPageActivator** -Seite erstellt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="9ef4a-289">Einzufügende **Durchsuchen** Ansicht nun registrieren Sie den benutzerdefinierten Abhängigkeitskonfliktlöser im Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="9ef4a-290">Open **Bootstrapper.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="9ef4a-291">Registrieren einer Instanz von **MessageService** in den Unity-Container, um den Dienst zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="9ef4a-292">(Codeausschnitt - *ASP.NET Lab - Ex02 - Register-Nachrichtendienst Injection*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="9ef4a-293">Hinzufügen eines Verweises auf **MvcMusicStore.Factories** Namespace.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="9ef4a-294">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Factorys Namespace*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="9ef4a-295">Registrieren Sie **CustomViewPageActivator** als eine Seite anzeigen Activator in Unity-Container:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="9ef4a-296">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="9ef4a-297">Ersetzen von ASP.NET MVC 4-Standard-Abhängigkeitskonfliktlöser mit einer Instanz von **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="9ef4a-298">Zu diesem Zweck ersetzen **Initialise** Methode, die mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="9ef4a-299">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex02 - Update-Abhängigkeitskonfliktlöser*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="9ef4a-300">ASP.NET MVC bietet standardmäßige Dependency Resolver-Klasse.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="9ef4a-301">Um mit benutzerdefinierten Abhängigkeitskonfliktlöser wie arbeiten, die wir für Unity erstellt haben, muss dieser Konfliktlöser ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="9ef4a-302">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="9ef4a-303">In dieser Aufgabe führen Sie die Anwendung überprüfen, ob der Browser Store nutzt den Dienst und zeigt das Bild und die Nachricht abgerufen:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="9ef4a-304">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="9ef4a-305">Klicken Sie auf **Rock** innerhalb des Genres-Menü und finden Sie unter wie die **MessageService** an die Ansicht eingefügt wurde und die Willkommensnachricht und das Bild geladen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="9ef4a-306">In diesem Beispiel werden wir eingeben, &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="9ef4a-307">![MVC Music Store - Ansichtsinjektion](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - Ansichtsinjektion")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="9ef4a-308">*MVC Music Store - Ansichtsinjektion*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="9ef4a-309">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="9ef4a-310">Übung 3: Einfügen von Aktionsfiltern</span><span class="sxs-lookup"><span data-stu-id="9ef4a-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="9ef4a-311">In der vorhergehenden praktische Übungseinheit **benutzerdefinierte Aktionsfilter** jetzt haben Sie Filter anpassen und Injection.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="9ef4a-312">In dieser Übung lernen Sie, wie Sie Filter mit Dependency Injection einfügen, mit der Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="9ef4a-313">Zu diesem Zweck fügen Sie mit der Music Store-Lösung eines benutzerdefinierten Aktionsfilters hinzu, das die Aktivitäten des Standorts verfolgen wird.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="9ef4a-314">Aufgabe 1: z. B. die Nachverfolgungs-Filter in der Projektmappe</span><span class="sxs-lookup"><span data-stu-id="9ef4a-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="9ef4a-315">In dieser Aufgabe werden Sie in der Music Store ein benutzerdefinierten Aktionsfilters zum Verfolgen von Ereignissen einschließen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="9ef4a-316">Als benutzerdefinierte Aktionsfilter werden Konzepte bereits in der vorherigen Lektion behandelt &quot;benutzerdefinierte Aktionsfilter&quot;, Sie werden nur die Filter-Klasse aus dem Ordner "Assets" dieser Übungseinheit, und klicken Sie dann einen Filteranbieter für Unity zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="9ef4a-317">Öffnen der **beginnen** Lösung befindet sich in der **Source\Ex03 - Aktion Filter\Begin einfügen** Ordner.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="9ef4a-318">Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9ef4a-319">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9ef4a-320">Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9ef4a-321">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9ef4a-322">Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9ef4a-323">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9ef4a-324">Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9ef4a-325">Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="9ef4a-326">Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="9ef4a-327">Umfassen **TraceActionFilter.cs** Datei **/Quellen/Assets** zu **/filtert** Ordner.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="9ef4a-328">Dieser Filter für die benutzerdefinierte Aktion ausführt, mit der ASP.NET-Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="9ef4a-329">Sie können überprüfen, &quot;ASP.NET MVC 4-lokal und dynamische Aktionsfilter&quot; Lab Weitere Verweise.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="9ef4a-330">Fügen Sie die leere Klasse **FilterProvider.cs** zum Projekt im Ordner   **/filtert.**</span><span class="sxs-lookup"><span data-stu-id="9ef4a-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="9ef4a-331">Hinzufügen der **System.Web.Mvc** und **Microsoft.Practices.Unity** Namespaces im **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="9ef4a-332">(Codeausschnitt - *Dependency Injection Lab - Ex03 - Filter ASP.NET-Anbieter hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="9ef4a-333">Legen Sie die Klasse, die von erben **IFilterProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="9ef4a-334">Hinzufügen einer **IUnityContainer** -Eigenschaft in der **FilterProvider** Klasse, und klicken Sie dann einen Klassenkonstruktor, um die Zuweisung von Container zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="9ef4a-335">(Codeausschnitt - *ASP.NET Dependency Injection-Lab - Ex03 - Filterkonstruktor Anbieter*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="9ef4a-336">Der Klassenkonstruktor des Filter-Anbieter ist nicht das Erstellen einer **neue** -Objekt innerhalb.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="9ef4a-337">Der Container als Parameter übergeben wird, und die Abhängigkeit von Unity gelöst wird.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="9ef4a-338">In der **FilterProvider** Klasse, implementieren Sie die Methode **GetFilters** aus **IFilterProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="9ef4a-339">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Filter-Anbieter GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="9ef4a-340">Aufgabe 2: registrieren und aktivieren den Filter</span><span class="sxs-lookup"><span data-stu-id="9ef4a-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="9ef4a-341">In dieser Aufgabe können Sie die Website nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="9ef4a-342">Zu diesem Zweck, registrieren Sie den Filter im **Bootstrapper.cs BuildUnityContainer** Methode, um die Ablaufverfolgung starten:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="9ef4a-343">Open **"Web.config"** befindet sich im Stammverzeichnis des Projekts und Aktivieren der nachverfolgung der Ablaufverfolgung auf "System.Web"-Gruppe.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="9ef4a-344">Open **Bootstrapper.cs** am Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="9ef4a-345">Hinzufügen eines Verweises auf die **MvcMusicStore.Filters** Namespace.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="9ef4a-346">(Codeausschnitt - *ASP.NET Dependency Injection, Lab - Ex03 - Bootstrapper, Hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="9ef4a-347">Wählen Sie die **BuildUnityContainer** -Methode und registrieren Sie den Filter im Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="9ef4a-348">Sie müssen beim Filteranbieter als auch den Aktionsfilter zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="9ef4a-349">(Codeausschnitt - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider und ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="9ef4a-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="9ef4a-350">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="9ef4a-351">In dieser Aufgabe, die Sie die Anwendung ausgeführt werden und testen, ob der Filter für die benutzerdefinierte Aktion die aktivitätsablaufverfolgung ist:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="9ef4a-352">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="9ef4a-353">Klicken Sie auf **Rock** innerhalb des Genres-Menüs.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="9ef4a-354">Sie können auf mehrere Genres durchsuchen, wenn Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="9ef4a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="9ef4a-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-356">*Music Store*</span></span>
3. <span data-ttu-id="9ef4a-357">Navigieren Sie zu **/Trace.axd** angezeigt, und klicken Sie dann auf die Anwendung-Ablaufverfolgung **Details anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="9ef4a-358">![Application-Ablaufverfolgungsprotokoll](aspnet-mvc-4-dependency-injection/_static/image12.png "Anwendung-Ablaufverfolgungsprotokoll")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="9ef4a-359">*Application-Ablaufverfolgungsprotokoll*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-359">*Application Trace Log*</span></span>

    <span data-ttu-id="9ef4a-360">![-Anwendungsüberwachung - Anforderungsdetails](aspnet-mvc-4-dependency-injection/_static/image13.png "Anwendung verfolgen - Anforderungsdetails")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="9ef4a-361">*-Anwendungsüberwachung - Request-Details*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="9ef4a-362">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9ef4a-363">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="9ef4a-363">Summary</span></span>

<span data-ttu-id="9ef4a-364">Von dieser praktischen Übungseinheit haben Sie gelernt, Dependency Injection in ASP.NET MVC 4 zu verwenden, indem Sie die Integration von Unity unter Verwendung eines NuGet-Pakets.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="9ef4a-365">Um dies zu erreichen, haben Sie Dependency Injection in Controllern, Ansichten und Aktionsfilter verwendet.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="9ef4a-366">Es wurden die folgenden Konzepte behandelt:</span><span class="sxs-lookup"><span data-stu-id="9ef4a-366">The following concepts were covered:</span></span>

- <span data-ttu-id="9ef4a-367">Abhängigkeitsinjektion in ASP.NET MVC 4-features</span><span class="sxs-lookup"><span data-stu-id="9ef4a-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="9ef4a-368">Unity-Integration mit Unity.Mvc3 NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="9ef4a-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="9ef4a-369">Abhängigkeitsinjektion in Controller</span><span class="sxs-lookup"><span data-stu-id="9ef4a-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="9ef4a-370">Abhängigkeitsinjektion in Ansichten</span><span class="sxs-lookup"><span data-stu-id="9ef4a-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="9ef4a-371">Abhängigkeitsinjektion von Aktionsfiltern</span><span class="sxs-lookup"><span data-stu-id="9ef4a-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9ef4a-372">Anhang A: Installieren von Visual Studio Express 2012 für Web</span><span class="sxs-lookup"><span data-stu-id="9ef4a-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9ef4a-373">Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9ef4a-374">Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9ef4a-375">Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9ef4a-376">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9ef4a-377">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-377">Click on **Install Now**.</span></span> <span data-ttu-id="9ef4a-378">Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9ef4a-379">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9ef4a-380">![Installieren von Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9ef4a-381">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9ef4a-382">Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="9ef4a-384">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9ef4a-385">Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-385">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="9ef4a-387">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-387">*Installation progress*</span></span>
6. <span data-ttu-id="9ef4a-388">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-388">When the installation completes, click **Finish**.</span></span>

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="9ef4a-390">*Die Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-390">*Installation completed*</span></span>
7. <span data-ttu-id="9ef4a-391">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9ef4a-392">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="9ef4a-394">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="9ef4a-395">Anhang B: Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="9ef4a-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="9ef4a-396">Mit Codeausschnitten müssen Sie den Code zur Hand benötigten.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9ef4a-397">Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9ef4a-398">![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-dependency-injection/_static/image19.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9ef4a-399">*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9ef4a-400">***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="9ef4a-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9ef4a-401">Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9ef4a-402">Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9ef4a-403">Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9ef4a-404">Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="9ef4a-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9ef4a-405">Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9ef4a-406">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-dependency-injection/_static/image20.png "Geben Sie den Namen des Ausschnitts")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9ef4a-407">*Geben Sie den Namen des Ausschnitts*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="9ef4a-408">![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-dependency-injection/_static/image21.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9ef4a-409">*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9ef4a-410">![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-dependency-injection/_static/image22.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9ef4a-411">*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9ef4a-412">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="9ef4a-413">Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="9ef4a-414">Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="9ef4a-415">Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="9ef4a-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9ef4a-416">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-dependency-injection/_static/image23.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9ef4a-417">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9ef4a-418">![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-dependency-injection/_static/image24.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="9ef4a-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9ef4a-419">*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="9ef4a-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
