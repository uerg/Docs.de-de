---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Die Überwachungsinformationen Besucher (Analytics) für eine ASP.NET-Webseiten (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: Nachdem Sie Ihre Website jetzt gelangt sind, empfiehlt es sich um Ihre Website-Datenverkehr zu analysieren.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: aabe3177ba9479bfafafe81e1ea99a58f29d5271
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828912"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Überwachungsinformationen Besucher (Analytics) für eine ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm, um Seiten auf einer Website für ASP.NET Web Pages (Razor) websiteanalyse hinzuzufügen.
> 
> Sie lernen Folgendes:
> 
> - Wie Sie Informationen zu Ihrer Website-Datenverkehr an einen analyseanbieter zu senden.
> 
> Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Funktionen dar:
> 
> - Die `Analytics` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (NuGet-Paket)


Analytics ist ein allgemeiner Begriff für die Technologie, die Datenverkehr auf Ihrer Website misst, damit Sie nachvollziehen können, wie Benutzer die Site verwenden. Viele Analytics-Dienste sind verfügbar, einschließlich Google, Yahoo, StatCounter und andere Dienste.

Die Möglichkeit Analyse funktioniert liegt darin, dass Sie sich für ein Konto mit dem Analytics-Anbieter registrieren, in dem Sie die Website registrieren, die Sie möchten nachverfolgen. Der Anbieter sendet Ihnen einen Ausschnitt des JavaScript-Code, der eine ID oder das Verfolgen von Code für Ihr Konto enthält. Die Webseiten auf der Website, die Sie nachverfolgen möchten, hinzugefügt den JavaScript-Codeausschnitt. (Hinzugefügt in der Regel den Analytics-Codeausschnitt eine Fußzeile oder das Layout der Seite oder andere HTML-Markup, das auf jeder Seite auf Ihrer Website angezeigt wird.) Wenn Benutzer eine Seite, die eine der folgenden JavaScript-Codeausschnitt enthält anfordern, sendet der Codeausschnitt Informationen über die aktuelle Seite auf den Analytics-Anbieter, der verschiedenen Details über die Seite zeichnet.

Wenn Sie einen Blick auf Ihre Website-Statistiken möchten, melden Sie sich in der Analyse Website des Anbieters. Sie können dann wie alle Arten von Berichten zu Ihrer Website anzeigen:

- Die Anzahl der Seitenaufrufe für einzelne Seiten. Dadurch sehen Sie (ungefähr) wie viele Personen die Website besuchen und welche Seiten auf Ihrer Website am häufigsten genutzt werden.
- Wie viel Zeit verbringen Benutzer auf bestimmte Seiten ein. Dies kann Ihnen sagen z.B. gibt an, ob Ihre Startseite geweckt Interesse beibehalten wird.
- Welche Websites Benutzer waren auf, bevor sie Ihre Website besucht. Dadurch können Sie ermitteln, ob Links von Suchvorgängen und So weiter Ihren Datenverkehr stammt.
- Wenn Benutzer Ihre Site besuchen, und wie lange sie bleiben.
- Welchen Ländern stammen die Besucher Ihrer aus.
- Welche Browser und Betriebssysteme die Besucher Ihrer verwenden.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Verwenden eines Hilfsprogramms Analytics zu einer Seite hinzufügen

ASP.NET Web Pages enthält mehrere Analytics-Hilfsprogramme (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, und `Analytics.GetStatCounterHtml`), mit denen die JavaScript-Codeausschnitte, die verwendet werden, für die Analyse zu verwalten. Statt herauszufinden, wie und wo platzieren Sie den JavaScript-Code müssen Sie lediglich das Hilfsobjekt zu einer Seite hinzufügen. Die einzige Informationen, die Sie bereitstellen müssen ist Ihr Kontoname, ID oder Nachverfolgungscode. (Für StatCounter müssen Sie auch einige zusätzliche Werte angeben.)

In diesem Verfahren erstellen Sie eine Layoutseite, verwendet der `GetGoogleHtml` Helper. Wenn Sie bereits über ein Konto mit einem der anderen analyseanbieter verfügen, können Sie verwenden Sie stattdessen dieses Konto und geringfügige Anpassungen vornehmen, je nach Bedarf.

> [!NOTE]
> Wenn Sie ein Analytics-Konto erstellen, registrieren Sie die URL der Website, die nachverfolgt werden sollen. Wenn Sie alles, was auf dem lokalen Computer testen, Sie wird nicht nachverfolgt werden tatsächliche Datenverkehr (nur der Datenverkehr ist Sie), damit Sie nicht aufzeichnen und Anzeigen von Websitestatistiken können. Aber dieses Verfahren zeigt, wie Sie eine Analytics-Hilfe zu einer Seite hinzufügen. Wenn Sie Ihre Website veröffentlichen, sendet die live-Website Informationen an Ihren analyseanbieter für die.


1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), falls Sie bereits hinzugefügt haben.
2. Erstellen Sie ein Konto mit Google Analytics, und notieren Sie den Kontonamen.
3. Erstellen Sie eine Layoutseite, die mit dem Namen *Analytics.cshtml* und fügen Sie das folgende Markup hinzu:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Müssen Sie den Aufruf zum Platzieren der `Analytics` Hilfsprogramm im Text einer Webseite (vor der `</body>` Tag). Andernfalls wird der Browser nicht das Skript ausgeführt.

    Wenn Sie einen anderen Analytics-Anbieter verwenden, verwenden Sie eine der folgenden Helfer stattdessen:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Ersetzen Sie dies `myaccount` mit dem Namen des Kontos, ID oder Nachverfolgungscode, die Sie in Schritt 1 erstellt haben.
5. Führen Sie die Seite im Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
6. Zeigen Sie den Quellcode der Seite im Browser. Sie werden den gerenderten Analytics-Code zu sehen:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Melden Sie sich auf der Website von Google Analytics, und überprüfen Sie die Statistiken für Ihre Website. Wenn Sie die Seite auf einer live-Website ausführen, sehen Sie einen Eintrag, der den Besuch auf Ihrer Seite protokolliert.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Google Analytics-Website](https://www.google.com/analytics/)
- [Yahoo! Web Analytics-Website](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter-Website](http://statcounter.com/)
