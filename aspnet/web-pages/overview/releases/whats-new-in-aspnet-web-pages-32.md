---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Neuigkeiten in der ASP.NET Web Pages 3.2 | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: cdb0e259bbf27d1d3dcf6ada11e6636c9cefcc9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Was ist neu in ASP.NET Web Pages 3.2
====================
durch [Microsoft](https://github.com/microsoft)

In diesem Thema wird beschrieben, mit ASP.NET Web Pages 3.2, Webseiten 3.2.2 Neuigkeiten und [Webseiten 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Diese Version behebt einen Fehler, und es wird eine neue Funktion eingeführt.

## <a name="download"></a>Herunterladen

Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung. Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation. Das ASP.NET Web Pages 3.2-Paket enthält die folgende Version: &ldquo;3.2.0&rdquo;. Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.

Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Neues Feature und Fehlerbehebung

Wir ein Fehler behoben und eine kleinere Erweiterung in dieser Version vorgenommen haben. Sie erhalten die Abfrage für dieselbe [hier](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Diese Version (Rollup) die Änderung der [Betaversion von ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) bietet eine deutliche leistungsverbesserung in großen Razor-Seiten zu rendern. Finden Sie unter[Codeplex Problem 585](https://aspnetwebstack.codeplex.com/workitem/585). Diese Version richtet mit MVC 5.2.2 Pakete, mit denen von dieser Version jetzt abhängig ist.

Wir arbeitet mit dem Team, MSN für umfangreiche Seiten zu rendern. Wenn mehr als 80 KB Daten Rendern von Seiten zum Schluss wir Objekte im großen Objektheap. Dieser Effekt kann multipliziert werden soll, wenn mehrere Ebenen von Layouts verwendet werden.

Das Ergebnis auf dem Server ist die zusätzliche CPU-Auslastung, längere Beibehaltungsdauer des Speichers und sogar lange angehalten, während der [Gen 2-Cleanup](https://msdn.microsoft.com/en-us/library/ms973837.aspx) in der Garbage Collector.

Im folgenden finden Sie eine Tabelle, die die Ergebnisse der Analyse von veranschaulichen eine [Perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) für eine Ausführung. Die CPU wird konstant bei ca. 68 % aufrechterhalten, während große Seiten gerendert werden. Die Tabelle zeigt, dass die Anzahl von Collections der Generation 2 fast vollständig gelöscht wurde, und das Ergebnis höher Anforderungsrate und einer erheblichen Verringerung von Pausen wegen der Garbagecollection ist.

| **Bereich** | **Vor (3.2)** | **Nach dem (3.2.1)** | **Delta %** |
| --- | --- | --- | --- |
| Gesamtanforderung (Anzahl) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Trace-Dauer (in Sekunden) | 196.20 | 198.60 | 1.20% |
| Anforderung/Sekunde | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU-Auslastung | 68.80% | 68.50% |  -0.40% |
| GC-CPU-Beispiele | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Gesamten speicherbelegungen (Anzahl) | 55,357,146 | 57,222,949 | 3.40% |
| Insgesamt GC anhalten (Beispiele) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (Anzahl) | 403 | 1,216 | 201.70% |
| Gen1 GC (Anzahl) | 290 | 367 | 26.60% |
| Gen2 GC (Anzahl) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / anfordern (Samples/Req) | 19.73 | 16.47 | -16.50% |

| Farbcodierung: | <font style="background-color: #00ff00">Core-Verbesserung</font> | <font style="background-color: #4bacc6">Positive Auswirkung auf die Leistung</font> |
| --- | --- | --- |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Diese Version enthält nur Fehlerkorrekturen. Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) auf die Liste der in dieser Version behobene Probleme anzuzeigen.
