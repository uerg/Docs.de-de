---
uid: whitepapers/side-by-side-with-10
title: ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1 | Microsoft Docs
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, wie .NET 1.0 und 1.1 von .NET installieren, auf dem Computer, ermöglicht eine ASP.NET-Webanwendung auf eine Version von den Fram ausgeführt wird...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530179"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="7b488-103">ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1</span><span class="sxs-lookup"><span data-stu-id="7b488-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="7b488-104">Dieses Whitepaper beschreibt, wie auf dem Computer, ermöglicht eine ASP.NET-Webanwendung zum Ausführen auf beide Versionen des Frameworks .NET 1.0 und 1.1 von .NET zu installieren.</span><span class="sxs-lookup"><span data-stu-id="7b488-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="7b488-105">Gilt für ASP.NET 1.0 und ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="7b488-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="7b488-106">In ASP.NET Datenändernde Anwendungen parallel ausgeführt werden, wenn sie auf dem gleichen Computer installiert sind, jedoch verschiedene Versionen von .NET Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="7b488-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="7b488-107">Das folgende Thema enthält Informationen zum Konfigurieren von ASP.NET-Anwendungen für Seite-an-Seite-Ausführung und enthält detaillierte Schritte für:</span><span class="sxs-lookup"><span data-stu-id="7b488-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="7b488-108">Verwalten Sie Ihre Webanwendung-Zuordnung für .NET Framework, Version 1.0, während der installation</span><span class="sxs-lookup"><span data-stu-id="7b488-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="7b488-109">Ordnen Sie eine Webanwendung auf eine bestimmte Version von .NET Framework</span><span class="sxs-lookup"><span data-stu-id="7b488-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="7b488-110">Suchen Sie die Version von .NET Framework, die eine Website verwendet wird</span><span class="sxs-lookup"><span data-stu-id="7b488-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="7b488-111">Wenn eine Komponente oder Anwendung auf einem Computer aktualisiert wird, wird die ältere Version bisher entfernt und durch die neuere Version ersetzt.</span><span class="sxs-lookup"><span data-stu-id="7b488-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="7b488-112">Wenn die neue Version nicht mit der vorherigen Version kompatibel ist, wird dies in der Regel andere Anwendungen, die der Komponente oder Anwendung verwenden unterbrochen.</span><span class="sxs-lookup"><span data-stu-id="7b488-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="7b488-113">.NET Framework bietet Unterstützung für Seite-an-Seite-Ausführung, bei dem mehrere Versionen einer Assembly oder Anwendung gleichzeitig auf demselben Computer installiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="7b488-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="7b488-114">Da mehrere Versionen gleichzeitig installiert werden können, können verwaltete Anwendungen auswählen, welche Version Sie verwenden, ohne Auswirkungen auf Anwendungen, die eine andere Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="7b488-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="7b488-115">Standardmäßig werden während der Installation von .NET Framework, Version 1.1, alle vorhandene ASP.NET-Anwendungen automatisch konfiguriert die neueste Version von .NET Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="7b488-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="7b488-116">Wenn Sie Ihre ASP.NET-Anwendungen .NET Framework 1.1 standardmäßig nicht wünschen, klicken Sie auf [hier](#1) zu erfahren, wie Sie dies während der Installation zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="7b488-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="7b488-117">Wenn Sie Ihre Web-Server, auf .NET Framework 1.1 aktualisieren und eine oder mehrere Webanwendungen zu .NET Framework 1.0 ausführen möchten, müssen Sie die Zuordnung der Internetinformationsdienste (Internet Information Services, IIS)-Skript zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="7b488-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="7b488-118">Die Skriptzuordnung ist der Mechanismus zum Zuordnen der Erweiterungs für eine bestimmte Webanwendung auf eine Version von .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7b488-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="7b488-119">Klicken Sie auf [hier](#2) Informationen zum Zuordnen einer Webanwendung auf eine bestimmte Version von .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7b488-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="7b488-120">Können Sie den Internetinformationsdienste-Manager Informationen oder die IIS-Registrierungstool (Aspnet\_regiis.exe) um herauszufinden, welche .NET Framework-Version eine bestimmte Webanwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7b488-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="7b488-121">Klicken Sie auf [hier](#3) um zu erfahren, wie Sie die Version von .NET Framework zu ermitteln, die eine Website verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7b488-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="7b488-122">Import berücksichtigen beim Migrieren zu .NET Framework 1.1 ist, dass jede Version von .NET Framework eine eigene Datei "Machine.config" verwendet.</span><span class="sxs-lookup"><span data-stu-id="7b488-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="7b488-123">Folglich müssen ein Webadministrator Änderungen an der Datei "Machine.config" vorgenommen wurde, diese Änderungen in die Datei Machine.config von .NET Framework 1.1 migriert werden.</span><span class="sxs-lookup"><span data-stu-id="7b488-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="7b488-124">Verwalten Ihre Webanwendung Zuordnung zu .NET Framework 1.0 während der installation</span><span class="sxs-lookup"><span data-stu-id="7b488-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="7b488-125">Standardmäßig werden alle vorhandene ASP.NET-Anwendungen automatisch während der Installation auf die neuere Version von .NET Framework neu konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="7b488-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="7b488-126">Verwenden die neuere Version von .NET Framework, können Anwendungen vollständige Verbesserungen und neuen Features in der neuen Version nutzen.</span><span class="sxs-lookup"><span data-stu-id="7b488-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="7b488-127">Zur gleichen Zeit Administrator Web, präzise steuern, welche Anwendungen sollten aktualisiert werden, können verhindern, dass die automatische neuzuordnung der alle vorhandenen ASP.NET-Anwendungen während der Installation von .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7b488-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="7b488-128">Um zu verhindern, dass die automatische neuzuordnung der gesamte ASP.NET-Anwendung auf die neuere Version von .NET Framework kann die Web-Administrator die Befehlszeilenoption /noaspupgrade in das Setupprogramm Dotnetfx.exe verwenden.</span><span class="sxs-lookup"><span data-stu-id="7b488-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="7b488-129">**Um zu verhindern, insgesamt neuzuordnung der ASP.NET-Anwendung auf neuere version**</span><span class="sxs-lookup"><span data-stu-id="7b488-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="7b488-130">Wechseln Sie zu **starten**.</span><span class="sxs-lookup"><span data-stu-id="7b488-130">Go to **Start**.</span></span>
2. <span data-ttu-id="7b488-131">Klicken Sie auf **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="7b488-131">Click on **run**.</span></span>
3. <span data-ttu-id="7b488-132">Geben Sie **cmd** ein.</span><span class="sxs-lookup"><span data-stu-id="7b488-132">Type **cmd**.</span></span>
4. <span data-ttu-id="7b488-133">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b488-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="7b488-134">Geben Sie an der Eingabeaufforderung die folgende Zeile zum Starten der Installation von .NET Framework: **Dotnetfx.exe c: "/noaspupgrade installieren?**.</span><span class="sxs-lookup"><span data-stu-id="7b488-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="7b488-135">Klicken Sie auf **Ja** in der Microsoft .NET Framework 1.1-Setuphilfe.</span><span class="sxs-lookup"><span data-stu-id="7b488-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="7b488-136">Dadurch wird den Setupvorgang von .NET Framework 1.1 gestartet.</span><span class="sxs-lookup"><span data-stu-id="7b488-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="7b488-137">Ordnen Sie eine Webanwendung auf eine bestimmte Version von .NET Framework</span><span class="sxs-lookup"><span data-stu-id="7b488-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="7b488-138">Jede Version von .NET Framework enthält eine Version des ASP.NET IIS-Registrierungstool (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="7b488-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="7b488-139">Mit diesem Tool können Administratoren, um anzugeben, dass eine Webanwendung unter einer bestimmten Version von .NET Framework ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="7b488-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="7b488-140">Dies wird als Zuordnung von einer Webanwendung auf eine Version von .NET Framework bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="7b488-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="7b488-141">Administratoren müssen auswählen, dass das Aspnet\_regiis.exe, der die Version von .NET Framework entspricht, die die Web-Anwendung zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="7b488-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="7b488-142">Beispielsweise muss ein Administrator, um anzugeben, dass eine Website mit .NET Framework 1.1 verwenden möchte, das Aspnet verwenden\_regiis.exe, der mit .NET Framework 1.1 enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="7b488-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="7b488-143">Das Aspnet\_regiis.exe für Version 1.0 befindet sich unter:</span><span class="sxs-lookup"><span data-stu-id="7b488-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="7b488-144">C:\WINDOWS\Microsoft.NET\Framework\**Version 1.0.3705** \aspnet\_Regiis</span><span class="sxs-lookup"><span data-stu-id="7b488-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="7b488-145">Das Aspnet\_regiis.exe für Version 1,1 befindet sich unter:</span><span class="sxs-lookup"><span data-stu-id="7b488-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="7b488-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_Regiis</span><span class="sxs-lookup"><span data-stu-id="7b488-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="7b488-147">Das Aspnet\_regiis.exe bietet zwei Optionen für Skripts, die eine Webanwendung zuordnen:</span><span class="sxs-lookup"><span data-stu-id="7b488-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="7b488-148">**-s** legt die Skriptzuordnung in den Pfad und den untergeordneten Verzeichnissen.</span><span class="sxs-lookup"><span data-stu-id="7b488-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="7b488-149">**-"sn"** legt die Skriptzuordnung in nur den Pfad fest.</span><span class="sxs-lookup"><span data-stu-id="7b488-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="7b488-150">Den Pfad definierten der IIS-Metadaten Webanwendungspfad, definiert in Form von W3SVC/ROOT / {WebSiteNumber} / {Anwendung\_Name}.</span><span class="sxs-lookup"><span data-stu-id="7b488-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="7b488-151">Für eine Webanwendung namens Portal befindet sich unter der Standardwebsite ist z. B. der Metabasispfad W3SVC/1/ROOT/Portal an.</span><span class="sxs-lookup"><span data-stu-id="7b488-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="7b488-152">Beachten Sie, dass Sie ein Tool Namens der Metabase-Editor auch zum Abrufen der Metabasispfad verwenden können.</span><span class="sxs-lookup"><span data-stu-id="7b488-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="7b488-153">Sie können dieses Tool von der Microsoft-Support-Website unter herunterladen [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="7b488-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="7b488-154">Führen Sie Aspnet\_Zuordnung und seine Subapplication regiis.exe -s W3SVC/1/ROOT /-Portal, um das IIS-Portal Aktualisieren von Skripts.</span><span class="sxs-lookup"><span data-stu-id="7b488-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="7b488-155">Führen Sie Aspnet\_regiis.exe-"sn" W3SVC/1/ROOT /-Portal, um das Portal IIS-Skript aktualisieren zuordnen, ohne Einfluss auf Anwendungen in das Portal? s Unterverzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="7b488-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="7b488-156">Suchen Sie die .NET Framework-Version, die eine Webanwendung verwendet wird</span><span class="sxs-lookup"><span data-stu-id="7b488-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="7b488-157">Ein Administrator kann Internetdienste-Manager verwenden, um zu ermitteln, welche Version von .NET Framework eine Website ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7b488-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="7b488-158">Starten Sie verschiedenen Betriebssystemversionen Internetdienste-Manager unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="7b488-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="7b488-159">Um den Dienst-Manager zu starten, führen Sie die unten aufgeführten Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="7b488-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="7b488-160">**Um Internetdienst-Manager zu starten.**</span><span class="sxs-lookup"><span data-stu-id="7b488-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="7b488-161">Wechseln Sie zu **starten**.</span><span class="sxs-lookup"><span data-stu-id="7b488-161">Go to **Start**.</span></span>
2. <span data-ttu-id="7b488-162">Klicken Sie auf **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="7b488-162">Click on **run**.</span></span>
3. <span data-ttu-id="7b488-163">Typ **Inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="7b488-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="7b488-164">Wählen Sie aus dem Internetdienst-Manager, die Web-Anwendung, deren Version von .NET Framework, die Sie wissen möchten.</span><span class="sxs-lookup"><span data-stu-id="7b488-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="7b488-165">Mit der rechten Maustaste auf die Webanwendung, und klicken Sie auf **Eigenschaften.**</span><span class="sxs-lookup"><span data-stu-id="7b488-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="7b488-166">Wählen Sie das Eigenschaftenfenster **Konfiguration.**</span><span class="sxs-lookup"><span data-stu-id="7b488-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="7b488-167">Wählen Sie in der Zuordnungstabelle Anwendung **aspx**, und klicken Sie auf **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="7b488-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="7b488-168">Aus der **ausführbare Datei** Textfeld Blick auf das Versionsverzeichnis durch Ausführen eines Bildlaufs.</span><span class="sxs-lookup"><span data-stu-id="7b488-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="7b488-169">Wenn das Versionsverzeichnis v.1.1.4322 ist, wird die Anwendung auf .NET Framework 1.1 zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7b488-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="7b488-170">Wenn das Versionsverzeichnis Version 1.0.3705 ist, wird hingegen die Anwendung .NET Framework 1.0 zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7b488-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
