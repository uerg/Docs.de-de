---
title: Gerüst Identität in ASP.NET Core-Projekten
author: rick-anderson
description: Erfahren Sie, wie das Gerüst für Identität in einem Projekt auf ASP.NET Core erstellen.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 8933cf9c4063bd94f7f3a9ba53b5372b443eb7c8
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758751"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="174bc-103">Gerüst Identität in ASP.NET Core-Projekten</span><span class="sxs-lookup"><span data-stu-id="174bc-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="174bc-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="174bc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="174bc-105">ASP.NET Core 2.1 und höher bietet [ASP.NET Core Identity](xref:security/authentication/identity) als eine [Razor-Klassenbibliothek](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="174bc-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="174bc-106">Anwendungen, die Identität können der Scaffolder um selektiv hinzufügen, den Quellcode in der Identität Razor Klasse Bibliothek (RCL) enthalten angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="174bc-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="174bc-107">Möglicherweise möchten Generieren von Quellcode, dadurch können Sie den Code zu ändern und das Verhalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="174bc-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="174bc-108">Sie können z. B. die Scaffolder zum Generieren des Codes in der Registrierung verwendete anweisen.</span><span class="sxs-lookup"><span data-stu-id="174bc-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="174bc-109">Generierter Code hat Vorrang vor den gleichen Code in der Identität RCL.</span><span class="sxs-lookup"><span data-stu-id="174bc-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="174bc-110">Anwendungen, die **nicht** enthalten Authentifizierung kann die Scaffolder zum Hinzufügen des Pakets RCL Identität anwenden.</span><span class="sxs-lookup"><span data-stu-id="174bc-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="174bc-111">Sie haben die Möglichkeit, auswählen von ID-Code generiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="174bc-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="174bc-112">Obwohl die Scaffolder Großteil der erforderliche Code generiert, müssen Sie Ihrem Projekt zum Abschließen des Vorgangs aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="174bc-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="174bc-113">Dieses Dokument erläutert die Schritte zum Abschließen eines Identität Gerüstbau Updates erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="174bc-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="174bc-114">Wenn die Identität Scaffolder ausgeführt wird, eine *ScaffoldingReadme.txt* Datei ist im Projektverzeichnis erstellt.</span><span class="sxs-lookup"><span data-stu-id="174bc-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="174bc-115">Die *ScaffoldingReadme.txt* -Datei enthält allgemeine Anweisungen auf, was erforderlich ist, um die Identität des Gerüstbaus Aktualisierung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="174bc-115">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="174bc-116">Dieses Dokument enthält ausführlichere Anweisungen als das Lesen der *ScaffoldingReadme.txt* Datei.</span><span class="sxs-lookup"><span data-stu-id="174bc-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="174bc-117">Es wird empfohlen, mit einem Quellcodeverwaltungssystem, die die Unterschiede zwischen und bietet die Möglichkeit, außerhalb des gültigen Änderungen sichern.</span><span class="sxs-lookup"><span data-stu-id="174bc-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="174bc-118">Überprüfen Sie die Änderungen nach dem Ausführen der Identität Scaffolder.</span><span class="sxs-lookup"><span data-stu-id="174bc-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="174bc-119">Gerüst Identität in ein leeres Projekt</span><span class="sxs-lookup"><span data-stu-id="174bc-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="174bc-120">Fügen Sie die folgenden hervorgehobenen Aufrufe an die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="174bc-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="174bc-121">Gerüst Identität in einer Razor-Projekt, ohne vorhandene Autorisierung</span><span class="sxs-lookup"><span data-stu-id="174bc-121">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="174bc-122">Identität konfiguriert ist, *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="174bc-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="174bc-123">Weitere Informationen finden Sie unter [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="174bc-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="174bc-124">Migrationen, UseAuthentication und layout</span><span class="sxs-lookup"><span data-stu-id="174bc-124">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="174bc-125">In der `Configure` Methode der `Startup` -Klasse, rufen Sie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) nach `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="174bc-125">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="174bc-126">Änderungen am Layout</span><span class="sxs-lookup"><span data-stu-id="174bc-126">Layout changes</span></span>

<span data-ttu-id="174bc-127">Optional: Fügen Sie der partiellen Anmeldung hinzu (`_LoginPartial`) in der Layoutdatei:</span><span class="sxs-lookup"><span data-stu-id="174bc-127">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="174bc-128">Gerüst Identität in einer Razor-Projekt mit Autorisierung</span><span class="sxs-lookup"><span data-stu-id="174bc-128">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="174bc-129">Einige identitätsoptionen im konfiguriert sind *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="174bc-129">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="174bc-130">Weitere Informationen finden Sie unter [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="174bc-130">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="174bc-131">Gerüst Identität in einer MVC-Projekt, ohne vorhandene Autorisierung</span><span class="sxs-lookup"><span data-stu-id="174bc-131">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="174bc-132">Optional: Fügen Sie der partiellen Anmeldung hinzu (`_LoginPartial`), die *Views/Shared/_Layout.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="174bc-132">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="174bc-133">Verschieben der *Pages/Shared/_LoginPartial.cshtml* Datei *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="174bc-133">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="174bc-134">Identität konfiguriert ist, *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="174bc-134">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="174bc-135">Weitere Informationen finden Sie unter IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="174bc-135">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="174bc-136">Rufen Sie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) nach `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="174bc-136">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="174bc-137">Gerüst Identität in einer MVC-Projekt mit Autorisierung</span><span class="sxs-lookup"><span data-stu-id="174bc-137">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="174bc-138">Löschen der *Seiten/freigegebene* Ordner und Dateien in diesem Ordner.</span><span class="sxs-lookup"><span data-stu-id="174bc-138">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
