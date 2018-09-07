---
title: Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie CORS als Standard zum Zulassen oder ablehnen von ursprungsübergreifenden Anforderungen in einer ASP.NET Core-app.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: f654260411f1bd5725a0e3d14951c7e9bbc893e8
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44039977"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core

Durch [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), und [Tom Dykstra](https://github.com/tdykstra)

Browsersicherheit verhindert, dass eine Webseite auf Anfragen zu einer anderen Domäne als der, die die Webseite verarbeitet. Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*. Die Richtlinie desselben Ursprungs wird verhindert, dass eine schädliche Website sensible Daten von einem anderen Standort lesen. In einigen Fällen empfiehlt es sich um zuzulassen, dass andere Standorte Cross-Origin-Anforderungen zu Ihrer app durchführen.

[Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht. Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen. CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](https://wikipedia.org/wiki/JSONP). In diesem Thema veranschaulicht das Aktivieren von CORS in einer ASP.NET Core-app.

## <a name="same-origin"></a>Denselben Ursprung

Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Diese zwei URLs haben denselben Ursprung an:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Diese URLs haben verschiedene Ursprünge als die vorherigen beiden URLs:

* `https://example.net` &ndash; Andere Domäne
* `https://www.example.com/foo.html` &ndash; Andere Unterdomäne
* `http://example.com/foo.html` &ndash; Anderes Schema
* `https://example.com:9000/foo.html` &ndash; Portnummer

> [!NOTE]
> Internet Explorer berücksichtigen nicht den Port Ursprünge zu vergleichen.

## <a name="register-cors-services"></a>CORS-Dienste registrieren

::: moniker range=">= aspnetcore-2.1"

Referenz der [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app) oder fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Referenz der [metapaket "Microsoft.aspnetcore.All"](xref:fundamentals/metapackage) oder fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.

::: moniker-end

Rufen Sie <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` CORS-Dienste in der app Service-Container hinzufügen:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Aktivieren von CORS

Verwenden Sie nach der Registrierung CORS-Dienste einen der folgenden Ansätze zum Aktivieren von CORS in einer ASP.NET Core-app aus:

* [CORS-Middleware](#enable-cors-with-cors-middleware) &ndash; Richtlinien für CORS gelten global für die app über Middleware.
* [Von CORS in MVC](#enable-cors-in-mvc) &ndash; CORS gelten Richtlinien pro Aktion oder pro Controller. CORS-Middleware wird nicht verwendet.

### <a name="enable-cors-with-cors-middleware"></a>Aktivieren von CORS mit CORS-Middleware

CORS-Middleware verarbeitet Cross-Origin-Anforderungen an die app. Rufen Sie zum Aktivieren von CORS-Middleware in der Anforderungsverarbeitungspipeline der <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> Erweiterungsmethode in `Startup.Configure`.

CORS-Middleware muss vor stehen Endpunkte definiert in Ihrer app, wo Sie möchten für die Unterstützung von Cross-Origin-Anfragen (z. B. vor dem Aufruf von `UseMvc` für MVC-Razor-Seiten-Middleware).

Ein *ursprungsübergreifende Richtlinie* kann angegeben werden, beim Hinzufügen der CORS-Middleware mithilfe der <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Klasse. Es gibt zwei Ansätze für eine CORS-Richtlinie definieren:

* Rufen Sie `UseCors` mit einem Lambda-Ausdruck:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  Das Lambda akzeptiert ein <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Objekt. [Optionen für die Konfiguration](#cors-policy-options), z. B. `WithOrigins`, werden weiter unten in diesem Thema. Im vorherigen Beispiel kann die Richtlinie Cross-Origin-Anforderungen von `https://example.com` und keine anderen Quellen.

  Die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`). Wenn die URL mit beendet `/`, gibt der Vergleich `false` und es wird kein Header zurückgegeben.

  `CorsPolicyBuilder` verfügt über eine fluent-API, damit Sie die Methodenaufrufe verketten können:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Definieren Sie mindestens eine benannte CORS-Richtlinie, und wählen Sie die Richtlinie anhand des Namens zur Laufzeit. Im folgenden Beispiel wird eine benutzerdefiniertes CORS-Richtlinie mit dem Namen *AllowSpecificOrigin*. Um die Richtlinie auswählen, übergeben Sie den Namen in `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Aktivieren von CORS in MVC

Sie können alternativ MVC verwenden, um Richtlinien für bestimmte CORS pro Aktion oder pro Controller anzuwenden. Wenn Sie MVC verwenden, um CORS zu aktivieren, werden die registrierten CORS-Dienste verwendet. Die CORS-Middleware wird nicht verwendet.

### <a name="per-action"></a>Pro Aktion

Um eine CORS-Richtlinie für eine bestimmte Aktion anzugeben, fügen die [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) -Attribut auf die Aktion. Geben Sie den Namen der Richtlinie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Pro controller

Fügen Sie zum Angeben der CORS-Richtlinie für einen bestimmten Controller die [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) -Attribut auf die Controllerklasse. Geben Sie den Namen der Richtlinie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

Die Rangfolge ist:

1. action
1. Controller

### <a name="disable-cors"></a>Deaktivieren von CORS

Wenn Sie um CORS für einen Controller oder die Aktion zu deaktivieren, verwenden die [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Attribut:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>CORS-Richtlinie-Optionen

Dieser Abschnitt beschreibt die verschiedenen Optionen, die Sie in einer CORS-Richtlinie festlegen können.

* [Legen Sie die zulässigen Ursprünge](#set-the-allowed-origins)
* [Legen Sie die zulässigen HTTP-Methoden](#set-the-allowed-http-methods)
* [Legt die zulässigen Anforderungsheader fest.](#set-the-allowed-request-headers)
* [Legen Sie die verfügbar gemachten Antwortheader](#set-the-exposed-response-headers)
* [Anmeldeinformationen im Cross-Origin-Anforderungen](#credentials-in-cross-origin-requests)
* [Die Preflight-Ablaufzeit festlegen](#set-the-preflight-expiration-time)

Für einige Optionen, es kann hilfreich sein, lesen Sie die [funktioniert wie CORS](#how-cors-works) im ersten Abschnitt.

### <a name="set-the-allowed-origins"></a>Legen Sie die zulässigen Ursprünge

Um eine oder mehrere bestimmte Ursprünge zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

Aufrufen, um alle Ursprünge zuzulassen, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen. Dadurch können Anforderungen von einem beliebigen Ursprung bedeutet, dass *eine beliebige Website* Cross-Origin-Anfragen an Ihre app vornehmen können.

Diese Einstellung wirkt sich auf [preflight-Anforderungen und der Access-Control-Allow-Origin-Header](#preflight-requests) (weiter unten in diesem Thema beschrieben).

### <a name="set-the-allowed-http-methods"></a>Legen Sie die zulässigen HTTP-Methoden

Um alle HTTP-Methoden zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

Diese Einstellung wirkt sich auf [preflight-Anforderungen und der Access-Control-Allow-Methods-Header](#preflight-requests) (weiter unten in diesem Thema beschrieben).

### <a name="set-the-allowed-request-headers"></a>Legt die zulässigen Anforderungsheader fest.

Wird aufgerufen, um bestimmte Header in einer CORS-Anforderung gesendet werden können *erstellen Anforderungsheader*, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> , und geben Sie die erlaubten Header:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Um zuzulassen, die alle erstellen Anforderungsheader, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Diese Einstellung wirkt sich auf [preflight-Anforderungen und der Access-Control-Request-Headers-Header](#preflight-requests) (weiter unten in diesem Thema beschrieben).

::: moniker range=">= aspnetcore-2.2"

Eine Übereinstimmung der CORS-Middleware-Richtlinien auf bestimmte Header gemäß `WithHeaders` ist nur möglich, wenn die Header gesendet wird, `Access-Control-Request-Headers` genau die Header in angegebenen `WithHeaders`.

Betrachten Sie beispielsweise eine app wie folgt konfiguriert:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS-Middleware eine preflight-Anforderung mit der folgenden Anforderungsheader ablehnt, da `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) ist nicht aufgeführt, `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Die app-gibt ein *200 OK* Antwort jedoch nicht wieder die CORS-Header sendet. Aus diesem Grund versucht der Browser nicht die cors-Anforderung.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS-Middleware können immer vier Header in der `Access-Control-Request-Headers` unabhängig von den Werten in CorsPolicy.Headers konfiguriert gesendet werden. Diese Liste der Header enthält:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Betrachten Sie beispielsweise eine app wie folgt konfiguriert:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS-Middleware erfolgreich beantwortet eine preflight-Anforderung mit der folgenden Anforderungsheader da `Content-Language` ist immer in der Whitelist enthalten:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Legen Sie die verfügbar gemachten Antwortheader

Standardmäßig nicht im Browser alle Antwortheader für die app verfügbar machen. Weitere Informationen finden Sie unter [W3C Cross-Origin Resource Sharing (Terminologie): einfache Antwortheader](https://www.w3.org/TR/cors/#simple-response-header).

Die Antwortheader, die standardmäßig verfügbar sind:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS-Spezifikation ruft diese Header *Header für die einfache Antwort*. Um weitere Header für die app verfügbar zu machen, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Anmeldeinformationen im Cross-Origin-Anforderungen

Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung. Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung. Anmeldeinformationen enthalten, Cookies und HTTP-Authentifizierungsschemas. Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client festgelegt `XMLHttpRequest.withCredentials` zu `true`.

Mithilfe von `XMLHttpRequest` direkt:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

In jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Darüber hinaus muss der Server die Anmeldeinformationen zulassen. Um ursprungsübergreifende Anmeldeinformationen ermöglichen möchten, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

Die HTTP-Antwort enthält ein `Access-Control-Allow-Credentials` -Header, der dem Browser darüber informiert, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.

Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort auf eine gültige beinhaltet keine `Access-Control-Allow-Credentials` -Header, der Browser nicht die Antwort auf die app verfügbar zu machen, und die cors-Anforderung schlägt fehl.

Seien Sie vorsichtig beim Cross-Origin-Anmeldeinformationen zulassen. Eine Website in einer anderen Domäne kann die Anmeldeinformationen eines angemeldeten Benutzers an die app auf den Namen des Benutzers, ohne das Wissen des Benutzers senden.

CORS-Spezifikation gibt auch an dieser Einstellung Ursprüngen `"*"` (alle Ursprünge) ist ungültig. wenn die `Access-Control-Allow-Credentials` Header vorhanden ist.

### <a name="preflight-requests"></a>Preflight-Anforderungen

Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung vor der tatsächlichen Anforderung an. Diese Anforderung wird aufgerufen, eine *preflight-Anforderung*. Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:

* Die Anforderungsmethode ist GET, HEAD oder POST.
* Die app keine Anforderungsheader außer festlegen `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, oder `Last-Event-ID`.
* Die `Content-Type` -Header, wenn festgelegt ist, verfügt über einen der eines der folgenden Werte:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

Die Regel auf die Anforderungsheader festzulegen, für die Header die Anforderung des Clients gilt, die die app durch den Aufruf festlegt `setRequestHeader` auf die `XMLHttpRequest` Objekt. CORS-Spezifikation ruft diese Header *erstellen Anforderungsheader*. Die Regel trifft nicht auf der Browser, wie z. B. festlegen kann Header `User-Agent`, `Host`, oder `Content-Length`.

Im folgenden finden ein Beispiel für eine preflight-Anforderung:

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode. Es enthält zwei spezielle Header:

* `Access-Control-Request-Method`: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.
* `Access-Control-Request-Headers`: Eine Liste der Anforderungsheader, die die app für die tatsächliche Anforderung festlegt. Wie bereits erwähnt, Dies beinhaltet keine Header, die der Browser legt, wie z. B. fest `User-Agent`.

Eine CORS-preflight-Anforderung sind zum Beispiel eine `Access-Control-Request-Headers` -Header, der auf dem Server die Header gibt an, die mit der tatsächlichen Anforderung gesendet werden.

Um bestimmte Header zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Um zuzulassen, die alle erstellen Anforderungsheader, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Browser sind nicht vollständig konsistent in diese Festlegung `Access-Control-Request-Headers`. Wenn Sie Header nicht festgelegt `"*"` (oder verwenden Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), Sie sollten mindestens einschließen `Accept`, `Content-Type`, und `Origin`, sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.

Im folgenden finden eine Beispielantwort auf die preflight-Anforderung (vorausgesetzt, dass der Server die Anforderung zulässt):

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Die Antwort enthält ein `Access-Control-Allow-Methods` Header, der die zulässigen Methoden enthält und optional eine `Access-Control-Allow-Headers` -Header, der die erlaubten Header aufgeführt sind. Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an.

Wenn die preflight-Anforderung abgelehnt wird, gibt die app eine *200 OK* Antwort jedoch nicht wieder die CORS-Header sendet. Aus diesem Grund versucht der Browser nicht die cors-Anforderung.

### <a name="set-the-preflight-expiration-time"></a>Die Preflight-Ablaufzeit festlegen

Die `Access-Control-Max-Age` Header gibt an, wie lange die Antwort auf die preflight-Anforderung zwischengespeichert werden kann. Rufen Sie zum Festlegen dieser Header <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>Funktionsweise von CORS

In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten. Es ist wichtig zum Verständnis der Funktionsweise von CORS, damit die CORS-Richtlinie ordnungsgemäß konfiguriert und debuggt werden, wenn die zu unerwarteten Ergebnissen führen kann.

CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen. Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen. Benutzerdefinierte JavaScript-Code ist nicht erforderlich, um CORS zu aktivieren.

Folgendes ist ein Beispiel für eine cors-Anforderung. Die `Origin` Header enthält, die Domäne der Website, die die Anforderung gesendet wird:

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Wenn der Server die Anforderung zulässt, wird die `Access-Control-Allow-Origin` Header in der Antwort. Entweder mit dem Wert dieses Headers übereinstimmt der `Origin` Header aus der Anforderung oder der Platzhalterwert `"*"`, Bedeutung, die einen beliebigen Ursprung zulässig ist:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Wenn die Antwort enthalten nicht die `Access-Control-Allow-Origin` -Header, die Cross-Origin-Anforderung schlägt fehl. Der Browser lässt, die die Anforderung. Selbst wenn der Server eine erfolgreiche Antwort gibt zurück, machen nicht im Browser die Antwort für die Client-app verfügbar.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
