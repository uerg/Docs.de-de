---
title: Gerüst-Identität in ASP.NET Core-Projekten
author: rick-anderson
description: Erfahren Sie, wie Sie die Identität in einem ASP.NET Core-Projekt zu erstellen.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217681"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="b5419-103">Gerüst-Identität in ASP.NET Core-Projekten</span><span class="sxs-lookup"><span data-stu-id="b5419-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="b5419-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b5419-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5419-105">ASP.NET Core 2.1 und höher bietet [ASP.NET Core Identity](xref:security/authentication/identity) als eine [Razor-Klassenbibliothek](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="b5419-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="b5419-106">Anwendungen, die Identität enthalten, können der gerüstbauer um selektiv hinzufügen, den in der Identität Razor Klasse-Bibliothek (RCL) enthaltenen Quellcode anwenden.</span><span class="sxs-lookup"><span data-stu-id="b5419-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="b5419-107">Sie sollten Quellcode generieren, um den Code und das Verhalten ändern zu können.</span><span class="sxs-lookup"><span data-stu-id="b5419-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="b5419-108">Sie können das Gerüst beispielsweise anweisen, den bei der Registrierung verwendeten Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="b5419-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="b5419-109">Generierter Code hat Vorrang vor dem gleichen Code in der Razor-Klassenbibliothek „Identität“.</span><span class="sxs-lookup"><span data-stu-id="b5419-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="b5419-110">Um erhalten vollständige Kontrolle über die Benutzeroberfläche, und verwenden Sie nicht die Standardeinstellung RCL, finden Sie im Abschnitt [erstellen vollständige Benutzeroberfläche identitätsquelle](#full).</span><span class="sxs-lookup"><span data-stu-id="b5419-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="b5419-111">Anwendungen, die **nicht** gehören Authentifizierung kann der gerüstbauer zum Hinzufügen des Pakets RCL Identität anwenden.</span><span class="sxs-lookup"><span data-stu-id="b5419-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="b5419-112">Sie können Code der Klassenbibliothek „Identität“ auswählen, der generiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="b5419-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="b5419-113">Obwohl der gerüstbauer größte Teil des erforderlichen Codes generiert, müssen Sie Ihr Projekt zum Abschließen des Vorgangs aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="b5419-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="b5419-114">Dieses Dokument erläutert die Schritte, um eine Identität Gerüstbau Aktualisierung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="b5419-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="b5419-115">Wenn der Identity-gerüstbauer ausgeführt wird, eine *ScaffoldingReadme.txt* Datei wird im Projektverzeichnis erstellt.</span><span class="sxs-lookup"><span data-stu-id="b5419-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="b5419-116">Die *ScaffoldingReadme.txt* -Datei enthält allgemeine Anweisungen, für welche Anforderungen für die Identity-Gerüstbau-Aktualisierung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="b5419-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="b5419-117">Dieses Dokument enthält ausführlichere Anweisungen als die *ScaffoldingReadme.txt* Datei.</span><span class="sxs-lookup"><span data-stu-id="b5419-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="b5419-118">Es empfiehlt sich ein Quellcodeverwaltungssystem, die Unterschiede zwischen zeigt und lässt sich aus Änderungen zurück.</span><span class="sxs-lookup"><span data-stu-id="b5419-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="b5419-119">Überprüfen Sie die Änderungen nach dem Ausführen der gerüstbauer Identität.</span><span class="sxs-lookup"><span data-stu-id="b5419-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="b5419-120">Gerüst-Identität in ein leeres Projekt</span><span class="sxs-lookup"><span data-stu-id="b5419-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b5419-121">Fügen Sie folgenden hervorgehobenen Aufrufe der `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b5419-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="b5419-122">Gerüst-Identität in einer Razor-Projekt ohne vorhandene Autorisierung</span><span class="sxs-lookup"><span data-stu-id="b5419-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="b5419-123">Identität wird im konfiguriert *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b5419-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b5419-124">Weitere Informationen finden Sie unter [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="b5419-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="b5419-125">Migrationen, UseAuthentication und layout</span><span class="sxs-lookup"><span data-stu-id="b5419-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="b5419-126">In der `Configure` Methode der `Startup` Klasse, rufen Sie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) nach `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="b5419-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="b5419-127">Änderungen am Layout</span><span class="sxs-lookup"><span data-stu-id="b5419-127">Layout changes</span></span>

<span data-ttu-id="b5419-128">Optional: Hinzufügen die Anmeldung, die teilweise (`_LoginPartial`) zur Layoutdatei:</span><span class="sxs-lookup"><span data-stu-id="b5419-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="b5419-129">Gerüst-Identität in einer Razor-Projekt mit Autorisierung</span><span class="sxs-lookup"><span data-stu-id="b5419-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]<span data-ttu-id="b5419-130"> Einige identitätsoptionen in konfiguriert *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b5419-130"> Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b5419-131">Weitere Informationen finden Sie unter [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="b5419-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="b5419-132">Gerüst-Identität in einem MVC-Projekt ohne vorhandene Autorisierung</span><span class="sxs-lookup"><span data-stu-id="b5419-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="b5419-133">Optional: Hinzufügen die Anmeldung, die teilweise (`_LoginPartial`) auf die *Views/Shared/_Layout.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="b5419-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="b5419-134">Verschieben der *Pages/Shared/_LoginPartial.cshtml* Datei *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b5419-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="b5419-135">Identität wird im konfiguriert *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b5419-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b5419-136">Weitere Informationen finden Sie unter IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="b5419-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="b5419-137">Rufen Sie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) nach `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="b5419-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="b5419-138">Gerüst-Identität in einem MVC-Projekt mit Autorisierung</span><span class="sxs-lookup"><span data-stu-id="b5419-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="b5419-139">Löschen der *Seiten/Shared* Ordner und die Dateien in diesem Ordner.</span><span class="sxs-lookup"><span data-stu-id="b5419-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="b5419-140">Erstellen Sie die vollständige Benutzeroberfläche identitätsquelle</span><span class="sxs-lookup"><span data-stu-id="b5419-140">Create full identity UI source</span></span>

<span data-ttu-id="b5419-141">Um vollständige Kontrolle über die Identity-Benutzeroberfläche gewährleisten zu können, führen Sie die Identity-gerüstbauer, und wählen Sie **alle Dateien überschreiben**.</span><span class="sxs-lookup"><span data-stu-id="b5419-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="b5419-142">Der folgende hervorgehobene Code zeigt die Änderungen an die Identity-Standardbenutzeroberfläche mit Identity in einer ASP.NET Core 2.1-Web-app zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="b5419-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="b5419-143">Möglicherweise möchten diese Option, um die vollständige Kontrolle über die Benutzeroberfläche der Identität haben.</span><span class="sxs-lookup"><span data-stu-id="b5419-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="b5419-144">Der Standard-Identität wird im folgenden Code ersetzt:</span><span class="sxs-lookup"><span data-stu-id="b5419-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="b5419-145">Die folgenden legt der Code die [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [logoutpath für](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), und [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="b5419-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="b5419-146">Registrieren einer `IEmailSender` Implementierung, z.B.:</span><span class="sxs-lookup"><span data-stu-id="b5419-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
