---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Verbessern der Leistung mit Output Zwischenspeichern (VB) | Microsoft Docs
author: microsoft
description: In diesem Lernprogramm erfahren Sie, wie Sie die Leistung Ihrer ASP.NET MVC-Webanwendungen deutlich verbessern können, durch Zwischenspeichern der Ausgabe nutzen. Sie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 8ee933b477307f5c3f2377e112a1a98d3d6bc337
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874420"
---
<a name="improving-performance-with-output-caching-vb"></a>Verbessern der Leistung mit Ausgabecaching (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Lernprogramm erfahren Sie, wie Sie die Leistung Ihrer ASP.NET MVC-Webanwendungen deutlich verbessern können, durch Zwischenspeichern der Ausgabe nutzen. Erfahren Sie, wie der Rückgabe eine Controlleraktion muss der gleiche Inhalt nicht jedes Mal erstellt werden soll, das ein neuer Benutzer die Aktion aufruft, zwischengespeichert.


Ziel dieses Lernprogramms wird erläutert, wie Sie die Leistung einer ASP.NET MVC-Anwendung deutlich verbessern können, durch die Nutzung des Ausgabecaches. Der Ausgabecache ermöglicht Ihnen, den Inhalt, der eine Controlleraktion zurückgegebenes Zwischenspeichern. Auf diese Weise muss der gleiche Inhalt nicht jedes Mal generiert werden soll, wenn, das die gleichen Controlleraktion aufgerufen wird.

Stellen Sie sich vor, z. B., dass Ihre ASP.NET MVC-Anwendung in einer Ansicht mit dem Namen Index zeigt eine Liste von Datenbankdatensätzen an. In der Regel muss jedes Mal, das ein Benutzer Controlleraktion aufruft, die die Indexansicht zurückgibt der Satz von Datenbank-Datensätze aus der Datenbank abgerufen werden durch das Ausführen einer Datenbankabfrage.

Wenn Sie auf der anderen Seite des Ausgabecaches nutzen können Sie vermeiden, Ausführen einer Datenbankabfrage, jedes Mal, wenn alle Benutzer die gleiche Controlleraktion aufruft. Die Sicht kann aus dem Cache anstelle von Controlleraktion erneut generierten abgerufen werden. Ermöglicht das Zwischenspeichern, arbeiten Sie vermeiden redundante ausführen auf dem Server.

#### <a name="enabling-output-caching"></a>Aktivieren die Ausgabe-Caching

Sie aktivieren Zwischenspeichern der Ausgabe durch Hinzufügen einer &lt;OutputCache&gt; -Attribut auf eine einzelne Controlleraktion oder eine gesamte Controllerklasse. Der Controller im Codebeispiel 1 gibt z. B. eine Aktion, die mit dem Namen Index(). Die Ausgabe der Index() Aktion wird für 10 Sekunden zwischengespeichert.

**1 – Controllers\HomeController.vb auflisten**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


Die Beta-Versionen von ASP.NET MVC, Ausgabecaching funktioniert nicht für eine URL wie [ http://www.MySite.com/ ](http://www.mysite.com/). Stattdessen geben Sie eine URL wie [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).


Im Codebeispiel 1 wird die Ausgabe des Index() für 10 Sekunden zwischengespeichert. Falls gewünscht, können Sie eine wesentlich länger Cachedauer angeben. Z. B. wenn die Ausgabe eine Controlleraktion für einen Tag zwischengespeichert werden sollen können Sie angeben einer Cachedauer 86400 Sekunden (60 Sekunden \* 60 Minuten \* 24 Stunden).

Es gibt keine Garantie dafür, dass Inhalt zwischengespeichert werden soll, für den Zeitraum, den Sie angeben. Nach verfügbaren Speicherressourcen auf Niedrig festgelegt werden, werden bei dem Versuch Inhalt in der Cache automatisch gestartet.

Der Home-Controller im Codebeispiel 1 gibt die Indexansicht Auflisten von 2 zurück. Es gibt keine besonderen zu dieser Ansicht aus. Die Indexansicht zeigt einfach an die aktuelle Uhrzeit (siehe Abbildung 1).

**2 – Views\Home\Index.aspx auflisten**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Abbildung 1 – Cached Indexansicht**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Wenn Sie die Aktion Index() mehrmals aufrufen, indem eingeben des URL/Home/Indexes in der Adressleiste des Browsers ein, und drücken die Schaltfläche Aktualisieren/neu laden im Browser ein, wiederholt, und klicken Sie dann die Zeit angezeigt, indem die Indexansicht wird nicht für 10 Sekunden ändern. Gleichzeitig wird angezeigt, da die Sicht zwischengespeichert werden.

Es ist wichtig zu verstehen, dass derselben Ansicht für "Jeder" zwischengespeichert wird, die Ihre Anwendung besuchen. Jeder, der die Aktion Index() aufruft erhalten die gleiche zwischengespeicherte Version die Indexansicht. Dies bedeutet, dass der Arbeitsaufwand, der der Webserver ausführen muss, um die Indexansicht dienen erheblich reduziert wird.

Die Ansicht im Codebeispiel 2 erfolgt in etwas ganz einfach auf diese Weise werden. Die Sicht werden nur die aktuelle Uhrzeit angezeigt. Allerdings können Sie genauso einfach Cache eine Sicht, eine Gruppe von Datenbankdatensätzen anzeigt. In diesem Fall müssten der Satz von Datenbankdatensätzen nicht jedes Mal aus der Datenbank abgerufen werden, wenn die Controlleraktion, die die Sicht zurück aufgerufen wird. Zwischenspeichern kann der verbleibende Arbeitsaufwand reduzieren, die sowohl den Webserver und Datenbankserver ausführen müssen.

Verwenden Sie die Seite nicht &lt;% @ OutputCache %&gt; -Direktive in einer MVC-Ansicht. Diese Richtlinie wird aus dem Web Forms hinüberlaufen und sollte nicht in einer ASP.NET MVC-Anwendung verwendet werden. 

#### <a name="where-content-is-cached"></a>Dem Inhalt zwischengespeichert werden

Standardmäßig wird bei der Verwendung der &lt;OutputCache&gt; Attribut Inhalt wird an drei Orten zwischengespeichert: der Webserver, alle Proxyserver und den Webbrowser. Sie können steuern, genau dem Inhalt zwischengespeichert werden durch Ändern der Location-Eigenschaft von der &lt;OutputCache&gt; Attribut.

Sie können die Location-Eigenschaft für jede der folgenden Werte festlegen:

> · Alle
> 
> · Client
> 
> · Downstreamserver
> 
> · Server
> 
> · Keine
> 
> · ServerAndClient


Die Location-Eigenschaft hat den Wert wird standardmäßig alle aus. Es gibt jedoch Situationen, in denen Sie Cache nur auf den Browser oder nur auf dem Server sollten. Z. B. wenn das Zwischenspeichern Informationen für jeden Benutzer persönlich zugeschnitten ist sollte dann Sie nicht die Informationen auf dem Server zwischengespeichert. Wenn Sie für verschiedene Benutzer unterschiedliche Informationen angezeigt werden, sollten Sie die Informationen nur auf dem Client Zwischenspeichern.

Der Controller im Codebeispiel 3 macht z. B. eine Aktion, die mit dem Namen GetName(), die den aktuellen Benutzernamen zurückgibt. Wenn bei der Website meldet und die GetName()-Aktion ruft gibt die Aktion die Zeichenfolge "Hi Stecker". Wenn anschließend Jill bei der Website meldet, und die Aktion GetName() Ruft erhalten sie auch die Zeichenfolge "Hi Stecker". Die Zeichenfolge wird auf dem Webserver für alle Benutzer nach Stecker anfänglich Controlleraktion ruft zwischengespeichert.

**3 – Controllers\BadUserController.vb auflisten**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Der Controller im Codebeispiel 3 funktioniert sehr wahrscheinlich nicht die Möglichkeit, die Sie möchten. Sie möchten nicht die Meldung "Hi Stecker" Jill anzuzeigen.

Sie sollten nie personalisierten Inhalt im Cache zwischengespeichert. Möglicherweise möchten jedoch den personalisierten Inhalt im Browsercache zur Verbesserung der Leistung Zwischenspeichern. Wenn Sie Inhalte im Browser Zwischenspeichern und ein Benutzer wird dieselbe Controlleraktion mehrmals aufgerufen, kann der Inhalt aus dem Browsercache anstatt am Server abgerufen werden.

Der geänderte Controller im Codebeispiel 4 wird die Ausgabe des GetName() zwischengespeichert. Der Inhalt wird jedoch nur auf den Browser und nicht auf dem Server zwischengespeichert. Auf diese Weise, wenn mehrere Benutzer die GetName()-Methode aufrufen, ruft jede Person den eigenen Benutzernamen und nicht in einer anderen Person Benutzernamen ab.

**4 – Controllers\UserController.vb auflisten**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Beachten Sie, dass die &lt;OutputCache&gt; -Attribut in der Liste 4 umfasst eine Location-Eigenschaft auf den Wert OutputCacheLocation.Client festgelegt. Die &lt;OutputCache&gt; Attribut enthält auch eine NoStore-Eigenschaft. Die NoStore-Eigenschaft wird verwendet, um Proxyservern und Browsern zu informieren, dass sie eine permanente Kopie des zwischengespeicherten Inhalts nicht gespeichert werden sollten.

#### <a name="varying-the-output-cache"></a>Den Ausgabecache VARYING

In einigen Fällen sollten Sie verschiedene zwischengespeicherte Versionen sehr derselbe Inhalt. Stellen Sie sich vor, z. B., dass Sie eine Master/Detail-Seite erstellt werden. Die Seite "master" zeigt eine Liste der Filmtitel. Wenn Sie einen Titel klicken, erhalten Sie Details für den ausgewählten Film an.

Wenn Sie die Seite "Details" Cache speichern, werden die Details für den gleichen Films unabhängig davon, welche Film Sie klicken angezeigt. Die ersten Films ausgewählt, der erste Benutzer wird für alle zukünftigen Benutzer angezeigt.

Sie können dieses Problem beheben, durch die Nutzung der VaryByParam-Eigenschaft der &lt;OutputCache&gt; Attribut. Diese Eigenschaft ermöglicht Ihnen, verschiedene zwischengespeicherte Versionen des Formulars Parameter selbe Inhalts erstellen oder Abfragezeichenfolgen-Parameter variiert.

Der Controller im Codebeispiel 5 stellt z. B. zwei Aktionen, die mit dem Namen Master() und Details(). Die Master()-Aktion gibt eine Liste der Filmtitel und die Details()-Aktion gibt die Details für den ausgewählten Film zurück.

**5 – Controllers\MoviesController.vb auflisten**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

Die Master()-Aktion enthält eine VaryByParam-Eigenschaft mit dem Wert "none". Werden zurückgegeben, wenn die Master() Aktion aufgerufen wird, wird die gleiche zwischengespeicherte Version des Master anzeigen. Alle Formularparameter oder eine Abfragezeichenfolge, die Parameter werden ignoriert (siehe Abbildung 2).

**Abbildung 2 – die /Movies/Master-Sicht**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Abbildung 3 – / Filme/Detailansicht**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

Die Details()-Aktion enthält eine VaryByParam-Eigenschaft mit dem Wert "Id". Wenn unterschiedliche Werte des Id-Parameters Controlleraktion übergeben werden, werden verschiedene zwischengespeicherte Versionen der Detailansicht generiert.

Es ist wichtig zu wissen, dass mit den Ergebnissen des VaryByParam-Eigenschaft in weitere Zwischenspeichern und nicht kleiner. Eine andere zwischengespeicherte Version der Detailansicht wird für jede einzelne verwendete Version, der den Id-Parameter erstellt.

Sie können die VaryByParam-Eigenschaft auf die folgenden Werte festgelegt:

> \* = Erstellen Sie eine andere zwischengespeicherte Version aus, wenn ein Formular oder Abfragezeichenfolgen-Parameters variieren.
> 
> None = nie verschiedene zwischengespeicherte Versionen erstellen
> 
> Durch Semikolons Liste von Parametern = verschiedene zwischengespeicherte Versionen erstellen, wenn keines der Zeichenfolgenparameter Formular oder eine Abfrage in der Liste ändert sich


#### <a name="creating-a-cache-profile"></a>Erstellen ein Cacheprofil

Als Alternative zum Konfigurieren von Eigenschaften der Ausgabespalten-Cache durch Ändern der Eigenschaften der &lt;OutputCache&gt; -Attribut, Sie können ein Cacheprofil in der Webkonfigurationsdatei (web.config) erstellen. Erstellen ein Cacheprofil in der Webkonfigurationsdatei bietet einige wichtige Vorteile.

Zunächst können durch Zwischenspeichern der Ausgabe in der Webkonfigurationsdatei konfigurieren, Sie steuern, wie Controlleraktionen Inhalt an einem zentralen Ort Zwischenspeichern. Sie können ein Cacheprofil erstellen und das Profil auf mehrere Controllern oder Controlleraktionen anwenden.

Zweitens können Sie die Webkonfigurationsdatei ändern, ohne Ihre Anwendung neu zu kompilieren. Wenn in diesem Fall müssen Sie eine deaktivieren Zwischenspeichern für eine Anwendung, die bereits in der produktionsumgebung bereitgestellt wurde, können Sie die Cacheprofile in der Webkonfigurationsdatei definiert einfach ändern. Alle Änderungen an der Webkonfigurationsdatei werden automatisch erkannt und angewendet.

Z. B. die &lt;zwischenspeichern&gt; Konfigurationsabschnitt auflisten 6 definiert ein Cacheprofil mit dem Namen Cache1Hour. Die &lt;caching&gt; Abschnitt angezeigt werden muss, in der &lt;system.web&gt; Abschnitt einer Web-Konfigurationsdatei.

**Auflisten von 6 – Caching-Abschnitt, um die Datei "Web.config"**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Der Controller im Codebeispiel 7 wird veranschaulicht, wie Sie das Profil Cache1Hour auf eine Controlleraktion mit anwenden können die &lt;OutputCache&gt; Attribut.

**Auflisten von 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Wenn Sie die Index()-Aktion, die von der Controller im Codebeispiel 7 verfügbar gemacht werden aufrufen, wird gleichzeitig für eine Stunde zurückgegeben.

#### <a name="summary"></a>Zusammenfassung

Ausgabe-caching bietet Ihnen eine sehr einfache Methode zum erhebliche Steigerung der Leistung Ihrer ASP.NET MVC-Anwendungen. In diesem Lernprogramm haben Sie gelernt, wie mithilfe der &lt;OutputCache&gt; Attribut, um die Ausgabe des Controlleraktionen zwischengespeichert. Außerdem haben Sie gelernt, Eigenschaften ändern die &lt;OutputCache&gt; Attribut wie die Duration "und" VaryByParam-Eigenschaften zu ändern, wie der Inhalt zwischengespeichert ruft. Schließlich haben Sie gelernt, wie Cacheprofile in der Konfigurationsdatei definiert.

> [!div class="step-by-step"]
> [Zurück](understanding-action-filters-vb.md)
> [Weiter](adding-dynamic-content-to-a-cached-page-vb.md)
