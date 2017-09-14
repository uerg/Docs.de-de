---
title: Zwischenspeichern von Antworten
author: rick-anderson
description: "Erklärt, wie Antwort zwischenspeichern konfigurieren, um Bandbreite zu senken und die Leistung verbessern."
keywords: ASP.NET Core, Antwort zwischenspeichern, HTTP-Header
ms.author: riande
manager: wpickett
ms.date: 7/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 8b20ac70f257213ae3830749e729156ee5ab6447
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="response-caching"></a>Zwischenspeichern von Antworten

Durch [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), und [Steve Smith](https://ardalis.com/)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>Was ist das Zwischenspeichern von Antworten

*Zwischenspeichern von Antworten* Antworten Cache-bezogene Header hinzugefügt. Diese Header angeben, wie Clients, Proxy und Middleware zum Zwischenspeichern von Antworten soll. Zwischenspeichern von Antworten reduzieren die Anzahl der Anforderungen, die ein Client oder Proxy auf dem Webserver vornimmt. Zwischenspeichern von Antworten kann außerdem verringern Sie die Menge der Arbeit der Webserver durchführt, um die Antwort zu generieren. 

Ist der primäre HTTP-Header, die zum Zwischenspeichern verwendeten `Cache-Control`. Finden Sie unter der [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) für Weitere Informationen. Allgemeine Cache-Anweisungen:

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [ohne-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)
* [Variieren](https://tools.ietf.org/html/rfc7231#section-7.1.4)

Die Webserver kann Antworten durch Hinzufügen der Antwort zwischenspeichern Middleware Zwischenspeichern. Finden Sie unter [Antwort zwischenspeichern Middleware](middleware.md) für Weitere Informationen.

## <a name="distributed-cache-tag-helper"></a>Verteilter Cache-Tag-Hilfsprogramm

Die [verteilten Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) ermöglicht Verteiltes Zwischenspeichern.


## <a name="responsecache-attribute"></a>ResponseCache-Attribut

Die `ResponseCacheAttribute` gibt die Parameter zum Festlegen der entsprechenden Header in Zwischenspeichern von Antworten erforderlich sind. Finden Sie unter [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) eine Beschreibung der Parameter.

>[!WARNING]
> Deaktivieren Sie das Zwischenspeichern für Inhalte, die Informationen für authentifizierte Clients enthält. Zwischenspeichern sollten nur aktiviert werden, für Inhalte, die nicht geändert wird auf der Identität eines Benutzers basieren, oder ob ein Benutzer angemeldet ist.

`VaryByQueryKeys string[]`(erfordert ASP.NET Core 1.1.0 und höher): Wenn festgelegt ist, die Antwort zwischenspeichern Middleware gespeicherte Antwort von den Werten der angegebenen Liste der Abfrageschlüssel variiert. Die Antwort zwischenspeichern Middleware muss aktiviert sein, zum Festlegen der `VaryByQueryKeys` -Eigenschaft, andernfalls eine Laufzeitausnahme ausgelöst. Es ist keine entsprechende HTTP-Header für die `VaryByQueryKeys` Eigenschaft. Diese Eigenschaft ist eine HTTP-Funktion, die von der Antwort zwischenspeichern Middleware verarbeitet. Für die Middleware, die eine zwischengespeicherte Antwort dient müssen der Abfragezeichenfolge und der Wert der Abfragezeichenfolge eine frühere Anforderung wieder übereinstimmen. Betrachten Sie beispielsweise die folgende Sequenz:

| Anforderung          | Ergebnis |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | vom Server zurückgegebene |
| `http://example.com?key1=value1` | von der Middleware zurückgegeben |
| `http://example.com?key1=value2` | vom Server zurückgegebene |

Die erste Anforderung wird vom Server zurückgegebenen und Middleware zwischengespeichert. Die zweite Anforderung wird von Middleware zurückgegeben, weil die Abfragezeichenfolge die vorhergehenden Anforderung übereinstimmt. Die dritte Anforderung ist nicht im Cache Middleware, da der Wert der Abfragezeichenfolge nicht mit eine frühere Anforderung übereinstimmt. 

Die `ResponseCacheAttribute` dient zum Erstellen und konfigurieren Sie (über `IFilterFactory`) eine `ResponseCacheFilter`. Die `ResponseCacheFilter` führt die Arbeit aktualisieren die entsprechenden HTTP-Header und die Funktionen der Antwort. Der Filter:

* Entfernt alle vorhandenen Header für `Vary`, `Cache-Control`, und `Pragma`. 
* Schreibt die entsprechenden Header auf Grundlage der Eigenschaften legen Sie in der `ResponseCacheAttribute`. 
* Aktualisiert die Antwort zwischenspeichern HTTP-Funktion, wenn `VaryByQueryKeys` festgelegt ist.

### <a name="vary"></a>variieren

Dieser Header wird nur geschrieben, wenn die `VaryByHeader` festgelegt wird. Es wird festgelegt, um die `Vary` den Wert der Eigenschaft. Das folgende Beispiel verwendet die `VaryByHeader` Eigenschaft.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Sie können die Antwortheader mit Ihrem Browser Netzwerktools anzeigen. Die folgende Abbildung zeigt die Ausgabe auf Edge F12 der **Netzwerk** Registerkarte, wenn die `About2` Aktionsmethode wird aktualisiert. 

![Edge-F12-Ausgabe auf die ** Netzwerk ** Registerkarte beim Aufrufen der Aktionsmethode "About2"](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore und Location.None

`NoStore`überschreibt die meisten anderen Eigenschaften. Wenn diese Eigenschaft festgelegt wird, um `true`die `Cache-Control` Header auf "Nein-Store" festgelegt. Wenn `Location` festgelegt ist, um `None`:

* Für `Cache-Control` ist `"no-store, no-cache"` festgelegt. 
* Für `Pragma` ist `no-cache` festgelegt. 

Wenn `NoStore` ist `false` und `Location` ist `None`, `Cache-Control` und `Pragma` festgelegt, um `no-cache`.

Legen Sie Sie in der Regel `NoStore` auf `true` auf Fehlerseiten. Zum Beispiel:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Dies führt die folgenden Header:

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Speicherort und die Dauer

So aktivieren Sie das Zwischenspeichern, `Duration` muss auf einen positiven Wert festgelegt werden und `Location` muss entweder `Any` (Standard) oder `Client`. In diesem Fall die `Cache-Control` Header wird auf die Location-Wert, gefolgt von der "Max-Age" der Antwort festgelegt werden.

> [!NOTE]
> `Location`die Optionen der `Any` und `Client` übersetzen in `Cache-Control` Headerwerte `public` und `private`bzw.. Wie bereits erwähnt, festlegen `Location` auf `None` wird, setzen Sie beide `Cache-Control` und `Pragma` Header `no-cache`.

Im folgenden ein Beispiel für die Header erstellt wird, durch Festlegen von `Duration` und lassen die Standardeinstellung `Location` Wert.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Erzeugt die folgenden Header:

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>Cacheprofile

Anstelle von duplizieren `ResponseCache` Einstellungen auf viele Aktion Attributen für Controller, CacheProfile können als Optionen konfiguriert werden, beim Einrichten von MVC in der `ConfigureServices` Methode in `Startup`. In ein Cacheprofil verwiesen wird, gefundenen Werte verwendet werden als die standardmäßig von der `ResponseCache` Attribut, und wird nach beliebigen Eigenschaften für das Attribut angegebenen überschrieben werden.

Einrichten von ein Cacheprofil:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Verweisen auf ein Cacheprofil aus:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

Die `ResponseCache` Attribut kann sowohl auf Aktionen (Methoden) sowie auf Domänencontrollern (Klassen) angewendet werden. Methodenebene Attribute werden die Attribute auf Klassenebene festgelegten Einstellungen überschrieben.

Im obigen Beispiel gibt ein Attribut auf Klassenebene eine Dauer von 30 Sekunden, während eine Methodenebene Attribute verweist auf ein Cacheprofil mit einer Dauer von 60 Sekunden festgelegt.

Die resultierende Header:

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Caching in HTTP aus der Spezifikation](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
