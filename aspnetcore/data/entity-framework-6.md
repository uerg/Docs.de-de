---
title: Erste Schritte mit ASP.NET Core und Entity Framework 6
author: tdykstra
description: In diesem Artikel wird die Verwendung von Entity Framework 6 in einer ASP.NET Core-Anwendung veranschaulicht.
keywords: Entity Framework, ASP.NET Core EF 6
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: e186568e27c067e29985b8a286e26b87c3186ac4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="ca7ea-104">Erste Schritte mit ASP.NET Core und Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ca7ea-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="ca7ea-105">Durch [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ca7ea-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ca7ea-106">In diesem Artikel wird die Verwendung von Entity Framework 6 in einer ASP.NET Core-Anwendung veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="ca7ea-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="ca7ea-107">Overview</span></span>

<span data-ttu-id="ca7ea-108">Um Entity Framework 6 verwenden zu können, muss das Projekt für .NET Framework kompilieren, da Entity Framework 6 .NET Core nicht unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="ca7ea-109">Wenn Sie plattformübergreifende Funktionen benötigen, müssen so aktualisieren Sie auf [Entity Framework Core](https://docs.efproject.net).</span><span class="sxs-lookup"><span data-stu-id="ca7ea-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.efproject.net).</span></span>

<span data-ttu-id="ca7ea-110">Die empfohlene Methode zum Verwenden von Entity Framework 6 in einer ASP.NET Core-Anwendung ist der Kontext EF6 versetzt, und Modellklassen in einer Klassenbibliothek-Projekt, dessen Ziel vollständige Framework.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="ca7ea-111">Fügen Sie einen Verweis auf die Klassenbibliothek aus dem ASP.NET Core-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="ca7ea-112">Finden Sie im Beispiel [Visual Studio-Projektmappe mit Projekten von EF6 und ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="ca7ea-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="ca7ea-113">Einen EF6 Kontext kann nicht in einem Projekt auf ASP.NET Core gesetzt werden, da Projekte von .NET Core alle Funktionen nicht unterstützen, die z. B. EF6 Befehle *Enable-Migrations* erfordern.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="ca7ea-114">Unabhängig von Projekttyp, in dem Sie den Kontext EF6 suchen, können nur EF6-Befehlszeilentools mit einem EF6 Kontext aus.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="ca7ea-115">Beispielsweise `Scaffold-DbContext` steht nur in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="ca7ea-116">Wenn Sie einer Datenbank in einem Modell EF6 zurückentwickeln müssen, finden Sie unter [Code First mit einer vorhandenen Datenbank](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="ca7ea-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="ca7ea-117">Verweis vollständigen Framework und EF6 im Projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7ea-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="ca7ea-118">Das ASP.NET Core-Projekt muss .NET Framework und EF6 verweisen.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="ca7ea-119">Z. B. die *csproj* -Datei des Projekts zu ASP.NET Core wird ähnlich wie im folgenden Beispiel aussehen (nur die relevanten Teile der Datei werden angezeigt).</span><span class="sxs-lookup"><span data-stu-id="ca7ea-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="ca7ea-120">Wenn Sie ein neues Projekt erstellen, verwenden Sie die **ASP.NET-Webanwendung für Core ((.NET Framework)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="ca7ea-121">Behandeln von Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="ca7ea-121">Handle connection strings</span></span>

<span data-ttu-id="ca7ea-122">Die EF6-Befehlszeilentools, die Sie in das Klassenbibliotheksprojekt EF6 verwenden erfordern einen Standardkonstruktor damit Kontext instanziiert werden können.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="ca7ea-123">Aber Sie möchten wahrscheinlich angeben, dass die Verbindungszeichenfolge im ASP.NET Core-Projekt in diesem Fall die Kontext-Konstruktor verwendet einen Parameter benötigen, der Sie in der Verbindungszeichenfolge übergeben können.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="ca7ea-124">Hier ist ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="ca7ea-125">Da EF6 Kontext nicht über einen parameterlosen Konstruktor verfügt, wird Ihre EF6-Projekt verfügt über eine Implementierung bereitstellen [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="ca7ea-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="ca7ea-126">Die Befehlszeilentools EF6 findet und den Kontext instanziiert werden können, dass diese Implementierung einsetzen.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="ca7ea-127">Hier ist ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="ca7ea-128">In diesem Beispielcode die `IDbContextFactory` Implementierung werden in eine hartcodierte Verbindungszeichenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="ca7ea-129">Dies ist die Verbindungszeichenfolge, die die Befehlszeilentools verwenden.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="ca7ea-130">Sie sollten eine Strategie, um sicherzustellen, dass die Klassenbibliothek dieselbe Verbindungszeichenfolge verwendet, die die aufrufende Anwendung verwendet.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="ca7ea-131">Beispielsweise konnte den Wert von einer Umgebungsvariablen in beiden Projekten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="ca7ea-132">Abhängigkeitsinjektion in ASP.NET Core Projekt einrichten</span><span class="sxs-lookup"><span data-stu-id="ca7ea-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="ca7ea-133">Die Core-Projekts *Startup.cs* Datei, richten Sie den Kontext EF6 für abhängigkeiteneinschleusung (DI) in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="ca7ea-134">EF Kontextobjekte sollte für eine pro Anforderung Lebensdauer begrenzt werden.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="ca7ea-135">Eine Instanz des Kontexts erhalten in Ihren Domänencontrollern von mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="ca7ea-136">Der Code ist ähnlich, was Sie für einen Kontext EF Core schreiben würden:</span><span class="sxs-lookup"><span data-stu-id="ca7ea-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="ca7ea-137">Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="ca7ea-137">Sample application</span></span>

<span data-ttu-id="ca7ea-138">Eine funktionierende beispielanwendung finden Sie unter der [Beispiel Visual Studio-Projektmappe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) , die in diesem Artikel begleitet.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="ca7ea-139">In diesem Beispiel kann durch die folgenden Schritte in Visual Studio von Grund auf neu erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="ca7ea-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="ca7ea-140">Erstellen Sie eine Projektmappe an.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-140">Create a solution.</span></span>

* <span data-ttu-id="ca7ea-141">**Hinzufügen eines neuen Projekts > Web > ASP.NET Core-Webanwendung ((.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="ca7ea-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="ca7ea-142">**Hinzufügen eines neuen Projekts > klassische Windows-Desktop >-Klassenbibliothek ((.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="ca7ea-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="ca7ea-143">In **Package Manager Console** (PMC) für beide Projekte, führen Sie den Befehl `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="ca7ea-144">Erstellen Sie in das Klassenbibliotheksprojekt Modell Datenklassen und Context-Klasse und eine Implementierung von `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="ca7ea-145">Führen Sie die Befehle in PMC für das Klassenbibliotheksprojekt, `Enable-Migrations` und `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="ca7ea-146">Wenn Sie das ASP.NET Core-Projekt als Startprojekt festgelegt haben, fügen Sie `-StartupProjectName EF6` auf diese Befehle.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="ca7ea-147">Fügen Sie in der Core-Projekt einen Projektverweis auf das Klassenbibliotheksprojekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="ca7ea-148">Im Kern-Projekt in *Startup.cs*, registrieren Sie den Kontext für DI.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="ca7ea-149">Im Kern-Projekt in *appsettings.json*, fügen Sie der Verbindungszeichenfolge angegeben.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="ca7ea-150">Fügen Sie das Projekt Core einem Controller und Ansichten, um sicherzustellen, dass Sie lesen und Schreiben von Daten können.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="ca7ea-151">(Beachten Sie, dass ASP.NET Core MVC-Gerüstbau nicht mit dem EF6-Kontext, die von der Klassenbibliothek verwiesen funktioniert.)</span><span class="sxs-lookup"><span data-stu-id="ca7ea-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="ca7ea-152">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ca7ea-152">Summary</span></span>

<span data-ttu-id="ca7ea-153">Dieser Artikel hat grundlegende Leitfäden zum Verwenden von Entity Framework 6 in einer ASP.NET Core-Anwendung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ca7ea-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca7ea-154">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ca7ea-154">Additional Resources</span></span>

* [<span data-ttu-id="ca7ea-155">Entity Framework - Code-basierte Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ca7ea-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
