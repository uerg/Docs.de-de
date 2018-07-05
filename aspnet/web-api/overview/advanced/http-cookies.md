---
uid: web-api/overview/advanced/http-cookies
title: HTTP-Cookies in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 21ba186c11f39bbeedd1c320b98476ba13af27e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819159"
---
<a name="http-cookies-in-aspnet-web-api"></a>HTTP-Cookies in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird beschrieben, wie zum Senden und Empfangen von HTTP-Cookies in Web-API wird.

## <a name="background-on-http-cookies"></a>Hintergrundinformationen zu HTTP-Cookies

Dieser Abschnitt enthält eine kurze Übersicht darüber, wie Cookies auf HTTP-Ebene implementiert werden. Weitere Informationen finden Sie in [RFC 6265](http://tools.ietf.org/html/rfc6265).

Ein Cookie ist ein Teil der Daten, die ein Server in der HTTP-Antwort sendet. Der Client speichert das Cookie (optional) und wird für Subsequet-Anforderungen zurückgegeben. Dadurch wird dem Client und Server aus möglich. Um ein Cookie festlegen, enthält der Server einen Set-Cookie-Header in der Antwort. Das Format eines Cookies ist ein Name / Wert-Paar, mit optionalen Attributen. Zum Beispiel:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Hier ist ein Beispiel mit Attributen:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Um ein Cookie an den Server zurückzugeben, fügt der Client einen Cookie-Header in höhere Anforderungen.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Eine HTTP-Antwort kann mehrere Set-Cookie-Header enthalten.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Der Client gibt mehrere Cookies, die mit einem einzelnen Cookieheader zurück.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Umfang und Dauer eines Cookies werden durch die folgenden Attribute im Set-Cookie-Header gesteuert:

- **Domäne**: teilt dem Client mit der Domäne des Cookies erhalten soll. Z. B. wenn die Domäne "Beispiel.com" ist, gibt der Client jede Unterdomäne von "example.com" das Cookie zurück. Wenn nicht angegeben, ist die Domäne dem Ursprungsserver.
- **Pfad**: das Cookie für den angegebenen Pfad innerhalb der Domäne beschränkt. Wenn nicht angegeben, wird der Pfad des Anforderungs-URI verwendet.
- **Läuft ab**: Legt ein Ablaufdatum für das Cookie fest. Der Client löscht das Cookie an, nach dessen Ablauf.
- **Max-Age**: Legt das maximale Alter für das Cookie. Der Client löscht den Cookie, wenn sie das maximale Alter erreicht.

Wenn beide `Expires` und `Max-Age` festgelegt sind, `Max-Age` hat Vorrang vor. Wenn keines von beiden festgelegt ist, löscht der Client das Cookie an, wenn die aktuelle Sitzung beendet. (Die genaue Bedeutung von "Session" richtet sich nach der Benutzer-Agent.)

Bedenken Sie jedoch, dass Clients möglicherweise Cookies ignoriert werden. Beispielsweise kann ein Benutzer die Cookies aus Datenschutzgründen deaktivieren. Clients können Cookies löschen, bevor sie ablaufen, oder die Anzahl der Cookies gespeichert. Aus Gründen des Datenschutzes ablehnen Clients häufig "Fremdanbieter" Cookies, in dem die Domäne nicht mit den Ursprungsserver übereinstimmt. Kurz gesagt: der Server nicht empfehlenswert Rückkehr die Cookies, die festgelegt.

## <a name="cookies-in-web-api"></a>Cookies in Web-API

Um eine HTTP-Antwort einen Cookie hinzugefügt haben, erstellen Sie eine **CookieHeaderValue** Instanz, die das Cookie darstellt. Rufen Sie dann die **AddCookies** Erweiterungsmethode, die in definierten die **System.Net.Http. HttpResponseHeadersExtensions** -Klasse, um das Cookie hinzuzufügen.

Der folgende Code fügt z. B. einen Cookie in eine Controlleraktion:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Beachten Sie, dass **AddCookies** akzeptiert ein Array von **CookieHeaderValue** Instanzen.

Um die Cookies aus eine Anforderung des Clients zu extrahieren, rufen die **GetCookies** Methode:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Ein **CookieHeaderValue** enthält eine Auflistung von **CookieState** Instanzen. Jede **CookieState** ein Cookie darstellt. Verwenden Sie die Indexermethode zum Abrufen einer **CookieState** anhand des Namens, wie gezeigt.

## <a name="structured-cookie-data"></a>Strukturierte Cookiedaten

Viele Browser einschränken, wie viele Cookies, die sie speichert&#8212;sowohl die Gesamtanzahl und die Anzahl der pro Domäne. Aus diesem Grund kann es nützlich, um strukturierte Daten in ein einzelnes Cookie, statt mehrere Cookies gespeichert sein.

> [!NOTE]
> RFC 6265 definiert nicht die Struktur der Cookiedaten.


Mithilfe der **CookieHeaderValue** -Klasse, können Sie eine Liste von Name / Wert-Paare für die Cookiedaten übergeben. Diese Name / Wert-Paare werden als URL-codierte Form der Daten in der Set-Cookie-Header codiert:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Der obige Code führt die folgenden Set-Cookie-Header:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

Die **CookieState** Klasse enthält eine Indexermethode, um die untergeordneten Werte von einem Cookie in der Anforderungsnachricht zu lesen:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Beispiel: Festlegen und Abrufen von Cookies in einen Meldungshandler

In den vorherigen Beispielen wurde gezeigt, wie Cookies aus einem Web-API-Controller verwendet wird. Eine weitere Möglichkeit ist die Verwendung [message-Handler](http-message-handlers.md). Meldungshandler werden weiter oben in der Pipeline als Controller aufgerufen. Ein Meldungshandler kann Cookies aus der Anforderung zu lesen, bevor die Anforderung den Controller erreichen oder Cookies an die Antwort hinzufügen, nachdem der Controller die Antwort generiert.

![](http-cookies/_static/image2.png)

Der folgende Code zeigt einen Meldungshandler für Sitzungs-IDs erstellen. Die Sitzungs-ID wird in einem Cookie gespeichert. Der Ereignishandler überprüft die Anforderung für das Sitzungscookie. Wenn das Cookie in die Anforderung nicht enthalten ist, wird der Handler eine neuen Sitzungs-ID generiert. In beiden Fällen speichert der Handler für die Sitzungs-ID in der **HttpRequestMessage.Properties** Eigenschaftenbehälter. Außerdem werden die HTTP-Antwort das Sitzungscookie hinzugefügt.

Diese Implementierung überprüft nicht, dass die Sitzungs-ID vom Client tatsächlich vom Server ausgestellt wurde. Verwenden Sie nicht als eine Form der Authentifizierung. Zum Zeitpunkt des im Beispiel wird HTTP-Cookie Management erläutert.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Ein Controller erhalten die Sitzungs-ID aus der **HttpRequestMessage.Properties** Eigenschaftenbehälter.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
