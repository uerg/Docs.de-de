---
uid: whitepapers/side-by-side-with-10
title: ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, wie sowohl .NET 1.0 und 1.1 von .NET auf dem Computer, können eine ASP.NET-Webanwendung zum Ausführen auf einer Version des der zeitsteuerung der installieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1b8bcebd59a900e54c759509fd4fc5ad1f4f8576
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391159"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="8ff0f-103">ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1</span><span class="sxs-lookup"><span data-stu-id="8ff0f-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="8ff0f-104">In diesem Whitepaper wird beschrieben, wie zum Installieren von sowohl .NET 1.0 und 1.1 von .NET auf dem Computer, sodass einer ASP.NET-Webanwendung auf beiden Versionen des Frameworks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="8ff0f-105">ASP.NET 1.1 zu ASP.NET 1.0 angewendet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="8ff0f-106">In ASP.NET Datenändernde Anwendungen nebeneinander ausgeführt werden, wenn sie auf dem gleichen Computer installiert sind, jedoch verschiedene Versionen von .NET Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="8ff0f-107">Das folgende Thema enthält Informationen zum Konfigurieren von ASP.NET-Anwendungen für Seite-an-Seite-Ausführung und bietet eine schrittweise Anleitung:</span><span class="sxs-lookup"><span data-stu-id="8ff0f-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="8ff0f-108">Verwalten Sie Ihrer Webanwendung-Zuordnung zu .NET Framework, Version 1.0, während der installation</span><span class="sxs-lookup"><span data-stu-id="8ff0f-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="8ff0f-109">Ordnen Sie einer Webanwendung in einer bestimmten Version von .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8ff0f-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="8ff0f-110">Suchen Sie die Version von .NET Framework, die eine Website verwendet wird</span><span class="sxs-lookup"><span data-stu-id="8ff0f-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="8ff0f-111">Wenn eine Komponente oder Anwendung auf einem Computer aktualisiert wird, wird die ältere Version in der Vergangenheit entfernt und durch die neuere Version ersetzt.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="8ff0f-112">Wenn die neue Version nicht mit der vorherigen Version kompatibel ist, unterbricht dies in der Regel andere Anwendungen, die der Komponente oder Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="8ff0f-113">.NET Framework bietet Unterstützung für Seite-an-Seite-Ausführung, bei dem mehrere Versionen einer Assembly oder Anwendung gleichzeitig auf demselben Computer installiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="8ff0f-114">Da mehrere Versionen gleichzeitig installiert werden können, können verwaltete Anwendungen auswählen, welche Version Sie verwenden, ohne Auswirkungen auf Anwendungen, die eine andere Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="8ff0f-115">Standardmäßig werden während der Installation von .NET Framework, Version 1.1, alle vorhandene ASP.NET-Anwendungen automatisch neu konfiguriert um die neueste Version von .NET Framework zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="8ff0f-116">Wenn Sie Ihre ASP.NET-Anwendungen .NET Framework 1.1 standardmäßig nicht möchten, klicken Sie auf [hier](#1) zu erfahren, wie Sie dies während der Installation zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="8ff0f-117">Wenn Sie Ihrem Webserver auf .NET Framework 1.1 aktualisieren und einen oder mehrere Webanwendungen auf .NET Framework 1.0 ausführen möchten, müssen Sie die Zuordnung der Internetinformationsdienste (Internet Information Services, IIS)-Skript aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="8ff0f-118">Der Skriptzuordnung ist der Mechanismus zum Zuordnen der Erweiterungs für eine bestimmte Webanwendung auf eine Version von .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="8ff0f-119">Klicken Sie auf [hier](#2) erfahren, wie eine Webanwendung auf eine bestimmte Version von .NET Framework zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="8ff0f-120">Sie können den Internetinformationsdienste-Manager Informationen oder das ASP.NET IIS Registration-Tool verwenden (Aspnet\_regiis.exe) zu ermitteln, welche .NET Framework-Version eine bestimmte Webanwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="8ff0f-121">Klicken Sie auf [hier](#3) zu erfahren, wie Sie die Version von .NET Framework zu ermitteln, die eine Website verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="8ff0f-122">Ein Import Aspekt bei der Migration zu .NET Framework 1.1 ist, dass jede Version von .NET Framework eine eigene Datei "Machine.config" verwendet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="8ff0f-123">Daher müssen ein Webadministrator die Datei "Machine.config" geändert hat, diese Änderungen in der Datei Machine.config-Datei von .NET Framework 1.1 migriert werden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="8ff0f-124">Verwalten Ihre Webanwendung-Zuordnung zu .NET Framework 1.0, während der installation</span><span class="sxs-lookup"><span data-stu-id="8ff0f-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="8ff0f-125">Standardmäßig werden alle vorhandene ASP.NET-Anwendungen automatisch während der Installation auf die neuere Version von .NET Framework neu konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="8ff0f-126">Verwenden die neuere Version von .NET Framework, können Anwendungen vollständige Verbesserungen und neuen Features in der neuen Version nutzen.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="8ff0f-127">Zur gleichen Zeit wird der Administrator Web präzise steuern, welche Anwendungen sollten aktualisiert werden, kann verhindern, dass die automatische neuzuordnung der alle vorhandenen ASP.NET-Anwendungen während der Installation von .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="8ff0f-128">Um zu verhindern, die Automatisches Neuzuordnen von der gesamten ASP.NET-Anwendung auf die neuere Version von .NET Framework, kann der Web-Administrator die Befehlszeilenoption "/noaspupgrade" in das Setupprogramm Dotnetfx.exe verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="8ff0f-129">**Um zu verhindern, insgesamt neuzuordnung der ASP.NET-Anwendung auf neuere version**</span><span class="sxs-lookup"><span data-stu-id="8ff0f-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="8ff0f-130">Wechseln Sie zu **starten**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-130">Go to **Start**.</span></span>
2. <span data-ttu-id="8ff0f-131">Klicken Sie auf **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-131">Click on **run**.</span></span>
3. <span data-ttu-id="8ff0f-132">Geben Sie **cmd** ein.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-132">Type **cmd**.</span></span>
4. <span data-ttu-id="8ff0f-133">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="8ff0f-134">Geben Sie an der Eingabeaufforderung die folgende Zeile zum Starten der Installation von .NET Framework: **Dotnetfx.exe/c: "Installieren von /noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="8ff0f-135">Klicken Sie auf **Ja** im Setup für Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="8ff0f-136">Dadurch wird der Einrichtung von .NET Framework 1.1 gestartet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="8ff0f-137">Ordnen Sie einer Webanwendung in einer bestimmten Version von .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8ff0f-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="8ff0f-138">Jede Version von .NET Framework umfasst eine Version des ASP.NET IIS Registration-Tool (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="8ff0f-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="8ff0f-139">Dieses Tool ermöglicht Administratoren, um anzugeben, dass eine Webanwendung unter einer bestimmten Version von .NET Framework ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="8ff0f-140">Dies wird wie die Zuordnung einer Webanwendung in eine Version von .NET Framework bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="8ff0f-141">Administratoren müssen das Aspnet auswählen\_regiis.exe, der die Version von .NET Framework entspricht, die mit der Web-Anwendung verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="8ff0f-142">Beispielsweise muss ein Administrator, um anzugeben, dass eine Website mit .NET Framework 1.1 verwenden möchte, das Aspnet verwenden\_regiis.exe, die in .NET Framework 1.1 enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="8ff0f-143">Das Aspnet\_regiis.exe für Version 1.0 befindet sich unter:</span><span class="sxs-lookup"><span data-stu-id="8ff0f-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="8ff0f-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_Regiis</span><span class="sxs-lookup"><span data-stu-id="8ff0f-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="8ff0f-145">Das Aspnet\_regiis.exe für Version 1,1 befindet sich unter:</span><span class="sxs-lookup"><span data-stu-id="8ff0f-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="8ff0f-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_Regiis</span><span class="sxs-lookup"><span data-stu-id="8ff0f-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="8ff0f-147">Das Aspnet\_regiis.exe bietet zwei Optionen für Skripts, die eine Webanwendung zuordnen:</span><span class="sxs-lookup"><span data-stu-id="8ff0f-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="8ff0f-148">**-s** legt die Skriptzuordnung in den Pfad und in untergeordneten Verzeichnissen.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="8ff0f-149">**-sn** legt die Skriptzuordnung im Pfad nur.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="8ff0f-150">Der Pfad definiert, die IIS-Metadaten Webanwendungspfad, die in Form von W3SVC/ROOT definiert ist / {WebSiteNumber} / {Anwendung\_Name}.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="8ff0f-151">Für eine Webanwendung namens Portal befindet sich unter der Standardwebsite ist beispielsweise der Metabasepfad des W3SVC/1/ROOT/Portal an.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="8ff0f-152">Beachten Sie, dass Sie auch ein Tool Namens der Metabase-Editor verwenden können, um den Metabase-Pfad abzurufen.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="8ff0f-153">Sie können dieses Tool von der Microsoft-Support-Website unter [ https://support.microsoft.com/default.aspx?scid=kb; En-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="8ff0f-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="8ff0f-154">Führen Sie Aspnet\_Zuordnung und die Subapplication regiis.exe -s W3SVC/1/ROOT /-Portal, um das IIS-Portal aktualisieren ein Skript.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="8ff0f-155">Führen Sie Aspnet\_regiis.exe -sn W3SVC/1/ROOT /-Portal, um das Skript für Portals IIS aktualisieren zuzuordnen, ohne Auswirkungen auf Anwendungen im Portal? s Unterverzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="8ff0f-156">Suchen Sie die .NET Framework-Version, die eine Webanwendung verwendet wird</span><span class="sxs-lookup"><span data-stu-id="8ff0f-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="8ff0f-157">Administratoren können den Internetdienste-Manager ermitteln, welche Version von .NET Framework eine Website ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="8ff0f-158">Starten Sie verschiedenen Betriebssystemversionen Internetdienste-Manager unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="8ff0f-159">Um den Dienst-Manager zu starten, führen Sie die nachstehenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="8ff0f-160">**Um Internetdienste-Manager zu starten.**</span><span class="sxs-lookup"><span data-stu-id="8ff0f-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="8ff0f-161">Wechseln Sie zu **starten**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-161">Go to **Start**.</span></span>
2. <span data-ttu-id="8ff0f-162">Klicken Sie auf **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-162">Click on **run**.</span></span>
3. <span data-ttu-id="8ff0f-163">Typ **Inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="8ff0f-164">Wählen Sie vom Internet-Manager, die Web-Anwendung, deren Version von .NET Framework, die Sie wissen möchten.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="8ff0f-165">Mit der rechten Maustaste auf die Webanwendung, und klicken Sie auf **Eigenschaften.**</span><span class="sxs-lookup"><span data-stu-id="8ff0f-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="8ff0f-166">Wählen Sie das Eigenschaftenfenster **Konfiguration.**</span><span class="sxs-lookup"><span data-stu-id="8ff0f-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="8ff0f-167">Wählen Sie in der Zuordnungstabelle für die Anwendung werden **aspx**, und klicken Sie auf **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="8ff0f-168">Von der **ausführbare Datei** Textfeld Blick auf das Versionsverzeichnis, indem Sie einen Bildlauf.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="8ff0f-169">Wenn das Versionsverzeichnis v.1.1.4322 ist, wird die Anwendung .NET Framework 1.1 zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="8ff0f-170">Wenn das Versionsverzeichnis Version 1.0.3705 ist, wird im Gegensatz dazu die Anwendung .NET Framework 1.0 zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="8ff0f-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
