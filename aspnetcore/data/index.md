---
title: Arbeiten mit Daten in ASP.NET Core
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe0ba2ef
ms.technology: aspnet
ms.prod: asp.net-core
ms.openlocfilehash: 3566127476289ae085a9161132b103638bc9b068
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-data-in-aspnet-core"></a><span data-ttu-id="f240e-103">Arbeiten mit Daten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f240e-103">Working with Data in ASP.NET Core</span></span> 

*   [<span data-ttu-id="f240e-104">Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f240e-104">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](ef-mvc/index.md)
    *   [<span data-ttu-id="f240e-105">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="f240e-105">Getting started</span></span>](ef-mvc/intro.md)
    *   [<span data-ttu-id="f240e-106">Create-, Read-, Update- und Delete-Vorgänge (CRUD)</span><span class="sxs-lookup"><span data-stu-id="f240e-106">Create, Read, Update, and Delete operations</span></span>](ef-mvc/crud.md)
    *   [<span data-ttu-id="f240e-107">Sortieren, Filtern, Paginieren und Gruppieren</span><span class="sxs-lookup"><span data-stu-id="f240e-107">Sorting, filtering, paging, and grouping</span></span>](ef-mvc/sort-filter-page.md)
    *   [<span data-ttu-id="f240e-108">Migrationen</span><span class="sxs-lookup"><span data-stu-id="f240e-108">Migrations</span></span>](ef-mvc/migrations.md)
    *   [<span data-ttu-id="f240e-109">Erstellen eines komplexen Datenmodells</span><span class="sxs-lookup"><span data-stu-id="f240e-109">Creating a complex data model</span></span>](ef-mvc/complex-data-model.md)
    *   [<span data-ttu-id="f240e-110">Lesen dazugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="f240e-110">Reading related data</span></span>](ef-mvc/read-related-data.md)
    *   [<span data-ttu-id="f240e-111">Aktualisieren dazugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="f240e-111">Updating related data</span></span>](ef-mvc/update-related-data.md)
    *   [<span data-ttu-id="f240e-112">Behandeln von Parallelitätskonflikten</span><span class="sxs-lookup"><span data-stu-id="f240e-112">Handling concurrency conflicts</span></span>](ef-mvc/concurrency.md)
    *   [<span data-ttu-id="f240e-113">Vererbung</span><span class="sxs-lookup"><span data-stu-id="f240e-113">Inheritance</span></span>](ef-mvc/inheritance.md)
    *   [<span data-ttu-id="f240e-114">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="f240e-114">Advanced topics</span></span>](ef-mvc/advanced.md)
* <span data-ttu-id="f240e-115">[ASP.NET Core mit EF Core – neue Datenbank](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core-Dokumentationswebsite)</span><span class="sxs-lookup"><span data-stu-id="f240e-115">[ASP.NET Core with EF Core - new database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core documentation site)</span></span>
* <span data-ttu-id="f240e-116">[ASP.NET Core mit EF Core – bestehende Datenbank](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core-Dokumentationswebsite)</span><span class="sxs-lookup"><span data-stu-id="f240e-116">[ASP.NET Core with EF Core - existing database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core documentation site)</span></span>
*   [<span data-ttu-id="f240e-117">Erste Schritte mit ASP.NET Core und Entity Framework Core 6</span><span class="sxs-lookup"><span data-stu-id="f240e-117">Getting Started with ASP.NET Core and Entity Framework 6</span></span>](entity-framework-6.md)
*   [<span data-ttu-id="f240e-118">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f240e-118">Azure Storage</span></span>](azure-storage/index.md)
    *   [<span data-ttu-id="f240e-119">Hinzufügen von Azure Storage mithilfe von verbundenen Visual Studio-Diensten</span><span class="sxs-lookup"><span data-stu-id="f240e-119">Adding Azure Storage by Using Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-azure-tools-connected-services-storage/)
    *   [<span data-ttu-id="f240e-120">Erste Schritte mit Azure Blob Storage und verbundenen Visual Studio-Diensten</span><span class="sxs-lookup"><span data-stu-id="f240e-120">Get Started with Azure Blob storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)
    *   [<span data-ttu-id="f240e-121">Erste Schritte mit Queue Storage und verbundenen Visual Studio-Diensten</span><span class="sxs-lookup"><span data-stu-id="f240e-121">Get Started with Queue Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-queues/)
    *   [<span data-ttu-id="f240e-122">Erste Schritte mit Azure Table Storage und verbundenen Visual Studio-Diensten</span><span class="sxs-lookup"><span data-stu-id="f240e-122">How to Get Started with Azure Table Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-tables/)
