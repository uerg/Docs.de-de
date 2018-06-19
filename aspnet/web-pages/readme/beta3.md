---
uid: web-pages/readme/beta3
title: Web Matrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei | Microsoft Docs
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892880"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="67c32-103">WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei</span><span class="sxs-lookup"><span data-stu-id="67c32-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="67c32-104">WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei</span><span class="sxs-lookup"><span data-stu-id="67c32-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="67c32-105">9 November 2010</span><span class="sxs-lookup"><span data-stu-id="67c32-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="67c32-106">Inhalt</span><span class="sxs-lookup"><span data-stu-id="67c32-106">Contents</span></span>

- [<span data-ttu-id="67c32-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="67c32-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="67c32-108">Installation</span><span class="sxs-lookup"><span data-stu-id="67c32-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="67c32-109">Neue Features, Änderungen und bekannte Probleme in der Beta 3-Version</span><span class="sxs-lookup"><span data-stu-id="67c32-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="67c32-110">Probleme bei der Installation von WebMatrix</span><span class="sxs-lookup"><span data-stu-id="67c32-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="67c32-111">ASP.NET-Webseiten 2</span><span class="sxs-lookup"><span data-stu-id="67c32-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="67c32-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="67c32-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="67c32-113">Installieren von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="67c32-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="67c32-114">Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="67c32-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="67c32-115">Andere Probleme</span><span class="sxs-lookup"><span data-stu-id="67c32-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="67c32-116">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="67c32-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="67c32-117">Übersicht</span><span class="sxs-lookup"><span data-stu-id="67c32-117">Overview</span></span>

> <span data-ttu-id="67c32-118">Betaversion von Microsoft WebMatrix ist ein kostenloses Web Development Stapel, der in Minuten installiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="67c32-119">Einen Webserver integriert mit der Datenbank und die Programmierung Frameworks So erstellen eine einzige, integrierte Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="67c32-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="67c32-120">Sie können WebMatrix Beta verwenden, um die Möglichkeit zu optimieren, die Sie schreiben, testen und Ihre eigenen ASP.NET- oder PHP-Website veröffentlichen, oder Sie können WebMatrix Beta verwenden, zum Starten einer neuen Website mithilfe beliebte Open Source-Anwendungen z. B. DotNetNuke, Umbraco, WordPress oder Joomla.</span><span class="sxs-lookup"><span data-stu-id="67c32-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="67c32-121">Betaversion von WebMatrix verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausführen werden, gestaltet sich der Übergang von der Entwicklung bis hin zur Produktion reibungs- und nahtlos.</span><span class="sxs-lookup"><span data-stu-id="67c32-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="67c32-122">Installation</span><span class="sxs-lookup"><span data-stu-id="67c32-122">Installation</span></span>

> <span data-ttu-id="67c32-123">Verwenden Sie für die Installation von WebMatrix Beta 3 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="67c32-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="67c32-124">Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zur Installation von WebMatrix Beta 3 verwenden.</span><span class="sxs-lookup"><span data-stu-id="67c32-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="67c32-125">Wenn Sie während der Installation Probleme auftreten, finden Sie in [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="67c32-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="67c32-126">Anweisungen zum Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="67c32-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="67c32-127">Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="67c32-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="67c32-128">Neue Funktionen, die Änderungen, AndKnown Probleme</span><span class="sxs-lookup"><span data-stu-id="67c32-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="67c32-129">WebMatrix Beta 3-Installation</span><span class="sxs-lookup"><span data-stu-id="67c32-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="67c32-130">Problem: WebMatrix Beta 3 ist nur verfügbar für Plattformen, die Microsoft .NET Framework 4 zu unterstützen</span><span class="sxs-lookup"><span data-stu-id="67c32-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="67c32-131">.NET Framework, Version 4 ist erforderlich für die Betaversion von WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="67c32-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="67c32-132">In bestimmten Fällen kann können der WebMatrix-Beta-Installer Sie versuchen, auf einer anderen Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist.</span><span class="sxs-lookup"><span data-stu-id="67c32-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="67c32-133">Insbesondere Windows Vista ohne das Update für SP1 können Sie die Installation von WebMatrix Beta zu starten, aber die .NET Framework 4-Komponente fehl, und blockieren die Installation.</span><span class="sxs-lookup"><span data-stu-id="67c32-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="67c32-134">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-134">**Workaround**</span></span>  
> <span data-ttu-id="67c32-135">Installieren Sie auf einer unterstützten Plattform, darunter:</span><span class="sxs-lookup"><span data-stu-id="67c32-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="67c32-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="67c32-136">Windows 7</span></span>
> - <span data-ttu-id="67c32-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="67c32-137">Windows Server 2008</span></span>
> - <span data-ttu-id="67c32-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="67c32-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="67c32-139">Windows Vista SP1 oder höher</span><span class="sxs-lookup"><span data-stu-id="67c32-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="67c32-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="67c32-140">Windows XP SP3</span></span>
> - <span data-ttu-id="67c32-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="67c32-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="67c32-142">Problem: WebMatrix Beta 3 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist</span><span class="sxs-lookup"><span data-stu-id="67c32-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="67c32-143">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-143">**Workaround**</span></span>  
> <span data-ttu-id="67c32-144">Installieren Sie [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) aus dem Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="67c32-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="67c32-145">Problem: Einige Assemblys für SQL Server Compact 4.0 werden nicht im GAC installiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="67c32-146">Die verwalteten Assemblys für SQL Server Compact 4.0 werden nicht im globalen Assemblycache (GAC) abgelegt, wenn Sie SQL Server Compact 4.0 auf einem 64-Bit-Computer installieren, und der Computer nur .NET Framework 3.5 SP1-Client Profile installiert verfügt.</span><span class="sxs-lookup"><span data-stu-id="67c32-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="67c32-147">Sind die verwalteten Assemblys, die nicht im GAC installiert werden:</span><span class="sxs-lookup"><span data-stu-id="67c32-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="67c32-148">*"System.Data.SqlServerCe.dll"* (ADO.NET-Anbieter)</span><span class="sxs-lookup"><span data-stu-id="67c32-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="67c32-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="67c32-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="67c32-150">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-150">**Workaround**</span></span>  
> <span data-ttu-id="67c32-151">Deinstallieren von SQLServer Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="67c32-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="67c32-152">Herunterladen Sie und installieren Sie die Vollversion von .NET Framework 3.5 SP1 vom folgenden Speicherort:</span><span class="sxs-lookup"><span data-stu-id="67c32-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="67c32-153">Microsoft .NET Framework 3.5 Service Pack 1 (gesamtes Paket)</span><span class="sxs-lookup"><span data-stu-id="67c32-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="67c32-154">Neuinstallieren von SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="67c32-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="67c32-155">Problem: Nicht SQL Server Compact über die Befehlszeile deinstallieren.</span><span class="sxs-lookup"><span data-stu-id="67c32-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="67c32-156">Deinstallation von SQL Server Compact über Befehlszeilenoptionen funktioniert nicht in dieser Version.</span><span class="sxs-lookup"><span data-stu-id="67c32-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="67c32-157">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-157">**Workaround**</span></span>  
> <span data-ttu-id="67c32-158">Verwendung *Programme und Funktionen* in der Windows-Systemsteuerung zum Deinstallieren von Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="67c32-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="67c32-159">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="67c32-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="67c32-160">Dieser Abschnitt des Dokuments Beschreibt neue Funktionen, Änderungen und bekannte Probleme bei der Beta 3-Version von ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="67c32-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="67c32-161">Neue Funktionen</span><span class="sxs-lookup"><span data-stu-id="67c32-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="67c32-162">Änderungen</span><span class="sxs-lookup"><span data-stu-id="67c32-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="67c32-163">Probleme</span><span class="sxs-lookup"><span data-stu-id="67c32-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="67c32-164">Neue Funktionen in der Beta 3 für ASP.NET Web Pages mit Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="67c32-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="67c32-165">Neu: "Html.Raw"-Methode nicht codierte Markup rendert</span><span class="sxs-lookup"><span data-stu-id="67c32-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="67c32-166">Die neue `Html.Raw` -Methode ermöglicht es Ihnen das Rendern von HTML-Markup als Markup und codierte Ausgabe rendern.</span><span class="sxs-lookup"><span data-stu-id="67c32-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="67c32-167">(Standardmäßig codiert ASP.NET Razor Zeichenfolgen vor dem Rendern sie.) Die Syntax lautet:</span><span class="sxs-lookup"><span data-stu-id="67c32-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="67c32-168">Das folgende Beispiel veranschaulicht die Verwendung von `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="67c32-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="67c32-169">Änderungen in der Beta 3 für ASP.NET Web Pages mit Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="67c32-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="67c32-170">Änderung: "HrefAttribute"-Methode entfernt</span><span class="sxs-lookup"><span data-stu-id="67c32-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="67c32-171">Die `HrefAttribute` Methode der `WebPage` Klasse wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="67c32-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="67c32-172">Dieses Hilfsprogramm wurde verwendet, um unsichere Zeichen in URLs zu codieren.</span><span class="sxs-lookup"><span data-stu-id="67c32-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="67c32-173">Es ist nicht mehr erforderlich, da ASP.NET Razor automatisch Zeichenfolgen codiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="67c32-174">(Verwenden Sie die neue `Html.Raw` aufzurufende Methode nicht codierte Zeichenfolgen zu rendern.)</span><span class="sxs-lookup"><span data-stu-id="67c32-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="67c32-175">Änderung: Syntax für deklarative "@helper" Hilfsprogramme geändert</span><span class="sxs-lookup"><span data-stu-id="67c32-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="67c32-176">In der Beta 3-Version ASP.NET ändert wie diese Hilfsprogramme analysiert, die erstellt werden, mithilfe der `@helper` Syntax.</span><span class="sxs-lookup"><span data-stu-id="67c32-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="67c32-177">Im Wesentlichen die `@helper` Syntax wird als Codeblock statt als Block des Markups, die Code enthalten, kann jetzt analysiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="67c32-178">Daher Code in das Hilfsprogramm muss nicht in eingeschlossen werden `@{ }` blockiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="67c32-179">Umgekehrt Markup in das Hilfsprogramm muss explizit eingefügt, in HTML-Elemente oder in ASP.NET Razor `<text></text>` Tags.</span><span class="sxs-lookup"><span data-stu-id="67c32-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="67c32-180">Beispielsweise die folgenden `@helper` Syntax funktioniert in der Beta 3-Version:</span><span class="sxs-lookup"><span data-stu-id="67c32-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="67c32-181">In der Beta 3-Version muss dieses Hilfsprogramm geändert werden, um wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="67c32-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="67c32-182">Beachten Sie, dass die `@{ }` Zeichen um den ursprünglichen Code in das Hilfsprogramm wird nicht mehr verwendet.</span><span class="sxs-lookup"><span data-stu-id="67c32-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="67c32-183">Dies ist, da der Inhalt der Hilfsprogramme standardmäßig als Codeblock behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="67c32-184">Das Hilfsobjekt rendert Markup, der mit dem öffnenden beginnt `<a>` Tag.</span><span class="sxs-lookup"><span data-stu-id="67c32-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="67c32-185">Wenn das Hilfsprogramm gerendert werden muss, nur-Text oder Tags, die kein schließendes Tag (z. B. `<meta>` Tags), muss der zu rendernden Inhalt `<text></text>` Tags.</span><span class="sxs-lookup"><span data-stu-id="67c32-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="67c32-186">Change: "WebPageContext.HttpContext" removed</span><span class="sxs-lookup"><span data-stu-id="67c32-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="67c32-187">Die `WebPageContext.HttpContext` Eigenschaft entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="67c32-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="67c32-188">Verwenden Sie stattdessen `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="67c32-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="67c32-189">(Die `WebPageContext.HttpContext` -Eigenschaft umschlossen dies einfach.)</span><span class="sxs-lookup"><span data-stu-id="67c32-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="67c32-190">Änderung: "Facebook"-Hilfsprogramm zum neuen Paket verschoben</span><span class="sxs-lookup"><span data-stu-id="67c32-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="67c32-191">Die `Facebook` Hilfsprogramm wurde in der *Facebook.Helper* Klassenbibliothek mit dem `Facebook` -Hilfsobjekt und zusätzliche Funktionen.</span><span class="sxs-lookup"><span data-stu-id="67c32-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="67c32-192">Sie müssen Installieren dieser Bibliothek als separates Paket, wie unter "Installieren von Hilfsprogrammen Paket-Manager" im Lernprogramm [erste Schritte mit ASP.NET Webseiten](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="67c32-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="67c32-193">Ändern: Wechselt Mitgliedschaft, Rollen und Sicherheit der Typen zur neuen assembly</span><span class="sxs-lookup"><span data-stu-id="67c32-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="67c32-194">Die folgenden Typen wurden verschoben, um die `WebMatrix.WebData` Assembly:</span><span class="sxs-lookup"><span data-stu-id="67c32-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="67c32-195">Änderung: "TagBuilder"-Klasse System.Web.WebPages.dll Assembly verschoben</span><span class="sxs-lookup"><span data-stu-id="67c32-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="67c32-196">Die `TagBuilde` R-Klasse auf die Assembly System.Web.WebPages.dll verschoben wurde.</span><span class="sxs-lookup"><span data-stu-id="67c32-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="67c32-197">Bisher war dies in einer Assembly, die Teil von ASP.NET MVC waren.</span><span class="sxs-lookup"><span data-stu-id="67c32-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="67c32-198">Diese Änderung bedeutet, dass Sie nicht ASP.NET MVC zu installieren, damit Sie verwenden die `TagBuilder` Klasse.</span><span class="sxs-lookup"><span data-stu-id="67c32-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="67c32-199">Die Klasse ist jedoch weiterhin in der `System.Web.Mvc` Namespace.</span><span class="sxs-lookup"><span data-stu-id="67c32-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="67c32-200">Zum Verwenden der `TagBuilder` -Klasse (z. B. in einem benutzerdefinierten ASP.NET Razor-Hilfsprogramm), müssen Sie den Namespace verweisen (z. B. durch Hinzufügen von `@using System.Web.Mvc` für Ihren Code).</span><span class="sxs-lookup"><span data-stu-id="67c32-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="67c32-201">: Änderungsanforderung Überprüfung Syntax geändert; Entfernt "Validation"-Klasse</span><span class="sxs-lookup"><span data-stu-id="67c32-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="67c32-202">In der Beta 3-Version zum Deaktivieren der Validierung für ein einzelnes Feld oder eine Gruppe von Feldern, Sie erreichen die `Validation.Exclude` -Methode auf und übergibt den oder die Namen der Felder, die aus der Überprüfung ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="67c32-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="67c32-203">Eine neue Syntax ist verfügbar in der Beta 3-Version für die Überprüfung umgehen.</span><span class="sxs-lookup"><span data-stu-id="67c32-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="67c32-204">Die `Validation` in der Beta 3 verwendete Methode entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="67c32-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="67c32-205">Wenn Sie die anforderungsüberprüfung, nicht deaktivieren, wenn Benutzer versuchen, die HTML-Markup (z. B. mithilfe eines rich-Text-Editors auf einer Seite) hochladen, meldet die Website eine Fehlermeldung wie *ein potenziell gefährlicher Request.Form-Wert wurde erkannt, vom Client*und die Benutzereingabe wird nicht akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="67c32-206">Wenn Anforderungsvalidierung Sie deaktivieren, müssen Sie manuell überprüfen von Benutzereingaben, um sicherzustellen, dass nicht potenziell gefährlichen Markup enthalten oder Skript verwenden, etwa die [Microsoft Anti-Cross-Site Scripting Bibliothek v4. 0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="67c32-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="67c32-207">Um automatische Anforderungsvalidierung deaktivieren, rufen die `Request.Unvalidated` -Methode, und übergeben sie den Namen des Felds oder andere Post-Objekt, das Anforderungsvalidierung für umgangen werden soll.</span><span class="sxs-lookup"><span data-stu-id="67c32-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="67c32-208">Sie können diese Methode verwenden, umgehen die Validierung für alle Elemente in der `Form`, `QueryString`, `Cookies`, und `ServerVariables` Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="67c32-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="67c32-209">Den folgenden Beispielen wird veranschaulicht, wie Sie die `Unvalidated` Methode:</span><span class="sxs-lookup"><span data-stu-id="67c32-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="67c32-210">Bekannte Probleme bei der ASP.NET Web Pages mit Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="67c32-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="67c32-211">Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="67c32-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="67c32-212">Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode.</span><span class="sxs-lookup"><span data-stu-id="67c32-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="67c32-213">(In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn die `autoCreateTables` Parameter dieser Methode wird festgelegt auf "true" (, es ist standardmäßig festgelegt in der Vorlage Starter Site "true"), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="67c32-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="67c32-214">Stattdessen wird automatisch der Tabelle erstellt.</span><span class="sxs-lookup"><span data-stu-id="67c32-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="67c32-215">Dies kann ein Problem sein, wenn Sie beabsichtigen, verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft jedoch übergeben den Namen der falschen Tabelle die `WebSecurity.InitializeDatabaseConnection` Methode.</span><span class="sxs-lookup"><span data-stu-id="67c32-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="67c32-216">Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung angezeigt, zu funktionieren.</span><span class="sxs-lookup"><span data-stu-id="67c32-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="67c32-217">Anwendungscode, der für die benutzerdefinierte Tabelle (und auf Felder) nutzt kann jedoch letztendlich mit unerwarteten Fehlern fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="67c32-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="67c32-218">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-218">**Workaround**</span></span>  
> <span data-ttu-id="67c32-219">Stellen Sie sicher, dass der Name im übergeben der `InitializeDatabaseConnection` Methode entspricht das Benutzerprofil in der Mitgliedschaftsdatenbank-Tabelle ab, oder stellen Sie sicher, dass die `autoCreateTables` Parameter auf "false" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="67c32-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="67c32-220">Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler</span><span class="sxs-lookup"><span data-stu-id="67c32-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="67c32-221">Wenn eine Webanwendung mit WebMatrix SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, wird möglicherweise einen Fehler angezeigt, der angibt, dass SQL Server nicht lokalen Anwendungspfad des Benutzers zur Laufzeit abrufen.</span><span class="sxs-lookup"><span data-stu-id="67c32-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="67c32-222">**Problemumgehung** stellen Sie sicher, dass das Windows-Konto, das die Anwendung unter (i. d. r. NETWORK SERVICE) ausgeführt wird, wie z. B. Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner hat *App\_Daten*.</span><span class="sxs-lookup"><span data-stu-id="67c32-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="67c32-223">Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [bei Problemen mit SQL Server Express Benutzerinstanziierung und ASP.net Web-Anwendungsprojekten](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="67c32-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="67c32-224">Problem: In Visual Studio-Namespaces für benutzerdefinierte Assemblys (DLLs) nicht automatisch importiert</span><span class="sxs-lookup"><span data-stu-id="67c32-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="67c32-225">Wenn Sie benutzerdefinierte Assemblys in einem Projekt in Visual Studio verwenden, werden die Namespaces deklariert, die in diesen Assemblys zur Entwurfszeit nicht automatisch importiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="67c32-226">Folglich Verweise auf benutzerdefinierte Typen möglicherweise nicht zur Entwurfszeit erkannt werden und werden als nicht erkannt, in Visual Studio (mit einem "Wellenlinie") gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="67c32-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="67c32-227">Dieses Problem tritt nur zur Entwurfszeit in Visual Studio; die Anwendung selbst wird ordnungsgemäß ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="67c32-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="67c32-228">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-228">**Workaround**</span></span>  
> <span data-ttu-id="67c32-229">Einschließen einer `using` Anweisung (`imports` in Visual Basic), die auf die Entitäten, die zur Entwurfszeit nicht erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="67c32-230">Problem: Visual Studio IntelliSense-Projekt Vorlagen und nur in ASP.NET MVC 3-Version verfügbar</span><span class="sxs-lookup"><span data-stu-id="67c32-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="67c32-231">Installieren ASP.NET Web Pages installiert nicht auch Tools für Visual Studio z. B. IntelliSense und Projekt Vorlagen für ASP.NET Web Pages-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="67c32-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="67c32-232">**Problemumgehung** um IntelliSense und Projekt Vorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="67c32-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="67c32-233">Problem: "&lt;Helper&gt; Klasse wurde nicht gefunden" Fehler</span><span class="sxs-lookup"><span data-stu-id="67c32-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="67c32-234">Nach dem upgrade auf Beta 3 wird möglicherweise eines Fehlers, die eine Hilfsklasse (z. B. die `Facebook` Klasse) kann nicht gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="67c32-235">Beginnend in Beta 2 und 3 Beta, wurden Hilfen in Paketen verschoben, die Sie explizit installieren müssen.</span><span class="sxs-lookup"><span data-stu-id="67c32-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="67c32-236">Vorhandene Websites nicht aktualisiert, um diese Pakete enthalten; Dazu gehören Standorte in der *\My Documents\IISExpress* oder *\My Documents\My Websites* Ordner.</span><span class="sxs-lookup"><span data-stu-id="67c32-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="67c32-237">Insbesondere, sehen Sie diesen Fehler bei der Verwendung der Standardwebsite in *Meine Standorte* (WebSite1), enthält einen Verweis auf die `Twitter` Helper.</span><span class="sxs-lookup"><span data-stu-id="67c32-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="67c32-238">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-238">**Workaround**</span></span>  
> <span data-ttu-id="67c32-239">Kommentieren Sie Aufrufe an alle Hilfsprogramme am Standort, und führen Sie die  *\_Admin* Seite, und installieren Sie das Paket oder Pakete, die die Hilfsprogramme enthalten, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="67c32-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="67c32-240">Nachdem Sie das Paket installiert haben, können Sie die auskommentierung der Zeilen, die Hilfsmethoden verweisen.</span><span class="sxs-lookup"><span data-stu-id="67c32-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="67c32-241">Problem: Bereitstellen von Beta 3 ASP.NET Razor-Assemblys in den Ordner "Bin" funktioniert möglicherweise nicht auf das Hosten von Websites</span><span class="sxs-lookup"><span data-stu-id="67c32-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="67c32-242">Wenn Sie eine ASP.NET Web Pages-Website für eine hosting-Website bereitstellen, und wenn Sie die ASP.NET Razor Beta 3-Assemblys für die Website bereit *"bin"* Ordner, treten möglicherweise Fehler auf, einschließlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="67c32-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="67c32-243">Dies kann geschehen, wenn der Hostinganbieter die ASP.NET Web Pages Beta 1-Assemblys in der Server globale Cache (GAC) installiert hat.</span><span class="sxs-lookup"><span data-stu-id="67c32-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="67c32-244">Assemblys im GAC erhalten Vorrang gegenüber Assemblys, die lokal im installiert die *"bin"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="67c32-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="67c32-245">**Problemumgehung** wenden Sie sich an Ihrem Hostinganbieter, um sicherzustellen, dass der Fehler wird angezeigt, werden aufgrund eines Konflikts zwischen verschiedenen Versionen des Anbieters werden die Assemblys und Ihren Instanzen-.</span><span class="sxs-lookup"><span data-stu-id="67c32-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="67c32-246">Wenn dies der Fall ist, fordern Sie, dass es sich bei der Hostinganbieter die Assemblys im GAC der Server aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="67c32-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="67c32-247">Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver</span><span class="sxs-lookup"><span data-stu-id="67c32-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="67c32-248">Wenn der Server mit dem Standort hinter einem Proxyserver ist, müssen Sie möglicherweise konfigurieren Proxyinformationen in die *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die sich außerhalb Ihrer Website stammen.</span><span class="sxs-lookup"><span data-stu-id="67c32-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="67c32-249">Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber vom Proxyserver blockiert werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="67c32-250">Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed vom Paketmanager verwendet verwendet werden die Proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="67c32-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="67c32-251">Wenn Sie Probleme beim Arbeiten mit einem externen Dienst oder Arbeiten mit dem feed-Paket auf, Stammverzeichnis Ihrer Anwendung die folgenden Elemente abgelegt *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="67c32-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="67c32-252">Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="67c32-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="67c32-253">Problem: "Die Microsoft.Web.Infrastructure.dll kann nicht geladen werden" angezeigt</span><span class="sxs-lookup"><span data-stu-id="67c32-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="67c32-254">Wenn Sie zuvor die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax installiert und dann die Beta 3-Version installieren, werden alle entsprechenden Assemblys im GAC außer installiert *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="67c32-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="67c32-255">Daher bei der Ausführung des ASP.NET Razor Seiten Fehlermeldung angezeigt wird, der angibt, *Microsoft.Web.Infrastructure.dll* konnte nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="67c32-256">Dieses Problem tritt nicht auf, wenn Sie über die Beta 3-Version auf einem "sauberen" Computer geladen werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="67c32-257">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-257">**Workaround**</span></span>  
> <span data-ttu-id="67c32-258">Deinstallieren Sie ASP.NET Web Pages, in der Systemsteuerung.</span><span class="sxs-lookup"><span data-stu-id="67c32-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="67c32-259">Installieren Sie die Beta 3-Version erneut.</span><span class="sxs-lookup"><span data-stu-id="67c32-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="67c32-260">Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert</span><span class="sxs-lookup"><span data-stu-id="67c32-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="67c32-261">Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist ASP.NET Web Pages mit Razor-Syntax deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="67c32-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="67c32-262">Seiten mit den *cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="67c32-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="67c32-263">ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* -Datei, und Entfernen von .NET Framework entfernt diese Datei.</span><span class="sxs-lookup"><span data-stu-id="67c32-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="67c32-264">Installieren .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="67c32-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="67c32-265">**Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="67c32-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="67c32-266">Dadurch wird das folgende Element auf der *"Web.config"* Datei im Stammverzeichnis Computer, i. d. r. an folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="67c32-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="67c32-267">Problem: Treten Fehler auf Anwendungen, die zuvor mit ASP.NET-Assemblys im Ordner "Bin" bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="67c32-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="67c32-268">Während der Bereitstellung von Kopien der ASP.NET Web Pages-Assemblys (z. B. *Microsoft.WebPages.dll*), die *"bin"* Ordner der Website auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="67c32-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="67c32-269">(Dies kann während der Bereitstellung automatisch Ursache sein oder weil der Entwickler explizit die Assemblys kopiert.) Wenn die Beta 3-Version installiert ist, Fehler erfolgt jedoch, wie z. B. Fehler, die bestimmte Typen nicht gefunden werden können.</span><span class="sxs-lookup"><span data-stu-id="67c32-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="67c32-270">Dies tritt auf, da eine Reihe von ASP.NET Web Pages-Datentypen in verschiedenen Namespaces für Beta 3-Version verschoben wurden.</span><span class="sxs-lookup"><span data-stu-id="67c32-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="67c32-271">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="67c32-271">**Workaround** </span></span>  
> <span data-ttu-id="67c32-272">Deaktivieren der *"bin"* Ordner der bereitgestellten Anwendung anzeigen, kopieren Sie den neuen Assemblys in den Ordner (oder erneute Bereitstellung der Anwendung), und klicken Sie dann die Anwendung neu starten.</span><span class="sxs-lookup"><span data-stu-id="67c32-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="67c32-273">Problem: URLs ohne Erweiterung.cshtml/.vbhtml Dateien unter IIS 7 oder IIS 7.5 nicht gefunden</span><span class="sxs-lookup"><span data-stu-id="67c32-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="67c32-274">Für IIS 7 oder IIS 7.5 mit einer URL wie die folgende Anforderungen können sich nicht mehr Seiten zu suchen, die *cshtml* oder *vbhtml* Erweiterung:</span><span class="sxs-lookup"><span data-stu-id="67c32-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="67c32-275">Das Problem tritt auf, da die URLs nicht standardmäßig für IIS 7 oder IIS 7.5 aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="67c32-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="67c32-276">Das likeliest Szenario ist, dass das Problem nicht angezeigt werden, werden Wenn es sich bei Tests lokal mit IIS Express jedoch auftreten, wenn Sie Ihre Website für eine hosting-Website bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="67c32-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="67c32-277">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="67c32-278">Wenn Sie die Kontrolle über den Server-Computer verfügen, installieren Sie auf dem Servercomputer in beschriebenen Updates [ein Update ist verfügbar, die ermöglicht, die bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="67c32-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="67c32-279">Wenn Sie keine Kontrolle über den Server-Computer verfügen (z. B., dass Sie die Bereitstellung für eine hosting-Website), fügen Sie Folgendes auf der Website *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="67c32-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="67c32-280">Problem: Verwenden von Webanwendungsprojekt oder ASP.NET MVC und ASP.NET Web Pages in derselben Anwendung</span><span class="sxs-lookup"><span data-stu-id="67c32-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="67c32-281">Wenn Sie ASP.NET Web Pages in einem Webanwendungsprojekt oder ASP.NET MVC-Anwendung verwendet haben, wird möglicherweise einen Fehler, *WebPageHttpApplication* kann nicht gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="67c32-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="67c32-282">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-282">**Workaround**</span></span>  
> <span data-ttu-id="67c32-283">Wenn Sie diesen Fehler erhalten, ändern Sie die Basisklasse, von der die Anwendung abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="67c32-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="67c32-284">In der *"Global.asax"* file, ändern Sie die folgende Zeile:</span><span class="sxs-lookup"><span data-stu-id="67c32-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="67c32-285">folgendermaßen:</span><span class="sxs-lookup"><span data-stu-id="67c32-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="67c32-286">Dies kehrt faktisch eine Änderung, die eingeführt wurde für die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="67c32-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="67c32-287">Problem: Bereitstellen einer Anwendung auf einem Computer, die keine SQL Server Compact installiert</span><span class="sxs-lookup"><span data-stu-id="67c32-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="67c32-288">Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, auf dem SQL Server Compact nicht installiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="67c32-289">Microsoft WebMatrix Beta 3 automatisch kopiert diese Binärdateien für Sie und führt die entsprechenden *"Web.config"* Datei Transformationen.</span><span class="sxs-lookup"><span data-stu-id="67c32-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="67c32-290">**Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="67c32-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="67c32-291">Kopieren Sie die Datenbank-Engine-Assemblys, die *"bin"* Ordner (und Unterordner) der Anwendung auf dem Zielcomputer:</span><span class="sxs-lookup"><span data-stu-id="67c32-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="67c32-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="67c32-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="67c32-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="67c32-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="67c32-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="67c32-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="67c32-295">Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="67c32-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="67c32-296">(In WebMatrix Beta 3 dieser Dateityp ist verfügbar, wenn Sie auf **alle** in der **wählen Sie einen Dateityp** (Dialogfeld).)</span><span class="sxs-lookup"><span data-stu-id="67c32-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="67c32-297">Fügen Sie das folgende Element als untergeordnetes Element von der **&lt;Konfiguration&gt;** Element (befindet sich nicht in der **&lt;system.web&gt;** Element):</span><span class="sxs-lookup"><span data-stu-id="67c32-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="67c32-298">Problem: Datenbank- und WebGrid Hilfsprogramme funktionieren nicht in in Visual Basic mit mittlerer Vertrauenswürdigkeit</span><span class="sxs-lookup"><span data-stu-id="67c32-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="67c32-299">Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung, die Verwendung mit mittlerer Vertrauenswürdigkeit festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="67c32-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="67c32-300">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-300">**Workaround**</span></span>  
> <span data-ttu-id="67c32-301">Legen Sie vorübergehend die Anwendung mit voller Vertrauenswürdigkeit aus.</span><span class="sxs-lookup"><span data-stu-id="67c32-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="67c32-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="67c32-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="67c32-303">Problem: Die Eigenschaft "Encrypt" wird nicht erkannt.</span><span class="sxs-lookup"><span data-stu-id="67c32-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="67c32-304">SQL Server Compact 4.0 nicht erkennt die `Encrypt` Eigenschaft von der `SqlCeConnection` Klasse.</span><span class="sxs-lookup"><span data-stu-id="67c32-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="67c32-305">Sie sollten diese Eigenschaft nicht verwenden, zum Verschlüsseln von Datenbankdateien.</span><span class="sxs-lookup"><span data-stu-id="67c32-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="67c32-306">Die `Encrypt` Eigenschaft wurde in SQLServer Compact 3.5-Version als veraltet markiert und nur für Abwärtskompatibilität beibehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="67c32-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="67c32-307">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-307">**Workaround**</span></span>  
> <span data-ttu-id="67c32-308">Verwenden der `Encryption Mode` Eigenschaft von der `SqlCeConnection` Klasse, um SQL Server Compact 4.0-Datenbankdateien zu verschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="67c32-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="67c32-309">Im folgende Beispiel wird gezeigt, wie zum Erstellen einer verschlüsselten SQL Server Compact 4.0-Datenbank, mit der `Encryption Mode` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="67c32-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="67c32-310">Um den Verschlüsselungsmodus einer vorhandenen SQL Server Compact 4.0-Datenbank zu ändern, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="67c32-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="67c32-311">Um eine nicht verschlüsselte SQL Server Compact 4.0-Datenbank zu verschlüsseln, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="67c32-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="67c32-312">Problem: Der Microsoft Visual C++ 2008-Laufzeitbibliotheken sind erforderlich</span><span class="sxs-lookup"><span data-stu-id="67c32-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="67c32-313">Die systemeigene DLLs von SQL Server Compact 4.0 benötigen, die Microsoft Visual C++ 2008-Laufzeitbibliotheken (x 86, IA64,- und x 64), Servicepack 1.</span><span class="sxs-lookup"><span data-stu-id="67c32-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="67c32-314">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-314">**Workaround**</span></span>  
> <span data-ttu-id="67c32-315">Installieren Sie .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="67c32-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="67c32-316">Dies wird auch das Visual C++ 2008-Runtime-Bibliotheken SP1 installiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="67c32-317">Sie können die Bibliotheken von folgendem Speicherort herunterladen:</span><span class="sxs-lookup"><span data-stu-id="67c32-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="67c32-318">ATL-Sicherheitsupdate für Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package</span><span class="sxs-lookup"><span data-stu-id="67c32-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="67c32-319">Beachten Sie diese Installation von .NET Framework 2.0, 3.0, oder 4 ist *nicht* Visual C++ 2008-Runtime-Bibliotheken SP1 installieren.</span><span class="sxs-lookup"><span data-stu-id="67c32-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="67c32-320">Problem: Wenn SQL Server Compact vor der Installation von .NET Framework auf dem Computer installiert, ist der invariante Anbietername nicht in der .NET Framework-Datei "Machine.config" registriert</span><span class="sxs-lookup"><span data-stu-id="67c32-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="67c32-321">SQL Server Compact kann auf einem Computer installiert werden, die nicht .NET Framework installiert werden, da SQL Server Compact .NET Framework erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="67c32-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="67c32-322">Wenn weder .NET Framework Version 3.5 noch 4 installiert ist, bevor Sie SQL Server Compact installieren, die SQL Server Compact-Setup registriert keine der invariante Name des Anbieters in der *"Machine.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="67c32-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="67c32-323">Jede Anwendung, auf dem SQL Server Compact Eintrag in beruht, der *"Machine.config"* Datei schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="67c32-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="67c32-324">Der invariante Name des Registrierungseintrags in *"Machine.config"* sieht wie im folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="67c32-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="67c32-325">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-325">**Workaround**</span></span>  
> <span data-ttu-id="67c32-326">Deinstallieren Sie SQL Server Compact 4.0 CTP1 an.</span><span class="sxs-lookup"><span data-stu-id="67c32-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="67c32-327">Herunterladen Sie und installieren Sie die Vollversion von .NET Framework von folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="67c32-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="67c32-328">Microsoft .NET Framework 3.5 Service Pack 1 (gesamtes Paket)</span><span class="sxs-lookup"><span data-stu-id="67c32-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="67c32-329">Microsoft .NET Framework 4.0-Version (vollständiges Paket)</span><span class="sxs-lookup"><span data-stu-id="67c32-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="67c32-330">Neuinstallieren von [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="67c32-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="67c32-331">Installieren von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="67c32-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="67c32-332">Problem: Installieren einer Anwendung kann sehr lange dauern, wenn Ordner Eigene Dokumente des Benutzers zu einer Netzwerkfreigabe umgeleitet wird</span><span class="sxs-lookup"><span data-stu-id="67c32-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="67c32-333">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-333">**Workaround**</span></span>  
> <span data-ttu-id="67c32-334">Keine</span><span class="sxs-lookup"><span data-stu-id="67c32-334">None.</span></span> <span data-ttu-id="67c32-335">Die Anwendung möglicherweise einige Zeit dauern zu installieren, jedoch ordnungsgemäß installiert.</span><span class="sxs-lookup"><span data-stu-id="67c32-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="67c32-336">Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="67c32-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="67c32-337">Problem: Standort funktioniert möglicherweise nicht nach dem Veröffentlichen, wenn das Feld "Ziel-URL" nicht mit http:// oder https:// vorangestellt ist</span><span class="sxs-lookup"><span data-stu-id="67c32-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="67c32-338">In der **Veröffentlichungseinstellungen** (Dialogfeld), wenn die Ziel-URL nicht mit beginnt `http://` oder `https://`, der Standort funktioniert möglicherweise nicht nach der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="67c32-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="67c32-339">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-339">**Workaround**</span></span>  
> <span data-ttu-id="67c32-340">Stellen Sie sicher, dass vor dem Veröffentlichen eines Standorts, die Ziel-URL in die **Veröffentlichungseinstellungen** Dialogfeld beginnt mit `http://` oder `https://`.</span><span class="sxs-lookup"><span data-stu-id="67c32-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="67c32-341">Problem: Veröffentlichen einer MySQL-Datenbank tritt der Fehler "Fehler beim Veröffentlichen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="67c32-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="67c32-342">Dies kann geschehen, wenn die Remotedatenbank das Skript ausgeführt werden kann."</span><span class="sxs-lookup"><span data-stu-id="67c32-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="67c32-343">Der Fehler kann eine Reihe von Gründen auftreten.</span><span class="sxs-lookup"><span data-stu-id="67c32-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="67c32-344">Ein Grund, dass Sie diese Fehlermeldung ist Datenbankskripts enthält eine einfache Anführungszeichen (') und Standard-Zeichensatzes für das Ziel MySQL-Datenbank ist nicht in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="67c32-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="67c32-345">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-345">**Workaround**</span></span>  
> <span data-ttu-id="67c32-346">Legen Sie den Standardzeichensatz für die MySQL-Remotedatenbank in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="67c32-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="67c32-347">Andere Probleme</span><span class="sxs-lookup"><span data-stu-id="67c32-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="67c32-348">Problem: Suchfilter/funktioniert nicht in Berichten für Group By: Problemtyp</span><span class="sxs-lookup"><span data-stu-id="67c32-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="67c32-349">Beim Ausführen eines Berichts für einen Standort bei der Eingabe von Text in der *Filter durch URL* Feld, und klicken Sie auf *Suche*, geschieht nichts.</span><span class="sxs-lookup"><span data-stu-id="67c32-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="67c32-350">Dies ist, da dieses Steuerelement nicht funktionsfähig ist die *Group By* Zustand des Berichts wird festgelegt, um *Problemtyp*, dies ist die Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="67c32-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="67c32-351">**Problemumgehung** In der *Group By* Registerkarte des Menübands, klicken Sie auf *URL* um die Einträge durch ihre Quell-URL zu gruppieren.</span><span class="sxs-lookup"><span data-stu-id="67c32-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="67c32-352">Das Textfeld und einer Schaltfläche zum Filtern der Einträge werden in diesem Zustand funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="67c32-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="67c32-353">Problem: WCF-Anwendungen nicht mit IIS Express ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="67c32-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="67c32-354">Navigieren zu einer WCF-Anwendung führt zu einem Fehler wie den folgenden Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="67c32-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="67c32-355">*Konnte nicht geladen werden, Datei oder Assembly "" Microsoft.Web.Administration ", Version = 7.0.0.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35" oder eine ihrer Abhängigkeiten. Das System konnte die angegebene Datei nicht finden.*</span><span class="sxs-lookup"><span data-stu-id="67c32-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="67c32-356">Dies tritt auf, da IIS Express-Betaversion WCF standardmäßig nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="67c32-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="67c32-357">**Problemumgehung** eine der folgenden problemumgehungen verwenden (problemumgehung #2 erfordert Microsoft Windows Vista oder höher):</span><span class="sxs-lookup"><span data-stu-id="67c32-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="67c32-358">Kopieren der *Microsoft.Web.dll* und *Microsoft.Web.Administration.dll* Assemblys aus dem WebMatrix-Installationsverzeichnis in die *"bin"* Verzeichnis WCF die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="67c32-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="67c32-359">Standardmäßig WebMatrix installiert ist, der *Microsoft WebMatrix* Unterordner des Systems *Programmdateien* Ordner.</span><span class="sxs-lookup"><span data-stu-id="67c32-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="67c32-360">Unter Microsoft Windows Vista oder höher, erstellen Sie eine "Symlink" für die Assemblys in der *"bin"* Verzeichnis mit den folgenden Befehlen.</span><span class="sxs-lookup"><span data-stu-id="67c32-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="67c32-361">(Dieser Ansatz hat den Vorteil, dass er eine Kopie der Assemblys keine erstellt wird.)</span><span class="sxs-lookup"><span data-stu-id="67c32-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="67c32-362">Installieren Sie die zwei Assemblys im GAC.</span><span class="sxs-lookup"><span data-stu-id="67c32-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="67c32-363">Führen Sie in einem Eingabeaufforderungsfenster mit erhöhten Rechten die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="67c32-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="67c32-364">Problem: WebMatrix Beta 3 kann bestimmte Aufgaben ausführen, die Erhöhung der Rechte erfordern.</span><span class="sxs-lookup"><span data-stu-id="67c32-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="67c32-365">WebMatrix Beta 3 kann bestimmte Aufgaben ausführen, für die Erhöhung der Rechte, z. B. beim Installieren von zusätzlicher Komponenten in den folgenden Situationen erforderlich:</span><span class="sxs-lookup"><span data-stu-id="67c32-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="67c32-366">Unter Windows Vista oder Windows 7 Sie mit einem Konto, das über keine Administratorrechte angemeldet sind und (User Account Control, UAC) ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="67c32-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="67c32-367">Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="67c32-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="67c32-368">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-368">**Workaround**</span></span>  
> <span data-ttu-id="67c32-369">Die meisten Tasks in WebMatrix Beta 3 sind keine Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="67c32-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="67c32-370">Sie können für diejenigen, die auch ausführen, führen Sie den Vorgang als Administrator oder gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="67c32-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="67c32-371">Unter Windows Vista oder Windows 7 aktiviert.</span><span class="sxs-lookup"><span data-stu-id="67c32-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="67c32-372">Fügen Sie unter Windows XP die Benutzer der Sicherheitsgruppe "Administratoren" hinzu.</span><span class="sxs-lookup"><span data-stu-id="67c32-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="67c32-373">Problem: "Website aus Web Gallery" ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="67c32-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="67c32-374">Die **Web Gallery-Website** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.</span><span class="sxs-lookup"><span data-stu-id="67c32-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="67c32-375">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-375">**Workaround**</span></span>  
> <span data-ttu-id="67c32-376">Installieren der [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="67c32-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="67c32-377">Problem: Unter Windows Server 2003, IIS Express nicht für einen Benutzer ohne Administratorrechte gestartet</span><span class="sxs-lookup"><span data-stu-id="67c32-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="67c32-378">Unter Windows Server 2003 beim Starten einer Seite oder starten IIS Express wird IIS Express nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="67c32-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="67c32-379">Für Webseiten wird eine Fehlermeldung angezeigt, der angibt, dass die Anwendung durch einen Benutzer ohne Administratorrechte gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="67c32-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="67c32-380">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-380">**Workaround**</span></span>  
> <span data-ttu-id="67c32-381">Starten Sie WebMatrix Beta 3 als Administrator an.</span><span class="sxs-lookup"><span data-stu-id="67c32-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="67c32-382">Weitere Informationen finden Sie unter den folgenden Knowledge Base-Artikel:</span><span class="sxs-lookup"><span data-stu-id="67c32-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="67c32-383">Eine Anwendung, die von einem Benutzer ohne Administratorrechte gestartet wird, kann nicht für den HTTP-Datenverkehr des Computers Lauschen auf dem die Anwendung in Windows Vista, Windows Server 2003 oder Windows XP ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="67c32-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="67c32-384">Problem: Google Chrome besteht keine Verfügbarkeit als eine Option ausführen</span><span class="sxs-lookup"><span data-stu-id="67c32-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="67c32-385">Google Chrome wird nicht angezeigt, in der Liste der Browser unter **ausführen** auf die **Home** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="67c32-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="67c32-386">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-386">**Workaround**</span></span>  
> <span data-ttu-id="67c32-387">Einige Versionen von Google Chrome registrieren nicht selbst ordnungsgemäß mit dem Standardprogramme-Feature in Windows.</span><span class="sxs-lookup"><span data-stu-id="67c32-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="67c32-388">Dieses Problem zu umgehen, starten Sie Google Chrome, klicken Sie auf die *anpassen und Google Chrome-Steuerelement* Menü klicken Sie auf *Optionen*, und klicken Sie dann auf *stellen Google Chrome Standardbrowser*.</span><span class="sxs-lookup"><span data-stu-id="67c32-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="67c32-389">Problem: Das Dialogfeld "Foreign Key" lässt nicht die Eingabe eines Primärschlüssels</span><span class="sxs-lookup"><span data-stu-id="67c32-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="67c32-390">Die **Foreign Key** Dialogfeld können Sie der Primärschlüsselname geben an der Primärschlüsseltabelle.</span><span class="sxs-lookup"><span data-stu-id="67c32-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="67c32-391">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-391">**Workaround**</span></span>  
> <span data-ttu-id="67c32-392">Dies ist beabsichtigt.</span><span class="sxs-lookup"><span data-stu-id="67c32-392">This is intentional.</span></span> <span data-ttu-id="67c32-393">Sie müssen nicht den Namen des primären Schlüssels auf die Primärschlüsseltabelle eingeben.</span><span class="sxs-lookup"><span data-stu-id="67c32-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="67c32-394">Problem: Die Schaltfläche "Beziehungen" ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="67c32-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="67c32-395">Die **Beziehungen** Schaltfläche unter der **Tabelle** Registerkarte der **Datenbanken** Arbeitsbereich ist für SQL Server Compact-Datenbanken deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="67c32-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="67c32-396">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-396">**Workaround**</span></span>  
> <span data-ttu-id="67c32-397">Keine</span><span class="sxs-lookup"><span data-stu-id="67c32-397">None.</span></span> <span data-ttu-id="67c32-398">SQL Server Compact unterstützt keine Beziehungen zwischen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="67c32-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="67c32-399">Problem: Parametrisierte SQL-Abfragen lösen Ausnahmen aus.</span><span class="sxs-lookup"><span data-stu-id="67c32-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="67c32-400">In SQL Server Compact 4.0, wenn Sie einen Datentyp nicht wie z. B. angeben `SqlDbType` oder `DbType` für Parameter in parametrisierten Abfragen, wird eine Ausnahme ausgelöst, wenn die Abfrage ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="67c32-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="67c32-401">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="67c32-401">**Workaround**</span></span>  
> <span data-ttu-id="67c32-402">Legen Sie den Datentyp für Parameter explizit wie z. B. `SqlDbType` oder `DbType`.</span><span class="sxs-lookup"><span data-stu-id="67c32-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="67c32-403">Dies ist wichtig, im Fall von BLOB-Datentypen (`image` und `ntext`).</span><span class="sxs-lookup"><span data-stu-id="67c32-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="67c32-404">Verwenden Sie Code wie folgt:</span><span class="sxs-lookup"><span data-stu-id="67c32-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="67c32-405">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="67c32-405">For More Information</span></span>

<span data-ttu-id="67c32-406">Weitere Informationen zu WebMatrix Beta 3 finden Sie unter den folgenden Websites:</span><span class="sxs-lookup"><span data-stu-id="67c32-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="67c32-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="67c32-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="67c32-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="67c32-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="67c32-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="67c32-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="67c32-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="67c32-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="67c32-411">Alle Rechte vorbehalten.</span><span class="sxs-lookup"><span data-stu-id="67c32-411">All Rights Reserved.</span></span> <span data-ttu-id="67c32-412">[Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="67c32-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
