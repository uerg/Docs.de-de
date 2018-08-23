---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Neuigkeiten in der ASP.NET Web Pages 3.2 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 31b462a3116b29770f534fb95b69a29ae665487c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833426"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Neuerungen in ASP.NET Web Pages 3.2
====================
durch [Microsoft](https://github.com/microsoft)

In diesem Thema wird beschrieben, neues für ASP.NET Web Pages 3.2, Webseiten 3.2.2 und [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Diese Version behebt einen Fehler, und es wird ein neues Feature eingeführt.

## <a name="download"></a>Herunterladen

Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht. Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation. Das ASP.NET Web Pages 3.2-Paket hat die folgende Version: &ldquo;3.2.0&rdquo;. Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.

Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Neue Features und behobenen Fehlern

Wir es wurde ein Problem behoben und eine kleine Funktion Verbesserung in dieser Version vorgenommen wurden. Sie finden die Abfrage für den gleichen [hier](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Diese Version (Rollup) die Änderung der [Betaversion der ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) die bietet einer deutliche leistungsverbesserung in großen Razor-Seiten zu rendern. Finden Sie unter[Codeplex Problem 585](https://aspnetwebstack.codeplex.com/workitem/585). Diese Version entspricht, mit MVC 5.2.2 Pakete, mit denen diese Version nun abhängig ist.

Wir zusammengearbeitet, mit dem MSN-Team für das Rendern von großer Seiten. Wenn Seiten mehr als 80 KB Daten rendern, erhalten wir Objekte auf den großen Objektheap. Dieser Effekt kann verschärft werden, wenn mehrere Ebenen von Layouts verwendet werden.

Das Ergebnis auf dem Server zusätzliche CPU-Auslastung, längere Aufbewahrung von Arbeitsspeicher, und dies auch noch lange anhält, während der [Gen 2-Bereinigung](https://msdn.microsoft.com/en-us/library/ms973837.aspx) beim Garbage Collector.

Im folgenden finden Sie eine Tabelle veranschaulicht die Ergebnisse der Analyse einer [Perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) für eine Ausführung. Die CPU wird auf ca. 68 %, Konstanten aufrechterhalten, während große Seiten gerendert werden. Die Tabelle zeigt, dass die Anzahl von Collections der Generation 2 fast vollständig gelöscht wurde, und das Ergebnis höhere Anforderungsrate und einer erheblichen Verringerung Pausen wegen der Garbagecollection ist.

| **Bereich** | **Bevor Sie (3.2)** | **Nach dem (3.2.1)** | **Delta %** |
| --- | --- | --- | --- |
| Gesamtanforderung (Anzahl) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Trace-Dauer (Sekunden) | 196.20 | 198.60 | 1.20% |
| Anforderung/Sekunde | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU-Auslastung | 68.80% | 68.50% |  -0.40% |
| GC-CPU-Beispiele | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Gesamtanzahl der Zuordnungen (Anzahl) | 55,357,146 | 57,222,949 | 3.40% |
| Gesamtanzahl der GC anhalten (Beispiele) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0-GC (Anzahl) | 403 | 1,216 | 201.70% |
| Gen 1 GC (Anzahl) | 290 | 367 | 26.60% |
| Gen 2 GC (Anzahl) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / anfordern (Beispiele/Req) | 19.73 | 16.47 | -16.50% |

| Farbcodierung: | <font style="background-color: #00ff00">Core-Verbesserung</font> | <font style="background-color: #4bacc6">Positive Auswirkungen auf die Leistung</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Diese Version enthält nur Fehler behoben. Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) um die Liste der in diesem Release behobene Probleme anzuzeigen.
