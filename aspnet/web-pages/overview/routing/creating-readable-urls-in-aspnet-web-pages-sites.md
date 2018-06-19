---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Lesbare URLs erstellen, in der ASP.NET Web Pages (Razor)-Websites | Microsoft Docs
author: tfitzmac
description: Dieser Artikel beschreibt in einer Website für ASP.NET Web Pages (Razor), und wie können Sie die URLs verwenden, die besser lesbar und besser für SEO routing. Was sind Sie in der...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529749"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Erstellen lesbare URLs in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt in einer Website für ASP.NET Web Pages (Razor), und wie können Sie die URLs verwenden, die besser lesbar und besser für SEO routing.
> 
> Lernen Sie:
> 
> - Wie ASP.NET verwendet routing, um Sie besser lesbar und durchsuchbaren URLs verwenden können.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


## <a name="about-routing"></a>Zum Routing

Die URLs für die Seiten auf Ihrer Website möglich wirkt sich auf die Website wie gut funktioniert. Eine URL, die &quot;angezeigten&quot; Ausgabenanzeige für Personen, die Website zu verwenden. Sie können auch mit Search Engine Optimization (SEO) für den Standort. ASP.NET-Websites bieten die Möglichkeit, automatisch friendly URLs verwendet.

ASP.NET können Sie aussagekräftige URLs erstellen, die Benutzeraktionen, sondern nur in eine Datei auf dem Server zu beschreiben. Betrachten Sie diese URLs für eine fiktive Blog:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Vergleichen Sie diese URLs in den folgenden Vorgängen:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Ein Benutzer müsste in das erste Paar zu wissen, dass im Blog mit angezeigt wird der *blog.cshtml* Seite und dann müssten Sie eine Abfragezeichenfolge erstellt, die die richtige Kategorie oder den Datumsbereich abruft. Der zweite Satz von Beispielen ist viel einfacher zu verstehen und zu erstellen.

Zeigen Sie die URLs für das erste Beispiel auch direkt an eine bestimmte Datei (*blog.cshtml*). Wenn aus irgendeinem Grund im Blog in einen anderen Ordner verschoben wurden, auf dem Server oder der Blog so umgeschrieben wurden, dass eine andere Seite verwendet, würde die Links falsch sein. Der zweite Satz von URLs zu einer bestimmten Seite zeigt nicht würden also auch, wenn die Blog-Implementierung oder den Speicherort geändert, die URLs weiterhin gültig sein.

In ASP.NET Web Pages können Sie benutzerfreundlicheren URLs wie in den obigen Beispielen erstellen, da ASP.NET verwendet *routing*. Routing erstellt logische Zuordnung von einer URL zu einem (Seiten), die die Anforderung erfüllt werden kann. Da die Zuordnung der logischen ist (nicht physischen, um eine bestimmte Datei), das routing bietet große Flexibilität beim wie Sie die URLs für Ihre Website definieren.

## <a name="how-routing-works"></a>Funktionsweise von Routing

Wenn ASP.NET eine Anforderung verarbeitet, liest er die URL, um zu bestimmen, wie für die Weiterleitung an. ASP.NET versucht, die einzelne Segmente für die URL zu Dateien auf dem Datenträger für den Wechsel von links nach rechts übereinstimmen. Wenn eine Übereinstimmung vorhanden ist, wird nichts in der URL verbleibende übergeben, auf die Seite als *Pfadinformationen*.

Stellen Sie sich vor, dass jemand eine Anforderung, die über diese URL sendet:

`http://www.contoso.com/a/b/c`

Die Suche ist wie folgt:

1. Gibt es eine Datei mit dem Pfad und Name der */a/b/c.cshtml*? Wenn dies der Fall ist, führen Sie diese Seite, und weitergegeben Sie es werden keine Informationen an diesen. Andernfalls...
2. Gibt es eine Datei mit dem Pfad und Name der */a/b.cshtml*? Wenn deshalb führen Sie diese Seite, und des Werts übergeben `c` darauf. Andernfalls...
3. Gibt es eine Datei mit dem Pfad und Name der */a.cshtml*? Wenn deshalb führen Sie diese Seite, und des Werts übergeben `b/c` darauf.

Wenn die Suche keine genaue gefunden für eine Übereinstimmung *cshtml* -Dateien im angegebenen Ordner ASP.NET weiterhin wiederum für diese Dateien zu suchen:

1. */a/b/c/default.cshtml* (keine Pfadinformationen).
2. */a/b/c/Index.cshtml* (keine Pfadinformationen).

> [!NOTE]
> Zu deaktivieren, werden Anforderungen für bestimmte Seiten (also Anforderungen, die implizit enthalten die *cshtml* Dateierweiterung) wie erwartet funktionieren. Eine Anforderung wie `http://www.contoso.com/a/b.cshtml` führen Sie die Seite *b.cshtml* einwandfrei.


Innerhalb einer Seite erhalten Sie Informationen über der Seite für den Pfad `UrlData` Eigenschaft, die ein Wörterbuch darstellt. Angenommen, Sie eine Datei namens haben *ViewCustomers.cshtml* und Ihre Website ruft diese Anforderung:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Wie im obigen Regeln beschrieben, wird die Anforderung zur Ihre Seite wechseln. Auf der Seite können Sie Code wie den folgenden abrufen und Anzeigen von Informationen für den Pfad (in diesem Fall werden die Werte &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Da routing vollständige Dateinamen nicht einschließen, treten möglicherweise Mehrdeutigkeiten bei Seiten, die den gleichen Namen aber unterschiedliche Dateinamenerweiterungen (z. B. *MyPage.cshtml* und *MyPage.html*) . Um Probleme mit routing zu vermeiden, empfiehlt es sich um sicherzustellen, dass Seiten auf Ihrer Website stehen Ihnen nicht nur in ihre Erweiterung, deren Namen zu unterscheiden.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[WebMatrix - URLs, UrlData und Routing für SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Dieser Blog von Mike Brind enthält einige weitere Details für das routing funktioniert in ASP.NET Web Pages.
