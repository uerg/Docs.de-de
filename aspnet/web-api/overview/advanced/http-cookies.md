---
uid: web-api/overview/advanced/http-cookies
title: HTTP-Cookies in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071628"
---
<a name="http-cookies-in-aspnet-web-api"></a>HTTP-Cookies in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird beschrieben, wie zum Senden und Empfangen von HTTP-Cookies in Web-API wird.

## <a name="background-on-http-cookies"></a>Hintergrundinformationen zu HTTP-Cookies

Dieser Abschnitt bietet einen kurzen Überblick darüber, wie Cookies auf HTTP-Ebene implementiert werden. Weitere Informationen finden Sie in [RFC 6265](http://tools.ietf.org/html/rfc6265).

Ein Cookie ist ein Teil der Daten, die ein Server in der HTTP-Antwort sendet. Der Client speichert das Cookie (optional) und wird für Anforderungen Subsequet zurückgegeben. Dies ermöglicht dem Client und Server, um Ihren Zustand freigeben. Um ein Cookie festlegen, enthält der Server einen Set-Cookie-Header in der Antwort. Das Format eines Cookies ist ein Name / Wert-Paar mit optionalen Attributen. Zum Beispiel:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Hier ist ein Beispiel mit Attributen:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Um ein Cookie an den Server zurückzugeben, fügt der Client einen Cookie-Header in späteren Anforderungen.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Eine HTTP-Antwort kann mehrere Set-Cookie-Header enthalten.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Der Client gibt mehrere Cookies, die mit einem einzelnen Cookieheader zurück.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Der Bereich und die Dauer eines Cookies werden durch die folgenden Attribute in der Set-Cookie-Header gesteuert:

- **Domäne**: teilt den Clientcomputern mit, welche Domäne des Cookies erhalten soll. Z. B. ein die Domäne "example.com" ist, gibt der Client das Cookie für jede Unterdomäne example.com zurück. Wenn nicht angegeben, ist die Domäne dem Ausgangsserver.
- **Pfad**: das Cookie an den angegebenen Pfad innerhalb der Domäne beschränkt. Wenn nicht angegeben ist, wird der Pfad der Anforderungs-URI verwendet.
- **Läuft ab**: Legt ein Ablaufdatum für das Cookie. Der Client löscht das Cookie an, wenn diese abläuft.
- **Max-Age**: Legt das maximale Alter für das Cookie. Der Client löscht das Cookie an, wenn sie das maximale Alter erreicht.

Wenn beide `Expires` und `Max-Age` festgelegt sind, `Max-Age` hat Vorrang vor. Wenn keiner festgelegt ist, löscht der Client das Cookie an, wenn die aktuelle Sitzung beendet. (Die genaue Bedeutung von "Session" wird durch den Benutzer-Agent bestimmt.)

Bedenken Sie jedoch, dass Clients Cookies ignoriert werden können. Beispielsweise kann ein Benutzer Cookies Gründen des Datenschutzes deaktivieren. Clients können Cookies zu löschen, bevor sie ablaufen, oder die Anzahl der Cookies gespeichert. Aus Gründen des Datenschutzes ablehnen Clients häufig "Fremdanbieter" Cookies, in der Domäne nicht den Ursprungsserver übereinstimmt. Kurz gesagt, sollten der Server nicht verlassen, zum Abrufen von Cookies, die er festlegt.

## <a name="cookies-in-web-api"></a>Cookies in Web-API

Um eine HTTP-Antwort einen Cookie hinzugefügt haben, erstellen Sie eine **CookieHeaderValue** Instanz, die das Cookie darstellt. Rufen Sie anschließend die **AddCookies** Erweiterungsmethode ist, definiert in der **System.Net.Http. HttpResponseHeadersExtensions** Klasse, um das Cookie hinzufügen.

Im folgende Code wird z. B. einen Cookie in eine Controlleraktion hinzugefügt:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Beachten Sie, dass **AddCookies** wird ein Array von **CookieHeaderValue** Instanzen.

Um die Cookies aus einer Clientanforderung zu extrahieren, rufen die **GetCookies** Methode:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Ein **CookieHeaderValue** enthält eine Auflistung von **CookieState** Instanzen. Jede **CookieState** ein Cookie darstellt. Verwenden Sie die Indexermethode zum Abrufen einer **CookieState** anhand des Namens, wie dargestellt.

## <a name="structured-cookie-data"></a>Strukturierte Cookiedaten

Viele Browser einschränken, wie viele Cookies sie speichern&#8212;sowohl die Gesamtzahl und die Anzahl pro Domäne. Aus diesem Grund kann es nützlich, um ein einzelnes Cookie, statt mehrere Cookies festzulegen strukturierte Daten abgelegt sein.

> [!NOTE]
> RFC 6265 werden keine Cookies Datenstruktur definiert.


Mithilfe der **CookieHeaderValue** -Klasse, Sie können eine Liste von Name / Wert-Paare für die Cookiedaten übergeben. Diese Name-Wert-Paare werden als URL-codiert-Daten in der Set-Cookie-Header codiert:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Der obige Code erzeugt die folgenden Set-Cookie-Headers:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

Die **CookieState** Klasse stellt eine Indexermethode, um die untergeordneten Werte aus einem Cookie in der Anforderungsnachricht zu lesen:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Beispiel: Festlegen und Abrufen von Cookies in einen Meldungshandler

In den vorherigen Beispielen wurde gezeigt, wie Cookies von innerhalb einer Web-API-Controller verwenden können. Eine andere Möglichkeit ist die Verwendung [message-Handler](http-message-handlers.md). Meldungshandler werden weiter oben in der Pipeline als Controller aufgerufen. Message-Handler kann Cookies aus der Anforderung zu lesen, bevor die Anforderung den Domänencontroller erreicht, oder Cookies an die Antwort hinzufügen, nachdem der Controller die Antwort generiert.

![](http-cookies/_static/image2.png)

Der folgende Code zeigt einen Meldungshandler für Sitzungs-IDs erstellen. Die Sitzungs-ID wird in einem Cookie gespeichert. Der Ereignishandler überprüft die Anforderung für das Sitzungscookie. Wenn die Anforderung nicht das Cookie enthält, generiert der Handler eine neue Sitzungs-ID. In beiden Fällen wird der Ereignishandler speichert die Sitzungs-ID in der **HttpRequestMessage.Properties** Eigenschaftenbehälter. Außerdem werden die HTTP-Antwort das Sitzungscookie hinzugefügt.

Diese Implementierung überprüft nicht, dass die Sitzungs-ID vom Client tatsächlich vom Server ausgestellt wurde. Verwenden Sie nicht als eine Art der Authentifizierung! Zum Zeitpunkt des im Beispiel werden HTTP-cookieverwaltung anzeigen.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Ein Controller erhalten die Sitzungs-ID aus der **HttpRequestMessage.Properties** Eigenschaftenbehälter.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
