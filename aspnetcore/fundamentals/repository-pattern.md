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
# <a name="repository-pattern-with-aspnet-core"></a>Repositorymuster mit ASP.NET Core

Von [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) und [Luke Latham](https://github.com/guardrex)

Das *Repositorymuster* ist ein Entwurfsmuster, das den Datenzugriff hinter Schnittstellenabstraktionen isoliert. Die Verbindung mit der Datenbank und die Bearbeitung der Datenspeicherobjekte erfolgen mithilfe von Methoden, die durch die Implementierung der Schnittstelle bereitgestellt werden. Daher ist es nicht notwendig, Code aufzurufen, um verschiedene datenbankbezogene Aspekte zu verarbeiten, z.B. Verbindungen, Befehle und Reader.

Die Implementierung des Repositorymusters mit ASP.NET Core bietet folgende Vorteile:

* Die Organisation einer App wird vereinfacht, da keine direkte gegenseitige Abhängigkeit zwischen der Geschäftsebene und der Datenzugriffsebene besteht.
* Code für den Datenbankzugriff lässt sich leichter wiederverwenden, weil der Code in einem oder mehreren Repositorys zentral verwaltet wird.
* Komponententests für die Geschäftsdomäne können unabhängig von der Datenbankebene durchgeführt werden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) verwendet das Repositorymuster, um eine Liste mit Namen von Filmfiguren zu initialisieren und anzuzeigen. Die App verwendet [Entity Framework Core](/ef/core/) und eine `ApplicationDbContext`-Klasse für die Datenpersistenz, aber die Datenbankinfrastruktur, in der der Datenzugriff erfolgt, ist nicht offensichtlich. Datenzugriff und Datenbankobjekte sind hinter einem [Repository](https://martinfowler.com/eaaCatalog/repository.html) abstrahiert.

## <a name="repository-interface"></a>Repositoryschnittstelle

Eine Repositoryschnittstelle definiert die Eigenschaften und Methoden für eine Implementierung. In der Beispiel-App lautet die Repositoryschnittstelle für die Filmfigurendaten `ICharacterRepository`. `ICharacterRepository` definiert die Methoden `ListAll` und `Add`, die zum Arbeiten mit `Character`-Instanzen in der App erforderlich sind:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` wird folgendermaßen definiert:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Konkreter Repositorytyp

Die Schnittstelle wird durch einen konkreten Typ implementiert. In der Beispiel-App verwaltet `CharacterRepository` den Datenbankkontext und implementiert die Methoden `ListAll` und `Add`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Registrieren des Repositorydiensts

Das Repository und der Datenbankkontext werden beim Dienstcontainer in `Startup.ConfigureServices` registriert. In der Beispiel-App wird `ApplicationDbContext` mit dem Aufruf der Erweiterungsmethode [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) konfiguriert. `ICharacterRepository` wird als bereichsbezogener Dienst registriert:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Einfügen einer Instanz des Repositorys

In einer Klasse, in der Datenbankzugriff erforderlich ist, wird eine Instanz des Repositorys über den Konstruktor angefordert und einem privaten Feld zur Verwendung durch Klassenmethoden zugewiesen. In der Beispiel-App wird `ICharacterRepository` zu folgenden Zwecken verwendet:

* Auffüllen der Datenbank, wenn keine Figuren vorhanden sind
* Abrufen einer Liste der Figuren zur Anzeige.

Beachten Sie, dass der aufrufende Code nur mit `CharacterRepository`, der Implementierung der Schnittstelle, interagiert. Der aufrufende Code verwendet den `ApplicationDbContext` nicht direkt:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Generische Repositoryschnittstelle

Dieses Thema und die Beispiel-App veranschaulichen die einfachste Implementierung des Repositorymusters, in der für jedes Geschäftsobjekt ein Repository erstellt wird. Wenn die App größer wird und mehr als nur einige wenige Objekte enthält, kann eine *generische Repositoryschnittstelle* die Menge an Code reduzieren, die zur Implementierung des Repositorymusters erforderlich ist. Weitere Informationen finden Sie unter [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/) (DevIQ: Repositorymuster: generische Repositoryschnittstelle).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [DevIQ: Repository Pattern](https://deviq.com/repository-pattern/) (DevIQ: Repositorymuster)
* [Dependency Injection](xref:fundamentals/dependency-injection)
* [Abhängigkeitsinjektion in Ansichten](xref:mvc/views/dependency-injection)
* [Abhängigkeitsinjektion in Controller](xref:mvc/controllers/dependency-injection)
* [Abhängigkeitsinjektion in Anforderungshandlern](xref:security/authorization/dependencyinjection)
* [Inversion of Control Containers and the Dependency Injection pattern](https://www.martinfowler.com/articles/injection.html) (Umkehrung von Steuerelementcontainern und das Abhängigkeitsinjektionsmuster)
