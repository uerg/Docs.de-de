---
title: "Antwort Komprimierung Middleware für ASP.NET Core"
author: guardrex
description: Informationen Sie zur Antwort Komprimierung sowie zum Antwort Komprimierung Middleware in ASP.NET Core-apps verwenden.
keywords: ASP.NET Core, Leistung, Antwort Komprimierung, Gzip, akzeptieren-Codierung, middleware
ms.author: riande
manager: wpickett
ms.date: 08/20/2017
ms.topic: article
ms.assetid: de621887-c5c9-4ac8-9efd-f5cc0457a134
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/response-compression
ms.openlocfilehash: ab004c038b82888aa57d5e25fcb69a06deec8411
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2017
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Antwort Komprimierung Middleware für ASP.NET Core

Durch [Luke Latham](https://github.com/guardrex)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)

Die Netzwerkbandbreite ist eine eingeschränkte Ressource. Verringern die Größe der Antwort in der Regel wird die Reaktionsfähigkeit einer App häufig erheblich erhöht. Eine Möglichkeit, verringern Sie die Größe der Nutzlast ist zum Komprimieren von Antworten für eine app.

## <a name="when-to-use-response-compression-middleware"></a>Antwort Komprimierung Middleware verwenden
Serverbasierte Antwort komprimierungstechnologien in IIS, Apache oder Nginx, in denen die Leistung der Middleware, die von der Servermodulen stimmt wird nicht, verwendet werden. Verwenden Sie die Komprimierung Middleware Antwort beim nicht verwenden:
* [Modul für die dynamische Komprimierung von IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
* [Apache Mod_deflate-Modul](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
* [NGINX-Komprimierung und Dekomprimierung](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* [HTTP.sys-Server](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener))
* [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Antwort-Komprimierung
In der Regel kann alle Antworten, die nicht komprimiert Antwort Komprimierung profitieren. In der Regel nicht systemintern komprimierte Antworten enthalten: CSS, JavaScript, HTML, XML und JSON. Sie sollten nicht systemintern komprimierte Ressourcen, wie z. B. PNG-Dateien komprimieren. Wenn Sie versuchen, eine systemintern komprimierte Antwort weiter zu komprimieren, wird jeder kleinen zusätzlichen Reduzierung der Größe und die Übertragung zeitlich wahrscheinlich nach der Zeit benötigt wurde, um die Komprimierung zu verarbeiten verdeckt. Komprimieren Sie Dateien, die weniger als etwa 150 1000 Bytes (je nach Inhalt der Datei und die Effizienz der Komprimierung) nicht. Der Aufwand für kleine Dateien komprimieren kann es sich um eine komprimierte Datei, die größer als die nicht komprimierte Datei führen.

Wenn ein Client komprimiertem Inhalt verarbeiten kann, muss der Client durch Senden den Server seine Funktionen informieren der `Accept-Encoding` Header mit der Anforderung. Wenn ein Server komprimierte Inhalte sendet, muss er Informationen in enthalten die `Content-Encoding` Kopfzeile auf wie die komprimierte Antwort codiert ist. Inhalt Codierung Bezeichnungen, die von der Middleware unterstützt werden in der folgenden Tabelle angezeigt.

| `Accept-Encoding`Headerwerte | Middleware unterstützt | Beschreibung                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | Nein                   | Komprimierte Brotli-Datenformat                               |
| `compress`                      | Nein                   | UNIX-Datenformat "komprimieren"                                 |
| `deflate`                       | Nein                   | Daten in das Datenformat "Zlib" komprimiert "Verkleinern"     |
| `exi`                           | Nein                   | W3C effiziente XML-Austausch                               |
| `gzip`                          | Ja (Standard)        | GZIP-Dateiformat                                            |
| `identity`                      | Ja                  | Bezeichner "Keine Codierung": die Antwort nicht codiert werden muss. |
| `pack200-gzip`                  | Nein                   | Netzwerk-Übertragungsformat für Java-Archive                   |
| `*`                             | Ja                  | Alle verfügbaren Inhalte, die Codierung wird nicht explizit angefordert     |

Weitere Informationen finden Sie unter der [IANA offizielle Schreiben von Code in der Inhaltsliste](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Die Middleware können Sie zusätzliche Komprimierung, die Anbieter für benutzerdefinierte hinzufügen `Accept-Encoding` Headerwerte. Weitere Informationen finden Sie unter [benutzerdefinierte Anbieter](#custom-providers) unten.

Die Middleware kann das Reagieren auf Qualitätswert (Qvalue, `q`) Gewichtung, wenn vom Client gesendet, Komprimierungsschemas zu priorisieren. Weitere Informationen finden Sie unter [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Komprimierungsalgorithmen unterliegen ein Kompromiss zwischen Geschwindigkeit der Komprimierung und die Effektivität der Komprimierung. *Effektivität* in diesem Kontext bezieht sich auf die Größe der Ausgabe nach der Komprimierung. Die kleinste Größe wird erreicht, indem die letzte *optimale* Komprimierung.

Die Header beteiligten anfordern, senden, Zwischenspeichern und Empfangen von komprimiertem Inhalt sind in der folgenden Tabelle beschrieben.

| Header             | Rolle |
| ------------------ | ---- |
| `Accept-Encoding`  | Vom Client an den Server an, dass der Inhalt an den Client zulässigen Schemas Codierung gesendet. |
| `Content-Encoding` | Vom Server an den Client an, dass die Codierung des Inhalts in der Nutzlast gesendet. |
| `Content-Length`   | Bei der Komprimierung, die `Content-Length` Header entfernt werden, da der Inhalt Text geändert, wenn die Antwort komprimiert wird. |
| `Content-MD5`      | Bei der Komprimierung, die `Content-MD5` Header entfernt, da der Inhalt des Nachrichtentexts geändert hat und der Hash nicht mehr gültig ist. |
| `Content-Type`     | Gibt den MIME-Typ des Inhalts an. Jede Antwort angeben sollten seine `Content-Type`. Die Middleware überprüft diesen Wert, um zu bestimmen, ob die Antwort, komprimiert werden sollen. Die Middleware gibt einen Satz von [Standard-MIME-Typen](#mime-types) , die codiert werden können, aber Sie können ersetzen oder Hinzufügen von MIME-Typen. |
| `Vary`             | Sendens vom Server mit einem Wert von `Accept-Encoding` für Clients und -Proxys, die `Vary` -Header angibt, an den Client oder Proxy, der zwischengespeichert werden soll (variieren) Antworten basierend auf den Wert der `Accept-Encoding` -Header der Anforderung. Das Ergebnis der Rückgabe von Inhalt mit der `Vary: Accept-Encoding` Header ist, dass beide komprimiert und nicht komprimierte Antworten getrennt zwischengespeichert werden. |

Untersuchen Sie die Funktionen der Middleware Komprimierung Antwort mit der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). Das Beispiel veranschaulicht:
* Die Komprimierung von app-Antworten, die mithilfe von Gzip und benutzerdefinierte Komprimierung Anbieter.
* Wie die Standardliste der MIME-Typen für die Komprimierung einen MIME-Typ hinzugefügt.

## <a name="package"></a>Package
Um die Middleware in Ihrem Projekt einzuschließen, fügen Sie einen Verweis auf die [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) Verpacken, oder verwenden Sie die [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Paket. Diese Funktion ist für apps für ASP.NET Core 1.1 oder höher verfügbar.

## <a name="configuration"></a>Konfiguration
Der folgende Code zeigt, wie die Antwort Komprimierung Middleware mit aktiviert die mit der Standardeinstellung Gzip-Komprimierung und für Standard-MIME-Typen.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> Verwenden Sie ein Tool wie [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), oder [Postman](https://www.getpostman.com/) festzulegende der `Accept-Encoding` Anforderungsheader und zu untersuchen, das die Antwortheader, Größe und Text.

Übermitteln eine Anforderung an die Beispiel-app ohne die `Accept-Encoding` Header und beobachten Sie, dass die Antwort nicht komprimiert ist. Die `Content-Encoding` und `Vary` Header nicht in der Antwort vorhanden sind.

![Fiddler-Fenster, das Ergebnis einer Anforderung ohne den Accept-Encoding-Header. Die Antwort wird nicht komprimiert.](response-compression/_static/request-uncompressed.png)

Übermitteln eine Anforderung an die Beispiel-app mit der `Accept-Encoding: gzip` Header und beobachten Sie, dass die Antwort komprimiert wird. Die `Content-Encoding` und `Vary` Header in der Antwort vorhanden sind.

![Fiddler-Fenster, Ergebnis der Anforderung mit den Accept-Encoding-Header und Wert Gzip anzeigt. Die Vary und Content-Encoding-Header werden in der Antwort hinzugefügt. Die Antwort wird komprimiert.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>Anbieter
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
Verwenden der `GzipCompressionProvider` zum Komprimieren von Antworten mit Gzip. Dies ist der Standardanbieter für die Komprimierung, keine Parameter angegeben. Sie können festlegen, die Komprimierung mit dem `GzipCompressionProviderOptions`. 

Die Gzip-Komprimierung-Anbieter wird standardmäßig auf die höchste Komprimierungsgrad (`CompressionLevel.Fastest`), die möglicherweise nicht die effizienteste Komprimierung erzeugen. Wenn die effizienteste Komprimierung gewünscht ist, können Sie die Middleware für die optimale Komprimierung konfigurieren.

| Komprimierungsgrad                | Beschreibung                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | Komprimierung sollte so schnell wie möglich abgeschlossen, auch wenn die resultierende Ausgabe nicht optimal komprimiert wird. |
| `CompressionLevel.NoCompression` | Keine Komprimierung muss ausgeführt werden.                                                                           |
| `CompressionLevel.Optimal`       | Antworten sollten optimal komprimiert werden, auch wenn die Komprimierung mehr Zeit in Anspruch nimmt.                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>MIME-Typen
Die Middleware gibt eine Reihe von MIME-Typen für die Komprimierung an:
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Sie können ersetzen oder MIME-Typen mit den Optionen für die Antwort Komprimierung Middleware angefügt werden soll. Beachten Sie diese Platzhalter-MIME-Typen, z. B. `text/*` werden nicht unterstützt. Die Beispiel-app Fügt einen MIME-Typ für `image/svg+xml` und komprimiert und dient der ASP.NET Core Bannerbild (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>Benutzerdefinierte Anbieter
Sie können benutzerdefinierte Komprimierung Implementierungen mit erstellen `ICompressionProvider`. Die `EncodingName` steht für die Codierung, die von dieser Inhalte `ICompressionProvider` erzeugt. Die Middleware verwendet diese Informationen, die anhand der Liste, die im angegebenen Anbieter auswählen die `Accept-Encoding` -Header der Anforderung.

Verwenden die Beispiel-app, sendet der Client eine Anforderung mit der `Accept-Encoding: mycustomcompression` Header. Die Middleware verwendet die Implementierung von benutzerdefinierten Komprimierung und gibt die Antwort mit einer `Content-Encoding: mycustomcompression` Header. Der Client muss dekomprimiert die benutzerdefinierte Codierung in der Reihenfolge für die Implementierung eines benutzerdefinierten Komprimierung arbeiten können.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Übermitteln eine Anforderung an die Beispiel-app mit der `Accept-Encoding: mycustomcompression` Header und beobachten Sie die Antwortheader. Die `Vary` und `Content-Encoding` Header in der Antwort vorhanden sind. Der Antworttext (nicht dargestellt) ist nicht im Beispiel komprimiert. Es ist nicht in eine Implementierung von Komprimierung der `CustomCompressionProvider` -Klasse des Beispiels. Allerdings wird im Beispiel, in dem Sie solche ein Komprimierungsalgorithmus implementiert würde.

![Fiddler-Fenster, Ergebnis der Anforderung mit den Accept-Encoding-Header und Wert Mycustomcompression anzeigt. Die Vary und Content-Encoding-Header werden in der Antwort hinzugefügt.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Komprimierung mit sicheres Protokoll
Komprimierte Antworten über sichere Verbindungen können gesteuert werden, mit der `EnableForHttps` Option ist standardmäßig deaktiviert. Dynamisch generierte Seiten mit Komprimierung Sicherheitsprobleme führen kann, wie z. B. die [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) und [Verletzung](https://wikipedia.org/wiki/BREACH_(security_exploit)) Angriffe.

## <a name="adding-the-vary-header"></a>Den Vary-Header hinzufügen
Bei der Komprimierung von Antworten auf Grundlage der `Accept-Encoding` -Header, es gibt potenziell mehrere komprimierte Versionen der Antwort und eine nicht komprimierte Version. Um den Client und Proxy-Caches anweisen, die mehrere Versionen vorhanden sind und gespeichert werden sollen, die `Vary` Kopfzeile wird hinzugefügt, und ein `Accept-Encoding` Wert. In ASP.NET Core 1.x, Hinzufügen der `Vary` Header in die Antwort erfolgt manuell. In ASP.NET Core 2.x, die Middleware fügt die `Vary` Header automatisch, wenn die Antwort komprimiert wird.

**ASP.NET Core nur 1.x**

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middlware-issue-when-behind-an-nginx-reverse-proxy"></a>Middlware Problem hinter einen Nginx-Reverse-proxy
Wenn eine Anforderung über einen Proxy von Nginx, ist die `Accept-Encoding` Header wird entfernt. Dadurch wird verhindert, dass die Middleware die Antwort zu komprimieren. Weitere Informationen finden Sie unter [NGINX: Komprimierung und Dekomprimierung](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Dieses Problem wird durch verfolgt [herausfinden, Pass-Through-Komprimierung für Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Arbeiten mit dynamischen IIS-Komprimierung
Eine aktive IIS dynamische Modul für die Komprimierung auf Serverebene, die Sie, deaktivieren Sie die App möchten konfiguriert haben, können Sie dies tun, mit der eine Ergänzung der *"Web.config"* Datei. Weitere Informationen finden Sie unter [Deaktivieren von IIS-Module](xref:hosting/iis-modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Problembehandlung
Ein Tool wie [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), oder [Postman](https://www.getpostman.com/), Ihnen festzulegende ermöglichen die `Accept-Encoding` Anforderungsheader und zu untersuchen, das die Antwortheader, Größe und Text. Die Antwort-Middleware-Komprimierung komprimiert Antworten, die die folgenden Bedingungen erfüllen:
* Die `Accept-Encoding` Header mit dem Wert vorhanden ist `gzip`, `*`, oder benutzerdefinierte Codierung, die einen benutzerdefinierten Komprimierung-Anbieter entspricht, die Sie eingerichtet haben. Der Wert darf nicht sein `identity` oder qualitätswerten (Qvalue, `q`) von 0 (null) festlegen.
* Der MIME-Typ (`Content-Type`) muss festgelegt sein und muss einem MIME-Typ auf konfiguriert entsprechen den `ResponseCompressionOptions`.
* Die Anforderung dürfen keine der `Content-Range` Header.
* Die Anforderung muss unsicheres-Protokoll (http) verwenden, es sei denn, in die Antwort Komprimierung middlewareoptionen sicheres Protokoll (Https) konfiguriert ist. *Beachten Sie die Gefahr [oben beschriebenen](#compression-with-secure-protocol) bei der Aktivierung der Komprimierung für sichere Inhalte.*

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Application Startup (Starten von Anwendungen)](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware)
* [Mozilla Developer Network: Accept-Encoding.](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 Abschnitt 3.1.2.1: Inhalt Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 Abschnitt 4.2.3: Gzip-Codierung](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [GZIP-Spezifikation Dateiformatversion 4.3](http://www.ietf.org/rfc/rfc1952.txt)
