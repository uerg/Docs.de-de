---
uid: web-pages/readme/beta3
title: WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei | Microsoft-Dokumentation
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831831"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="6272f-103">WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei</span><span class="sxs-lookup"><span data-stu-id="6272f-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="6272f-104">WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei</span><span class="sxs-lookup"><span data-stu-id="6272f-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="6272f-105">9 November 2010</span><span class="sxs-lookup"><span data-stu-id="6272f-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="6272f-106">Inhalt</span><span class="sxs-lookup"><span data-stu-id="6272f-106">Contents</span></span>

- [<span data-ttu-id="6272f-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="6272f-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="6272f-108">Installation</span><span class="sxs-lookup"><span data-stu-id="6272f-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="6272f-109">Neue Features, Änderungen und bekannte Probleme in der Beta 3-Version</span><span class="sxs-lookup"><span data-stu-id="6272f-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="6272f-110">Probleme bei der Installation von WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6272f-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="6272f-111">ASP.NET-Webseiten 2</span><span class="sxs-lookup"><span data-stu-id="6272f-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="6272f-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="6272f-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="6272f-113">Installieren von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="6272f-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="6272f-114">Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="6272f-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="6272f-115">Andere Probleme</span><span class="sxs-lookup"><span data-stu-id="6272f-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="6272f-116">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="6272f-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="6272f-117">Übersicht</span><span class="sxs-lookup"><span data-stu-id="6272f-117">Overview</span></span>

> <span data-ttu-id="6272f-118">Microsoft WebMatrix Beta ist einen kostenloser Web Development-Stapel, der innerhalb von Minuten installiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="6272f-119">Einen Webserver integriert mit der Datenbank und Programmierframeworks erstellen Sie eine einzige, integrierte Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="6272f-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="6272f-120">Können Sie die Möglichkeit zu optimieren, die Sie programmieren, testen und veröffentlichen Ihre eigene Website ASP.NET oder PHP, WebMatrix Beta, oder Sie können WebMatrix Beta verwenden, um eine neue Website mithilfe beliebter Open-Source-apps, z. B. DotNetNuke, Umbraco, WordPress oder Joomla.</span><span class="sxs-lookup"><span data-stu-id="6272f-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="6272f-121">WebMatrix-Beta verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausgeführt wird, gestaltet sich der Übergang von der Entwicklung zur Produktion reibungs- und nahtlos.</span><span class="sxs-lookup"><span data-stu-id="6272f-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="6272f-122">Installation</span><span class="sxs-lookup"><span data-stu-id="6272f-122">Installation</span></span>

> <span data-ttu-id="6272f-123">Verwenden Sie zum Installieren von WebMatrix Beta 3 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="6272f-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="6272f-124">Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zum Installieren von WebMatrix Beta 3 verwenden.</span><span class="sxs-lookup"><span data-stu-id="6272f-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="6272f-125">Wenn Sie während der Installation Probleme auftreten, finden Sie unter [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="6272f-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="6272f-126">Anweisungen zum Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="6272f-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="6272f-127">Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="6272f-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="6272f-128">Neue Features, Änderungen AndKnown Probleme</span><span class="sxs-lookup"><span data-stu-id="6272f-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="6272f-129">WebMatrix-Beta 3-Installation</span><span class="sxs-lookup"><span data-stu-id="6272f-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="6272f-130">Problem: WebMatrix Beta 3 steht nur auf Plattformen, die Unterstützung von Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="6272f-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="6272f-131">.NET Framework, Version 4 ist erforderlich, für die Betaversion von WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6272f-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="6272f-132">In bestimmten Fällen kann können der WebMatrix-Beta-Installer Sie versuchen, auf einer Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist.</span><span class="sxs-lookup"><span data-stu-id="6272f-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="6272f-133">Insbesondere Windows Vista ohne das SP1-Update können Sie die Installation von WebMatrix Beta zu starten, aber die .NET Framework 4-Komponente fehl und die Installation blockieren.</span><span class="sxs-lookup"><span data-stu-id="6272f-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="6272f-134">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-134">**Workaround**</span></span>  
> <span data-ttu-id="6272f-135">Installieren Sie auf eine unterstützte Plattform, einschließlich:</span><span class="sxs-lookup"><span data-stu-id="6272f-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="6272f-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="6272f-136">Windows 7</span></span>
> - <span data-ttu-id="6272f-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="6272f-137">Windows Server 2008</span></span>
> - <span data-ttu-id="6272f-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="6272f-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="6272f-139">Windows Vista SP1 oder höher</span><span class="sxs-lookup"><span data-stu-id="6272f-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="6272f-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="6272f-140">Windows XP SP3</span></span>
> - <span data-ttu-id="6272f-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="6272f-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="6272f-142">Problem: WebMatrix Beta 3 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist</span><span class="sxs-lookup"><span data-stu-id="6272f-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="6272f-143">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-143">**Workaround**</span></span>  
> <span data-ttu-id="6272f-144">Installieren Sie [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) aus dem Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="6272f-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="6272f-145">Problem: Einige Assemblys für SQL Server Compact 4.0 sind nicht im globalen Assemblycache installiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="6272f-146">Die verwalteten Assemblys für SQL Server Compact 4.0 werden nicht im globalen Assemblycache (GAC) abgelegt, wenn Sie SQL Server Compact 4.0 auf einem 64-Bit-Computer installieren, und der Computer nur das .NET Framework 3.5 SP1-Client Profile installiert verfügt.</span><span class="sxs-lookup"><span data-stu-id="6272f-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="6272f-147">Sind die verwalteten Assemblys, die nicht im GAC installiert werden:</span><span class="sxs-lookup"><span data-stu-id="6272f-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="6272f-148">*System.Data.SqlServerCe.dll* (ADO.NET-Anbieter)</span><span class="sxs-lookup"><span data-stu-id="6272f-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="6272f-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="6272f-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="6272f-150">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-150">**Workaround**</span></span>  
> <span data-ttu-id="6272f-151">Deinstallieren von SQLServer Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="6272f-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="6272f-152">Herunterladen Sie und installieren Sie die Vollversion von .NET Framework 3.5 SP1 von folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="6272f-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="6272f-153">Microsoft .NET Framework 3.5 Service Pack 1 (vollständiges Paket)</span><span class="sxs-lookup"><span data-stu-id="6272f-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="6272f-154">Installieren Sie SQL Server Compact 4.0 klicken Sie dann erneut.</span><span class="sxs-lookup"><span data-stu-id="6272f-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="6272f-155">Problem: Nicht SQL Server Compact über die Befehlszeile deinstallieren.</span><span class="sxs-lookup"><span data-stu-id="6272f-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="6272f-156">Deinstallation von SQL Server Compact mithilfe von Befehlszeilenoptionen funktioniert nicht in dieser Version.</span><span class="sxs-lookup"><span data-stu-id="6272f-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="6272f-157">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-157">**Workaround**</span></span>  
> <span data-ttu-id="6272f-158">Verwendung *Programme und Funktionen* in der Windows-Systemsteuerung zum Deinstallieren von Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="6272f-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="6272f-159">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="6272f-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="6272f-160">Dieser Abschnitt des Dokuments Beschreibt neue Features, Änderungen und bekannte Probleme bei der Beta 3-Version von ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="6272f-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="6272f-161">Neue features</span><span class="sxs-lookup"><span data-stu-id="6272f-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="6272f-162">Änderungen</span><span class="sxs-lookup"><span data-stu-id="6272f-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="6272f-163">Probleme</span><span class="sxs-lookup"><span data-stu-id="6272f-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="6272f-164">Neue Funktionen in Beta 3 für ASP.NET Web Pages mit Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="6272f-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="6272f-165">Neu: "Von Html.Raw"-Methode nicht codierte Markup rendert</span><span class="sxs-lookup"><span data-stu-id="6272f-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="6272f-166">Die neue `Html.Raw` -Methode können Sie das Rendern von HTML-Markup als Markup und nicht codierte Ausgabe rendern.</span><span class="sxs-lookup"><span data-stu-id="6272f-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="6272f-167">(Standardmäßig codiert ASP.NET Razor Zeichenfolgen vor dem Rendern sie.) Die Syntax lautet:</span><span class="sxs-lookup"><span data-stu-id="6272f-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="6272f-168">Das folgende Beispiel veranschaulicht die Verwendung von `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="6272f-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="6272f-169">Änderungen in der Betaversion 3 für ASP.NET Web Pages mit Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="6272f-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="6272f-170">Änderung: "HrefAttribute"-Methode wurde entfernt</span><span class="sxs-lookup"><span data-stu-id="6272f-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="6272f-171">Die `HrefAttribute` Methode der `WebPage` Klasse wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="6272f-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="6272f-172">Dieses Hilfsprogramm wurde verwendet, um unsicheren Zeichen in URLs zu codieren.</span><span class="sxs-lookup"><span data-stu-id="6272f-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="6272f-173">Es ist nicht mehr erforderlich, da ASP.NET Razor automatisch Zeichenfolgen codiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="6272f-174">(Verwenden Sie die neue `Html.Raw` Methode, um eine nicht codierte Zeichenfolgen zu rendern.)</span><span class="sxs-lookup"><span data-stu-id="6272f-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="6272f-175">Änderung: Syntax für die deklarative "@helper" Hilfsprogramme geändert</span><span class="sxs-lookup"><span data-stu-id="6272f-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="6272f-176">In der Beta 3-Version ASP.NET ändert wie diese Hilfsprogramme analysiert, die erstellt werden, mit der `@helper` Syntax.</span><span class="sxs-lookup"><span data-stu-id="6272f-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="6272f-177">Im Wesentlichen die `@helper` Syntax wird als Codeblock anstatt als Block des Markups, die Code enthalten, kann jetzt analysiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="6272f-178">Aus diesem Grund Code in der Hilfe muss nicht eingeschlossen werden `@{ }` Blöcke.</span><span class="sxs-lookup"><span data-stu-id="6272f-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="6272f-179">Im Gegensatz dazu Markup in der Hilfe muss explizit eingefügt, in HTML-Elemente oder in ASP.NET Razor `<text></text>` Tags.</span><span class="sxs-lookup"><span data-stu-id="6272f-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="6272f-180">Beispielsweise die folgenden `@helper` Syntax funktioniert in der Beta 3-Version:</span><span class="sxs-lookup"><span data-stu-id="6272f-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="6272f-181">In der Beta 3-Version muss dieses Hilfsprogramm geändert werden, um wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="6272f-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="6272f-182">Beachten Sie, dass die `@{ }` Zeichen für den anfänglichen Code in das Hilfsprogramm wird nicht mehr verwendet.</span><span class="sxs-lookup"><span data-stu-id="6272f-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="6272f-183">Dies ist, da der Inhalt der Hilfsprogramme standardmäßig als Codeblock behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="6272f-184">Das Hilfsprogramm rendert Markup, das mit dem öffnenden beginnt `<a>` Tag.</span><span class="sxs-lookup"><span data-stu-id="6272f-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="6272f-185">Wenn das Hilfsprogramm Rendern muss, nur-Text oder Tags, die keinen schließendes Tag enthalten (z. B. `<meta>` Tags), die zu rendernden Inhalt muss sich im `<text></text>` Tags.</span><span class="sxs-lookup"><span data-stu-id="6272f-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="6272f-186">Änderung: "WebPageContext.HttpContext" entfernt</span><span class="sxs-lookup"><span data-stu-id="6272f-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="6272f-187">Die `WebPageContext.HttpContext` Eigenschaft entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="6272f-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="6272f-188">Verwenden Sie stattdessen `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="6272f-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="6272f-189">(Die `WebPageContext.HttpContext` Eigenschaft umschlossen einfach dadurch.)</span><span class="sxs-lookup"><span data-stu-id="6272f-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="6272f-190">Änderung: Neue Paket verschoben "Facebook"-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="6272f-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="6272f-191">Die `Facebook` Hilfsprogramm wurde in der *Facebook.Helper* -Bibliothek, die umfasst die `Facebook` -Hilfsobjekt und zusätzliche Funktionen.</span><span class="sxs-lookup"><span data-stu-id="6272f-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="6272f-192">Müssen Sie installieren diese Bibliothek als separates Paket, wie in "Hilfsprogramme mit Paket-Manager installieren" beschrieben im Tutorial [erste Schritte mit ASP.NET Webseiten](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="6272f-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="6272f-193">Änderung: Typen Mitgliedschaft, Rollen und Sicherheit verschiebt, in der neuen assembly</span><span class="sxs-lookup"><span data-stu-id="6272f-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="6272f-194">Die folgenden Typen wurden verschoben, um die `WebMatrix.WebData` Assembly:</span><span class="sxs-lookup"><span data-stu-id="6272f-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="6272f-195">Änderung: System.Web.WebPages.dll Assembly verschoben "TagBuilder"-Klasse</span><span class="sxs-lookup"><span data-stu-id="6272f-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="6272f-196">Die `TagBuilde` R-Klasse auf die Assembly System.Web.WebPages.dll verschoben wurde.</span><span class="sxs-lookup"><span data-stu-id="6272f-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="6272f-197">Früher war dies in einer Assembly, die Teil von ASP.NET MVC war.</span><span class="sxs-lookup"><span data-stu-id="6272f-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="6272f-198">Diese Änderung bedeutet, dass Sie keine ASP.NET MVC zu installieren, um Sie verwenden die `TagBuilder` Klasse.</span><span class="sxs-lookup"><span data-stu-id="6272f-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="6272f-199">Die Klasse ist jedoch nach wie vor in den `System.Web.Mvc` Namespace.</span><span class="sxs-lookup"><span data-stu-id="6272f-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="6272f-200">Zum Verwenden der `TagBuilder` -Klasse (z. B. in einem benutzerdefinierten ASP.NET Razor-Hilfsprogramm), müssen Sie den Namespace verweisen (z. B. durch Hinzufügen von `@using System.Web.Mvc` an Ihrem Code).</span><span class="sxs-lookup"><span data-stu-id="6272f-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="6272f-201">: Änderungsanforderung Überprüfung Syntax geändert; Entfernt "Validation"-Klasse</span><span class="sxs-lookup"><span data-stu-id="6272f-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="6272f-202">In der Version Beta 3 zum Deaktivieren der Validierung für ein einzelnes Feld oder einen Satz von Feldern, können Sie Aufrufen der `Validation.Exclude` Methode und übergeben den Namen oder die Namen der Felder zum Ausschließen der Validierung.</span><span class="sxs-lookup"><span data-stu-id="6272f-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="6272f-203">Eine neue Syntax wird in der Beta 3-Version zum Umgehen der Validierung.</span><span class="sxs-lookup"><span data-stu-id="6272f-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="6272f-204">Die `Validation` Methode für Beta 3 wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="6272f-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="6272f-205">Wenn Sie Request-Überprüfung nicht deaktivieren, wenn Benutzer versuchen, das Hochladen von HTML-Markup (z. B. mithilfe eines rich-Text-Editors auf einer Seite), die Website meldet einen Fehler, z. B. *ein potenziell gefährlicher Request.Form-Wert wurde erkannt, auf dem Client*und die Benutzereingabe wird nicht akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="6272f-206">Wenn die Anforderungsvalidierung Sie deaktivieren, müssen Sie manuell überprüfen, auf Benutzereingaben, um sicherzustellen, dass nicht potenziell gefährliche Markup enthalten oder Skript, z. B. die [Microsoft Anti-Cross Site Scripting Library v4. 0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="6272f-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="6272f-207">Um automatische Request-Überprüfung zu deaktivieren, rufen die `Request.Unvalidated` -Methode, und übergeben sie den Namen des Felds oder andere Post-Objekt, das Sie Anforderungsvalidierung für umgehen möchten.</span><span class="sxs-lookup"><span data-stu-id="6272f-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="6272f-208">Sie können diese Methode verwenden, umgehen die Überprüfung für alle Elemente in der `Form`, `QueryString`, `Cookies`, und `ServerVariables` Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="6272f-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="6272f-209">Die folgenden Beispiele zeigen, wie Sie mit der `Unvalidated` Methode:</span><span class="sxs-lookup"><span data-stu-id="6272f-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="6272f-210">Bekannte Probleme bei der ASP.NET Web Pages mit Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="6272f-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="6272f-211">Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="6272f-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="6272f-212">Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode.</span><span class="sxs-lookup"><span data-stu-id="6272f-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="6272f-213">(In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn der `autoCreateTables` Parameter dieser Methode wird festgelegt, auf "true" (standardmäßig es nastaven NA hodnotu true in der Vorlage Starter Site), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="6272f-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="6272f-214">Stattdessen wird automatisch in der Tabelle erstellt.</span><span class="sxs-lookup"><span data-stu-id="6272f-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="6272f-215">Dies kann ein Problem sein, wenn Sie beabsichtigen, übergeben den falschen Tabellennamen an aber verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft der `WebSecurity.InitializeDatabaseConnection` Methode.</span><span class="sxs-lookup"><span data-stu-id="6272f-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="6272f-216">Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung zu funktionieren scheinen.</span><span class="sxs-lookup"><span data-stu-id="6272f-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="6272f-217">Allerdings kann Anwendungscode, der für die benutzerdefinierte Tabelle (und darin enthaltenen Felder) stützt sich schließlich unerwartete Fehlern auftreten.</span><span class="sxs-lookup"><span data-stu-id="6272f-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="6272f-218">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-218">**Workaround**</span></span>  
> <span data-ttu-id="6272f-219">Stellen Sie sicher, dass der Name in übergeben die `InitializeDatabaseConnection` Methode entspricht, das Benutzerprofil in der Mitgliedschaftsdatenbank Tabelle, oder stellen Sie sicher, dass, die `autoCreateTables` Parameter auf "false" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="6272f-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="6272f-220">Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler</span><span class="sxs-lookup"><span data-stu-id="6272f-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="6272f-221">Wenn eine WebMatrix-Web-Anwendung SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, können Sie ein Fehler angezeigt, der angibt, dass SQL Server des Pfads des Benutzers lokale Anwendung zur Laufzeit abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="6272f-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="6272f-222">**Problemumgehung** stellen sicher, dass das Windows-Konto, das die Anwendung ausgeführt, unter dem Konto (normalerweise Netzwerkdienst wird) Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner wie z. B. *App\_Daten*.</span><span class="sxs-lookup"><span data-stu-id="6272f-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="6272f-223">Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [Probleme mit der SQL Server Express Benutzerinstanziierung und ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="6272f-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="6272f-224">Problem: In Visual Studio werden die Namespaces für benutzerdefinierte Assemblys (DLLs) nicht automatisch importiert</span><span class="sxs-lookup"><span data-stu-id="6272f-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="6272f-225">Wenn Sie benutzerdefinierte Assemblys in einem Projekt in Visual Studio verwenden, werden die Namespaces deklariert, die in diesen Assemblys zur Entwurfszeit nicht automatisch importiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="6272f-226">Verweise auf benutzerdefinierte Typen wird daher möglicherweise nicht zur Entwurfszeit erkannt werden und als nicht erkannt, in Visual Studio (unter Verwendung einer "Wellenlinie") gekennzeichnet sind.</span><span class="sxs-lookup"><span data-stu-id="6272f-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="6272f-227">Dieses Problem tritt nur zur Entwurfszeit in Visual Studio; die Anwendung selbst ordnungsgemäß ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6272f-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="6272f-228">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-228">**Workaround**</span></span>  
> <span data-ttu-id="6272f-229">Einschließen einer `using` Anweisung (`imports` in Visual Basic), die auf die Entitäten, die zur Entwurfszeit nicht erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="6272f-230">Problem: Visual Studio IntelliSense und Projektvorlagen nur in ASP.NET MVC 3-Version verfügbar</span><span class="sxs-lookup"><span data-stu-id="6272f-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="6272f-231">Installieren von ASP.NET Web Pages wird nicht auch Tools für Visual Studio wie IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen installiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="6272f-232">**Problemumgehung** um IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio zu verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="6272f-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="6272f-233">Problem: "&lt;Helper&gt; Klasse wurde nicht gefunden" Fehler</span><span class="sxs-lookup"><span data-stu-id="6272f-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="6272f-234">Nach dem upgrade auf Beta 3 können Sie möglicherweise ein Fehler angezeigt, die eine Hilfsklasse (z. B. die `Facebook` Klasse) kann nicht gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="6272f-235">In Beta 2 wird gestartet und Weiter geht es im Beta 3 wurden Hilfsprogramme zu Paketen verschoben, die Sie explizit installieren müssen.</span><span class="sxs-lookup"><span data-stu-id="6272f-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="6272f-236">Vorhandene Websites werden nicht aktualisiert, um diese Pakete enthalten; Dies schließt die Standorte in der *\My Documents\IISExpress* oder *\My Documents\My Websites* Ordner.</span><span class="sxs-lookup"><span data-stu-id="6272f-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="6272f-237">Insbesondere werden Ihnen dieser Fehler angezeigt, wenn Sie die Standardwebsite in verwenden *My Sites* (WebSite1), enthält einen Verweis auf die `Twitter` Helper.</span><span class="sxs-lookup"><span data-stu-id="6272f-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="6272f-238">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-238">**Workaround**</span></span>  
> <span data-ttu-id="6272f-239">Kommentieren Sie Aufrufe an alle Hilfsprogramme in der Website, und führen Sie die  *\_Admin* Seite, und installieren Sie das Paket oder Pakete, die die Hilfsprogramme enthalten, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="6272f-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="6272f-240">Nachdem Sie das Paket installiert haben, können Sie die auskommentierung der Zeilen, die Hilfsmethoden zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="6272f-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="6272f-241">Problem: Bereitstellen von Beta 3 ASP.NET Razor-Assemblys in den Ordner "Bin" funktioniert möglicherweise nicht auf das Hosten von Websites</span><span class="sxs-lookup"><span data-stu-id="6272f-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="6272f-242">Wenn Sie eine ASP.NET Web Pages-Website auf einer hosting-Site bereitstellen und Sie die ASP.NET Razor-Beta 3-Assemblys auf der Website bereitstellen *Bin* Ordner treten möglicherweise Fehler auf, einschließlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="6272f-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="6272f-243">Dies kann vorkommen, wenn der Hostinganbieter die ASP.NET Web Pages Beta 1-Assemblys des Servers globalen Anwendungscache (GAC) installiert hat.</span><span class="sxs-lookup"><span data-stu-id="6272f-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="6272f-244">Assemblys im GAC erhalten Vorrang gegenüber Assemblys, die lokal im installiert die *Bin* Ordner.</span><span class="sxs-lookup"><span data-stu-id="6272f-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="6272f-245">**Problemumgehung** wenden Sie sich an Ihr Hostinganbieter, um sicherzustellen, dass der Fehler wird angezeigt, aufgrund eines Konflikts zwischen der Anbieter Versionen sind der Assemblys und können frei wählen.</span><span class="sxs-lookup"><span data-stu-id="6272f-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="6272f-246">Wenn dies der Fall ist, fordern Sie, dass der Hostinganbieter die Assemblys im GAC mit dem Server aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="6272f-247">Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver</span><span class="sxs-lookup"><span data-stu-id="6272f-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="6272f-248">Ist der Server mit der Website hinter einem Proxyserver befinden, müssen Sie möglicherweise konfigurieren Informationen zum Proxy in der *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die von außerhalb Ihrer Website stammt.</span><span class="sxs-lookup"><span data-stu-id="6272f-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="6272f-249">Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber von Ihrem Proxyserver blockiert werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="6272f-250">Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed ein, die im Paket-Manager verwendet werden-Proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="6272f-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="6272f-251">Bei Problemen in arbeiten mit einem externen Dienst oder Arbeiten mit den paketfeed ein, fügen Sie die folgenden Elemente in Ihrer Anwendung Stamm *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="6272f-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="6272f-252">Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="6272f-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="6272f-253">Problem: "Microsoft.Web.Infrastructure.dll kann nicht geladen werden"-Fehler</span><span class="sxs-lookup"><span data-stu-id="6272f-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="6272f-254">Wenn Sie zuvor die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax installiert, und klicken Sie dann die Beta 3-Version installieren, werden alle entsprechenden Assemblys im GAC mit Ausnahme von installierten *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="6272f-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="6272f-255">Infolgedessen wird bei der ASP.NET Razor Pages ausführen Fehlermeldung angezeigt wird, der angibt, *Microsoft.Web.Infrastructure.dll* konnte nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="6272f-256">Dieses Problem tritt nicht auf, wenn Sie die Beta-3-Version auf einem Computer ohne geladen.</span><span class="sxs-lookup"><span data-stu-id="6272f-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="6272f-257">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-257">**Workaround**</span></span>  
> <span data-ttu-id="6272f-258">In der Systemsteuerung Deinstallieren von ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6272f-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="6272f-259">Installieren Sie Beta 3-Version erneut.</span><span class="sxs-lookup"><span data-stu-id="6272f-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="6272f-260">Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert</span><span class="sxs-lookup"><span data-stu-id="6272f-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="6272f-261">Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist die ASP.NET Web Pages mit Razor-Syntax deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="6272f-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="6272f-262">Seiten mit den *.cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="6272f-263">ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* Datei und Entfernen von .NET Framework wird diese Datei entfernt.</span><span class="sxs-lookup"><span data-stu-id="6272f-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="6272f-264">Neuinstallation von .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6272f-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="6272f-265">**Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="6272f-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="6272f-266">Dadurch wird das folgende Element zum hinzugefügt. die *"Web.config"* -Datei in das Stammverzeichnis des Computer, der in der Regel an folgendem Speicherort ist:</span><span class="sxs-lookup"><span data-stu-id="6272f-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="6272f-267">Problem: Treten Fehler auf Anwendungen, die zuvor mit ASP.NET-Assemblys im Ordner "Bin" bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6272f-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="6272f-268">Während der Bereitstellung, Kopien der ASP.NET Web Pages-Assemblys (z. B. *Microsoft.WebPages.dll*) auf die *Bin* Ordner der Website auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="6272f-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="6272f-269">(Dies könnte bei der Bereitstellung automatisch stattgefunden haben, oder weil der Entwickler explizit auf die Assemblys kopiert.) Wenn die Beta-3-Version installiert ist, Fehler erfolgt jedoch, wie z. B. Fehler, die bestimmte Typen nicht gefunden.</span><span class="sxs-lookup"><span data-stu-id="6272f-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="6272f-270">Dies tritt auf, da eine Anzahl von ASP.NET Web Pages-Typen in verschiedene Namespaces für Beta 3-Version verschoben wurden.</span><span class="sxs-lookup"><span data-stu-id="6272f-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="6272f-271">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="6272f-271">**Workaround** </span></span>  
> <span data-ttu-id="6272f-272">Deaktivieren der *Bin* Ordner der bereitgestellten Anwendung, kopieren Sie die neuen Assemblys in den Ordner (oder erneute Bereitstellung der Anwendung), und klicken Sie dann die Anwendung neu zu starten.</span><span class="sxs-lookup"><span data-stu-id="6272f-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="6272f-273">Problem: URLs ohne Erweiterung.cshtml/.vbhtml-Dateien auf IIS 7 oder IIS 7.5 nicht finden</span><span class="sxs-lookup"><span data-stu-id="6272f-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="6272f-274">Auf IIS 7 oder IIS 7.5, Anforderungen mit einer URL wie im folgenden sind nicht Seiten zu finden, die die *.cshtml* oder *vbhtml* Erweiterung:</span><span class="sxs-lookup"><span data-stu-id="6272f-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="6272f-275">Das Problem tritt auf, weil der URL-Umschreibung nicht standardmäßig für IIS 7 oder IIS 7.5 aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="6272f-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="6272f-276">Das likeliest Szenario ist, dass das Problem nicht angezeigt werden, wird wenn Tests lokal mithilfe von IIS Express, aber Sie entdecken Sie es beim Bereitstellen Ihrer Websites auf einer hosting-Website.</span><span class="sxs-lookup"><span data-stu-id="6272f-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="6272f-277">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="6272f-278">Wenn Sie die Kontrolle über den Server-Computer verfügen, installieren Sie auf dem Computer das Update, das beschrieben ist [ein Update ist verfügbar, können Sie bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="6272f-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="6272f-279">Wenn Sie keine Kontrolle über den Server-Computer verfügen (z. B. Sie bereitstellen auf einer hosting-Website), fügen Sie Folgendes auf der Website *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="6272f-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="6272f-280">Problem: Mithilfe von Web Application Project oder ASP.NET MVC und ASP.NET Web Pages in derselben Anwendung</span><span class="sxs-lookup"><span data-stu-id="6272f-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="6272f-281">Wenn Sie ASP.NET Web Pages in einem Webanwendungsprojekt oder ASP.NET MVC-Anwendung verwendet haben, können Sie möglicherweise ein Fehler angezeigt, die *WebPageHttpApplication* wurde nicht gefunden.</span><span class="sxs-lookup"><span data-stu-id="6272f-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="6272f-282">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-282">**Workaround**</span></span>  
> <span data-ttu-id="6272f-283">Wenn Sie diesen Fehler erhalten, ändern Sie die Basisklasse, von der die Anwendung abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="6272f-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="6272f-284">In der *"Global.asax"* Datei, die folgende Zeile ändern:</span><span class="sxs-lookup"><span data-stu-id="6272f-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="6272f-285">Folgendermaßen:</span><span class="sxs-lookup"><span data-stu-id="6272f-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="6272f-286">Dadurch werden wirksam, eine Änderung, die eingeführt wurde für die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="6272f-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="6272f-287">Problem: Bereitstellen einer Anwendung auf einem Computer, die keinen SQL Server Compact installiert</span><span class="sxs-lookup"><span data-stu-id="6272f-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="6272f-288">Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, in dem SQL Server Compact nicht installiert ist.</span><span class="sxs-lookup"><span data-stu-id="6272f-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="6272f-289">Microsoft WebMatrix Beta 3 wird automatisch diese Binärdateien für die Sie kopiert und führt die entsprechende *"Web.config"* Datei transformiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="6272f-290">**Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6272f-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="6272f-291">Kopieren Sie die Datenbank-Engine Assemblys, die *Bin* Ordner (sowie Unterordner) der Anwendung auf dem Zielcomputer:</span><span class="sxs-lookup"><span data-stu-id="6272f-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="6272f-292">Kopie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **zu** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="6272f-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="6272f-293">Kopie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **zu** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="6272f-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="6272f-294">Kopie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **zu** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="6272f-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="6272f-295">Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="6272f-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="6272f-296">(In WebMatrix Beta 3 dieser Dateityp ist verfügbar, wenn Sie auf **alle** in die **wählen Sie einen Dateityp** Dialogfeld.)</span><span class="sxs-lookup"><span data-stu-id="6272f-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="6272f-297">Fügen Sie das folgende Element als untergeordnetes Element der **&lt;Konfiguration&gt;** Element (nicht in der **&lt;"System.Web"&gt;** Element):</span><span class="sxs-lookup"><span data-stu-id="6272f-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="6272f-298">Problem: Datenbank- und WebGrid-Hilfsprogramme funktionieren nicht in der mittleren Vertrauensebene in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="6272f-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="6272f-299">Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung festgelegt wurde, auf die mittlere Vertrauenswürdigkeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="6272f-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="6272f-300">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-300">**Workaround**</span></span>  
> <span data-ttu-id="6272f-301">Legen Sie vorübergehend die Anwendung volle Vertrauenswürdigkeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="6272f-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="6272f-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="6272f-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="6272f-303">Problem: "Encrypt"-Eigenschaft wurde nicht erkannt.</span><span class="sxs-lookup"><span data-stu-id="6272f-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="6272f-304">SQL Server Compact 4.0 nicht erkennt die `Encrypt` Eigenschaft der `SqlCeConnection` Klasse.</span><span class="sxs-lookup"><span data-stu-id="6272f-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="6272f-305">Sie sollten diese Eigenschaft nicht verwenden, um Datenbankdateien zu verschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="6272f-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="6272f-306">Die `Encrypt` Eigenschaft wurde in SQLServer Compact 3.5-Version als veraltet markiert und nur für die Abwärtskompatibilität beibehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="6272f-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="6272f-307">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-307">**Workaround**</span></span>  
> <span data-ttu-id="6272f-308">Verwenden der `Encryption Mode` Eigenschaft der `SqlCeConnection` Klasse, um SQL Server Compact 4.0-Datenbankdateien zu verschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="6272f-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="6272f-309">Das folgende Beispiel zeigt die Vorgehensweise: Erstellen einer verschlüsselten SQL Server Compact 4.0-Datenbank, mithilfe der `Encryption Mode` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="6272f-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="6272f-310">Führen Sie folgende Schritte aus, um den Verschlüsselungsmodus für eine vorhandene SQL Server Compact 4.0-Datenbank zu ändern:</span><span class="sxs-lookup"><span data-stu-id="6272f-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="6272f-311">Um eine unverschlüsselte SQL Server Compact 4.0-Datenbank zu verschlüsseln, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="6272f-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="6272f-312">Problem: Microsoft Visual C++ 2008-Runtime-Bibliotheken sind erforderlich</span><span class="sxs-lookup"><span data-stu-id="6272f-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="6272f-313">Die systemeigene DLLs von SQL Server Compact 4.0 benötigen, die Microsoft Visual C++ 2008-Laufzeitbibliotheken (x 86, IA64 und x 64), Servicepack 1.</span><span class="sxs-lookup"><span data-stu-id="6272f-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="6272f-314">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-314">**Workaround**</span></span>  
> <span data-ttu-id="6272f-315">Installieren Sie .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="6272f-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="6272f-316">Dadurch wird auch die Visual C++ 2008-Runtime-Bibliotheken SP1 installiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="6272f-317">Sie können die Bibliotheken von folgendem Speicherort herunterladen:</span><span class="sxs-lookup"><span data-stu-id="6272f-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="6272f-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span><span class="sxs-lookup"><span data-stu-id="6272f-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="6272f-319">Beachten Sie, dass die Installation der .NET Framework 2.0, 3.0 oder 4 ist *nicht* Visual C++ 2008-Runtime-Bibliotheken SP1 zu installieren.</span><span class="sxs-lookup"><span data-stu-id="6272f-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="6272f-320">Problem: Wenn SQL Server Compact vor der Installation von .NET Framework auf dem Computer installiert, ist der invariante Anbietername nicht in der .NET Framework-Datei "Machine.config" registriert</span><span class="sxs-lookup"><span data-stu-id="6272f-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="6272f-321">SQL Server Compact kann auf einem Computer installiert werden, die keine .NET Framework installiert werden, da SQL Server Compact .NET Framework erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="6272f-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="6272f-322">Wenn weder .NET Framework, Version 3.5 noch 4 installiert ist, bevor Sie SQL Server Compact installieren, registriert den SQL Server Compact-Setup keine der invariante Name des Anbieters in der *"Machine.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="6272f-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="6272f-323">Jede Anwendung, die abhängig von den SQL Server Compact-Eintrag in der *"Machine.config"* Datei schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="6272f-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="6272f-324">Der invariante Name des Registrierungseintrags in *"Machine.config"* sieht wie im folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6272f-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="6272f-325">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-325">**Workaround**</span></span>  
> <span data-ttu-id="6272f-326">Deinstallieren Sie SQL Server Compact 4.0-CTP1.</span><span class="sxs-lookup"><span data-stu-id="6272f-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="6272f-327">Herunterladen Sie und installieren Sie den Vollversionen von .NET Framework von folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="6272f-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="6272f-328">Microsoft .NET Framework 3.5 Service Pack 1 (vollständiges Paket)</span><span class="sxs-lookup"><span data-stu-id="6272f-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="6272f-329">Microsoft .NET Framework 4.0-Version (vollständiges Paket)</span><span class="sxs-lookup"><span data-stu-id="6272f-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="6272f-330">Installieren Sie erneut [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="6272f-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="6272f-331">Installieren von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="6272f-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="6272f-332">Problem: Installieren einer Anwendung kann eine lange dauern, wenn Ordner "Eigene Dateien" des Benutzers auf einer Netzwerkfreigabe umgeleitet wird</span><span class="sxs-lookup"><span data-stu-id="6272f-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="6272f-333">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-333">**Workaround**</span></span>  
> <span data-ttu-id="6272f-334">Keine</span><span class="sxs-lookup"><span data-stu-id="6272f-334">None.</span></span> <span data-ttu-id="6272f-335">Die Anwendung möglicherweise einige Zeit dauern installieren, jedoch ordnungsgemäß installiert wird.</span><span class="sxs-lookup"><span data-stu-id="6272f-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="6272f-336">Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="6272f-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="6272f-337">Problem: Website funktioniert möglicherweise nicht nach dem Veröffentlichen, wenn das Feld "Ziel-URL" nicht mit http:// oder https:// vorangestellt ist</span><span class="sxs-lookup"><span data-stu-id="6272f-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="6272f-338">In der **Veröffentlichungseinstellungen** Dialogfeld, wenn die Ziel-URL nicht mit beginnt `http://` oder `https://`, die Website funktioniert möglicherweise nicht nach der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="6272f-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="6272f-339">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-339">**Workaround**</span></span>  
> <span data-ttu-id="6272f-340">Stellen Sie sicher, dass vor dem Veröffentlichen einer Website, die Ziel-URL in die **Veröffentlichungseinstellungen** Dialogfeld beginnt mit `http://` oder `https://`.</span><span class="sxs-lookup"><span data-stu-id="6272f-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="6272f-341">Problem: Veröffentlichen einer MySQL-Datenbank tritt der Fehler "Fehler beim Veröffentlichen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6272f-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="6272f-342">Dies kann passieren, wenn die Remotedatenbank das Skript ausgeführt werden kann."</span><span class="sxs-lookup"><span data-stu-id="6272f-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="6272f-343">Der Fehler kann aus verschiedenen Gründen auftreten.</span><span class="sxs-lookup"><span data-stu-id="6272f-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="6272f-344">Ein möglicher Grund kann dieser Fehler angezeigt wird, wenn das Datenbankskript enthält eine einfache Anführungszeichen (') und der Ziel-MySQL-Datenbank der Standard-Zeichensatz nicht in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="6272f-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="6272f-345">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-345">**Workaround**</span></span>  
> <span data-ttu-id="6272f-346">Legen Sie den Standardzeichensatz für die MySQL-Remotedatenbank in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="6272f-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="6272f-347">Andere Probleme</span><span class="sxs-lookup"><span data-stu-id="6272f-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="6272f-348">Problem: Search-Filtern funktioniert nicht in Berichten für Group By: Problemtyp</span><span class="sxs-lookup"><span data-stu-id="6272f-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="6272f-349">Beim Ausführen eines Berichts für einen Standort, wenn Sie Text in eingeben der *Filter nach URL* ein, und klicken Sie auf *Suche*, geschieht nichts.</span><span class="sxs-lookup"><span data-stu-id="6272f-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="6272f-350">Dies ist, da dieses Steuerelement nicht funktionsfähig ist die *Group By* Zustand des Berichts wird festgelegt, um *Problemtyp*, dies ist die Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="6272f-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="6272f-351">**Problemumgehung** In die *Group By* Registerkarte des Menübands, klicken Sie auf *URL* um die Einträge nach ihrer Quell-URL zu gruppieren.</span><span class="sxs-lookup"><span data-stu-id="6272f-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="6272f-352">Das Textfeld und eine Schaltfläche zum Filtern der Einträge werden in diesem Zustand funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="6272f-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="6272f-353">Problem: WCF-Anwendungen nicht, mit IIS Express ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="6272f-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="6272f-354">Navigieren zu einer WCF-Anwendung tritt ein Fehler wie den folgenden Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="6272f-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="6272f-355">*Konnte nicht geladen werden, Datei oder Assembly ' "Microsoft.Web.Administration", Version = 7.0.0.0, Kultur = Neutral, PublicKeyToken = 31bf3856ad364e35' oder eine ihrer Abhängigkeiten. Das System konnte die angegebene Datei nicht finden.*</span><span class="sxs-lookup"><span data-stu-id="6272f-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="6272f-356">Dies tritt auf, da IIS Express-Betaversion WCF standardmäßig nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6272f-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="6272f-357">**Problemumgehung** eine der folgenden problemumgehungen verwenden (problemumgehung #2 erfordert Microsoft Windows Vista oder höher):</span><span class="sxs-lookup"><span data-stu-id="6272f-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="6272f-358">Kopieren der *Microsoft.Web.dll* und *Microsoft.Web.Administration.dll* Assemblys aus dem WebMatrix-Installationsverzeichnis in die *Bin* WCF Verzeichnis die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6272f-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="6272f-359">In der Standardeinstellung WebMatrix installiert ist, der *Microsoft WebMatrix* Unterordner des Systems *Programmdateien* Ordner.</span><span class="sxs-lookup"><span data-stu-id="6272f-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="6272f-360">Unter Microsoft Windows Vista oder höher verwenden, erstellen Sie einen Symlink zu den Assemblys in der *Bin* Verzeichnis mit den folgenden Befehlen.</span><span class="sxs-lookup"><span data-stu-id="6272f-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="6272f-361">(Dieser Ansatz hat den Vorteil, dass sie eine Kopie der Assemblys nicht erstellt werden.)</span><span class="sxs-lookup"><span data-stu-id="6272f-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="6272f-362">Die beiden Assemblys im GAC installiert.</span><span class="sxs-lookup"><span data-stu-id="6272f-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="6272f-363">Führen Sie eine Eingabeaufforderung mit erhöhten Rechten die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="6272f-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="6272f-364">Problem: WebMatrix Beta 3 ist nicht möglich, bestimmte Aufgaben ausführen, die erhöhte Rechte erfordern.</span><span class="sxs-lookup"><span data-stu-id="6272f-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="6272f-365">WebMatrix Beta 3 kann nicht für bestimmte Aufgaben, die eine Erhöhung der Rechte wie z. B. die Installation zusätzlicher Komponenten in den folgenden Situationen erfordern:</span><span class="sxs-lookup"><span data-stu-id="6272f-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="6272f-366">Klicken Sie auf Windows Vista oder Windows 7 Sie angemeldet sind, sich mit einem Konto an, die nicht über Administratorberechtigungen verfügt, und (User Account Control, UAC) ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="6272f-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="6272f-367">Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="6272f-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="6272f-368">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-368">**Workaround**</span></span>  
> <span data-ttu-id="6272f-369">Die meisten Aufgaben in WebMatrix Beta 3 sind keine Administratorberechtigungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6272f-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="6272f-370">Für diejenigen, die ausführen, können Sie den Vorgang als Administrator ausführen, oder gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6272f-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="6272f-371">Auf Windows Vista oder Windows 7 aktiviert.</span><span class="sxs-lookup"><span data-stu-id="6272f-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="6272f-372">Unter Windows XP müssen fügen Sie den Benutzer auf die Sicherheitsgruppe "Administratoren hinzu".</span><span class="sxs-lookup"><span data-stu-id="6272f-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="6272f-373">Problem: "Site from Web Gallery" ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="6272f-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="6272f-374">Die **Standort Web Gallery** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.</span><span class="sxs-lookup"><span data-stu-id="6272f-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="6272f-375">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-375">**Workaround**</span></span>  
> <span data-ttu-id="6272f-376">Installieren Sie die [Microsoft Webplattform-Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="6272f-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="6272f-377">Problem: Unter Windows Server 2003, IIS Express nicht für einen Benutzer ohne Administratorrechte gestartet</span><span class="sxs-lookup"><span data-stu-id="6272f-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="6272f-378">Unter Windows Server 2003 beim Starten von einer Seite oder starten IIS Express wird IIS Express nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="6272f-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="6272f-379">Für Webseiten wird eine Fehlermeldung angezeigt, der angibt, dass die Anwendung von einem Benutzer ohne Administratorrechte gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="6272f-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="6272f-380">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-380">**Workaround**</span></span>  
> <span data-ttu-id="6272f-381">Starten Sie WebMatrix Beta 3 als Administrator an.</span><span class="sxs-lookup"><span data-stu-id="6272f-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="6272f-382">Weitere Informationen finden Sie unter den folgenden Knowledge Base-Artikel:</span><span class="sxs-lookup"><span data-stu-id="6272f-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="6272f-383">Eine Anwendung, die von einem Benutzer ohne Administratorrechte gestartet wird kann den HTTP-Datenverkehr des Computers nicht Lauschen auf dem die Anwendung in Windows Vista, Windows Server 2003 oder Windows XP ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6272f-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="6272f-384">Problem: Google Chrome ist nicht verfügbar ist, als eine ausführoption</span><span class="sxs-lookup"><span data-stu-id="6272f-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="6272f-385">Google Chrome wird nicht angezeigt, in der Liste der Browser unter **ausführen** auf die **Startseite** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="6272f-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="6272f-386">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-386">**Workaround**</span></span>  
> <span data-ttu-id="6272f-387">Einige Versionen von Google Chrome ist nicht selbst ordnungsgemäß mit dem Default Programs-Feature in Windows registriert werden.</span><span class="sxs-lookup"><span data-stu-id="6272f-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="6272f-388">Dieses Problem zu umgehen, starten Sie Google Chrome, klicken Sie auf die *anpassen und Google Chrome-Steuerelement* Menü klicken Sie auf *Optionen*, und klicken Sie dann auf *stellen Google Chrome mein Standardbrowser*.</span><span class="sxs-lookup"><span data-stu-id="6272f-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="6272f-389">Problem: Das Dialogfeld "Foreign Key" lässt keine zu einen primären Schlüssel eingeben</span><span class="sxs-lookup"><span data-stu-id="6272f-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="6272f-390">Die **Fremdschlüssel** Dialogfeld lässt nicht zu der Primärschlüsselname geben an der Primärschlüsseltabelle.</span><span class="sxs-lookup"><span data-stu-id="6272f-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="6272f-391">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-391">**Workaround**</span></span>  
> <span data-ttu-id="6272f-392">Dies ist beabsichtigt.</span><span class="sxs-lookup"><span data-stu-id="6272f-392">This is intentional.</span></span> <span data-ttu-id="6272f-393">Sie müssen nicht den Namen des primären Schlüssels auf der Primärschlüsseltabelle eingeben.</span><span class="sxs-lookup"><span data-stu-id="6272f-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="6272f-394">Problem: Die Schaltfläche "Beziehungen" ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="6272f-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="6272f-395">Die **Beziehungen** Schaltfläche der **Tabelle** Registerkarte die **Datenbanken** Arbeitsbereich ist für SQL Server Compact-Datenbanken deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="6272f-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="6272f-396">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-396">**Workaround**</span></span>  
> <span data-ttu-id="6272f-397">Keine</span><span class="sxs-lookup"><span data-stu-id="6272f-397">None.</span></span> <span data-ttu-id="6272f-398">SQL Server Compact unterstützt keine Beziehungen zwischen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="6272f-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="6272f-399">Problem: Parametrisierte SQL-Abfragen Auslösen von Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="6272f-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="6272f-400">In SQL Server Compact 4.0, wenn Sie einen Datentyp nicht wie z. B. angeben `SqlDbType` oder `DbType` für Parameter in parametrisierten Abfragen, wird eine Ausnahme ausgelöst, wenn die Abfrage ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6272f-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="6272f-401">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="6272f-401">**Workaround**</span></span>  
> <span data-ttu-id="6272f-402">Legen Sie den Datentyp für Parameter explizit z.B. `SqlDbType` oder `DbType`.</span><span class="sxs-lookup"><span data-stu-id="6272f-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="6272f-403">Dies ist besonders wichtig, im Fall von BLOB-Datentypen (`image` und `ntext`).</span><span class="sxs-lookup"><span data-stu-id="6272f-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="6272f-404">Verwenden Sie Code wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6272f-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="6272f-405">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="6272f-405">For More Information</span></span>

<span data-ttu-id="6272f-406">Weitere Informationen zu WebMatrix Beta 3 finden Sie unter den folgenden Websites:</span><span class="sxs-lookup"><span data-stu-id="6272f-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="6272f-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="6272f-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="6272f-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6272f-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="6272f-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="6272f-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="6272f-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="6272f-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="6272f-411">Alle Rechte vorbehalten.</span><span class="sxs-lookup"><span data-stu-id="6272f-411">All Rights Reserved.</span></span> <span data-ttu-id="6272f-412">[Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="6272f-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
