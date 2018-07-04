---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Zwischenspeichern von Daten in einer ASP.NET Web-Website (Razor) für eine bessere Leistung von Seiten | Microsoft-Dokumentation
author: tfitzmac
description: Sie können beschleunigen, Ihre Website, dass – d. h. Speichern von Cache – die Ergebnisse der Daten, die normalerweise dauern würde sehr viel Zeit zum Abrufen oder Verarbeiten einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 4134c80d7eed4752c90a06aab796a0fd8c2a9782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383408"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Zwischenspeichern von Daten in einer ASP.NET Web Pages (Razor)-Website für eine bessere Leistung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie eine Hilfsprogramm, mit dem Cache-Informationen für eine bessere Leistung auf einer Website für ASP.NET Web Pages (Razor) verwendet wird. Sie können Ihre Website, dass speichern beschleunigen &#8212; zwischenspeichern, d. h. &#8212; die Ergebnisse der Daten, die normalerweise würde erhebliche Zeit in Anspruch nehmen abrufen oder verarbeiten, die nicht häufig ändern.
> 
> **Sie lernen Folgendes:** 
> 
> - Informationen zur Verwendung von caching zur Verbesserung der Reaktionsfähigkeit Ihrer Website.
> 
> Dies sind die Funktionen von ASP.NET in diesem Artikel:
> 
> - Die `WebCache` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


Jedes Mal, wenn ein Benutzer auf Ihrer Website eine Seite anfordert, muss der Webserver einiges zu tun, um die Anforderung zu erfüllen. Der Server möglicherweise für einige Ihrer Seiten Aufgaben, die ein (relativ) wie das Abrufen von Daten aus einer Datenbank dauern. Auch wenn diese Aufgaben lange in absoluten Ausdrücken erfordern, wenn Ihre Website viel Datenverkehr auftritt, kann eine ganze Reihe von einzelnen Anforderungen, die dazu führen, den Webserver dass für die langsam oder komplizierte Aufgabe bis zu sehr viel Arbeit hinzufügen. Dies kann letztlich die Leistung der Website beeinträchtigen.

Eine Möglichkeit zur Verbesserung der Leistung Ihrer Website unter Umständen wie folgt, ist zum Zwischenspeichern von Daten. Wenn Ihre Website wiederholte Anforderungen für dieselben Informationen angezeigt Ruft, die Informationen muss nicht für jede Person, die geändert werden und kann nicht Zeit vertraulich, anstatt erneut abgerufen oder neu zu berechnen, können die Daten einmal abrufen und speichern Sie die Ergebnisse. Das nächste Mal, das eine Anforderung für diesen eingeht, machen Informationen Sie sich einfach sie aus dem Cache.

Im Allgemeinen Zwischenspeichern Sie Informationen, die nicht häufig ändern. Wenn Sie die Informationen im Cache ablegen, wird es im Arbeitsspeicher auf dem Webserver gespeichert. Sie können angeben, wie lange es, von Sekunden auf Tage zwischengespeichert werden soll. Nach Ablauf des Zeitraums der Zwischenspeicherung ist die Informationen automatisch aus dem Cache entfernt.

> [!NOTE]
> Einträge im Cache möglicherweise Gründen außer entfernt, die abgelaufen sind. Beispielsweise der Webserver kann vorübergehend führen mit niedriger Arbeitsspeicher, und eine Möglichkeit, die er Speicher freigeben kann, ist durch das Auslösen von Einträgen aus dem Cache. Sie sehen auch, wenn Sie die Informationen in den Cache eingefügt haben, müssen Sie sicherstellen, dass sie immer noch vorhanden ist, wenn Sie sie benötigen.


Angenommen Sie, Ihre Website eine Seite enthält, die die aktuelle Temperatur und die Wettervorhersage anzeigt. Um diese Art von Informationen zu erhalten, können Sie eine Anforderung an einen externen Dienst senden. Da diese Informationen nicht viel (innerhalb eines zweistündigen Zeitraums, z. B.) ändert, und da externer Aufrufe Zeit und Bandbreite erforderlich ist, ist es ein guter Kandidat für die Zwischenspeicherung.

## <a name="adding-caching-to-a-page"></a>Hinzufügen von Caching zu einer Seite

ASP.NET umfasst eine `WebCache` Hilfsmethode, die Zwischenspeicherung auf Ihrer Website hinzufügen und Hinzufügen von Daten in den Cache erleichtert. In diesem Verfahren erstellen Sie eine Seite, die die aktuelle Zeit zwischenspeichert. Dies ist ein praktisches Beispiel, nicht, da die aktuelle Uhrzeit etwas ist, das häufig ändert, und, die darüber hinaus ist nicht komplex zu berechnen. Es ist jedoch eine gute Möglichkeit zur Zwischenspeicherung in Aktion zu veranschaulichen.

1. Hinzufügen einer neuen Seite, die mit dem Namen *WebCache.cshtml* auf der Website.
2. Im folgenden Code und Markup der Seite hinzufügen:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Wenn Sie die Daten zwischenspeichern, fügen Sie es den Cache unter Verwendung eines Namens Dies ist für die Website eindeutig. In diesem Fall verwenden Sie einen Cacheeintrag mit dem Namen `CachedTime`. Dies ist die `cacheItemKey` im Codebeispiel gezeigt.

    Der Code zuerst liest die `CachedTime` des Cacheeintrags. Wenn ein Wert zurückgegeben wird (d.h., wenn der Cacheeintrag nicht null ist), legt der Code den Wert der Zeitvariablen nur die Daten im Cache fest.

    Aber wenn der Cacheeintrag nicht vorhanden ist (d. h. es ist null), der Code legt den Time-Wert, fügt es dem Cache hinzu und einem Ablaufwert auf eine Minute. Nach einer Minute wird der Cacheeintrag verworfen. (Der Standardwert für die Ablaufzeit für ein Element im Cache ist 20 Minuten.) Der Befehl `WebCache.Set(cacheItemKey, time, 1, false)` zeigt, wie der Cache den aktuellen Wert der Zeit hinzu, und die Ablaufzeit auf 1 Minute festgelegt. Festlegen der *SlidingExpiration* Parameter `false` bedeutet, dass die Ablaufzeit wird nicht jedes Mal, die sie angefordert wird verlängert. Läuft es genau 1 Minute, nachdem er ursprünglich mit dem Cache hinzugefügt wurde. Wenn Sie diesen Wert, um festlegen `true` die Ablaufzeit für eine Minute wird jedes Mal zurückgesetzt, der Wert aus dem Cache angefordert wird.

    Dieser Code veranschaulicht das Muster, das Sie beim Zwischenspeichern von Daten immer verwenden sollten. Bevor Sie etwas aus dem Cache erhalten, überprüfen Sie immer zuerst, ob die `WebCache.Get` Methode hat null zurückgegeben. Denken Sie daran, dass der Cacheeintrag möglicherweise abgelaufen oder möglicherweise einem anderen Grund, entfernt wurde sodass jeder Eintrag garantiert nie im Cache ist.
3. Führen Sie *WebCache.cshtml* in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Beim ersten der Seite abrufen, die Time-Daten nicht im Cache sind, und der Code muss in der Time-Werten in den Cache hinzufügen.

    ![Cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aktualisieren Sie *WebCache.cshtml* im Browser. Diesmal ist die Zeitdaten im Cache. Beachten Sie, dass die Zeit seit dem letzten geändert hat, Sie auf die Seite angezeigt.

    ![Cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Warten Sie eine Minute, bis der Cache geleert werden, und klicken Sie dann aktualisieren Sie die Seite. Die Seite gibt erneut an, dass die Zeitdaten wurde nicht im Cache gefunden, und die Uhrzeit der Aktualisierung wird aus dem Cache hinzugefügt.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Anzeigen von Daten in einem Diagramm](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache-API-Referenz](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
