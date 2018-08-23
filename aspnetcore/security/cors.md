---
title: Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie CORS als Standard zum Zulassen oder ablehnen von ursprungsübergreifenden Anforderungen in einer ASP.NET Core-app.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "41839039"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core

Durch [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), und [Tom Dykstra](https://github.com/tdykstra)

Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen in eine andere Domäne. Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*, und verhindert, dass eine schädliche Website sensible Daten von einer anderen Website liest. Aber möchten manchmal andere Standorte, die Cross-Origin-Anforderungen an Ihre Web-API senden können.

[Cross-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht. Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen. CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](https://wikipedia.org/wiki/JSONP). In diesem Thema veranschaulicht das Aktivieren von CORS in ASP.NET Core-Anwendungen.

## <a name="what-is-same-origin"></a>Was ist "denselben Ursprung"?

Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Diese zwei URLs haben denselben Ursprung an:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Diese URLs haben verschiedene Ursprünge als die vorherige zwei:

* `http://example.net` -Andere Domäne

* `http://www.example.com/foo.html` -Andere Unterdomäne

* `https://example.com/foo.html` -Andere Partitionsschema

* `http://example.com:9000/foo.html` -Portnummer

> [!NOTE]
> Internet Explorer berücksichtigen nicht den Port Ursprünge zu vergleichen.

## <a name="enable-cors"></a>Aktivieren von CORS

::: moniker range="<= aspnetcore-1.1"

Richten Sie CORS für Ihre Anwendung hinzufügen der `Microsoft.AspNetCore.Cors` Paket Ihrem Projekt.

::: moniker-end

Rufen Sie [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Aktivieren von CORS mit middleware

Um CORS zu aktivieren, fügen Sie die CORS-Middleware hinzu, um die Anforderungspipeline mithilfe der `UseCors` -Erweiterungsmethode. Die CORS-Middleware muss vor stehen Endpunkte definiert in Ihrer app, wo Sie möchten für die Unterstützung von Cross-Origin-Anfragen (z. B. vor dem Aufruf `UseMvc`).

Eine Cross-Origin-Richtlinie kann angegeben werden, beim Hinzufügen der CORS-Middleware mithilfe der [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) Klasse. Hierfür gibt es zwei Möglichkeiten. Die erste besteht darin, rufen Sie `UseCors` mit einem Lambda-Ausdruck:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Hinweis:** die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`). Wenn die URL mit beendet `/`, der Vergleich zurück `false` und kein Header zurückgegeben werden.

Das Lambda akzeptiert ein `CorsPolicyBuilder` Objekt. Sie finden eine Liste mit den [Konfigurationsoptionen](#cors-policy-options) weiter unten in diesem Thema. In diesem Beispiel kann die Richtlinie für Cross-Origin-Anforderungen von `http://example.com` und keine anderen Quellen.

CorsPolicyBuilder hat eine fluent-API, damit Sie die Methodenaufrufe verketten können:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Der zweite Ansatz ist auf eine oder mehrere benannte CORS-Richtlinien zu definieren, und wählen Sie dann zur Laufzeit anhand des Namens der Richtlinie.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

In diesem Beispiel wird eine CORS-Richtlinie, die mit dem Namen "AllowSpecificOrigin". Um die Richtlinie auswählen, übergeben Sie den Namen in `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Aktivieren von CORS in MVC

Sie können alternativ MVC verwenden, um bestimmte CORS pro Aktion, pro Controller oder global für alle Controller anzuwenden. Die gleichen CORS-Dienste werden verwendet, wenn Sie MVC verwenden, um CORS zu aktivieren, aber die CORS-Middleware nicht ist.

### <a name="per-action"></a>Pro Aktion

Geben Sie eine CORS-Richtlinie für eine bestimmte Aktion hinzufügen der `[EnableCors]` -Attribut auf die Aktion. Geben Sie den Namen der Richtlinie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Pro controller

Geben Sie die CORS-Richtlinie für einen bestimmten Controller Hinzufügen der `[EnableCors]` -Attribut auf die Controllerklasse. Geben Sie den Namen der Richtlinie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Global

Sie können CORS global für alle Controller durch Hinzufügen der `CorsAuthorizationFilterFactory` Filter, um die globale filterauflistung:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Die Rangfolge ist: Aktion, Controller, globale. Aktion-basierten Richtlinien haben Vorrang vor auf Controllerebene Richtlinien, und auf Controllerebene Richtlinien haben Vorrang vor globalen Richtlinien.

### <a name="disable-cors"></a>Deaktivieren von CORS

Wenn Sie um CORS für einen Controller oder die Aktion zu deaktivieren, verwenden die `[DisableCors]` Attribut.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS-Richtlinie-Optionen

Dieser Abschnitt beschreibt die verschiedenen Optionen, die Sie in einer CORS-Richtlinie festlegen können.

* [Legen Sie die zulässigen Ursprünge](#set-the-allowed-origins)

* [Legen Sie die zulässigen HTTP-Methoden](#set-the-allowed-http-methods)

* [Legt die zulässigen Anforderungsheader fest.](#set-the-allowed-request-headers)

* [Legen Sie die verfügbar gemachten Antwortheader](#set-the-exposed-response-headers)

* [Anmeldeinformationen im Cross-Origin-Anforderungen](#credentials-in-cross-origin-requests)

* [Die Preflight-Ablaufzeit festlegen](#set-the-preflight-expiration-time)

Für einige Optionen, es kann hilfreich sein, lesen Sie [funktioniert wie CORS](#how-cors-works) erste.

### <a name="set-the-allowed-origins"></a>Legen Sie die zulässigen Ursprünge

Um eine oder mehrere bestimmte Ursprünge zuzulassen:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Alle Ursprünge erlaubt:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen. Es bedeutet, dass praktisch jeder Website AJAX-Aufrufe Ihrer API kann.

### <a name="set-the-allowed-http-methods"></a>Legen Sie die zulässigen HTTP-Methoden

Um alle HTTP-Methoden zu ermöglichen:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Dies wirkt sich auf preflightanforderungen und Access-Control-Allow-Methods-Header.

### <a name="set-the-allowed-request-headers"></a>Legt die zulässigen Anforderungsheader fest.

Eine CORS-preflight-Anforderung sind zum Beispiel einen Access-Control-Request-Headers-Header, die HTTP-Header, die von der Anwendung festgelegte auflisten (den so genannten "author-Anforderungsheader").

Auf eine Whitelist bestimmte Header:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Damit können erstellen alle Anforderungsheader:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Browser sind nicht vollständig konsistent, in wie sie Access-Control-Request-Headers festgelegt. Wenn Sie Header nicht festgelegt "*", aufzunehmen mindestens "accept", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.

### <a name="set-the-exposed-response-headers"></a>Legen Sie die verfügbar gemachten Antwortheader

Standardmäßig nicht im Browser alle Antwortheader für die Anwendung verfügbar machen. (Finden Sie unter [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Die Antwortheader, die standardmäßig verfügbar sind:

* Cache-Control

* Content-Language

* Inhaltstyp

* Läuft ab

* Zuletzt geändert

* Pragma

Die CORS-Spezifikation ruft diese *Header für die einfache Antwort*. Um weitere Header an die Anwendung verfügbar zu machen:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Anmeldeinformationen im Cross-Origin-Anforderungen

Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung. Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung. Anmeldeinformationen enthalten, Cookies sowie HTTP-Authentifizierungsschemas. Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client XMLHttpRequest.withCredentials auf "true" festgelegt.

Mithilfe von XMLHttpRequest direkt:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

In jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Darüber hinaus muss der Server die Anmeldeinformationen zulassen. So ermöglichen Sie Cross-Origin-Anmeldeinformationen:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Jetzt umfasst die HTTP-Antwort einen Access-Control-Allow-Credentials-Header, der dem Browser darüber informiert, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.

Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort keinen gültigen Access-Control-Allow-Credentials-Header enthalten, wird der Browser wird nicht die Antwort an die Anwendung verfügbar machen, und die AJAX-Anforderung ein Fehler auftritt.

Seien Sie vorsichtig beim Cross-Origin-Anmeldeinformationen zulassen. Eine Website in einer anderen Domäne kann die Anmeldeinformationen eines angemeldeten Benutzers an die app auf den Namen des Benutzers, ohne das Wissen des Benutzers senden. CORS-Spezifikation gibt auch an dieser Einstellung Ursprüngen `"*"` (alle Ursprünge) ist ungültig. wenn die `Access-Control-Allow-Credentials` Header vorhanden ist.

### <a name="set-the-preflight-expiration-time"></a>Die Preflight-Ablaufzeit festlegen

Die Access-Control-Max-Age-Header gibt an, wie lange die Antwort auf die preflight-Anforderung zwischengespeichert werden kann. So legen Sie diesen Header fest:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Funktionsweise von CORS

In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten. Es ist wichtig zum Verständnis der Funktionsweise von CORS, damit die CORS-Richtlinie ordnungsgemäß konfiguriert und debuggt werden, wenn die zu unerwarteten Ergebnissen führen kann.

CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen. Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen. Benutzerdefinierte JavaScript-Code ist nicht erforderlich, um CORS zu aktivieren.

Hier ist ein Beispiel für eine cors-Anforderung. Die `Origin` Header enthält, die Domäne der Website, die die Anforderung gesendet wird:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Wenn der Server die Anforderung zulässt, wird der Access-Control-Allow-Origin-Header in der Antwort. Der Wert dieses Headers entspricht den Origin-Header aus der Anforderung oder ist Sie den Platzhalterwert "*", Bedeutung, die einen beliebigen Ursprung zulässig ist:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Die AJAX-Anforderung schlägt fehl, wenn die Antwort den Access-Control-Allow-Origin-Header enthalten, nicht. Der Browser lässt, die die Anforderung. Auch wenn der Server eine erfolgreiche Antwort zurückgibt, stellen nicht im Browser die Antwort an die Clientanwendung zur Verfügung.

### <a name="preflight-requests"></a>Preflight-Anforderungen

Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung, die eine "preflight-Anforderung", aufgerufen, bevor die tatsächliche Anforderung für die Ressource gesendet. Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:

* Die Anforderungsmethode ist GET, HEAD oder POST, und

* Die Anwendung nicht als Accept, Accept-Language, Inhaltssprache, Anforderungsheader festlegen Content-Type oder letzten-Ereignis-ID, und

* Der Content-Type-Header (falls festgelegt) ist eine der folgenden:

  * application/x-www-form-urlencoded

  * Multipart/Form-data

  * Text/plain

Die Regel zu Anforderungsheader gilt für Header, die Anwendung wird durch Aufrufen von SetRequestHeader für das XMLHttpRequest-Objekt. (Die CORS-Spezifikation als diese "Author-Anforderungsheader" bezeichnet). Die Regel gilt nicht für Header, die der Browser, z. B. Benutzer-Agent, Host und Content-Length festlegen kann haben.

Hier ist ein Beispiel für eine preflight-Anforderung:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode. Es enthält zwei spezielle Header:

* Access-Control-Request-Method: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.

* Access-Control-Request-Headers: Eine Liste der Anforderungsheader, die die Anwendung für die tatsächliche Anforderung festgelegt. (In diesem Fall nicht dazu gehören Header, die der Browser legt diese fest.)

Hier ist eine Beispielantwort, vorausgesetzt, dass der Server die Anforderung zulässt:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Die Antwort enthält eine Access-Control-Allow-Methods-Header, der die zulässigen Methoden aufgeführt, und optional eine Access-Control-Allow-Headers-Header, der die erlaubten Header aufgeführt sind. Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an, wie oben beschrieben.
