---
uid: web-pages/readme/overview
title: WebMatrix Readme | Microsoft Docs
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Version 1.0-Infodatei
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 924bf04772a1d73c7fdfb1168090daef388438ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398799"
---
<a name="webmatrix-readme"></a><span data-ttu-id="2053e-103">WebMatrix-Infodatei</span><span class="sxs-lookup"><span data-stu-id="2053e-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="2053e-104">13 Januar 2011</span><span class="sxs-lookup"><span data-stu-id="2053e-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="2053e-105">Inhalt</span><span class="sxs-lookup"><span data-stu-id="2053e-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="2053e-106">Diese Infodatei gilt für Version 1.0 von WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="2053e-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="2053e-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2053e-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="2053e-108">Installation</span><span class="sxs-lookup"><span data-stu-id="2053e-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="2053e-109">Gewusst wie: Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2053e-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="2053e-110">Änderungen und Probleme</span><span class="sxs-lookup"><span data-stu-id="2053e-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="2053e-111">WebMatrix-1.0-Installation</span><span class="sxs-lookup"><span data-stu-id="2053e-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="2053e-112">ASP.NET-Webseiten 2</span><span class="sxs-lookup"><span data-stu-id="2053e-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="2053e-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="2053e-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="2053e-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="2053e-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="2053e-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="2053e-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="2053e-116">Installieren von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2053e-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="2053e-117">Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2053e-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="2053e-118">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="2053e-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="2053e-119">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2053e-119">Overview</span></span>

> <span data-ttu-id="2053e-120">Microsoft WebMatrix 1.0 ist ein kostenloser Web-entwicklungsstapel, die innerhalb von Minuten installiert.</span><span class="sxs-lookup"><span data-stu-id="2053e-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="2053e-121">Einen Webserver integriert mit der Datenbank und Programmierframeworks erstellen Sie eine einzige, integrierte Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="2053e-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="2053e-122">Sie können mithilfe von WebMatrix die zur Optimierung die Sie programmieren, testen und veröffentlichen Ihre eigene Website ASP.NET oder PHP, oder Sie können WebMatrix verwenden, um eine neue Website mithilfe beliebter Open-Source-apps, z. B. DotNetNuke, Umbraco, WordPress oder Joomla.</span><span class="sxs-lookup"><span data-stu-id="2053e-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="2053e-123">WebMatrix verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausgeführt wird, gestaltet sich der Übergang von der Entwicklung zur Produktion reibungs- und nahtlos.</span><span class="sxs-lookup"><span data-stu-id="2053e-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="2053e-124">Installation</span><span class="sxs-lookup"><span data-stu-id="2053e-124">Installation</span></span>

> <span data-ttu-id="2053e-125">Um WebMatrix 1.0 installieren, installieren Sie zuerst die [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="2053e-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="2053e-126">Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zum Installieren von WebMatrix verwenden.</span><span class="sxs-lookup"><span data-stu-id="2053e-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="2053e-127">Wenn Sie während der Installation Probleme auftreten, finden Sie unter [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="2053e-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="2053e-128">Gewusst wie: Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2053e-128">How to Publish Applications</span></span>

> <span data-ttu-id="2053e-129">Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="2053e-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="2053e-130">Änderungen und Probleme</span><span class="sxs-lookup"><span data-stu-id="2053e-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="2053e-131">WebMatrix 1.0 Installation Issues</span><span class="sxs-lookup"><span data-stu-id="2053e-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="2053e-132">Problem: WebMatrix 1.0 steht nur auf Plattformen, die Unterstützung von Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="2053e-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="2053e-133">.NET Framework, Version 4 ist erforderlich für WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="2053e-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="2053e-134">In bestimmten Fällen kann können der WebMatrix-1.0-Installer Sie versuchen, auf einer Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="2053e-135">Insbesondere Windows Vista ohne das SP1-Update können Sie die Installation von WebMatrix beginnen, aber die .NET Framework 4-Komponente fehl und die Installation blockieren.</span><span class="sxs-lookup"><span data-stu-id="2053e-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="2053e-136">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-136">**Workaround**</span></span>  
> <span data-ttu-id="2053e-137">Installieren Sie auf eine unterstützte Plattform, einschließlich:</span><span class="sxs-lookup"><span data-stu-id="2053e-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="2053e-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="2053e-138">Windows 7</span></span>
> - <span data-ttu-id="2053e-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="2053e-139">Windows Server 2008</span></span>
> - <span data-ttu-id="2053e-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="2053e-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="2053e-141">Windows Vista SP1 oder höher</span><span class="sxs-lookup"><span data-stu-id="2053e-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="2053e-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="2053e-142">Windows XP SP3</span></span>
> - <span data-ttu-id="2053e-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="2053e-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="2053e-144">Problem: WebMatrix 1.0 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist</span><span class="sxs-lookup"><span data-stu-id="2053e-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="2053e-145">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-145">**Workaround**</span></span>  
> <span data-ttu-id="2053e-146">Installieren Sie [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) aus dem Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="2053e-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="2053e-147">Problem: Einige Assemblys für SQL Server Compact 4.0 sind nicht im globalen Assemblycache installiert.</span><span class="sxs-lookup"><span data-stu-id="2053e-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="2053e-148">Die verwalteten Assemblys für SQL Server Compact 4.0 werden nicht im globalen Assemblycache (GAC) abgelegt, wenn Sie SQL Server Compact 4.0 auf einem 64-Bit-Computer installieren, und der Computer nur das .NET Framework 3.5 SP1-Client Profile installiert verfügt.</span><span class="sxs-lookup"><span data-stu-id="2053e-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="2053e-149">Sind die verwalteten Assemblys, die nicht im GAC installiert werden:</span><span class="sxs-lookup"><span data-stu-id="2053e-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="2053e-150">*System.Data.SqlServerCe.dll* (ADO.NET-Anbieter)</span><span class="sxs-lookup"><span data-stu-id="2053e-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="2053e-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="2053e-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="2053e-152">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-152">**Workaround**</span></span>  
> <span data-ttu-id="2053e-153">Deinstallieren von SQLServer Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="2053e-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="2053e-154">Herunterladen Sie und installieren Sie die Vollversion von .NET Framework 3.5 SP1 von folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="2053e-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="2053e-155">Microsoft .NET Framework 3.5 Service Pack 1 (vollständiges Paket)</span><span class="sxs-lookup"><span data-stu-id="2053e-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="2053e-156">Installieren Sie SQL Server Compact 4.0 klicken Sie dann erneut.</span><span class="sxs-lookup"><span data-stu-id="2053e-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="2053e-157">Problem: Nicht SQL Server Compact über die Befehlszeile deinstallieren.</span><span class="sxs-lookup"><span data-stu-id="2053e-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="2053e-158">Deinstallation von SQL Server Compact mithilfe von Befehlszeilenoptionen funktioniert nicht in dieser Version.</span><span class="sxs-lookup"><span data-stu-id="2053e-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="2053e-159">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-159">**Workaround**</span></span>  
> <span data-ttu-id="2053e-160">Verwendung *Programme und Funktionen* in der Windows-Systemsteuerung zum Deinstallieren von Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="2053e-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="2053e-161">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="2053e-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="2053e-162">Dieser Abschnitt des Dokuments Beschreibt neue Features, Änderungen und bekannte Probleme mit der Version 1.0 von ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="2053e-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="2053e-163">Neue features</span><span class="sxs-lookup"><span data-stu-id="2053e-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="2053e-164">Änderungen</span><span class="sxs-lookup"><span data-stu-id="2053e-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="2053e-165">Probleme</span><span class="sxs-lookup"><span data-stu-id="2053e-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="2053e-166">Neue Features</span><span class="sxs-lookup"><span data-stu-id="2053e-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="2053e-167">Neu: Konfigurationseinstellung hinzugefügt, um den Paket-Manager zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="2053e-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="2053e-168">Ein neues `asp:AdminManagerEnabled` Schlüssel steht für die `<appSettings>` Element in der *"Web.config"* -Datei, die den Paket-Manager vollständig deaktivieren kann.</span><span class="sxs-lookup"><span data-stu-id="2053e-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="2053e-169">Der Standardwert für dieses Element ist "true", was bedeutet, dass, wenn es nicht, in enthalten ist der *"Web.config"* der Paket-Manager-Datei aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="2053e-170">Fügen Sie das folgende Element hinzu, um den Paket-Manager Deaktivieren der *"Web.config"* Datei im Stammverzeichnis der Website:</span><span class="sxs-lookup"><span data-stu-id="2053e-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="2053e-171">Änderungen</span><span class="sxs-lookup"><span data-stu-id="2053e-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="2053e-172">Änderung: "WebPages:AdminFolderVirtualPath" Schlüssel "Asp: AdminFolderVirtualPath" umbenannt</span><span class="sxs-lookup"><span data-stu-id="2053e-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="2053e-173">Die `webPages:AdminFolderVirtualPath` Schlüssel, der hinzugefügt werden kann die *"Web.config"* Datei an den Speicherort des Paket-Managers Verwendung umbenannt wurde die `asp:` -Namespace anstelle des der `webPages` Namespace.</span><span class="sxs-lookup"><span data-stu-id="2053e-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="2053e-174">Wenn Sie dieses Element verwendet haben, müssen Sie es in der Konfigurationsdatei umbenennen.</span><span class="sxs-lookup"><span data-stu-id="2053e-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="2053e-175">Bekannte Probleme</span><span class="sxs-lookup"><span data-stu-id="2053e-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="2053e-176">Problem: Von Kennwörtern für Mitgliedschaftsbenutzer, die nicht mehr erkannt</span><span class="sxs-lookup"><span data-stu-id="2053e-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="2053e-177">Der Algorithmus zum Erstellen und Speichern von Kennwörtern für Mitgliedschaft (Anmeldename) wurde geändert, um Sie sicherer werden.</span><span class="sxs-lookup"><span data-stu-id="2053e-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="2053e-178">Die für Mitglieder (Benutzer) in der Beta-Versionen von ASP.NET Razor erstellte gespeicherte Kennwörter werden daher nicht erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="2053e-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="2053e-179">**Problemumgehung** Wenn der Standort noch nicht in der Produktion gespeichert wurde, entfernen Sie die Benutzerdatensätze aus der Mitgliedschaftsdatenbank von.</span><span class="sxs-lookup"><span data-stu-id="2053e-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="2053e-180">Wenn die Datenbank aktiv ist, generieren Sie vorhandene Kennwörter in der Mitgliedschaftsdatenbank programmgesteuert neu.</span><span class="sxs-lookup"><span data-stu-id="2053e-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="2053e-181">Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="2053e-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="2053e-182">Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode.</span><span class="sxs-lookup"><span data-stu-id="2053e-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="2053e-183">(In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn der `autoCreateTables` Parameter dieser Methode wird festgelegt, auf "true" (standardmäßig es nastaven NA hodnotu true in der Vorlage Starter Site), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2053e-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="2053e-184">Stattdessen wird automatisch in der Tabelle erstellt.</span><span class="sxs-lookup"><span data-stu-id="2053e-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="2053e-185">Dies kann ein Problem sein, wenn Sie beabsichtigen, übergeben den falschen Tabellennamen an aber verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft der `WebSecurity.InitializeDatabaseConnection` Methode.</span><span class="sxs-lookup"><span data-stu-id="2053e-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="2053e-186">Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung zu funktionieren scheinen.</span><span class="sxs-lookup"><span data-stu-id="2053e-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="2053e-187">Allerdings kann Anwendungscode, der für die benutzerdefinierte Tabelle (und darin enthaltenen Felder) stützt sich schließlich unerwartete Fehlern auftreten.</span><span class="sxs-lookup"><span data-stu-id="2053e-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="2053e-188">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-188">**Workaround**</span></span>  
> <span data-ttu-id="2053e-189">Stellen Sie sicher, dass der Name in übergeben die `InitializeDatabaseConnection` Methode entspricht, das Benutzerprofil in der Mitgliedschaftsdatenbank Tabelle, oder stellen Sie sicher, dass, die `autoCreateTables` Parameter auf "false" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="2053e-190">Problem: Fehlermeldung "das Admin-Modul erfordert Zugriff auf ~/App\_Daten"</span><span class="sxs-lookup"><span data-stu-id="2053e-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="2053e-191">Unter bestimmten Umständen möchten Benutzer erstellen oder arbeiten andernfalls mit dem ASP.NET-Mitgliedschaftssystem kann dazu führen, dass die Seite zum Anzeigen des Fehlers *der Admin-Modul erfordert Zugriff auf ~/App\_Daten*.</span><span class="sxs-lookup"><span data-stu-id="2053e-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="2053e-192">Dies tritt auf, wenn das Konto, das unter IIS oder IIS Express ausgeführt wird nicht über Berechtigungen zum Erstellen und Schreiben in die *App\_Daten* Ordner unter dem Stammverzeichnis der Website.</span><span class="sxs-lookup"><span data-stu-id="2053e-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="2053e-193">**Problemumgehung** erstellen Sie manuell eine *App\_Daten* Ordner für die Website.</span><span class="sxs-lookup"><span data-stu-id="2053e-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="2053e-194">Stellen Sie sicher, dass das Windows-Konto, das die Anwendung ausgeführt, unter dem Konto (normalerweise Netzwerkdienst wird) Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner, z. B. App hat\_Daten.</span><span class="sxs-lookup"><span data-stu-id="2053e-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="2053e-195">Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [Probleme mit der SQL Server Express Benutzerinstanziierung und ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="2053e-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="2053e-196">Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler</span><span class="sxs-lookup"><span data-stu-id="2053e-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="2053e-197">Wenn eine WebMatrix-Web-Anwendung SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, können Sie ein Fehler angezeigt, der angibt, dass SQL Server des Pfads des Benutzers lokale Anwendung zur Laufzeit abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="2053e-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="2053e-198">**Problemumgehung** stellen sicher, dass das Windows-Konto, das die Anwendung ausgeführt, unter dem Konto (normalerweise Netzwerkdienst wird) Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner wie z. B. *App\_Daten*.</span><span class="sxs-lookup"><span data-stu-id="2053e-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="2053e-199">Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [Probleme mit der SQL Server Express Benutzerinstanziierung und ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="2053e-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="2053e-200">Problem: Dateien, die Paket-Manager-Ressourcen oder Paket-Manager-Kennwörter enthalten sind, bereitstellbar unter IIS 6.0 und früher</span><span class="sxs-lookup"><span data-stu-id="2053e-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="2053e-201">Wenn Sie eine ASP.NET Web Pages (Razor)-Anwendung bereitstellen, die mit dem RC2-Release erstellt wurde, und wenn die Anwendung enthält eine *password.txt* oder *packagesources.txt* Datei   */App\_ Daten/Admin*, IIS 6.0 wird die Datei, falls angefordert, wodurch möglicherweise die Kennwörter für die Paket-Manager-Instanz, dienen.</span><span class="sxs-lookup"><span data-stu-id="2053e-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="2053e-202">**Problemumgehung** Umbenennen der *password.txt* oder *packagesources.txt* Datei *password.config* oder *packagesources.config*. Standardmäßig IIS 6.0 nicht dienen Dateien, die die *config* Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="2053e-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="2053e-203">(In IIS 7 können keine Dateien in die *App\_Daten* Ordner verarbeitet werden, damit Sie nicht benötigen, um die Dateien umzubenennen.)</span><span class="sxs-lookup"><span data-stu-id="2053e-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="2053e-204">Problem: Deinstallieren von Paketen installiert, mit der Beta 3-Version nicht vollständig Paketkomponenten entfernt</span><span class="sxs-lookup"><span data-stu-id="2053e-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="2053e-205">Wenn Sie ein Paket mit der Paket-Manager in der Beta 3-Version installiert, und wiederholen Sie dann die aktuelle Version deinstallieren, wird das Paket wurde nicht vollständig deinstalliert.</span><span class="sxs-lookup"><span data-stu-id="2053e-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="2053e-206">Mithilfe des Paket-Managers **Deinstallieren** Schaltfläche entfernt einige Komponenten, jedoch bleibt der Code für die Paket Bibliothek und aktualisiert nicht die *Datei "Package.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="2053e-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="2053e-207">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-207">**Workaround** </span></span>  
> <span data-ttu-id="2053e-208">Gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="2053e-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="2053e-209">Löschen der *App\_Data\packages* Ordner.</span><span class="sxs-lookup"><span data-stu-id="2053e-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="2053e-210">Dadurch werden alle Pakete entfernt.</span><span class="sxs-lookup"><span data-stu-id="2053e-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="2053e-211">Löschen der *"Packages.config"* Datei im Stammverzeichnis der Website.</span><span class="sxs-lookup"><span data-stu-id="2053e-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="2053e-212">Problem: In Visual Studio Aufrufen der Web-basierte Paket-Manager die Anwendung offline geschaltet</span><span class="sxs-lookup"><span data-stu-id="2053e-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="2053e-213">Wenn Sie in Visual Studio (nicht WebMatrix) arbeiten, und verwenden Sie die  *\_Admin* Funktionalität, um den Paket-Manager, Visual Studio zu starten, wird die Anwendung offline geschaltet, und stellt die *app\_ offline.htm* in den Stammordner der Website, unterbricht die Ihre Fähigkeit, den Paket-Manager verwenden.</span><span class="sxs-lookup"><span data-stu-id="2053e-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="2053e-214">Obwohl i. d. r. dieses Verhalten angezeigt werden, wenn die Benutzeroberfläche des webbasierten-Paket-Manager verwenden, tritt das gleiche Verhalten auf, wenn Sie hinzufügen, entfernen oder ändern alle Dateien in die *App\_Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="2053e-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="2053e-215">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-215">**Workaround** </span></span>  
> <span data-ttu-id="2053e-216">Um mit Pakete in Visual Studio arbeiten, verwenden Sie die NuGet-Erweiterung anstelle der webbasierten-Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="2053e-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="2053e-217">Weitere Informationen finden Sie unter den [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="2053e-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="2053e-218">Bei Verwendung mit anderen Dateien in die *App\_Daten* Ordner, sollten Sie die Dateien behalten an einem anderen Ort, um dieses Problem zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="2053e-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="2053e-219">Wenn dies nicht möglich ist, löschen Sie die *app\_offline.htm* Datei manuell, oder warten Sie, bis der Standort automatisch (standardmäßig nach 30 Sekunden) wieder online geschaltet wird.</span><span class="sxs-lookup"><span data-stu-id="2053e-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="2053e-220">Problem: Visual Studio IntelliSense und Projektvorlagen nur in ASP.NET MVC 3-Version verfügbar</span><span class="sxs-lookup"><span data-stu-id="2053e-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="2053e-221">Installieren von ASP.NET Web Pages wird nicht auch Tools für Visual Studio wie IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen installiert.</span><span class="sxs-lookup"><span data-stu-id="2053e-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="2053e-222">**Problemumgehung** um IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio zu verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="2053e-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="2053e-223">Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver</span><span class="sxs-lookup"><span data-stu-id="2053e-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="2053e-224">Ist der Server mit der Website hinter einem Proxyserver befinden, müssen Sie möglicherweise konfigurieren Informationen zum Proxy in der *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die von außerhalb Ihrer Website stammt.</span><span class="sxs-lookup"><span data-stu-id="2053e-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="2053e-225">Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber von Ihrem Proxyserver blockiert werden.</span><span class="sxs-lookup"><span data-stu-id="2053e-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="2053e-226">Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed ein, die im Paket-Manager verwendet werden-Proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="2053e-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="2053e-227">Bei Problemen in arbeiten mit einem externen Dienst oder Arbeiten mit den paketfeed ein, fügen Sie die folgenden Elemente in Ihrer Anwendung Stamm *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="2053e-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="2053e-228">Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="2053e-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="2053e-229">Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert</span><span class="sxs-lookup"><span data-stu-id="2053e-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="2053e-230">Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist die ASP.NET Web Pages mit Razor-Syntax deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2053e-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="2053e-231">Seiten mit den *.cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2053e-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="2053e-232">ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* Datei und Entfernen von .NET Framework wird diese Datei entfernt.</span><span class="sxs-lookup"><span data-stu-id="2053e-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="2053e-233">Neuinstallation von .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2053e-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="2053e-234">**Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="2053e-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="2053e-235">Dadurch wird das folgende Element zum hinzugefügt. die *"Web.config"* -Datei in das Stammverzeichnis des Computer, der in der Regel an folgendem Speicherort ist:</span><span class="sxs-lookup"><span data-stu-id="2053e-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="2053e-236">Problem: URLs ohne Erweiterung.cshtml/.vbhtml-Dateien auf IIS 7 oder IIS 7.5 nicht finden</span><span class="sxs-lookup"><span data-stu-id="2053e-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="2053e-237">Auf IIS 7 oder IIS 7.5, Anforderungen mit einer URL wie im folgenden sind nicht Seiten zu finden, die die *.cshtml* oder *vbhtml* Erweiterung:</span><span class="sxs-lookup"><span data-stu-id="2053e-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="2053e-238">Das Problem tritt auf, weil der URL-Umschreibung nicht standardmäßig für IIS 7 oder IIS 7.5 aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="2053e-239">Das likeliest Szenario ist, dass das Problem nicht angezeigt werden, wird wenn Tests lokal mithilfe von IIS Express, aber Sie entdecken Sie es beim Bereitstellen Ihrer Websites auf einer hosting-Website.</span><span class="sxs-lookup"><span data-stu-id="2053e-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="2053e-240">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="2053e-241">Wenn Sie die Kontrolle über den Server-Computer verfügen, installieren Sie auf dem Computer das Update, das beschrieben ist [ein Update ist verfügbar, können Sie bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="2053e-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="2053e-242">Wenn Sie keine Kontrolle über den Server-Computer verfügen (z. B. Sie bereitstellen auf einer hosting-Website), fügen Sie Folgendes auf der Website *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="2053e-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="2053e-243">Problem: Bereitstellen einer Anwendung auf einem Computer, die keinen SQL Server Compact installiert</span><span class="sxs-lookup"><span data-stu-id="2053e-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="2053e-244">Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, in dem SQL Server Compact nicht installiert ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="2053e-245">Microsoft WebMatrix 1.0 automatisch diese Binärdateien für die Sie kopiert und führt die entsprechende *"Web.config"* Datei transformiert.</span><span class="sxs-lookup"><span data-stu-id="2053e-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="2053e-246">**Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="2053e-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="2053e-247">Kopieren Sie die Datenbank-Engine Assemblys, die *Bin* Ordner (sowie Unterordner) der Anwendung auf dem Zielcomputer:</span><span class="sxs-lookup"><span data-stu-id="2053e-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="2053e-248">Kopie *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="2053e-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="2053e-249">**um** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="2053e-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="2053e-250">Kopie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>zu</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="2053e-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="2053e-251">Kopie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>zu</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="2053e-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="2053e-252">Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="2053e-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="2053e-253">(In WebMatrix 1.0 dieser Dateityp ist verfügbar, wenn Sie auf **alle** in die **wählen Sie einen Dateityp** Dialogfeld.)</span><span class="sxs-lookup"><span data-stu-id="2053e-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="2053e-254">Fügen Sie das folgende Element als untergeordnetes Element der `<configuration>` Element (nicht in der `<system.web>` Element):</span><span class="sxs-lookup"><span data-stu-id="2053e-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="2053e-255">Problem: "Database" und "WebGrid" Hilfsprogramme funktionieren nicht in der mittleren Vertrauensebene in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="2053e-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="2053e-256">Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung festgelegt wurde, auf die mittlere Vertrauenswürdigkeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="2053e-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="2053e-257">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-257">**Workaround**</span></span>  
> <span data-ttu-id="2053e-258">Wenn Sie Visual Studio 2010 verwenden, können Sie dieses Problem beheben, installieren Sie die Service Pack 1-Version.</span><span class="sxs-lookup"><span data-stu-id="2053e-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="2053e-259">Bis die endgültige Version von der SP1-Version verfügbar ist, können Sie die Betaversion von SP1 von der [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Seite im Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="2053e-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="2053e-260">Legen die Anwendung mit voller Vertrauenswürdigkeit, wenn dies nicht praktikabel ist, oder wenn Sie nicht Visual Studio 2010 verwenden, Sie vorübergehend können.</span><span class="sxs-lookup"><span data-stu-id="2053e-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="2053e-261">Problem: "ApplicationPart" Ressourcen extern zugänglich sind</span><span class="sxs-lookup"><span data-stu-id="2053e-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="2053e-262">Wenn eine Assembly Objekte, die enthält von abgeleitet ist die `ApplicationPart` Klasse, die, die Ressourcen-Assembly verfügbar gemacht werden, indem die `ResourceRouteHandler` Klasse.</span><span class="sxs-lookup"><span data-stu-id="2053e-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="2053e-263">Nehmen wir beispielsweise die folgende URL:</span><span class="sxs-lookup"><span data-stu-id="2053e-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="2053e-264">Diese Anforderung lädt alle Ressourcenzeichenfolgen in der *System.Web.WebPages.Administration.dll* Assembly.</span><span class="sxs-lookup"><span data-stu-id="2053e-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="2053e-265">Alle von eingebetteten Ressourcen (einschließlich der nicht als statische Inhalte bereitgestellt werden sollen) werden heruntergeladen.</span><span class="sxs-lookup"><span data-stu-id="2053e-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="2053e-266">Wenn die eingebetteten Ressourcen sensible Informationen enthalten, kann dies ein Sicherheitsrisiko darstellen.</span><span class="sxs-lookup"><span data-stu-id="2053e-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="2053e-267">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-267">**Workaround** </span></span>  
> <span data-ttu-id="2053e-268">Bei der Erstellung einer **ApplicationPart** Objekt, stellen Sie sicher, dass die eingebetteten Ressourcen, die zugeordnet **ApplicationPart** Assembly Objekts enthalten keine vertraulichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="2053e-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="2053e-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="2053e-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="2053e-270">Weitere Informationen zu Installationsproblemen für WebMatrix, finden Sie unter [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation) weiter oben in diesem Dokument.</span><span class="sxs-lookup"><span data-stu-id="2053e-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="2053e-271">In diesem Abschnitt des Dokuments beschreibt bekannte Probleme bei der WebMatrix-Entwicklungsumgebung.</span><span class="sxs-lookup"><span data-stu-id="2053e-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="2053e-272">Problem: Änderungen in der Benutzername oder das Kennwort für eine Datenbank-Verbindungszeichenfolge in einer Datei "Web.config" werden im Arbeitsbereich "Datenbanken" nicht widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="2053e-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="2053e-273">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="2053e-274">In der *"Web.config"* Datei, die Namen der Datenbank, in der Verbindungszeichenfolge ändern (z. B. "1" darin hinzufügen).</span><span class="sxs-lookup"><span data-stu-id="2053e-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="2053e-275">Speichern Sie die *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="2053e-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="2053e-276">Klicken Sie auf **Datenbanken** und zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2053e-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="2053e-277">Ändern Sie den Datenbanknamen in der Verbindungszeichenfolge in der *"Web.config"* Datei, die den ursprünglichen Datenbanknamen zurück.</span><span class="sxs-lookup"><span data-stu-id="2053e-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="2053e-278">Speichern Sie die *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="2053e-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="2053e-279">Klicken Sie auf **Datenbanken** und zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2053e-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="2053e-280">Problem: Von WebMatrix erstellt Ordner können nicht gelöscht werden</span><span class="sxs-lookup"><span data-stu-id="2053e-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="2053e-281">Wenn WebMatrix mithilfe erhöhter Berechtigungen ausgeführt wird (d. h. Schritten mit der WebMatrix die **als Administrator ausführen** -Option in Windows), Ordnern, die von WebMatrix erstellt werden können nicht gelöscht werden, mit dem Windows-Explorer.</span><span class="sxs-lookup"><span data-stu-id="2053e-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="2053e-282">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-282">**Workaround**</span></span>  
> <span data-ttu-id="2053e-283">Führen Sie Windows-Explorer mit erweiterten Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="2053e-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="2053e-284">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2053e-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="2053e-285">Klicken Sie in Windows, auf **starten**.</span><span class="sxs-lookup"><span data-stu-id="2053e-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="2053e-286">Geben Sie "Windows-Explorer", und mit der rechten Maustaste in des Eintrags für **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="2053e-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="2053e-287">Klicken Sie auf **als Administrator ausführen**.</span><span class="sxs-lookup"><span data-stu-id="2053e-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="2053e-288">Anschließend können Sie die Ordner löschen.</span><span class="sxs-lookup"><span data-stu-id="2053e-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="2053e-289">Problem: WebMatrix 1.0 ist nicht möglich, bestimmte Aufgaben ausführen, die erhöhte Rechte erfordern.</span><span class="sxs-lookup"><span data-stu-id="2053e-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="2053e-290">WebMatrix 1.0 kann nicht für bestimmte Aufgaben, die eine Erhöhung der Rechte wie z. B. die Installation zusätzlicher Komponenten in den folgenden Situationen erfordern:</span><span class="sxs-lookup"><span data-stu-id="2053e-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="2053e-291">Klicken Sie auf Windows Vista oder Windows 7 Sie angemeldet sind, sich mit einem Konto an, die nicht über Administratorberechtigungen verfügt, und (User Account Control, UAC) ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2053e-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="2053e-292">Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="2053e-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="2053e-293">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-293">**Workaround**</span></span>  
> <span data-ttu-id="2053e-294">Die meisten Aufgaben in WebMatrix 1.0 sind keine Administratorberechtigungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2053e-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="2053e-295">Für diejenigen, die ausführen, können Sie den Vorgang als Administrator ausführen, oder gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="2053e-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="2053e-296">Auf Windows Vista oder Windows 7 aktiviert.</span><span class="sxs-lookup"><span data-stu-id="2053e-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="2053e-297">Unter Windows XP müssen fügen Sie den Benutzer auf die Sicherheitsgruppe "Administratoren hinzu".</span><span class="sxs-lookup"><span data-stu-id="2053e-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="2053e-298">Problem: "Site from Web Gallery" ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2053e-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="2053e-299">Die **Standort Web Gallery** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="2053e-300">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-300">**Workaround**</span></span>  
> <span data-ttu-id="2053e-301">Installieren Sie die [Microsoft Webplattform-Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="2053e-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="2053e-302">Problem: Google Chrome ist nicht verfügbar ist, als eine ausführoption</span><span class="sxs-lookup"><span data-stu-id="2053e-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="2053e-303">Google Chrome wird nicht angezeigt, in der Liste der Browser unter **ausführen** auf die **Startseite** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="2053e-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="2053e-304">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-304">**Workaround**</span></span>  
> <span data-ttu-id="2053e-305">Einige Versionen von Google Chrome ist nicht selbst ordnungsgemäß mit dem Default Programs-Feature in Windows registriert werden.</span><span class="sxs-lookup"><span data-stu-id="2053e-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="2053e-306">Dieses Problem zu umgehen, starten Sie Google Chrome, klicken Sie auf die *anpassen und Google Chrome-Steuerelement* Menü klicken Sie auf *Optionen*, und klicken Sie dann auf *stellen Google Chrome mein Standardbrowser*.</span><span class="sxs-lookup"><span data-stu-id="2053e-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="2053e-307">Problem: Das Dialogfeld "Foreign Key" lässt keine zu einen primären Schlüssel eingeben</span><span class="sxs-lookup"><span data-stu-id="2053e-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="2053e-308">Die **Fremdschlüssel** Dialogfeld lässt nicht zu der Primärschlüsselname geben an der Primärschlüsseltabelle.</span><span class="sxs-lookup"><span data-stu-id="2053e-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="2053e-309">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-309">**Workaround**</span></span>  
> <span data-ttu-id="2053e-310">Dies ist beabsichtigt.</span><span class="sxs-lookup"><span data-stu-id="2053e-310">This is intentional.</span></span> <span data-ttu-id="2053e-311">Sie müssen nicht den Namen des primären Schlüssels auf der Primärschlüsseltabelle eingeben.</span><span class="sxs-lookup"><span data-stu-id="2053e-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="2053e-312">Sollte möglichst die Website erneut zu veröffentlichen (oder dessen Veröffentlichung) mit nicht-Administrator-Anmeldeinformationen für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2053e-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="2053e-313">Weitere Informationen zu WebMatrix 1.0 finden Sie unter den folgenden Websites:</span><span class="sxs-lookup"><span data-stu-id="2053e-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="2053e-314">Allerdings ist sie nicht für andere Sprachen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2053e-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="2053e-315">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-315">**Workaround** </span></span>  
> <span data-ttu-id="2053e-316">Keine</span><span class="sxs-lookup"><span data-stu-id="2053e-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="2053e-317">Problem: IntelliSense für HTML- und CSS-vorgeschlagen Elemente, die nicht angemessen sind.</span><span class="sxs-lookup"><span data-stu-id="2053e-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="2053e-318">IntelliSense für Markup in WebMatrix unterstützt die Verwendung von HTML-die [XHTML 1.0 Transitional Schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) und CSS verwenden die [CSS 2.1-Schema](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="2053e-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="2053e-319">Da IntelliSense für diese bestimmte Schemas basiert, können bestimmte Tags, Attribute oder Eigenschaften vorgeschlagen, die nicht für die aktuelle Seite oder Definition geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="2053e-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="2053e-320">Für HTML kann es auch zu unerwartete Vorschläge in Inhalten führen, die interpretiert werden kann, als falsch formatierte XHTML (z. B., wenn Tags nicht geschlossen werden).</span><span class="sxs-lookup"><span data-stu-id="2053e-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="2053e-321">Dieses Problem möglicherweise deutlicher bemerkbar, wenn die Einfügemarke innerhalb eines Tags nicht vollständig ist; IntelliSense kann in diesem Fall empfohlen, neue öffnenden Tags oder bieten andere falsche Vorschläge.</span><span class="sxs-lookup"><span data-stu-id="2053e-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="2053e-322">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-322">**Workaround** </span></span>  
> <span data-ttu-id="2053e-323">Für HTML sicher, dass Sie in eine wohlgeformte, vollständige XHTML-Seite arbeiten.</span><span class="sxs-lookup"><span data-stu-id="2053e-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="2053e-324">Für CSS gibt es keine problemumgehung.</span><span class="sxs-lookup"><span data-stu-id="2053e-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="2053e-325">Problem: IntelliSense wird nicht aufgerufen werden, während der Eingabe</span><span class="sxs-lookup"><span data-stu-id="2053e-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="2053e-326">IntelliSense kann in einigen Fällen nicht aufgerufen werden, wie HTML oder CSS-Code im Editor eingegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2053e-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="2053e-327">Dies kann insbesondere bei die Einfügemarke direkt neben einem anderen Element oder am Ende einer Datei ist vorkommen.</span><span class="sxs-lookup"><span data-stu-id="2053e-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="2053e-328">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-328">**Workaround** </span></span>  
> <span data-ttu-id="2053e-329">Stellen Sie sicher, dass Leerstellen um die Einfügemarke vorhanden ist, dass die Einfügemarke nicht am Ende einer Datei vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2053e-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="2053e-330">Sie können IntelliSense auch manuell aufrufen, durch Drücken von STRG + LEERTASTE.</span><span class="sxs-lookup"><span data-stu-id="2053e-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="2053e-331">Problem: Keine Benutzeroberfläche wird zum Deaktivieren von IntelliSense zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="2053e-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="2053e-332">WebMatrix-1.0 stellt keine Benutzeroberfläche oder einer Geste zum Deaktivieren von IntelliSense bereit.</span><span class="sxs-lookup"><span data-stu-id="2053e-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="2053e-333">**Dieses Problem zu umgehen** </span><span class="sxs-lookup"><span data-stu-id="2053e-333">**Workaround** </span></span>  
> <span data-ttu-id="2053e-334">Starten Sie WebMatrix mithilfe des folgenden Befehls, einen Switch enthält, der IntelliSense deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="2053e-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="2053e-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="2053e-335">IIS Express</span></span>

<span data-ttu-id="2053e-336">IIS Express hat eine eigene Readme-Datei, die unter der folgenden URL verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="2053e-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="2053e-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; Clcid = 0 x 409</span><span class="sxs-lookup"><span data-stu-id="2053e-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="2053e-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="2053e-338">SQL Server Compact</span></span>

<span data-ttu-id="2053e-339">SQL Server Compact verfügt über eine eigene Readme-Datei, die unter der folgenden URL verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="2053e-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="2053e-340">Weitere Informationen zu Problemen, bei denen Installation von SQL Server Compact als Teil von WebMatrix finden Sie unter [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation) weiter oben in diesem Dokument.</span><span class="sxs-lookup"><span data-stu-id="2053e-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="2053e-341">Installieren von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2053e-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="2053e-342">Problem: Installieren einer Anwendung kann eine lange dauern, wenn Ordner "Eigene Dateien" des Benutzers auf einer Netzwerkfreigabe umgeleitet wird</span><span class="sxs-lookup"><span data-stu-id="2053e-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="2053e-343">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-343">**Workaround**</span></span>  
> <span data-ttu-id="2053e-344">Keine</span><span class="sxs-lookup"><span data-stu-id="2053e-344">None.</span></span> <span data-ttu-id="2053e-345">Die Anwendung möglicherweise einige Zeit dauern installieren, jedoch ordnungsgemäß installiert wird.</span><span class="sxs-lookup"><span data-stu-id="2053e-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="2053e-346">Veröffentlichen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="2053e-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="2053e-347">Problem: "erforderlich, dass die Berechtigungen können nicht abgerufen werden" Fehler bei der Veröffentlichung von SQL Compact-Datenbank</span><span class="sxs-lookup"><span data-stu-id="2053e-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="2053e-348">WebMatrix unterstützt das Bereitstellen von unterstützender Binärdateien für SQL Server Compact auf einem Server, der .NET Framework, Version 3.5 mit einer Konfiguration für mittlere Vertrauenswürdigkeit ausgeführt wird nicht vollständig.</span><span class="sxs-lookup"><span data-stu-id="2053e-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="2053e-349">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-349">**Workaround**</span></span>  
> <span data-ttu-id="2053e-350">Die bevorzugte umgangen werden, die .NET Framework 4 auf dem Server zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2053e-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="2053e-351">Führen Sie alternativ folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2053e-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="2053e-352">Die folgenden Elemente zum Hinzufügen der `SecurityClasses` im Abschnitt *Web\_MediumTrust.config* Datei:</span><span class="sxs-lookup"><span data-stu-id="2053e-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="2053e-353">Erstellen Sie einen neuen Berechtigungssatz, der der *Web\_MediumTrust.config* -Datei mit den folgenden Berechtigungen:</span><span class="sxs-lookup"><span data-stu-id="2053e-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="2053e-354">Wenden Sie die Berechtigung auf SQL Server Compact festgelegt werden, indem Sie die folgenden Elemente im Platzieren der *Web\_MediumTrust.config* Datei:</span><span class="sxs-lookup"><span data-stu-id="2053e-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="2053e-355">Problem: Katalog und PhpBB Webanwendungen zeigt einen Fehler "Dienst ist nicht verfügbar" nach der Veröffentlichung</span><span class="sxs-lookup"><span data-stu-id="2053e-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="2053e-356">In einigen Fällen verursacht das Veröffentlichen einer Anwendung einen Fehler "Dienst ist nicht verfügbar" aus.</span><span class="sxs-lookup"><span data-stu-id="2053e-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="2053e-357">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-357">**Workaround**</span></span>  
> <span data-ttu-id="2053e-358">Fügen Sie einen umgekehrten Schrägstrich in WebMatrix (\) am Ende den Servernamen in der **Veröffentlichungseinstellungen** Fenster, und klicken Sie dann die Anwendung erneut veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2053e-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="2053e-359">Problem: Moodle-Website-Layout und die Links sind defekt nach der Veröffentlichung</span><span class="sxs-lookup"><span data-stu-id="2053e-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="2053e-360">Nachdem Sie eine Moodle-Anwendung veröffentlichen, funktioniert die Anwendung nicht ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="2053e-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="2053e-361">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-361">**Workaround**</span></span>  
> <span data-ttu-id="2053e-362">Fügen Sie in WebMatrix einen Schrägstrich (/) am Ende der **Standortname** Feld der **Veröffentlichungseinstellungen** Fenster, und klicken Sie dann die Anwendung erneut veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="2053e-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="2053e-363">Problem: Veröffentlichen von NopCommerce tritt ein Fehler</span><span class="sxs-lookup"><span data-stu-id="2053e-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="2053e-364">Veröffentlichung NopCommerce schlägt fehl, und meldet ein Fehler wie "Fügen Sie in der Nop\_Protokolltabelle ist fehlgeschlagen."</span><span class="sxs-lookup"><span data-stu-id="2053e-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="2053e-365">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="2053e-366">Klicken Sie in WebMatrix auf **ausführen** NopCommerce lokal zu starten.</span><span class="sxs-lookup"><span data-stu-id="2053e-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="2053e-367">Melden Sie sich die Seite "Verwaltung".</span><span class="sxs-lookup"><span data-stu-id="2053e-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="2053e-368">Klicken Sie auf die **System** Menü.</span><span class="sxs-lookup"><span data-stu-id="2053e-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="2053e-369">Klicken Sie auf die **Log** Option.</span><span class="sxs-lookup"><span data-stu-id="2053e-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="2053e-370">Klicken Sie auf die **Protokoll löschen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="2053e-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="2053e-371">Veröffentlichen Sie NopCommerce erneut.</span><span class="sxs-lookup"><span data-stu-id="2053e-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="2053e-372">Problem: Silverstripe CMS zeigt eine "HTTP 500 PHP FCGI Error", wenn Sie eine veröffentlichte Website herunterladen</span><span class="sxs-lookup"><span data-stu-id="2053e-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="2053e-373">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-373">**Workaround**</span></span>  
> <span data-ttu-id="2053e-374">Nachdem Sie auf **Download veröffentlichte Website**, überspringen Sie `silverstripe-cache/manifest_main` in **Vorschau veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="2053e-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="2053e-375">Diese Datei wird zum Zwischenspeichern verwendet und bezieht sich auf jedem Computer.</span><span class="sxs-lookup"><span data-stu-id="2053e-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="2053e-376">Problem: Subtext wird "Serverfehler in Anwendung '/'" angezeigt. Wenn Sie eine veröffentlichte Site herunterladen</span><span class="sxs-lookup"><span data-stu-id="2053e-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="2053e-377">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-377">**Workaround**</span></span>  
> <span data-ttu-id="2053e-378">Öffnen Sie die Website des *"Web.config"* -Datei und Ersetzen Sie den Benutzer-ID und das Kennwort in der Datenbank-Verbindungszeichenfolge mit den SQL Server-Administrator-Anmeldeinformationen (die Anmeldeinformationen von "sa").</span><span class="sxs-lookup"><span data-stu-id="2053e-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="2053e-379">Sie können auch wie folgt vorgehen, um das Benutzerkonto zu bieten, mit dem Sie angemeldet sind `db_owner` Berechtigungen:</span><span class="sxs-lookup"><span data-stu-id="2053e-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="2053e-380">Installieren Sie SQL Server Management Studio mit dem Webplattform-Installer.</span><span class="sxs-lookup"><span data-stu-id="2053e-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="2053e-381">Verbinden mit der lokalen SQL Server Express-Instanz (standardmäßig `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="2053e-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="2053e-382">Klicken Sie auf **Datenbanken** &gt; *[LocalSubtextDatabase]* &gt; **Sicherheit** &gt; **Benutzer** &gt; *[LocalSubtextUser*] (der Standardwert ist `subtextuser`], mit der rechten Maustaste, und klicken Sie auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="2053e-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="2053e-383">Wählen Sie **Db\_Besitzer** im Abschnitt Mitgliedschaft Rolle.</span><span class="sxs-lookup"><span data-stu-id="2053e-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="2053e-384">Problem: Website funktioniert möglicherweise nicht nach dem Veröffentlichen, wenn das Feld "Ziel-URL" nicht mit http:// oder https:// vorangestellt ist</span><span class="sxs-lookup"><span data-stu-id="2053e-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="2053e-385">In der **Veröffentlichungseinstellungen** Dialogfeld, wenn die Ziel-URL nicht mit beginnt `http://` oder `https://`, die Website funktioniert möglicherweise nicht nach der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="2053e-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="2053e-386">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-386">**Workaround**</span></span>  
> <span data-ttu-id="2053e-387">Stellen Sie sicher, dass vor dem Veröffentlichen einer Website, die Ziel-URL in die **Veröffentlichungseinstellungen** Dialogfeld beginnt mit `http://` oder `https://`.</span><span class="sxs-lookup"><span data-stu-id="2053e-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="2053e-388">Problem: Veröffentlichen einer MySQL-Datenbank tritt der Fehler "Fehler beim Veröffentlichen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2053e-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="2053e-389">Dies kann passieren, wenn die Remotedatenbank das Skript ausgeführt werden kann."</span><span class="sxs-lookup"><span data-stu-id="2053e-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="2053e-390">Der Fehler kann aus verschiedenen Gründen auftreten.</span><span class="sxs-lookup"><span data-stu-id="2053e-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="2053e-391">Ein möglicher Grund kann dieser Fehler angezeigt wird, wenn das Datenbankskript enthält eine einfache Anführungszeichen (') und der Ziel-MySQL-Datenbank der Standard-Zeichensatz nicht in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="2053e-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="2053e-392">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-392">**Workaround**</span></span>  
> <span data-ttu-id="2053e-393">Legen Sie den Standardzeichensatz für die MySQL-Remotedatenbank in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="2053e-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="2053e-394">Problem: Einige Links sind nicht sichtbar in DotNetNuke nach dem Veröffentlichen oder die Website herunterladen</span><span class="sxs-lookup"><span data-stu-id="2053e-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="2053e-395">Wenn Sie veröffentlichen oder eine DotNetNuke-Website herunterladen, müssen Sie zum Löschen des Caches, rufen Sie die neuen Links auf der Website angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2053e-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="2053e-396">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="2053e-397">Melden Sie sich als "Host".</span><span class="sxs-lookup"><span data-stu-id="2053e-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="2053e-398">Wechseln Sie zum Hostmenü, und wählen Sie **Hosteinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="2053e-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="2053e-399">Führen Sie einen Bildlauf nach unten, und klicken Sie unter **Erweiterte Einstellungen**, erweitern Sie **Leistungseinstellungen**.</span><span class="sxs-lookup"><span data-stu-id="2053e-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="2053e-400">Klicken Sie auf die **Cache löschen** Link, um Seiten.</span><span class="sxs-lookup"><span data-stu-id="2053e-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="2053e-401">Wechseln Sie zum unteren Rand der Seite, und starten Sie die Anwendung neu.</span><span class="sxs-lookup"><span data-stu-id="2053e-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="2053e-402">Problem: Einige Links in AtomSite werden unterbrochen, nachdem Sie eine veröffentlichte Website heruntergeladen haben.</span><span class="sxs-lookup"><span data-stu-id="2053e-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="2053e-403">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-403">**Workaround**</span></span>  
> <span data-ttu-id="2053e-404">In der *service.config* Datei *users.config* -Datei, und alle *XML* -Dateien, ersetzen Sie die URL-Zeichenfolge (z. B. `http://myhost.com/atomsite`) mit den lokalen Endpunkt (z. B. `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="2053e-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="2053e-405">Problem: MySQL-basierte Anwendungen wie WordPress nicht veröffentlichen und einen Fehler gemeldet</span><span class="sxs-lookup"><span data-stu-id="2053e-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="2053e-406">WebMatrix installiert standardmäßig MySQL mit dem UTF-8-Zeichensatz.</span><span class="sxs-lookup"><span data-stu-id="2053e-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="2053e-407">Wenn Sie MySQL selbst installieren, und der Zeichensatz nicht UTF-8 (z. B. befindet er sich Latin1), der Veröffentlichungsprozess für Datenbanken fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="2053e-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="2053e-408">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="2053e-409">Ändern Sie den Zeichensatz für MySQL in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="2053e-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="2053e-410">(Weitere Informationen finden Sie unter [Server Zeichensatz und Sortierreihenfolge](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) auf der MySQL-Website.)</span><span class="sxs-lookup"><span data-stu-id="2053e-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="2053e-411">Installieren Sie die Anwendung neu.</span><span class="sxs-lookup"><span data-stu-id="2053e-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="2053e-412">Veröffentlichen Sie die Anwendung erneut.</span><span class="sxs-lookup"><span data-stu-id="2053e-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="2053e-413">Problem: "Download Standort veröffentlicht" für Anwendungen, Browser-basiertes Setup ein Fehler auftritt</span><span class="sxs-lookup"><span data-stu-id="2053e-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="2053e-414">Einige Anwendungen (z. B. Kentico CMS), müssen Sie sie im Browser zu starten, um nach der Installation von z. B. das Erstellen einer Datenbank auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2053e-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="2053e-415">Wenn Sie eine Anwendung wie folgt, ohne das Browser-basierte Setup abschließen veröffentlichen, schlägt fehl, es wird versucht, die am selben Standort von einem Remoteserver herunterladen.</span><span class="sxs-lookup"><span data-stu-id="2053e-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="2053e-416">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-416">**Workaround**</span></span>  
> <span data-ttu-id="2053e-417">Schließen Sie vor dem Veröffentlichen der Website-Browser-basiertes Setup.</span><span class="sxs-lookup"><span data-stu-id="2053e-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="2053e-418">Problem: "Download Standort veröffentlicht" schlägt fehl mit eines Datenbankfehlers DotNetNuke und Kooboo-CMS</span><span class="sxs-lookup"><span data-stu-id="2053e-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="2053e-419">Wenn Sie versuchen, eine Anwendung von einem Server herunterzuladen, und Sie über Administratoranmeldeinformationen, in der Datenbank-Verbindungszeichenfolge in verfügen der **Veröffentlichungseinstellungen** Dialogfeld möglicherweise den folgenden Fehler im Protokoll veröffentlichen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2053e-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="2053e-420">**Dieses Problem zu umgehen**</span><span class="sxs-lookup"><span data-stu-id="2053e-420">**Workaround**</span></span>  
> <span data-ttu-id="2053e-421">Sollte möglichst die Website erneut zu veröffentlichen (oder dessen Veröffentlichung) mit nicht-Administrator-Anmeldeinformationen für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2053e-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="2053e-422">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="2053e-422">For More Information</span></span>

<span data-ttu-id="2053e-423">Weitere Informationen zu WebMatrix 1.0 finden Sie unter den folgenden Websites:</span><span class="sxs-lookup"><span data-stu-id="2053e-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="2053e-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="2053e-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="2053e-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2053e-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="2053e-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="2053e-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="2053e-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="2053e-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="2053e-428">Alle Rechte vorbehalten.</span><span class="sxs-lookup"><span data-stu-id="2053e-428">All Rights Reserved.</span></span> <span data-ttu-id="2053e-429">[Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="2053e-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
