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
# <a name="optimize-build-performance-for-solution"></a>Optimieren der Buildleistung für Lösung
Visual Studio 2017 15.8 und ein neues Menüelement in einem späteren Zeitpunkt hinzugefügt **erstellen > ASP.NET-Kompilierung > Projektmappenverzeichnis erstellen Leistung optimieren**.

![Screenshot des neuen Menüelements](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET kompiliert die Ansichten zur Laufzeit, was bedeutet, dass es sich bei Ihrem ASP.NET-Projekt mit sich eine Kopie der Compiler führt. Jedoch auf einem Entwicklercomputer wird, wenn die Kopie des Compilers nicht Visual Studio kopieren, entspricht die Buildleistung Größenordnung von 1 bis 3 Sekunden pro inkrementeller Build beeinträchtigt. Diese Funktion wird aktualisiert, Ihres Projekts Kopieren des Compilers mit Visual Studio übereinstimmen, die inkrementelle Builds zu beschleunigen soll.

Dies gilt für nur ASP.NET Framework-Projekte, es gilt nicht für ASP.NET Core.
