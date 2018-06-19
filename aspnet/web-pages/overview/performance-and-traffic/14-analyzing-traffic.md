---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Die Überwachungsinformationen Besucher (Analytics) für eine ASP.NET-Webseiten (Razor) Standort | Microsoft Docs
author: tfitzmac
description: Nachdem Sie Ihre Website über gelangt sind, empfiehlt es sich um Website-Datenverkehr zu analysieren.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528759"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Überwachungsinformationen Besucher (Analytics) für eine ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie mithilfe ein Hilfsprogramms Website Analytics Seiten auf einer Website für ASP.NET Web Pages (Razor) hinzugefügt wird.
> 
> Lernen Sie:
> 
> - Informationen zum Senden von Informationen zu Ihrer Website-Datenverkehr an einen analyseanbieter.
> 
> Hierbei handelt es sich um den ASP.NET Programmieren von Funktionen, die im Artikel:
> 
> - Die `Analytics` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (NuGet-Paket)


Analytics ist ein allgemeiner Begriff für Technologie, die Datenverkehr auf Ihrer Website misst, damit Sie verstehen können, wie Benutzer die Site verwenden. Viele Analytics Services sind verfügbar, einschließlich der Dienste von Google, Yahoo und StatCounter.

Die Möglichkeit Analytics Works liegt darin, dass Sie sich für ein Konto mit der analyseanbieter registrieren, wo Sie die Website registrieren, die Sie möchten nachverfolgen. Der Anbieter sendet Ihnen einen Ausschnitt des JavaScript-Code, der eine ID oder das Nachverfolgen von Code für Ihr Konto enthält. Die Webseiten, auf die Website, die Sie nachverfolgen möchten hinzugefügt den JavaScript-Ausschnitt. (Fügen Sie üblicherweise in den Ausschnitt Analytics eine Fußzeile oder das Layout der Seite oder andere HTML-Markup, das auf jeder Seite an Ihrem Standort angezeigt wird.) Wenn Benutzer eine Seite, die einen dieser JavaScript-Ausschnitte enthält anfordern, sendet der Ausschnitt Informationen über die aktuelle Seite an die analyseanbieter, wer die verschiedenen Details über die Seite zeichnet.

Wenn Sie einen Blick auf Ihre Website-Statistiken verwenden möchten, melden Sie sich bei der analyseanbieter-Website. Sie können dann wie alle Arten von Berichten zu Ihrer Website anzeigen:

- Die Anzahl der Seitenaufrufe für einzelne Seiten. Dadurch können Sie ablesen (ungefähr), wie viele Personen die Site besuchen, und die Seiten auf Ihrer Website am häufigsten sind.
- Wie lange Personen auf bestimmte Seiten verbringen. Dadurch können Sie erkennen Dinge gibt an, ob Ihre Startseite Personen zu überwachende beibehalten wird.
- Welche Standorte Personen wurden auf, bevor sie Ihre Website besucht hat. Dadurch können Sie wissen, ob der Datenverkehr von Verknüpfungen, von Suchvorgängen usw. stammt.
- Wenn Personen Ihre Site besuchen, und wie lange sie bleiben.
- Welche Länder die Besucher Ihrer sind.
- Welche Browser und Betriebssysteme Besucher Ihrer verwenden.

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Verwenden eine Hilfsmethode Analytics zu einer Seite hinzufügen

ASP.NET Web Pages enthält mehrere Analytics-Hilfsprogramme (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, und `Analytics.GetStatCounterHtml`), die erleichtern das Verwalten der JavaScript-Ausschnitte, die für die Analyse verwendet. Statt zu ermitteln, wie und wo versetzen Sie den JavaScript-Code müssen Sie lediglich lediglich das Hilfsobjekt zu einer Seite hinzufügen. Die einzige Informationen, die Sie bereitstellen müssen ist der Kontoname, ID oder Nachverfolgungscode. (Für StatCounter müssen Sie auch einige zusätzliche Werte angeben.)

In diesem Verfahren erstellen Sie eine Layoutseite, verwendet die `GetGoogleHtml` Helper. Wenn Sie bereits über ein Konto mit einem anderen Anbieter Analytics haben, können Sie verwenden Sie stattdessen dieses Konto und geringfügige Anpassungen vornehmen, nach Bedarf.

> [!NOTE]
> Wenn Sie eine Analytics-Konto erstellen, registrieren Sie die URL der Site, die nachverfolgt werden sollen. Wenn Sie alles, was auf dem lokalen Computer testen, Sie wird nicht nachverfolgt werden tatsächlichen Datenverkehr (nur Datenverkehr ist Sie), damit Sie Website aufzeichnen und Anzeigen von Statistiken nicht. Aber diese Prozedur zeigt, wie Sie eine Analyse Hilfe zu einer Seite hinzufügen. Wenn Sie Ihre Website veröffentlichen, sendet live-Standort Informationen an Ihren analyseanbieter für die.


1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), wenn Sie bereits hinzugefügt haben.
2. Erstellen Sie ein Konto mit Google Analytics, und notieren Sie den Kontonamen.
3. Erstellen eine Layoutseite mit dem Namen *Analytics.cshtml* und fügen Sie das folgende Markup hinzu:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Richten Sie den Aufruf der `Analytics` Helper im Text einer Webseite (vor der `</body>` Tag). Andernfalls wird der Browser das Skript nicht ausgeführt.

    Wenn Sie einen anderen analyseanbieter verwenden, verwenden Sie eine der folgenden Helfer stattdessen:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Ersetzen Sie `myaccount` durch den Namen des der Konto-ID bzw. Nachverfolgungscode, die Sie in Schritt 1 erstellt haben.
5. Führen Sie die Seite im Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
6. Zeigen Sie im Browser die Seitenquelle. Sie müssen den gerenderten Analytics-Code zu sehen sein:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Melden Sie sich auf der Google Analytics-Website, und untersuchen Sie die Statistiken für Ihre Website. Wenn Sie die Seite auf einer live-Website ausführen, sehen Sie einen Eintrag, der den Besuch der Seite protokolliert.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Google Analytics-Website](https://www.google.com/analytics/)
- [Yahoo! Analytics-Website](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter-Website](http://statcounter.com/)
