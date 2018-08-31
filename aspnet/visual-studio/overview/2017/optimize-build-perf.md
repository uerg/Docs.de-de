---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimieren der Buildleistung für Lösung
author: AngelosP
description: Optimieren der Buildleistung für Lösung
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312140"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="0a2a4-103">Optimieren der Buildleistung für Lösung</span><span class="sxs-lookup"><span data-stu-id="0a2a4-103">Optimize build performance for solution</span></span>

<span data-ttu-id="0a2a4-104">Visual Studio 2017 15.8 oder höheren Versionen gehören ein Menüelement: **erstellen** > **ASP.NET-Kompilierung** > **erstellen Leistung optimieren, für die Lösung**.</span><span class="sxs-lookup"><span data-stu-id="0a2a4-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Screenshot des neuen Menüelements](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="0a2a4-106">ASP.NET kompiliert die Ansichten zur Laufzeit, was bedeutet, dass ein ASP.NET-Projekt mit sich eine Kopie der Compiler führt.</span><span class="sxs-lookup"><span data-stu-id="0a2a4-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="0a2a4-107">Jedoch auf einem Computer des Entwicklers bei der die Kopie des Compilers nicht mit Visual Studio Kopie übereinstimmt Buildleistung Größenordnung von 1 bis 3 Sekunden pro inkrementeller Build betroffen ist.</span><span class="sxs-lookup"><span data-stu-id="0a2a4-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="0a2a4-108">Dieses Feature wird aktualisiert, Ihres Projekts Kopieren des Compilers mit Visual Studio übereinstimmen, inkrementelle Builds in der Regel beschleunigt wird.</span><span class="sxs-lookup"><span data-stu-id="0a2a4-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="0a2a4-109">**Dies gilt für ASP.NET Framework 4.7.1 oder höher nur Projekte, es gilt nicht für ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="0a2a4-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
