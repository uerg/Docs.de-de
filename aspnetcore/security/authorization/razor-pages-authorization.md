---
title: Razor-Seiten Autorisierung Konventionen in ASP.NET Core
author: guardrex
description: "Informationen Sie zum Zugriff auf den Seiten mit den Konventionen zu steuern, die Autorisierung von Benutzern und ermöglichen anonyme Benutzern den Zugriff auf Seiten oder Ordner von Seiten."
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: bbef653c6cf968527e753df9c853f5972640cc03
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="e0658-103">Razor-Seiten Autorisierung Konventionen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0658-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="e0658-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e0658-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e0658-105">Eine Möglichkeit zum Steuern des Zugriffs in Ihrer app Razor-Seiten ist Autorisierung Konventionen beim Start verwendet.</span><span class="sxs-lookup"><span data-stu-id="e0658-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e0658-106">Diese Konventionen können Sie zum Autorisieren von Benutzern und Zulassen von anonymen Benutzern den Zugriff auf einzelne Seiten oder Ordner von Seiten.</span><span class="sxs-lookup"><span data-stu-id="e0658-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e0658-107">Die Konventionen, die automatisch in diesem Thema beschriebenen gelten [Autorisierungsfilter](xref:mvc/controllers/filters#authorization-filters) zum Steuern des Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="e0658-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e0658-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e0658-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e0658-109">Autorisierung zum Zugriff auf einer Seite erforderlich</span><span class="sxs-lookup"><span data-stu-id="e0658-109">Require authorization to access a page</span></span>

<span data-ttu-id="e0658-110">Verwenden der [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf der Seite unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="e0658-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e0658-111">Der angegebene Pfad ist Ansichtsmodul-Pfads, der Razor-Seiten Stamm relativen Pfad ohne Erweiterung und enthält nur Schrägstriche ist.</span><span class="sxs-lookup"><span data-stu-id="e0658-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e0658-112">Ein [AuthorizePage Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="e0658-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e0658-113">Benötigen Sie die Autorisierung zum Zugriff auf eines Ordners von Seiten</span><span class="sxs-lookup"><span data-stu-id="e0658-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e0658-114">Verwenden der [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="e0658-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e0658-115">Der angegebene Pfad ist Ansichtsmodul-Pfads, der relative Razor-Seiten Stammpfad ist.</span><span class="sxs-lookup"><span data-stu-id="e0658-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e0658-116">Ein [AuthorizeFolder Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="e0658-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e0658-117">Anonymen Zugriff auf eine Seite zulassen</span><span class="sxs-lookup"><span data-stu-id="e0658-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="e0658-118">Verwenden der [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) zu einer Seite im angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="e0658-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e0658-119">Der angegebene Pfad ist Ansichtsmodul-Pfads, der Razor-Seiten Stamm relativen Pfad ohne Erweiterung und enthält nur Schrägstriche ist.</span><span class="sxs-lookup"><span data-stu-id="e0658-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e0658-120">Anonymen Zugriff auf einen Ordner der Seiten zulassen</span><span class="sxs-lookup"><span data-stu-id="e0658-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e0658-121">Verwenden der [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="e0658-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e0658-122">Der angegebene Pfad ist Ansichtsmodul-Pfads, der relative Razor-Seiten Stammpfad ist.</span><span class="sxs-lookup"><span data-stu-id="e0658-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e0658-123">Hinweis zur Kombination von autorisiert und anonymem Zugriff</span><span class="sxs-lookup"><span data-stu-id="e0658-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e0658-124">Es ist perfekt geeignet, um anzugeben, dass einen Ordner der Seiten, die Autorisierung erfordern, und gibt an, dass eine Seite in diesem Ordner den anonymen Zugriff zulässt:</span><span class="sxs-lookup"><span data-stu-id="e0658-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e0658-125">Das Gegenteil ist jedoch nicht "true".</span><span class="sxs-lookup"><span data-stu-id="e0658-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="e0658-126">Sie können nicht deklarieren einen Ordner der Seiten, die für den anonymen Zugriff, und geben Sie die Seite für die Autorisierung in:</span><span class="sxs-lookup"><span data-stu-id="e0658-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="e0658-127">Die notwendige Autorisierung auf der Seite "Private" funktioniert nicht, da bei der sowohl die `AllowAnonymousFilter` und `AuthorizeFilter` Filter werden angewendet, um die Seite die `AllowAnonymousFilter` gewinnt, und steuert den Zugriff.</span><span class="sxs-lookup"><span data-stu-id="e0658-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="e0658-128">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="e0658-128">See also</span></span>

* [<span data-ttu-id="e0658-129">Benutzerdefinierte Routen- und Seitenmodellanbieter für Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="e0658-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="e0658-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) Klasse</span><span class="sxs-lookup"><span data-stu-id="e0658-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
