---
title: Autorisierungskonventionen für Razor Pages in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Zugriff auf Seiten mit den Konventionen steuern, die Autorisierung von Benutzern und Fall jeder anonyme Benutzer den Zugriff auf Seiten oder Ordner von Seiten.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207263"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Autorisierungskonventionen für Razor Pages in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Eine Möglichkeit zum Steuern des Zugriffs in Ihrer app für Razor Pages ist autorisierungskonventionen beim Start verwendet. Diese Konventionen können Sie zum Autorisieren von Benutzern und Fall jeder anonyme Benutzer den Zugriff auf einzelne Seiten oder Ordner von Seiten. Das Übernehmen von Konventionen, die automatisch in diesem Thema beschriebenen [Autorisierungsfilter](xref:mvc/controllers/filters#authorization-filters) zum Steuern des Zugriffs.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

Die beispielanwendung verwendet [Cookieauthentifizierung ohne ASP.NET Core Identity](xref:security/authentication/cookie). Das Benutzerkonto für den hypothetischen Benutzer Maria Rodriguez, ist in der app hartcodiert. Verwenden Sie den Benutzernamen des e-Mail-Adresse "maria.rodriguez@contoso.com" und eines Kennworts zum Anmelden des Benutzers. Der Benutzer wird authentifiziert, der `AuthenticateUser` -Methode in der die *Pages/Account/Login.cshtml.cs* Datei. Im Real-World-Beispiel würde der Benutzer für eine Datenbank authentifiziert werden. Um ASP.NET Core Identity zu verwenden, befolgen Sie die Anweisungen in der [Einführung in die Identität in ASP.NET Core](xref:security/authentication/identity) Thema. Die Konzepte und die Beispiele in diesem Thema gelten auch für apps, die ASP.NET Core Identity verwenden.

## <a name="require-authorization-to-access-a-page"></a>Eine Autorisierung auf eine Seite zuzugreifen

Verwenden der [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) zur Seite mit den angegebenen Pfad:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Der angegebene Pfad ist der Ansichts-Engine-Pfad, der Razor-Seiten Stamm relative Pfad ohne eine Erweiterung und enthält, die nur Schrägstriche handelt.

Ein [AuthorizePage Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Ein `AuthorizeFilter` kann angewendet werden, um eine Seite-Model-Klasse mit dem `[Authorize]` Standardsicherheitsfilter-Attribut. Weitere Informationen finden Sie unter [Authorize-Filter-Attribut](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Eine Autorisierung auf einen Ordner der Seiten zuzugreifen

Verwenden der [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Der angegebene Pfad ist der Ansichts-Engine-Pfad, der relative Pfad des Razor-Seiten-Stamm handelt.

Ein [AuthorizeFolder Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Autorisierung zum Zugriff auf die Bereichsseite ein erforderlich

Verwenden der [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf der Seite "Bereich", unter dem angegebenen Pfad:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Der Seitenname ist der Pfad der Datei ohne Erweiterung relativ zum Stammverzeichnis Seiten für den angegebenen Bereich. Z. B. der Seitenname für die Datei *Areas/Identity/Pages/Manage/Accounts.cshtml* ist */Manage/Konten*.

Ein [AuthorizeAreaPage Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Eine Autorisierung auf einen Ordner mit Bereichen

Verwenden der [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) auf alle Bereiche in einem Ordner unter dem angegebenen Pfad:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Der Ordnerpfad ist der Pfad des Ordners, relativ zum Stammverzeichnis Seiten für den angegebenen Bereich. Beispielsweise ist der Ordnerpfad für die Dateien unter *Bereiche/Identity/Pages/verwalten/* ist */Verwalten von*.

Ein [AuthorizeAreaFolder Überladung](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) ist verfügbar, wenn Sie eine Autorisierungsrichtlinie angeben müssen.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Anonymen Zugriff auf eine Seite zulassen

Verwenden der [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) zu einer Seite unter dem angegebenen Pfad:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Der angegebene Pfad ist der Ansichts-Engine-Pfad, der Razor-Seiten Stamm relative Pfad ohne eine Erweiterung und enthält, die nur Schrägstriche handelt.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zulassen des anonymen Zugriffs in einem Ordner von Seiten

Verwenden der [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konvention über [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) Hinzufügen einer [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) für alle Seiten in einem Ordner unter dem angegebenen Pfad:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Der angegebene Pfad ist der Ansichts-Engine-Pfad, der relative Pfad des Razor-Seiten-Stamm handelt.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Hinweis zur Kombination von autorisiert und anonymer Zugriff

Es ist durchaus gültig ist, um anzugeben, dass einen Ordner der Seiten, die eine Autorisierung, und geben an, dass eine Seite in diesem Ordner den anonymen Zugriff zulässt:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Das Gegenteil ist jedoch nicht "true". Sie können nicht deklariert einen Ordner der Seiten, die für den anonymen Zugriff, und geben Sie die Seite für die Autorisierung in:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Eine Autorisierung auf der Seite für die Private erfordert funktioniert nicht, da bei der sowohl die `AllowAnonymousFilter` und `AuthorizeFilter` Filter werden angewendet, auf der Seite die `AllowAnonymousFilter` wins und steuert den Zugriff.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Benutzerdefinierte Routen- und Seitenmodellanbieter für Razor Pages](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) Klasse
