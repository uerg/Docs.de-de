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
# <a name="performance-diagnostic-tools"></a>Leistung-Diagnosetools

Durch [Mike Rousos](https://github.com/mjrousos)

Dieser Artikel beschreibt die Tools zum Diagnostizieren von Leistungsproblemen in ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio-Diagnosetools

Die [profilerstellung und Diagnosetools](/visualstudio/profiling) in Visual Studio integriert sind ein guter Ausgangspunkt für die Untersuchung Leistungsprobleme auftreten. Diese Tools sind leistungsstark und praktisch, in der Visual Studio-Entwicklungsumgebung zu verwenden. Das Tool ermöglicht die Analyse der CPU-Auslastung, speicherauslastung und Leistungsereignisse in ASP.NET Core-apps.

Weitere Informationen finden Sie in [Visual Studio-Dokumentation](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) bietet ausführlichere Leistungsdaten für Ihre app. Application Insights erfasst automatisch Daten, auf die Antwortrate, Fehlerraten, Reaktionszeiten von Abhängigkeiten und vieles mehr. Application Insights unterstützt benutzerdefinierte Ereignisse und Metriken, die bestimmte zu Ihrer app anmelden.

Application Insights können in einer Vielzahl-Umgebungen verwendet werden:

* Für die Zusammenarbeit in Azure optimiert.
* Funktioniert in Staging, Entwicklung und Produktion.
* Lokal funktioniert [Visual Studio](/azure/application-insights/app-insights-visual-studio) oder in anderen Hostumgebungen.

Weitere Informationen finden Sie unter [Application Insights for ASP.NET Core (Application Insights für ASP.NET Core)](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) ist ein Performance Analysetool durch das .NET Team speziell für das Diagnostizieren von Leistungsproblemen .NET erstellt. PerfView ermöglicht die Analyse der CPU-Auslastung, Arbeitsspeicher und GC Verhalten, Leistungsereignisse und gesamtbetrachtungszeit.

Erfahren Sie mehr über PerfView und erste Schritte mit [Videotutorials für PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) oder durch Lesen das Benutzerhandbuch zur Verfügung, in das Tool oder [auf GitHub](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

Während PerfView eine nützliche leistungsanalysetools für Szenarien mit .NET ist, wird er nur auf Windows, sodass Sie es verwenden können, um die Ablaufverfolgung von ASP.NET Core-apps unter Linux-Umgebungen zu sammeln.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) ist ein Bash-Skript, das native Linux Profilerstellungstools verwendet ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) und [LTTng](https://lttng.org/)) um die Ablaufverfolgung unter Linux zu sammeln, die mit PerfView analysiert werden kann. PerfCollect ist nützlich, wenn Probleme mit der Leistung in Linux-Umgebungen angezeigt, in denen PerfView direkt verwendet werden kann. Stattdessen kann PerfCollect Sammeln von ablaufverfolgungen von .NET Core-apps, die dann analysiert werden auf einem Windows-Computer, die mithilfe von PerfView.

Weitere Informationen zum Installieren und erste Schritte mit PerfCollect [auf GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
