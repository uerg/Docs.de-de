---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Verbessern der Leistung mit Ausgabe Zwischenspeichern (c#) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie die Leistung Ihrer ASP.NET MVC-Webanwendungen erheblich verbessern können, durch die Nutzung der ausgabezwischenspeicherung. Sie...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 26e65cb5f0e256d4ca819dfde4a748f00d56f08e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832541"
---
<a name="improving-performance-with-output-caching-c"></a>Verbessern der Leistung mit Ausgabezwischenspeicherung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Tutorial erfahren Sie, wie Sie die Leistung Ihrer ASP.NET MVC-Webanwendungen erheblich verbessern können, durch die Nutzung der ausgabezwischenspeicherung. Erfahren Sie, wie Sie die Zwischenspeicherung des Ergebnisses über eine Controlleraktion zurückgegeben werden, sodass derselbe Inhalt muss nicht jedes Mal erstellt werden, das ein neuer Benutzer die Aktion aufruft.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie die Leistung von ASP.NET MVC-Anwendungen erheblich verbessern können, durch die Nutzung des Ausgabecaches. Der Ausgabecache ermöglicht Ihnen, den Inhalt, der eine Controlleraktion vom Zwischenspeichern. Auf diese Weise muss der gleiche Inhalt nicht jedes Mal generiert werden soll, wenn, das die gleichen Controlleraktion aufgerufen wird.

Stellen Sie sich vor, z. B., dass Ihre ASP.NET MVC-Anwendung eine Liste von Datenbank-Datensätzen in einer Ansicht mit dem Namen Index angezeigt. In der Regel muss jedes Mal, das ein Benutzer die Controlleraktion aufruft, die Ansicht "Index", gibt der Satz von Datenbank-Datensätzen aus der Datenbank abgerufen werden durch Ausführen einer Datenbankabfrage.

Wenn Sie auf der anderen Seite des Ausgabecaches nutzen können Sie vermeiden, eine Datenbankabfrage ausführt, jedes Mal, wenn alle Benutzer die gleiche Controlleraktion aufruft. Die Sicht kann aus dem Zwischenspeicher, anstatt Sie von der Controlleraktion Objektebenendatei verhindert abgerufen werden. Zwischenspeichern können arbeiten Sie vermeiden redundante ausführen auf dem Server.

## <a name="enabling-output-caching"></a>Aktivieren der Zwischenspeicherung der Ausgabe

Sie ermöglichen das Zwischenspeichern der Ausgabe durch ein [OutputCache]-Attribut entweder eine einzelne Controlleraktion oder eine gesamte Controllerklasse hinzufügen. Der Controller in Codebeispiel 1 stellt beispielsweise eine Aktion, die mit dem Namen Index(). Die Ausgabe der Aktion Index() ist 10 Sekunden lang zwischengespeichert.

**Codebeispiel 1 – Controllers\HomeController. cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

In der Beta-Versionen von ASP.NET MVC, ausgabezwischenspeicherung funktioniert nicht für eine URL wie [ http://www.MySite.com/ ](http://www.mysite.com/). Stattdessen geben Sie eine URL wie [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

Im Codebeispiel 1 wird die Ausgabe der Aktion Index() für 10 Sekunden zwischengespeichert. Falls gewünscht, können Sie eine viel längere Cachedauer angeben. Z. B. wenn die Ausgabe des eine Controlleraktion einen Tag lang zwischengespeichert werden sollen. anschließend können Sie angeben eine Cachedauer von 86400 Sekunden (60 Sekunden \* 60 Minuten \* 24 Stunden).

Es gibt keine Garantie dafür, dass Inhalt im Cache für den Zeitraum, den Sie angeben. Wenn Speicherressourcen niedrig sind, wird der Cache bei dem Versuch Inhalt automatisch gestartet.

Die Home-Controller in Codebeispiel 1 gibt die Ansicht "Index" in Liste 2 zurück. Es gibt keine besonderen zu dieser Ansicht. Ansicht "Index" die aktuelle Uhrzeit einfach angezeigt (siehe Abbildung 1).

**Codebeispiel 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Abbildung 1 – Cached Indexansicht**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Wenn Sie die Aktion Index() mehrmals aufrufen, indem der URL/Home/Index in die Adressleiste des Browsers eingeben, und drücken die Schaltfläche "aktualisieren und laden" in Ihrem Browser ein, wiederholt, und klicken Sie dann die Zeit, die von der Ansicht "Index" angezeigt wird nicht für 10 Sekunden ändern. Gleichzeitig wird angezeigt, da die Sicht zwischengespeichert werden.

Es ist wichtig zu verstehen, dass die gleiche Ansicht für alle Benutzer zwischengespeichert werden, die Ihre Anwendung besucht. Jeder, der die Aktion Index() aufruft erhalten die gleiche zwischengespeicherte Version der Ansicht "Index". Dies bedeutet, dass der Arbeitsaufwand, die der Webserver ausführen muss, um die Ansicht "Index" dienen der erheblich reduziert wird.

Die Ansicht im Codebeispiel 2 erfolgt etwas ganz einfach durchgeführt werden. Die Ansicht zeigt nur die aktuelle Zeit. Allerdings können Sie genauso einfach Cache eine Ansicht, die einen Satz von Datenbank-Datensätzen wird angezeigt. In diesem Fall müssten der Satz von Datenbank-Datensätzen nicht jedes Mal aus der Datenbank abgerufen werden soll, die aufgerufen wird, dass Sie die Controlleraktion, die die Sicht zurückgibt. Durch das Zwischenspeichern kann der verbleibende Arbeitsaufwand reduzieren, die sowohl den Webserver und Datenbankserver ausführen müssen.

Verwenden Sie die Seite nicht &lt;% @ OutputCache %&gt; -Direktive in einer MVC-Ansicht. Diese Richtlinie wird von der Web Forms-Welt hinüberlaufen und sollte nicht in einer ASP.NET MVC-Anwendung verwendet werden.

## <a name="where-content-is-cached"></a>Wo der Inhalt zwischengespeichert wird

Standardmäßig bei der Verwendung des Attributs [OutputCache], Inhalte zwischengespeichert an drei Stellen: der Webserver, alle Proxyserver und den Webbrowser. Sie können steuern, genau, in denen Inhalte zwischengespeichert werden durch Ändern der Location-Eigenschaft des Attributs [OutputCache].

Sie können die Location-Eigenschaft auf eine der folgenden Werte festlegen:

> · Alle
> 
> · Client
> 
> · Downstream
> 
> · Server
> 
> · Keine
> 
> · ServerAndClient


Die Location-Eigenschaft hat den Wert in der Standardeinstellung alle aus. Es gibt jedoch Situationen, in denen Cache nur auf den Browser oder nur auf dem Server empfiehlt. Z. B. Wenn Sie Informationen zwischenspeichern, die für jeden Benutzer personalisiert wird sollte dann Sie nicht die Informationen auf dem Server Zwischenspeichern. Wenn Sie verschiedene Informationen zu einem anderen Benutzer angezeigt werden, sollten Sie die Informationen nur auf dem Client Zwischenspeichern.

Der Controller in Programmausdruck 3 stellt beispielsweise eine Aktion, die mit dem Namen GetName()"eingeben, die den aktuellen Benutzernamen zurückgibt. Wenn Jack an der Website anmeldet und die Aktion GetName()"eingeben ruft gibt die Aktion die Zeichenfolge"Jack"Hi". Wenn Sie anschließend Jill an der Website anmeldet, und ruft die Aktion GetName()"eingeben erhalten sie auch die Zeichenfolge"Jack"Hi". Die Zeichenfolge wird auf dem Webserver für alle Benutzer zwischengespeichert, nach Jack zunächst die Controlleraktion aufgerufen.

**Codebeispiel 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Der Controller in Programmausdruck 3 funktioniert in den meisten Fällen nicht die Möglichkeit, die Sie möchten. Sie möchten nicht die Meldung "Hi Jack" Jill anzuzeigen.

Sie sollten niemals personalisierte Inhalte im Server-Cache Zwischenspeichern. Allerdings empfiehlt es sich, die personalisierte Inhalte im Browser-Cache zur leistungsoptimierung zwischenzuspeichern. Wenn Sie Inhalt im Browser zwischengespeichert, und Benutzer die gleichen Controlleraktion mehrere Male rufen, kann der Inhalt aus dem Browsercache anstatt auf dem Server abgerufen werden.

Der geänderte Controller in Listing 4 speichert die Ausgabe der Aktion GetName()"eingeben. Der Inhalt wird jedoch nur in den Browser und nicht auf dem Server zwischengespeichert. Auf diese Weise, wenn mehrere Benutzer die GetName()"eingeben-Methode aufrufen, ruft jede Person, die den eigenen Benutzernamen und die Benutzernamen nicht in einer anderen Person ab.

**Programmausdruck 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Beachten Sie, dass das Attribut [OutputCache] in Listing 4 eine Location-Eigenschaft, die auf den Wert OutputCacheLocation.Client festgelegt enthält. Das Attribut [OutputCache] enthält auch eine NoStore-Eigenschaft. Die NoStore-Eigenschaft wird verwendet, um Proxyservern und Browser darüber zu informieren, dass sie eine dauerhafte Kopie des zwischengespeicherten Inhalts nicht gespeichert werden sollten.

## <a name="varying-the-output-cache"></a>Variieren des Ausgabecaches

In einigen Fällen sollten Sie verschiedene zwischengespeicherte Versionen sehr derselbe Inhalt. Angenommen Sie, Sie erstellen eine Master/Detail-Seite. Die Masterseite zeigt eine Liste von Filmtiteln. Wenn Sie einen Titel klicken, erhalten Sie Details für den ausgewählten Film an.

Wenn Sie auf die Seite Details zum Zwischenspeichern, werden die Details für denselben Film unabhängig davon, welche Film, die Sie klicken angezeigt werden. Die ersten Films, die durch den ersten Benutzer ausgewählt wird für alle zukünftigen Benutzer angezeigt.

Sie können dieses Problem beheben, durch die Nutzung der VaryByParam-Eigenschaft des Attributs [OutputCache]. Diese Eigenschaft ermöglicht Ihnen die Erstellung von verschiedenen zwischengespeicherten Versionen der sehr gleichen Inhalts, wenn ein Formularparameter oder Abfragezeichenfolgen-Parameter variiert.

Der Controller in Listing 5 stellt z. B. zwei Aktionen, die mit dem Namen Master() und Details() zur Verfügung. Die Master()-Aktion gibt eine Liste von Filmtiteln und die Details()-Aktion gibt die Details für den ausgewählten Film zurück.

**Programmausdruck 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Die Aktion Master() enthält eine VaryByParam-Eigenschaft mit dem Wert "none". Werden zurückgegeben, wenn die Master() Aktion aufgerufen wird, wird die gleiche zwischengespeicherte Version des Master-Shapes anzeigen. Alle Formularparameter oder die Abfragezeichenfolge Parameter werden ignoriert (siehe Abbildung 2).

**Abbildung 2 – die /Movies/Master-Ansicht**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Abbildung 3 – die/Filme/Detailansicht**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Die Aktion Details() enthält eine Eigenschaft vom VaryByParam-Element mit dem Wert "Id". Wenn andere Werte, der den Id-Parameter an die Controlleraktion übergeben werden, werden die verschiedenen zwischengespeicherte Versionen der Detailansicht generiert.

Es ist wichtig zu wissen, dass mit den Ergebnissen des VaryByParam-Eigenschaft in Cacheregeln und kein kleiner. Eine andere zwischengespeicherte Version der Detailansicht wird für jede andere Version des Id-Parameters erstellt.

Sie können die VaryByParam-Eigenschaft auf die folgenden Werte festlegen:

> \* = Erstellt Sie eine andere zwischengespeicherte Version aus, wenn ein Formular oder Abfragezeichenfolgen-Parameters variieren.
> 
> None = nie verschiedene zwischengespeicherte Versionen erstellen
> 
> Durch Semikolons Liste von Parametern = erstellen verschiedene zwischengespeicherte Versionen aus, wenn das Formular oder Abfrage Zeichenfolge-Parameter in der Liste variiert


## <a name="creating-a-cache-profile"></a>Erstellen eines Cacheprofils

Als Alternative zum Konfigurieren von Eigenschaften der Ausgabespalten-Cache durch Ändern der Eigenschaften des Attributs [OutputCache] können Sie ein Cacheprofil in der Webkonfigurationsdatei (web.config) erstellen. Erstellen ein Cacheprofil, in der Webkonfigurationsdatei bietet einige wichtige Vorteile.

Erstens können durch Zwischenspeichern der Ausgabe in der Konfigurationsdatei konfigurieren, Sie steuern Sie die Controlleraktionen Inhalte an einem zentralen Ort Zwischenspeicherung. Sie können eine Cacheprofil erstellen, und wenden das Profil auf mehrere Controller oder Controlleraktionen.

Zweitens können Sie die Konfigurationsdatei ändern, ohne Erneutes Kompilieren der Anwendung. Wenn Sie müssen zum Deaktivieren der Zwischenspeicherung für eine Anwendung, die bereits in der produktionsumgebung bereitgestellt wurde, können Sie die Cacheprofile in der Webkonfigurationsdatei definiert einfach ändern. Alle Änderungen an der Webkonfigurationsdatei werden automatisch erkannt und angewendet.

Z. B. die &lt;zwischenspeichern&gt; Web-Konfigurationsabschnitt im Codebeispiel 6 definiert ein Cacheprofil, das mit dem Namen Cache1Hour. Die &lt;zwischenspeichern&gt; Abschnitt muss angezeigt werden, in der &lt;"System.Web"&gt; Teil einer Webkonfigurationsdatei.

**Codebeispiel 6: Zwischenspeichern im Abschnitt "Web.config"**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Der Controller im Codebeispiel 7 veranschaulicht, wie Sie das Profil Cache1Hour auf eine Controlleraktion mit dem Attribut [OutputCache] anwenden können.

**Auflisten von 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Wenn Sie die Index()-Aktion, die verfügbar gemacht werden, durch den Controller im Codebeispiel 7 aufrufen wird gleichzeitig für 1 Stunde zurückgegeben.

## <a name="summary"></a>Zusammenfassung

Zwischenspeichern der Ausgabe bietet Ihnen eine sehr einfache Methode zum erhebliche Steigerung der Leistung von ASP.NET MVC-Anwendungen. In diesem Tutorial haben Sie gelernt, wie das [OutputCache]-Attribut zu verwenden, um die Ausgabe von Controlleraktionen zwischenzuspeichern. Sie haben zudem die Vorgehensweise beim Ändern der Eigenschaften des Attributs [OutputCache], z. B. die Dauer und VaryByParam-Eigenschaften zu ändern, wie Inhalte zwischengespeichert werden sollen. Schließlich haben Sie die Cacheprofile in der Webkonfigurationsdatei definieren.

> [!div class="step-by-step"]
> [Zurück](understanding-action-filters-cs.md)
> [Weiter](adding-dynamic-content-to-a-cached-page-cs.md)
