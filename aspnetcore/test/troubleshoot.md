---
title: Problembehandlung bei ASP.NET Core-Projekten
author: Rick-Anderson
description: In diesem Artikel werden Warnungen und Fehler erläutert. Außerdem erfahren Sie, wie die Problembehandlung in ASP.NET Core-Projekten funktioniert.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274592"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="84f14-103">Problembehandlung bei ASP.NET Core-Projekten</span><span class="sxs-lookup"><span data-stu-id="84f14-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="84f14-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84f14-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84f14-105">Die folgenden Links erhalten Sie Anleitungen zur Fehlerbehebung:</span><span class="sxs-lookup"><span data-stu-id="84f14-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="84f14-106">Problembehandlung bei ASP.NET Core in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="84f14-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="84f14-107">Problembehandlung bei ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="84f14-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="84f14-108">Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84f14-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="84f14-109">NDC Konferenz (London 2018): Diagnostizieren von Problemen in ASP.NET-Anwendungen für Core</span><span class="sxs-lookup"><span data-stu-id="84f14-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="84f14-110">ASP.NET-Blog: Beheben von Leistungsproblemen für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84f14-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="84f14-111">.NET Core SDK-Warnungen</span><span class="sxs-lookup"><span data-stu-id="84f14-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="84f14-112">Sowohl die 32-Bit und 64-Bit-Versionen von .NET Core SDK installiert sind</span><span class="sxs-lookup"><span data-stu-id="84f14-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="84f14-113">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="84f14-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="84f14-114">32- und 64-Bit-Versionen von .NET Core SDK installiert sind.</span><span class="sxs-lookup"><span data-stu-id="84f14-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="84f14-115">Nur Vorlagen für die 64-Bit-Versionen installiert "" c: "\\Programmdateien\\Dotnet\\Sdk\\" wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="84f14-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="84f14-117">Diese Warnung wird angezeigt, wenn (x86) 32-Bit und 64-Bit (x 64)-Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind.</span><span class="sxs-lookup"><span data-stu-id="84f14-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="84f14-118">Häufige Ursachen für die beide Versionen installiert werden können umfassen:</span><span class="sxs-lookup"><span data-stu-id="84f14-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="84f14-119">Sie ursprünglich heruntergeladen eine 32-Bit-Computer mit .NET Core SDK-Installationsprogramm aber anschließend kopiert er über und ihn auf einem 64-Bit-Computer installiert.</span><span class="sxs-lookup"><span data-stu-id="84f14-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="84f14-120">Die 32-Bit-.NET Core SDK wurde durch eine andere Anwendung installiert.</span><span class="sxs-lookup"><span data-stu-id="84f14-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="84f14-121">Die falsche Version wurde heruntergeladen und installiert.</span><span class="sxs-lookup"><span data-stu-id="84f14-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="84f14-122">Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="84f14-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="84f14-123">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="84f14-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="84f14-124">Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="84f14-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="84f14-125">.NET Core SDK ist in mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="84f14-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="84f14-126">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="84f14-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="84f14-127">.NET Core SDK ist in mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="84f14-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="84f14-128">Nur Vorlagen für die SDKs an installierten "" c: "\\Programmdateien\\Dotnet\\Sdk\\" wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="84f14-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="84f14-130">Diese Meldung wird angezeigt, wenn Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb von haben *"c:"\\Programmdateien\\Dotnet\\Sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="84f14-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="84f14-131">Dies geschieht normalerweise, wenn der .NET Core SDK auf einem Computer mit Kopieren/Einfügen anstelle der MSI-Installationsprogramm bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="84f14-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="84f14-132">Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="84f14-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="84f14-133">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="84f14-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="84f14-134">Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="84f14-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="84f14-135">Es wurden keine .NET Core-SDKs erkannt.</span><span class="sxs-lookup"><span data-stu-id="84f14-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="84f14-136">In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="84f14-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="84f14-137">Keine .NET Core-SDKs erkannt wurden, stellen Sie sicher, dass sie in der Umgebungsvariablen "PATH" enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="84f14-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="84f14-139">Diese Warnung wird angezeigt, wenn die Umgebungsvariable `PATH` zeigt nicht auf eine beliebige .NET Core-SDKs auf dem Computer.</span><span class="sxs-lookup"><span data-stu-id="84f14-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="84f14-140">Um dieses Problem zu lösen:</span><span class="sxs-lookup"><span data-stu-id="84f14-140">To resolve this problem:</span></span>

* <span data-ttu-id="84f14-141">Installieren, oder stellen Sie sicher, dass die .NET Core SDK installiert ist.</span><span class="sxs-lookup"><span data-stu-id="84f14-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="84f14-142">Überprüfen Sie die `PATH` -Umgebungsvariable zeigt auf den Speicherort, die das SDK installiert ist.</span><span class="sxs-lookup"><span data-stu-id="84f14-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="84f14-143">Normalerweise legt der Installer die `PATH`.</span><span class="sxs-lookup"><span data-stu-id="84f14-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="84f14-144">Verwendung von IHtmlHelper.Partial möglicherweise in app-Deadlocks führen.</span><span class="sxs-lookup"><span data-stu-id="84f14-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="84f14-145">In ASP.NET Core 2.1 und höher Aufrufen `Html.Partial` führt zu einer Warnung Analyzer aufgrund der Möglichkeit von Deadlocks.</span><span class="sxs-lookup"><span data-stu-id="84f14-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="84f14-146">Die Warnung angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="84f14-146">The warning message is:</span></span>

> <span data-ttu-id="84f14-147">Verwendung von IHtmlHelper.Partial kann in der anwendungsdeadlocks führen.</span><span class="sxs-lookup"><span data-stu-id="84f14-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="84f14-148">Erwägen Sie `<partial>` Tag Helper oder `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="84f14-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="84f14-149">Aufrufe von `@Html.Partial` ersetzt werden sollte, indem `@await Html.PartialAsync` oder das Hilfsprogramm writeid `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="84f14-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
