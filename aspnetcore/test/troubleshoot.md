---
title: Problembehandlung bei ASP.NET Core-Projekten
author: Rick-Anderson
description: In diesem Artikel werden Warnungen und Fehler erläutert. Außerdem erfahren Sie, wie die Problembehandlung in ASP.NET Core-Projekten funktioniert.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889011"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="e0e74-103">Problembehandlung bei ASP.NET Core-Projekten</span><span class="sxs-lookup"><span data-stu-id="e0e74-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="e0e74-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0e74-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e0e74-105">Die folgenden Links erhalten Anleitungen zur Fehlerbehebung:</span><span class="sxs-lookup"><span data-stu-id="e0e74-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="e0e74-106">Problembehandlung bei ASP.NET Core in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e0e74-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="e0e74-107">Problembehandlung bei ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="e0e74-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="e0e74-108">Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0e74-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="e0e74-109">NDC Conference (London, 2018): Diagnostizieren von Problemen in ASP.NET Core-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="e0e74-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="e0e74-110">ASP.NET-Blog: Problembehandlung bei Leistungsproblemen von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0e74-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="e0e74-111">.NET Core SDK-Warnungen</span><span class="sxs-lookup"><span data-stu-id="e0e74-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="e0e74-112">Die 32-Bit und 64-Bit-Versionen von .NET Core SDK sind installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="e0e74-113">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e0e74-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="e0e74-114">Es werden sowohl 32- und 64-Bit-Versionen von .NET Core SDK installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="e0e74-115">Nur Vorlagen aus die 64-Bit-Versionen installiert ' "c:"\\Programmdateien\\Dotnet\\Sdk\\"wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e0e74-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot des Dialogfelds mit die Warnmeldung OneASP.NET](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="e0e74-117">Diese Warnung wird angezeigt, wenn sowohl die 32-Bit-(x86) als auch die 64-Bit (x 64) Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind.</span><span class="sxs-lookup"><span data-stu-id="e0e74-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="e0e74-118">Häufige Gründe, die beide Versionen installiert sind enthalten:</span><span class="sxs-lookup"><span data-stu-id="e0e74-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="e0e74-119">Sie ursprünglich heruntergeladen das .NET Core SDK-Installationsprogramm, das mit einem 32-Bit-Computer, aber klicken Sie dann über kopiert und es auf einem 64-Bit-Computer installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="e0e74-120">Das 32-Bit .NET Core SDK wurde durch eine andere Anwendung installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="e0e74-121">Die falsche Version wurde heruntergeladen und installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="e0e74-122">Deinstallieren Sie die 32-Bit .NET Core-SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="e0e74-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="e0e74-123">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="e0e74-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="e0e74-124">Wenn Sie verstehen, warum die Warnung tritt auf, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="e0e74-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="e0e74-125">Das .NET Core SDK ist an mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="e0e74-126">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e0e74-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="e0e74-127">Das .NET Core SDK ist an mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="e0e74-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="e0e74-128">Nur Vorlagen aus dem SDK unter "C:\\Programmdateien\\Dotnet\\Sdk\\" wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e0e74-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot des Dialogfelds mit die Warnmeldung OneASP.NET](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="e0e74-130">Diese Meldung angezeigt, wenn Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb des haben *C:\\Programmdateien\\Dotnet\\Sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="e0e74-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="e0e74-131">Dies geschieht normalerweise, wenn das .NET Core SDK auf einem Computer mit Kopieren/Einfügen, anstatt das MSI-Installationsprogramm bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="e0e74-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="e0e74-132">Deinstallieren Sie die 32-Bit .NET Core-SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="e0e74-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="e0e74-133">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="e0e74-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="e0e74-134">Wenn Sie verstehen, warum die Warnung tritt auf, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="e0e74-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="e0e74-135">Es wurden keine .NET Core SDKs erkannt.</span><span class="sxs-lookup"><span data-stu-id="e0e74-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="e0e74-136">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e0e74-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="e0e74-137">Es wurden keine .NET Core SDKs erkannt, stellen Sie sicher, dass sie in der Umgebungsvariablen "PATH" enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="e0e74-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Screenshot des Dialogfelds mit die Warnmeldung OneASP.NET](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="e0e74-139">Diese Warnung wird angezeigt, wenn die Umgebungsvariable `PATH` verweist nicht auf alle .NET Core-SDKs auf dem Computer.</span><span class="sxs-lookup"><span data-stu-id="e0e74-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="e0e74-140">Um dieses Problem zu beheben:</span><span class="sxs-lookup"><span data-stu-id="e0e74-140">To resolve this problem:</span></span>

* <span data-ttu-id="e0e74-141">Installieren, oder stellen Sie sicher, dass das .NET Core SDK installiert ist.</span><span class="sxs-lookup"><span data-stu-id="e0e74-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="e0e74-142">Überprüfen Sie, ob die `PATH` Umgebungsvariable verweist auf den Speicherort, in dem das SDK installiert ist.</span><span class="sxs-lookup"><span data-stu-id="e0e74-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="e0e74-143">Der Installer normalerweise legt der `PATH`.</span><span class="sxs-lookup"><span data-stu-id="e0e74-143">The installer normally sets the `PATH`.</span></span>