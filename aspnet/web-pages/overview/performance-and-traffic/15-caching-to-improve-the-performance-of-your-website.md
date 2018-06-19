---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Zwischenspeichern von Daten in einer ASP.NET Web-Seiten (Razor) Standort für eine bessere Leistung | Microsoft Docs
author: tfitzmac
description: Sie können beschleunigte Websites durch Speichern – d. h. dass Cache - Daten, die normalerweise würde sehr lange dauern abrufen oder Verarbeiten der Ergebnisse einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039195"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Zwischenspeichern von Daten an einem Standort der ASP.NET Web Pages (Razor) für eine bessere Leistung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie eine Hilfsmethode zum Cacheinformationen für eine bessere Leistung in einer Website für ASP.NET Web Pages (Razor) verwendet wird. Sie können Ihrer Website beschleunigen, indem Sie ihm Store &#8212; d. h. Cache &#8212; die Ergebnisse, die normalerweise in Anspruch nehmen würde sehr viel Zeit zum Abrufen oder verarbeiten und die Daten werden nicht häufig geändert.
> 
> **Lernen Sie:** 
> 
> - Wie Zwischenspeicherung verwenden, um die Reaktionsfähigkeit Ihrer Website zu verbessern.
> 
> Dies sind die Funktionen von ASP.NET im Artikel:
> 
> - Die `WebCache` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


Jedes Mal, wenn ein Benutzer eine Seite über Ihren Standort anfordert, muss der Webserver einige arbeiten zu erledigen, um die Anforderung zu erfüllen. Der Server u. für einige der Seiten um Aufgaben auszuführen, die (vergleichsweise), wie z. B. das Abrufen von Daten aus einer Datenbank dauert. Auch wenn diese Aufgaben in absoluten Ausdrücken lang erfordern, wenn Ihre Website viel Datenverkehr auftritt, kann eine ganze Reihe von einzelnen Anforderungen, die dazu führen, der Webserver zum Ausführen der Aufgabe langsamen oder komplizierten dass zu viel Arbeit summieren. Dies kann letztendlich die Leistung des Standorts auswirken.

Eine Möglichkeit zum Verbessern der Leistung Ihrer Website unter Umständen wie folgt ist zum Zwischenspeichern von Daten. Wenn Ihre Website wiederholte Anforderungen für die gleichen Informationen ruft müssen die Informationen nicht für jede Person geändert werden und es nicht Zeit ist beachtet wird, anstatt erneut abrufen oder Neuberechnen, können die Daten einmal abrufen und speichern Sie die Ergebnisse. Das nächste Mal, das eine Anforderung für diesen eingeht, herunterladen Informationen Sie einfach aus dem Cache.

Im Allgemeinen Zwischenspeichern Sie Informationen, die nicht häufig ändern. Wenn Sie Informationen im Cache ablegen, wird es im Arbeitsspeicher auf dem Webserver gespeichert. Sie können angeben, wie lange es, von Sekunden auf Tage zwischengespeichert werden soll. Ablauf des Zeitraums der Zwischenspeicherung wird die Informationen automatisch aus dem Cache entfernt.

> [!NOTE]
> Einträge im Cache möglicherweise Gründen außer entfernt, die abgelaufen sind. Beispielsweise der Webserver möglicherweise vorübergehend viel Arbeitsspeicher, und eine Möglichkeit, die es Arbeitsspeicher freigeben kann, ist durch das Auslösen von Einträgen aus dem Cache. Wie Sie sehen, auch wenn Sie die Informationen in den Cache gesteckt haben, müssen Sie sicherstellen, dass sie immer noch vorhanden ist, bei Bedarf.


Angenommen Sie, Ihre Website eine Seite ist, die die aktuelle Temperatur und Wettervorhersage anzeigt. Um diese Art von Informationen zu erhalten, können Sie eine Anforderung an einen externen Dienst senden. Da diese Informationen nicht viel (innerhalb eines zweistündigen Zeitraums, z. B.) ändern und externe Aufrufe Zeit und Bandbreite erfordern, ist es ein guter Kandidat für das caching.

## <a name="adding-caching-to-a-page"></a>Hinzufügen von Caching zu einer Seite

ASP.NET umfasst einen `WebCache` -Hilfsprogramm, das erleichtert das caching auf Ihrer Website hinzufügen und den Cache Daten hinzuzufügen. In diesem Verfahren erstellen Sie eine Seite, die die aktuelle Zeit zwischenspeichert. Dies ist ein praktisches Beispiel nicht, da die aktuelle Uhrzeit etwas, das häufig ändert, und, ist, die darüber hinaus ist nicht komplex zum Berechnen. Es ist jedoch eine gute Möglichkeit zum Zwischenspeichern in Aktion zu veranschaulichen.

1. Fügen Sie eine neue Seite mit dem Namen *WebCache.cshtml* auf der Website.
2. Der folgende Code und Markup der Seite hinzufügen:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Wenn Sie Daten zwischenzuspeichern, Sie ihn im Cache abgelegt unter Verwendung eines Namens Dies ist eindeutig innerhalb der Website. In diesem Fall verwenden Sie einen Cacheeintrag, der mit dem Namen `CachedTime`. Dies ist die `cacheItemKey` im Codebeispiel gezeigt.

    Der Code zunächst liest die `CachedTime` des Cacheeintrags. Wenn ein Wert zurückgegeben wird (d. h., wenn der Cacheeintrag nicht null ist), legt der Code einfach den Wert der Variablen Zeit auf das Zwischenspeichern von Daten fest.

    Jedoch, wenn der Cacheeintrag nicht vorhanden ist (d. h. er null ist), der Code legt das Time-Werten, dem Cache hinzugefügt und einem Ablaufwert auf eine Minute. Nach einer Minute wird der Cacheeintrag verworfen. (Der Standardwert für die Ablaufzeit für ein Element im Cache ist 20 Minuten.) Der Befehl `WebCache.Set(cacheItemKey, time, 1, false)` wird gezeigt, wie der Cache den aktuellen Zeitwert hinzu, und legen Sie dessen Ablauf auf 1 Minute. Festlegen der *SlidingExpiration* Parameter `false` bedeutet, dass die Ablaufzeit wird nicht jedes Mal, die sie angefordert wird erneuert. Es läuft genau 1 Minute, nachdem sie ursprünglich mit dem Cache hinzugefügt wurde. Wenn Sie diesen Wert, um festlegen `true` die Ablaufzeit von einer Minute wird jedes Mal zurückgesetzt, der Wert aus dem Cache angefordert wird.

    Dieser Code veranschaulicht das Muster, das Sie beim Zwischenspeichern von Daten immer verwenden soll. Bevor Sie ein Element aus dem Cache erhalten, überprüfen Sie zunächst immer, ob die `WebCache.Get` Methode hat null zurückgegeben. Denken Sie daran, dass der Cacheeintrag möglicherweise abgelaufen oder möglicherweise einem anderen Grund entfernt wurde, ein bestimmten Eintrag garantiert nie im Cache befinden.
3. Führen Sie *WebCache.cshtml* in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Beim ersten Anforderung von der Seite die Zeitdaten befindet sich nicht im Cache und der Code muss in der Time-Werten in den Cache hinzufügen.

    ![Cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aktualisieren Sie *WebCache.cshtml* im Browser. Diesmal ist die Zeitdaten im Cache. Beachten Sie, dass die Zeit seit der letzten Ausführung geändert hat, Sie auf die Seite angezeigt.

    ![Cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Warten Sie einen Moment der Zwischenspeicher geleert werden, und aktualisieren Sie die Seite. Die Seite gibt erneut an, dass die Uhrzeitdaten wurde nicht im Cache gefunden, und die aktualisierte Zeit zum Cache hinzugefügt wird.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Anzeigen von Daten in einem Diagramm](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache-API-Referenz](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
