---
title: Repositorymuster mit ASP.NET Core
author: ardalis
description: Hier erfahren Sie, wie Sie das Entwurfsmuster für Repositorys in einer ASP.NET Core-App implementieren.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342630"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="d6827-103">Repositorymuster mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6827-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="d6827-104">Von [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d6827-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d6827-105">Das *Repositorymuster* ist ein Entwurfsmuster, das den Datenzugriff hinter Schnittstellenabstraktionen isoliert.</span><span class="sxs-lookup"><span data-stu-id="d6827-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="d6827-106">Die Verbindung mit der Datenbank und die Bearbeitung der Datenspeicherobjekte erfolgen mithilfe von Methoden, die durch die Implementierung der Schnittstelle bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d6827-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="d6827-107">Daher ist es nicht notwendig, Code aufzurufen, um verschiedene datenbankbezogene Aspekte zu verarbeiten, z.B. Verbindungen, Befehle und Reader.</span><span class="sxs-lookup"><span data-stu-id="d6827-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="d6827-108">Die Implementierung des Repositorymusters mit ASP.NET Core bietet folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="d6827-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="d6827-109">Die Organisation einer App wird vereinfacht, da keine direkte gegenseitige Abhängigkeit zwischen der Geschäftsebene und der Datenzugriffsebene besteht.</span><span class="sxs-lookup"><span data-stu-id="d6827-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="d6827-110">Code für den Datenbankzugriff lässt sich leichter wiederverwenden, weil der Code in einem oder mehreren Repositorys zentral verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="d6827-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="d6827-111">Komponententests für die Geschäftsdomäne können unabhängig von der Datenbankebene durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="d6827-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="d6827-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6827-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d6827-113">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) verwendet das Repositorymuster, um eine Liste mit Namen von Filmfiguren zu initialisieren und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d6827-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="d6827-114">Die App verwendet [Entity Framework Core](/ef/core/) und eine `ApplicationDbContext`-Klasse für die Datenpersistenz, aber die Datenbankinfrastruktur, in der der Datenzugriff erfolgt, ist nicht offensichtlich.</span><span class="sxs-lookup"><span data-stu-id="d6827-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="d6827-115">Datenzugriff und Datenbankobjekte sind hinter einem [Repository](https://martinfowler.com/eaaCatalog/repository.html) abstrahiert.</span><span class="sxs-lookup"><span data-stu-id="d6827-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="d6827-116">Repositoryschnittstelle</span><span class="sxs-lookup"><span data-stu-id="d6827-116">Repository interface</span></span>

<span data-ttu-id="d6827-117">Eine Repositoryschnittstelle definiert die Eigenschaften und Methoden für eine Implementierung.</span><span class="sxs-lookup"><span data-stu-id="d6827-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="d6827-118">In der Beispiel-App lautet die Repositoryschnittstelle für die Filmfigurendaten `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="d6827-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="d6827-119">`ICharacterRepository` definiert die Methoden `ListAll` und `Add`, die zum Arbeiten mit `Character`-Instanzen in der App erforderlich sind:</span><span class="sxs-lookup"><span data-stu-id="d6827-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d6827-120">`Character` wird folgendermaßen definiert:</span><span class="sxs-lookup"><span data-stu-id="d6827-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="d6827-121">Konkreter Repositorytyp</span><span class="sxs-lookup"><span data-stu-id="d6827-121">Repository concrete type</span></span>

<span data-ttu-id="d6827-122">Die Schnittstelle wird durch einen konkreten Typ implementiert.</span><span class="sxs-lookup"><span data-stu-id="d6827-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="d6827-123">In der Beispiel-App verwaltet `CharacterRepository` den Datenbankkontext und implementiert die Methoden `ListAll` und `Add`:</span><span class="sxs-lookup"><span data-stu-id="d6827-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="d6827-124">Registrieren des Repositorydiensts</span><span class="sxs-lookup"><span data-stu-id="d6827-124">Register the repository service</span></span>

<span data-ttu-id="d6827-125">Das Repository und der Datenbankkontext werden beim Dienstcontainer in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="d6827-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d6827-126">In der Beispiel-App wird `ApplicationDbContext` mit dem Aufruf der Erweiterungsmethode [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="d6827-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="d6827-127">`ICharacterRepository` wird als bereichsbezogener Dienst registriert:</span><span class="sxs-lookup"><span data-stu-id="d6827-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="d6827-128">Einfügen einer Instanz des Repositorys</span><span class="sxs-lookup"><span data-stu-id="d6827-128">Inject an instance of the repository</span></span>

<span data-ttu-id="d6827-129">In einer Klasse, in der Datenbankzugriff erforderlich ist, wird eine Instanz des Repositorys über den Konstruktor angefordert und einem privaten Feld zur Verwendung durch Klassenmethoden zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="d6827-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="d6827-130">In der Beispiel-App wird `ICharacterRepository` zu folgenden Zwecken verwendet:</span><span class="sxs-lookup"><span data-stu-id="d6827-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="d6827-131">Auffüllen der Datenbank, wenn keine Figuren vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="d6827-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="d6827-132">Abrufen einer Liste der Figuren zur Anzeige.</span><span class="sxs-lookup"><span data-stu-id="d6827-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="d6827-133">Beachten Sie, dass der aufrufende Code nur mit `CharacterRepository`, der Implementierung der Schnittstelle, interagiert.</span><span class="sxs-lookup"><span data-stu-id="d6827-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="d6827-134">Der aufrufende Code verwendet den `ApplicationDbContext` nicht direkt:</span><span class="sxs-lookup"><span data-stu-id="d6827-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="d6827-135">Generische Repositoryschnittstelle</span><span class="sxs-lookup"><span data-stu-id="d6827-135">Generic repository interface</span></span>

<span data-ttu-id="d6827-136">Dieses Thema und die Beispiel-App veranschaulichen die einfachste Implementierung des Repositorymusters, in der für jedes Geschäftsobjekt ein Repository erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d6827-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="d6827-137">Wenn die App größer wird und mehr als nur einige wenige Objekte enthält, kann eine *generische Repositoryschnittstelle* die Menge an Code reduzieren, die zur Implementierung des Repositorymusters erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="d6827-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="d6827-138">Weitere Informationen finden Sie unter [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/) (DevIQ: Repositorymuster: generische Repositoryschnittstelle).</span><span class="sxs-lookup"><span data-stu-id="d6827-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6827-139">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d6827-139">Additional resources</span></span>

* <span data-ttu-id="d6827-140">[DevIQ: Repository Pattern](https://deviq.com/repository-pattern/) (DevIQ: Repositorymuster)</span><span class="sxs-lookup"><span data-stu-id="d6827-140">[DevIQ: Repository Pattern](https://deviq.com/repository-pattern/)</span></span>
* [<span data-ttu-id="d6827-141">Dependency Injection</span><span class="sxs-lookup"><span data-stu-id="d6827-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="d6827-142">Abhängigkeitsinjektion in Ansichten</span><span class="sxs-lookup"><span data-stu-id="d6827-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="d6827-143">Abhängigkeitsinjektion in Controller</span><span class="sxs-lookup"><span data-stu-id="d6827-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="d6827-144">Abhängigkeitsinjektion in Anforderungshandlern</span><span class="sxs-lookup"><span data-stu-id="d6827-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* <span data-ttu-id="d6827-145">[Inversion of Control Containers and the Dependency Injection pattern](https://www.martinfowler.com/articles/injection.html) (Umkehrung von Steuerelementcontainern und das Abhängigkeitsinjektionsmuster)</span><span class="sxs-lookup"><span data-stu-id="d6827-145">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html)</span></span>
