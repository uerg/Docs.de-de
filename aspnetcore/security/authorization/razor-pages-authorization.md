---
title: Razor-Seiten Autorisierung Konventionen in ASP.NET Core
author: guardrex
description: Informationen Sie zum Zugriff auf den Seiten mit den Konventionen zu steuern, die Autorisierung von Benutzern und ermöglichen anonyme Benutzern den Zugriff auf Seiten oder Ordner von Seiten.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272674"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="a0943-103">Razor-Seiten Autorisierung Konventionen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0943-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="a0943-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a0943-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a0943-105">Eine Möglichkeit zum Steuern des Zugriffs in Ihrer app Razor-Seiten ist Autorisierung Konventionen beim Start verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0943-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="a0943-106">Diese Konventionen können Sie zum Autorisieren von Benutzern und Zulassen von anonymen Benutzern den Zugriff auf einzelne Seiten oder Ordner von Seiten.</span><span class="sxs-lookup"><span data-stu-id="a0943-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="a0943-107">Die Konventionen, die automatisch in diesem Thema beschriebenen gelten [Autorisierungsfilter](xref:mvc/controllers/filters#authorization-filters) zum Steuern des Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="a0943-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="a0943-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0943-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a0943-109">Das Beispiel-app verwendet [Cookieauthentifizierung ohne ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="a0943-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="a0943-110">Das Benutzerkonto für den Benutzer hypothetischen Maria Rodriguez, ist in der Anwendung hartcodiert.</span><span class="sxs-lookup"><span data-stu-id="a0943-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="a0943-111">Verwenden Sie die e-Mail-Benutzernamens "maria.rodriguez@contoso.com" und einem Kennwort zur Anmeldung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="a0943-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="a0943-112">Der Benutzer wird authentifiziert, der `AuthenticateUser` Methode in der *Pages/Account/Login.cshtml.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="a0943-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="a0943-113">In einem realen Beispiel würde der Benutzer für eine Datenbank authentifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="a0943-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="a0943-114">Um ASP.NET Core Identity zu verwenden, führen Sie die Anweisungen in der [Einführung in die Identität auf ASP.NET Core](xref:security/authentication/identity) Thema.</span><span class="sxs-lookup"><span data-stu-id="a0943-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="a0943-115">Die Konzepte und Beispiele in diesem Thema gelten auch für apps, die ASP.NET Core Identity zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="a0943-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="a0943-116">Autorisierung zum Zugriff auf einer Seite erforderlich</span><span class="sxs-lookup"><span data-stu-id="a0943-116">Require authorization to access a page</span></span>

<span data-ttu-id="a0943-117">Verwenden der [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf der Seite unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="a0943-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="a0943-118">Der angegebene Pfad ist Ansichtsmodul-Pfads, der Razor-Seiten Stamm relativen Pfad ohne Erweiterung und enthält nur Schrägstriche ist.</span><span class="sxs-lookup"><span data-stu-id="a0943-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="a0943-119">Ein [AuthorizePage Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="a0943-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="a0943-120">Ein `AuthorizeFilter` kann angewendet werden, um eine Modellklasse Seite mit den `[Authorize]` Standardsicherheitsfilter-Attribut.</span><span class="sxs-lookup"><span data-stu-id="a0943-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="a0943-121">Weitere Informationen finden Sie unter [autorisieren Filterattribut](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="a0943-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="a0943-122">Benötigen Sie die Autorisierung zum Zugriff auf eines Ordners von Seiten</span><span class="sxs-lookup"><span data-stu-id="a0943-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="a0943-123">Verwenden der [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="a0943-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="a0943-124">Der angegebene Pfad ist Ansichtsmodul-Pfads, der relative Razor-Seiten Stammpfad ist.</span><span class="sxs-lookup"><span data-stu-id="a0943-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="a0943-125">Ein [AuthorizeFolder Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="a0943-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="a0943-126">Anonymen Zugriff auf eine Seite zulassen</span><span class="sxs-lookup"><span data-stu-id="a0943-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="a0943-127">Verwenden der [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) zu einer Seite im angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="a0943-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="a0943-128">Der angegebene Pfad ist Ansichtsmodul-Pfads, der Razor-Seiten Stamm relativen Pfad ohne Erweiterung und enthält nur Schrägstriche ist.</span><span class="sxs-lookup"><span data-stu-id="a0943-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="a0943-129">Anonymen Zugriff auf einen Ordner der Seiten zulassen</span><span class="sxs-lookup"><span data-stu-id="a0943-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="a0943-130">Verwenden der [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="a0943-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="a0943-131">Der angegebene Pfad ist Ansichtsmodul-Pfads, der relative Razor-Seiten Stammpfad ist.</span><span class="sxs-lookup"><span data-stu-id="a0943-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="a0943-132">Hinweis zur Kombination von autorisiert und anonymem Zugriff</span><span class="sxs-lookup"><span data-stu-id="a0943-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="a0943-133">Es ist perfekt geeignet, um anzugeben, dass einen Ordner der Seiten, die Autorisierung erfordern, und gibt an, dass eine Seite in diesem Ordner den anonymen Zugriff zulässt:</span><span class="sxs-lookup"><span data-stu-id="a0943-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="a0943-134">Das Gegenteil ist jedoch nicht "true".</span><span class="sxs-lookup"><span data-stu-id="a0943-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="a0943-135">Sie können nicht deklarieren einen Ordner der Seiten, die für den anonymen Zugriff, und geben Sie die Seite für die Autorisierung in:</span><span class="sxs-lookup"><span data-stu-id="a0943-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="a0943-136">Die notwendige Autorisierung auf der Seite "Private" funktioniert nicht, da bei der sowohl die `AllowAnonymousFilter` und `AuthorizeFilter` Filter werden angewendet, um die Seite die `AllowAnonymousFilter` gewinnt, und steuert den Zugriff.</span><span class="sxs-lookup"><span data-stu-id="a0943-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0943-137">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a0943-137">Additional resources</span></span>

* [<span data-ttu-id="a0943-138">Benutzerdefinierte Routen- und Seitenmodellanbieter für Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a0943-138">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="a0943-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) Klasse</span><span class="sxs-lookup"><span data-stu-id="a0943-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
