---
title: Autorisierungskonventionen für Razor Pages in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Zugriff auf Seiten mit den Konventionen steuern, die Autorisierung von Benutzern und Fall jeder anonyme Benutzer den Zugriff auf Seiten oder Ordner von Seiten.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827472"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="dd537-103">Autorisierungskonventionen für Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd537-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="dd537-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dd537-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dd537-105">Eine Möglichkeit zum Steuern des Zugriffs in Ihrer app für Razor Pages ist autorisierungskonventionen beim Start verwendet.</span><span class="sxs-lookup"><span data-stu-id="dd537-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="dd537-106">Diese Konventionen können Sie zum Autorisieren von Benutzern und Fall jeder anonyme Benutzer den Zugriff auf einzelne Seiten oder Ordner von Seiten.</span><span class="sxs-lookup"><span data-stu-id="dd537-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="dd537-107">Das Übernehmen von Konventionen, die automatisch in diesem Thema beschriebenen [Autorisierungsfilter](xref:mvc/controllers/filters#authorization-filters) zum Steuern des Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="dd537-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="dd537-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dd537-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dd537-109">Die beispielanwendung verwendet [Cookieauthentifizierung ohne ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="dd537-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="dd537-110">Das Benutzerkonto für den hypothetischen Benutzer Maria Rodriguez, ist in der app hartcodiert.</span><span class="sxs-lookup"><span data-stu-id="dd537-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="dd537-111">Verwenden Sie den Benutzernamen des e-Mail-Adresse "maria.rodriguez@contoso.com" und eines Kennworts zum Anmelden des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="dd537-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="dd537-112">Der Benutzer wird authentifiziert, der `AuthenticateUser` -Methode in der die *Pages/Account/Login.cshtml.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="dd537-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="dd537-113">Im Real-World-Beispiel würde der Benutzer für eine Datenbank authentifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="dd537-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="dd537-114">Um ASP.NET Core Identity zu verwenden, befolgen Sie die Anweisungen in der [Einführung in die Identität in ASP.NET Core](xref:security/authentication/identity) Thema.</span><span class="sxs-lookup"><span data-stu-id="dd537-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="dd537-115">Die Konzepte und die Beispiele in diesem Thema gelten auch für apps, die ASP.NET Core Identity verwenden.</span><span class="sxs-lookup"><span data-stu-id="dd537-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="dd537-116">Eine Autorisierung auf eine Seite zuzugreifen</span><span class="sxs-lookup"><span data-stu-id="dd537-116">Require authorization to access a page</span></span>

<span data-ttu-id="dd537-117">Verwenden der [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) zur Seite mit den angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="dd537-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="dd537-118">Der angegebene Pfad ist der Ansichts-Engine-Pfad, der Razor-Seiten Stamm relative Pfad ohne eine Erweiterung und enthält, die nur Schrägstriche handelt.</span><span class="sxs-lookup"><span data-stu-id="dd537-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="dd537-119">Ein [AuthorizePage Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="dd537-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="dd537-120">Ein `AuthorizeFilter` kann angewendet werden, um eine Seite-Model-Klasse mit dem `[Authorize]` Standardsicherheitsfilter-Attribut.</span><span class="sxs-lookup"><span data-stu-id="dd537-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="dd537-121">Weitere Informationen finden Sie unter [Authorize-Filter-Attribut](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="dd537-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="dd537-122">Eine Autorisierung auf einen Ordner der Seiten zuzugreifen</span><span class="sxs-lookup"><span data-stu-id="dd537-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="dd537-123">Verwenden der [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="dd537-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="dd537-124">Der angegebene Pfad ist der Ansichts-Engine-Pfad, der relative Pfad des Razor-Seiten-Stamm handelt.</span><span class="sxs-lookup"><span data-stu-id="dd537-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="dd537-125">Ein [AuthorizeFolder Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="dd537-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="dd537-126">Autorisierung zum Zugriff auf die Bereichsseite ein erforderlich</span><span class="sxs-lookup"><span data-stu-id="dd537-126">Require authorization to access an area page</span></span>

<span data-ttu-id="dd537-127">Verwenden der [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf der Seite "Bereich", unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="dd537-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="dd537-128">Der Seitenname ist der Pfad der Datei ohne Erweiterung relativ zum Stammverzeichnis Seiten für den angegebenen Bereich.</span><span class="sxs-lookup"><span data-stu-id="dd537-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="dd537-129">Z. B. der Seitenname für die Datei *Areas/Identity/Pages/Manage/Accounts.cshtml* ist */Manage/Konten*.</span><span class="sxs-lookup"><span data-stu-id="dd537-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="dd537-130">Ein [AuthorizeAreaPage Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="dd537-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="dd537-131">Eine Autorisierung auf einen Ordner mit Bereichen</span><span class="sxs-lookup"><span data-stu-id="dd537-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="dd537-132">Verwenden der [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf alle Bereiche in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="dd537-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="dd537-133">Der Ordnerpfad ist der Pfad des Ordners, relativ zum Stammverzeichnis Seiten für den angegebenen Bereich.</span><span class="sxs-lookup"><span data-stu-id="dd537-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="dd537-134">Beispielsweise ist der Ordnerpfad für die Dateien unter *Bereiche/Identity/Pages/verwalten/* ist */Verwalten von*.</span><span class="sxs-lookup"><span data-stu-id="dd537-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="dd537-135">Ein [AuthorizeAreaFolder Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="dd537-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="dd537-136">Anonymen Zugriff auf eine Seite zulassen</span><span class="sxs-lookup"><span data-stu-id="dd537-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="dd537-137">Verwenden der [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) zu einer Seite unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="dd537-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="dd537-138">Der angegebene Pfad ist der Ansichts-Engine-Pfad, der Razor-Seiten Stamm relative Pfad ohne eine Erweiterung und enthält, die nur Schrägstriche handelt.</span><span class="sxs-lookup"><span data-stu-id="dd537-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="dd537-139">Zulassen des anonymen Zugriffs in einem Ordner von Seiten</span><span class="sxs-lookup"><span data-stu-id="dd537-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="dd537-140">Verwenden der [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:</span><span class="sxs-lookup"><span data-stu-id="dd537-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="dd537-141">Der angegebene Pfad ist der Ansichts-Engine-Pfad, der relative Pfad des Razor-Seiten-Stamm handelt.</span><span class="sxs-lookup"><span data-stu-id="dd537-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="dd537-142">Hinweis zur Kombination von autorisiert und anonymer Zugriff</span><span class="sxs-lookup"><span data-stu-id="dd537-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="dd537-143">Es ist durchaus gültig ist, um anzugeben, dass einen Ordner der Seiten, die eine Autorisierung, und geben an, dass eine Seite in diesem Ordner den anonymen Zugriff zulässt:</span><span class="sxs-lookup"><span data-stu-id="dd537-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="dd537-144">Das Gegenteil ist jedoch nicht "true".</span><span class="sxs-lookup"><span data-stu-id="dd537-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="dd537-145">Sie können nicht deklariert einen Ordner der Seiten, die für den anonymen Zugriff, und geben Sie die Seite für die Autorisierung in:</span><span class="sxs-lookup"><span data-stu-id="dd537-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="dd537-146">Eine Autorisierung auf der Seite für die Private erfordert funktioniert nicht, da bei der sowohl die `AllowAnonymousFilter` und `AuthorizeFilter` Filter werden angewendet, auf der Seite die `AllowAnonymousFilter` wins und steuert den Zugriff.</span><span class="sxs-lookup"><span data-stu-id="dd537-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd537-147">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="dd537-147">Additional resources</span></span>

* [<span data-ttu-id="dd537-148">Benutzerdefinierte Routen- und Seitenmodellanbieter für Razor Pages</span><span class="sxs-lookup"><span data-stu-id="dd537-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="dd537-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) Klasse</span><span class="sxs-lookup"><span data-stu-id="dd537-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
