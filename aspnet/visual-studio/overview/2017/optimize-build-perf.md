---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimieren der Buildleistung für Lösung
author: tfitzmac
description: Optimieren der Buildleistung für Lösung
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909961"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="520fd-103">Optimieren der Buildleistung für Lösung</span><span class="sxs-lookup"><span data-stu-id="520fd-103">Optimize build performance for solution</span></span>
<span data-ttu-id="520fd-104">Visual Studio 2017 15.8 und ein neues Menüelement in einem späteren Zeitpunkt hinzugefügt **erstellen > ASP.NET-Kompilierung > Projektmappenverzeichnis erstellen Leistung optimieren**.</span><span class="sxs-lookup"><span data-stu-id="520fd-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![Screenshot des neuen Menüelements](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="520fd-106">ASP.NET kompiliert die Ansichten zur Laufzeit, was bedeutet, dass es sich bei Ihrem ASP.NET-Projekt mit sich eine Kopie der Compiler führt.</span><span class="sxs-lookup"><span data-stu-id="520fd-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="520fd-107">Jedoch auf einem Entwicklercomputer wird, wenn die Kopie des Compilers nicht Visual Studio kopieren, entspricht die Buildleistung Größenordnung von 1 bis 3 Sekunden pro inkrementeller Build beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="520fd-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="520fd-108">Diese Funktion wird aktualisiert, Ihres Projekts Kopieren des Compilers mit Visual Studio übereinstimmen, die inkrementelle Builds zu beschleunigen soll.</span><span class="sxs-lookup"><span data-stu-id="520fd-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="520fd-109">Dies gilt für nur ASP.NET Framework-Projekte, es gilt nicht für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="520fd-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
