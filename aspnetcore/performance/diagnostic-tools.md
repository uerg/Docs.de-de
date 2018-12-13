---
title: Diagnose von Leistungstools
author: mjrousos
description: Hilfreiche Tools zum Diagnostizieren von Leistungsproblemen in ASP.NET Core-apps.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329199"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="dacd9-103">Leistung-Diagnosetools</span><span class="sxs-lookup"><span data-stu-id="dacd9-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="dacd9-104">Durch [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="dacd9-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="dacd9-105">Dieser Artikel beschreibt die Tools zum Diagnostizieren von Leistungsproblemen in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dacd9-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="dacd9-106">Visual Studio-Diagnosetools</span><span class="sxs-lookup"><span data-stu-id="dacd9-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="dacd9-107">Die [profilerstellung und Diagnosetools](/visualstudio/profiling) in Visual Studio integriert sind ein guter Ausgangspunkt für die Untersuchung Leistungsprobleme auftreten.</span><span class="sxs-lookup"><span data-stu-id="dacd9-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="dacd9-108">Diese Tools sind leistungsstark und praktisch, in der Visual Studio-Entwicklungsumgebung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="dacd9-109">Das Tool ermöglicht die Analyse der CPU-Auslastung, speicherauslastung und Leistungsereignisse in ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="dacd9-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="dacd9-110">Weitere Informationen finden Sie in [Visual Studio-Dokumentation](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="dacd9-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="dacd9-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="dacd9-111">Application Insights</span></span>

<span data-ttu-id="dacd9-112">[Application Insights](/azure/application-insights/app-insights-overview) bietet ausführlichere Leistungsdaten für Ihre app.</span><span class="sxs-lookup"><span data-stu-id="dacd9-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="dacd9-113">Application Insights erfasst automatisch Daten, auf die Antwortrate, Fehlerraten, Reaktionszeiten von Abhängigkeiten und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="dacd9-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="dacd9-114">Application Insights unterstützt benutzerdefinierte Ereignisse und Metriken, die bestimmte zu Ihrer app anmelden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="dacd9-115">Application Insights können in einer Vielzahl-Umgebungen verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="dacd9-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="dacd9-116">Für die Zusammenarbeit in Azure optimiert.</span><span class="sxs-lookup"><span data-stu-id="dacd9-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="dacd9-117">Funktioniert in Staging, Entwicklung und Produktion.</span><span class="sxs-lookup"><span data-stu-id="dacd9-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="dacd9-118">Lokal funktioniert [Visual Studio](/azure/application-insights/app-insights-visual-studio) oder in anderen Hostumgebungen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="dacd9-119">Weitere Informationen finden Sie unter [Application Insights for ASP.NET Core (Application Insights für ASP.NET Core)](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="dacd9-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="dacd9-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="dacd9-120">PerfView</span></span>

<span data-ttu-id="dacd9-121">[PerfView](https://github.com/Microsoft/perfview) ist ein Performance Analysetool durch das .NET Team speziell für das Diagnostizieren von Leistungsproblemen .NET erstellt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="dacd9-122">PerfView ermöglicht die Analyse der CPU-Auslastung, Arbeitsspeicher und GC Verhalten, Leistungsereignisse und gesamtbetrachtungszeit.</span><span class="sxs-lookup"><span data-stu-id="dacd9-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="dacd9-123">Erfahren Sie mehr über PerfView und erste Schritte mit [Videotutorials für PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) oder durch Lesen das Benutzerhandbuch zur Verfügung, in das Tool oder [auf GitHub](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="dacd9-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="dacd9-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="dacd9-124">PerfCollect</span></span>

<span data-ttu-id="dacd9-125">Während PerfView eine nützliche leistungsanalysetools für Szenarien mit .NET ist, wird er nur auf Windows, sodass Sie es verwenden können, um die Ablaufverfolgung von ASP.NET Core-apps unter Linux-Umgebungen zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="dacd9-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="dacd9-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) ist ein Bash-Skript, das native Linux Profilerstellungstools verwendet ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) und [LTTng](https://lttng.org/)) um die Ablaufverfolgung unter Linux zu sammeln, die mit PerfView analysiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="dacd9-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="dacd9-127">PerfCollect ist nützlich, wenn Probleme mit der Leistung in Linux-Umgebungen angezeigt, in denen PerfView direkt verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="dacd9-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="dacd9-128">Stattdessen kann PerfCollect Sammeln von ablaufverfolgungen von .NET Core-apps, die dann analysiert werden auf einem Windows-Computer, die mithilfe von PerfView.</span><span class="sxs-lookup"><span data-stu-id="dacd9-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="dacd9-129">Weitere Informationen zum Installieren und erste Schritte mit PerfCollect [auf GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="dacd9-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
