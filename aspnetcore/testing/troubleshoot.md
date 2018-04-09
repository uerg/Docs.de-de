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
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="ba867-103">Problembehandlung bei ASP.NET Core-Projekten</span><span class="sxs-lookup"><span data-stu-id="ba867-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="ba867-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ba867-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ba867-105">Die folgenden Links erhalten Sie Anleitungen zur Fehlerbehebung:</span><span class="sxs-lookup"><span data-stu-id="ba867-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="ba867-106">Problembehandlung bei ASP.NET Core in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ba867-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="ba867-107">Problembehandlung bei ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="ba867-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="ba867-108">Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba867-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="ba867-109">.NET Core SDK-Warnungen</span><span class="sxs-lookup"><span data-stu-id="ba867-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="ba867-110">Sowohl die 32 und 64 Bit-Versionen von .NET Core SDK installiert sind</span><span class="sxs-lookup"><span data-stu-id="ba867-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="ba867-111">In der **neues Projekt** Dialogfeld für ASP.NET Core, möglicherweise die folgende Warnung oben angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="ba867-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="ba867-113">Diese Warnung wird angezeigt, wenn (x86) 32-Bit und 64-Bit (x 64)-Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind.</span><span class="sxs-lookup"><span data-stu-id="ba867-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="ba867-114">Häufige Gründe, die beide Versionen installiert werden können umfassen:</span><span class="sxs-lookup"><span data-stu-id="ba867-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="ba867-115">Sie ursprünglich ein 32-Bit-Computer mit .NET Core SDK-Installationsprogramm heruntergeladen aber anschließend kopiert er über und ihn auf einem 64-Bit-Computer installiert.</span><span class="sxs-lookup"><span data-stu-id="ba867-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="ba867-116">Die 32-Bit-.NET Core SDK wurde durch eine andere Anwendung installiert.</span><span class="sxs-lookup"><span data-stu-id="ba867-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="ba867-117">Die falsche Version wurde heruntergeladen und installiert.</span><span class="sxs-lookup"><span data-stu-id="ba867-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="ba867-118">Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ba867-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="ba867-119">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="ba867-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="ba867-120">Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="ba867-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="ba867-121">.NET Core SDK ist in mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="ba867-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="ba867-122">In der **neues Projekt** Dialogfeld für ASP.NET Core, die Sie möglicherweise die folgende Warnung oben angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="ba867-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="ba867-123">.NET Core SDK ist in mehreren Speicherorten installiert.</span><span class="sxs-lookup"><span data-stu-id="ba867-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="ba867-124">Nur Vorlagen für die SDKs an installierten "c:\Programme\Microsoft Files\dotnet\sdk\' wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ba867-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="ba867-126">Sie sehen diese Nachricht, da Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb von besitzen * c:\Programme\Microsoft Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="ba867-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="ba867-127">Dies geschieht in der Regel, die bei .NET Core SDK auf einem Computer mit Kopieren/Einfügen anstelle der MSI-Installationsprogramm bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ba867-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="ba867-128">Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ba867-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="ba867-129">Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**.</span><span class="sxs-lookup"><span data-stu-id="ba867-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="ba867-130">Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="ba867-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
