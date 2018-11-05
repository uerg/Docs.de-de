---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Erstellen von lesbaren URLs in der ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Artikel beschreibt die in einer Website für ASP.NET Web Pages (Razor), und wie diese können Sie URLs verwenden, die besser lesbar und besser für SEO-routing. Was sind Sie in der...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 26d8f94b2e38fe5205a37e3d37b4e3bd509a3874
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021081"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Erstellen von lesbaren URLs in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt die in einer Website für ASP.NET Web Pages (Razor), und wie diese können Sie URLs verwenden, die besser lesbar und besser für SEO-routing.
> 
> Sie lernen Folgendes:
> 
> - Wie ASP.NET verwendet routing, um Sie besser lesbar und durchsuchbaren URLs verwenden können.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


## <a name="about-routing"></a>Informationen zum Routing

Die URLs für die Seiten auf Ihrer Website haben Auswirkungen auf die Website wie gut funktioniert. Eine URL, die &quot;benutzerfreundliche&quot; können sie leichter für Personen, die Website zu verwenden. Sie können auch mit der Such-Engine Optimization (SEO) für den Standort. ASP.NET-Websites umfassen die Möglichkeit, freundlichen URLs automatisch verwenden.

ASP.NET können Sie aussagekräftige URLs zu erstellen, die Aktionen des Benutzers statt nur auf eine Datei auf dem Server Zeigt beschreiben. Betrachten Sie diese URLs für einen fiktiven Blog:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Vergleichen Sie diese URLs, die unten aufgeführten:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Im ersten Paar ist, muss ein Benutzer wissen, dass das Blog mit angezeigt wird der *blog.cshtml* Seite und müsste dann eine Abfragezeichenfolge erstellt, die die richtige Kategorie oder den Datumsbereich abruft. Die zweite Gruppe der Beispiele ist viel einfacher zu verstehen und zu erstellen.

Zeigen Sie die URLs für das erste Beispiel auch direkt auf eine bestimmte Datei (*blog.cshtml*). Wenn aus irgendeinem Grund im Blog in einen anderen Ordner auf dem Server verschoben wurden, oder im Blog neu geschrieben wurden, um eine andere Seite verwenden, würde die Links auch falsch sein. Der zweite Satz von URLs nicht mit einer bestimmten Seite, zeigen Sie würden also selbst, wenn die Blog-Implementierung oder den Speicherort geändert, die URLs weiterhin gültig sein.

In ASP.NET Web Pages, können Sie freundlicher URLs wie in den obigen Beispielen erstellen, da ASP.NET verwendet *routing*. Routing erstellt logische Zuordnung über eine URL zu einer Seite (Seiten), die die Anforderung erfüllen kann. Da die Zuordnung logischer ist (nicht physischen, auf eine bestimmte Datei), routing bietet viel Flexibilität bei, wie Sie die URLs für Ihre Website definieren.

## <a name="how-routing-works"></a>Funktionsweise des Routing

Wenn ASP.NET eine Anforderung verarbeitet, liest er die URL, um zu bestimmen, wie die Weiterleitung erfolgt. ASP.NET versucht, einzelne Segmente der URL, um Dateien auf dem Datenträger, von links nach rechts. Wenn eine Übereinstimmung vorliegt, wird nichts verbleiben in der URL übergeben, auf die Seite als *Pfadinformationen*.

Stellen Sie sich, dass jemand eine Anforderung, die über diese URL sendet:

`http://www.contoso.com/a/b/c`

Die Suche läuft folgendermaßen ab:

1. Gibt es eine Datei mit den Pfad und Namen der */a/b/c.cshtml*? Wenn dies der Fall ist, führen Sie diese Seite, und weitergeben Sie es sind keine Informationen an die Klasse. Andernfalls...
2. Gibt es eine Datei mit den Pfad und Namen der */a/b.cshtml*? Falls Ja, führen Sie diese Seite, und übergeben Sie den Wert `c` zuzuweisen. Andernfalls...
3. Gibt es eine Datei mit den Pfad und Namen der */a.cshtml*? Falls Ja, führen Sie diese Seite, und übergeben Sie den Wert `b/c` zuzuweisen.

Wenn für die Suche finden Sie nicht genau übereinstimmt *.cshtml* -Dateien in ihre angegebene Ordner ASP.NET bleibt auch weiterhin im Gegenzug für diese Dateien zu suchen:

1. */a/b/c/default.cshtml* (keine Pfadinformationen).
2. */a/b/c/Index.cshtml* (keine Pfadinformationen).

> [!NOTE]
> Zur Anforderungen für bestimmte Seiten (d.h. Anforderungen, die enthalten die *.cshtml* Dateierweiterung) funktionieren nur, wie Sie erwarten. Eine Anforderung wie `http://www.contoso.com/a/b.cshtml` führen Sie die Seite *"b.cshtml"* einwandfrei.


Innerhalb einer Seite erhalten Sie die Pfadinformationen, mit der `UrlData` -Eigenschaft, die ein Wörterbuch handelt. Angenommen, Sie haben, dass eine Datei namens *ViewCustomers.cshtml* und Ihre Website ruft diese Anforderung ab:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Wie in der oben genannten Regeln beschrieben, wird die Anforderung zur Ihre Seite wechseln. In der Seite können Sie Code wie den folgenden abrufen und Anzeigen von Informationen über die Pfade (in diesem Fall wird der Wert &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Da routing vollständige Dateinamen beinhalten nicht möglich Mehrdeutigkeiten bei Seiten, die den gleichen Namen aber unterschiedliche Dateinamenerweiterungen (z. B. *MyPage.cshtml* und *MyPage.html*) . Um Probleme mit dem routing zu vermeiden, empfiehlt es sich um sicherzustellen, dass Sie Seiten auf Ihrer Website nicht nur in ihrer Erweiterung, deren Namen zu unterscheiden.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[WebMatrix - URLs, UrlData und Routing für SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). In diesem Blogeintrag von Mike Brind enthält einige weitere Details auf der Funktionsweise von routing in ASP.NET Web Pages.
