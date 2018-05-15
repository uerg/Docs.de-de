---
title: Problembehandlung für ASP.NET Core
author: Rick-Anderson
description: Grundlagen und Problembehandlung für Warnungen und Fehlern mit Assistenten für ASP.NET Core-Projekte.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: f2c785bfe27ddd67db0313b8ee1c077a8cc06e05
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="eab1e-103">Problembehandlung bei ASP.NET Core-Projekten</span><span class="sxs-lookup"><span data-stu-id="eab1e-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="eab1e-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eab1e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eab1e-105">Die folgenden Links erhalten Sie Anleitungen zur Fehlerbehebung:</span><span class="sxs-lookup"><span data-stu-id="eab1e-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="eab1e-106">Problembehandlung bei ASP.NET Core in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eab1e-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="eab1e-107">Problembehandlung bei ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="eab1e-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="eab1e-108">Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eab1e-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="eab1e-109">YouTube: Diagnostizieren von Problemen in ASP.NET Core-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="eab1e-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="eab1e-110">.NET Core SDK-Warnungen</span><span class="sxs-lookup"><span data-stu-id="eab1e-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="eab1e-111">Sowohl die 32-Bit und 64-Bit-Versionen von .NET Core SDK installiert sind</span><span class="sxs-lookup"><span data-stu-id="eab1e-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="eab1e-112">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="eab1e-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="eab1e-114">Diese Warnung wird angezeigt, wenn (x86) 32-Bit und 64-Bit (x 64)-Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind.</span><span class="sxs-lookup"><span data-stu-id="eab1e-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="eab1e-115">Häufige Gründe, die beide Versionen installiert werden können umfassen:</span><span class="sxs-lookup"><span data-stu-id="eab1e-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="eab1e-116">Sie ursprünglich ein 32-Bit-Computer mit .NET Core SDK-Installationsprogramm heruntergeladen aber anschließend kopiert er über und ihn auf einem 64-Bit-Computer installiert.</span><span class="sxs-lookup"><span data-stu-id="eab1e-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="eab1e-117">Die 32-Bit-.NET Core SDK wurde durch eine andere Anwendung installiert.</span><span class="sxs-lookup"><span data-stu-id="eab1e-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="eab1e-118">Die falsche Version wurde heruntergeladen und installiert.</span><span class="sxs-lookup"><span data-stu-id="eab1e-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="eab1e-119">Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="eab1e-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="eab1e-120">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="eab1e-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="eab1e-121">Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="eab1e-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="eab1e-122">.NET Core SDK ist in mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="eab1e-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="eab1e-123">In der **neues Projekt** Dialogfeld für ASP.NET Core möglicherweise die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="eab1e-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="eab1e-124">.NET Core SDK ist in mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="eab1e-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="eab1e-125">Nur Vorlagen für die SDKs an installierten "c:\Programme\Microsoft Files\dotnet\sdk\' wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="eab1e-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="eab1e-127">Diese Meldung wird angezeigt, wenn Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb von haben * c:\Programme\Microsoft Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="eab1e-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="eab1e-128">Dies geschieht in der Regel, die bei .NET Core SDK auf einem Computer mit Kopieren/Einfügen anstelle der MSI-Installationsprogramm bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="eab1e-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="eab1e-129">Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="eab1e-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="eab1e-130">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="eab1e-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="eab1e-131">Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="eab1e-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="eab1e-132">Es wurden keine .NET Core-SDKs erkannt.</span><span class="sxs-lookup"><span data-stu-id="eab1e-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="eab1e-133">In der **neues Projekt** Dialogfeld für ASP.NET Core möglicherweise die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="eab1e-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="eab1e-134">**Keine .NET Core-SDKs erkannt wurden, stellen Sie sicher, dass sie in der Umgebungsvariablen "PATH" enthalten sind**</span><span class="sxs-lookup"><span data-stu-id="eab1e-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="eab1e-136">Diese Warnung wird angezeigt, wenn die Umgebungsvariable `PATH` zeigt nicht auf eine beliebige .NET Core-SDKs auf dem Computer.</span><span class="sxs-lookup"><span data-stu-id="eab1e-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="eab1e-137">Um dieses Problem zu lösen:</span><span class="sxs-lookup"><span data-stu-id="eab1e-137">To resolve this problem:</span></span>

* <span data-ttu-id="eab1e-138">Installieren, oder stellen Sie sicher, dass die .NET Core SDK installiert ist.</span><span class="sxs-lookup"><span data-stu-id="eab1e-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="eab1e-139">Überprüfen Sie die `PATH` -Umgebungsvariable zeigt auf den Speicherort, die das SDK installiert ist.</span><span class="sxs-lookup"><span data-stu-id="eab1e-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="eab1e-140">Normalerweise legt der Installer die `PATH`.</span><span class="sxs-lookup"><span data-stu-id="eab1e-140">The installer normally sets the `PATH`.</span></span>