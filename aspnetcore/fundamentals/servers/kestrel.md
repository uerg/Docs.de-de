---
title: Implementierung des Webservers Kestrel in ASP.NET Core
author: tdykstra
description: "Einführung in Kestrel, der plattformübergreifende Webserver für ASP.NET Core, der auf libuv basiert."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Einführung in die Implementierung des Webservers Kestrel in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) und [Stephen Halter](https://twitter.com/halter73)

Kestrel ist ein plattformübergreifender [Webserver für ASP.NET Core](index.md), der auf der plattformübergreifenden asynchronen E/A-Bibliothek [libuv](https://github.com/libuv/libuv) basiert. Kestrel ist der Webserver, der standardmäßig in ASP.NET Core-Projektvorlagen enthalten ist. 

Kestrel unterstützt die folgenden Features:

  * HTTPS
  * Nicht transparente Upgrades, die zum Aktivieren von [WebSockets](https://github.com/aspnet/websockets) verwendet werden
  * Unix-Sockets für eine hohe Leistung im Hintergrund von Nginx 

Kestrel wird auf allen Plattformen und für alle Versionen unterstützt, die .NET Core unterstützt.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Anzeigen oder Herunterladen von Beispielcode für 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Anzeigen oder Herunterladen von Beispielcode für 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Verwenden von Kestrel mit einem Reverseproxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Sie können Kestrel als eigenständigen Webserver oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Jede der beiden Konfigurationen &mdash;, egal ob mit oder ohne Reverseproxyserver &mdash;, kann auch dann verwendet werden, wenn Kestrel nur in einem internen Netzwerk verfügbar gemacht wird.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn Ihre Anwendung nur Anforderungen aus einem internen Netzwerk akzeptiert, können Sie Kestrel als eigenständigen Webserver verwenden.

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

Wenn Sie Ihre Anwendung im Internet verfügbar machen, müssen Sie IIS, Nginx oder Apache als *Reverseproxyserver* verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Ein Reverseproxy ist aus Sicherheitsgründen für Edge-Bereitstellungen erforderlich, die Datenverkehr aus dem Internet ausgesetzt sind. Die Kestrel-Versionen 1.x besitzen keine umfassenden Schutzmaßnahmen gegenüber Angriffen. Geeignete Timeouts und Größenlimits sowie eine Beschränkung der Anzahl gleichzeitiger Verbindungen sind beispielsweise nicht vorhanden.

---

Wenn mehrere Anwendungen auf einem einzigen Server ausgeführt werden, die über die gleiche IP und den gleichen Port verfügen, ist ein Reverseproxy erforderlich. Dieses Szenario kann nicht direkt mit Kestrel ausgeführt werden, da Kestrel das Freigeben der gleichen IP und des gleichen Ports für mehrere Prozesse nicht unterstützt. Wenn Sie Kestrel dafür konfigurieren, einem Port zu lauschen, wird der gesamte Datenverkehr dieses Ports unabhängig vom Hostheader verarbeitet. Ein Reverseproxy, der Ports freigeben kann, muss eine eindeutige IP und einen eindeutigen Port an Kestrel weiterleiten.

Auch wenn kein Reverseproxyserver erforderlich ist, kann die Verwendung eines solchen aus anderen Gründen empfehlenswert sein:

* Er kann die verfügbar gemachte Oberfläche einschränken.
* Er stellt eine optionale zusätzliche Ebene für Konfiguration und Schutz bereit.
* Er kann sich besser in die vorhandene Infrastruktur integrieren.
* Er vereinfacht den Lastenausgleich und das Setup von SSL. Nur der Reverseproxyserver erfordert ein SSL-Zertifikat. Dieser Server kann mithilfe von HTTP mit Ihren Anwendungsservern auf dem internen Netzwerk kommunizieren.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Verwenden von Kestrel in ASP.NET Core-Apps

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Das Paket [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) ist im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten.

ASP.NET Core-Projektvorlagen verwenden Kestrel standardmäßig. Der Vorlagencode in *Program.cs* ruft `CreateDefaultBuilder` auf, wodurch [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) im Hintergrund aufgerufen wird.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Wenn Sie die Kestrel-Optionen konfigurieren müssen, rufen Sie wie im folgenden Beispiel dargestellt `UseKestrel` in *Program.cs* auf:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Rufen Sie die Erweiterungsmethode [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) unter `WebHostBuilder` in Ihrer `Main`-Methode auf, und geben Sie dabei wie im folgenden Abschnitt dargestellt alle erforderlichen [Kestrel-Optionen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) an.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel-Optionen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Der Kestrel-Webserver verfügt über einschränkende Konfigurationsoptionen, die besonders nützlich bei Bereitstellungen mit Internetzugriff sind. Sie können folgende Grenzwerte festlegen:

- Maximale Anzahl der Clientverbindungen
- Maximale Größe des Anforderungstexts
- Minimale Datenrate des Anforderungstexts

Sie können diese und andere Einschränkungen in der `Limits`-Eigenschaft der [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)-Klasse festlegen. Die `Limits`-Eigenschaft enthält eine Instanz der [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)-Klasse. 

**Maximale Anzahl von Clientverbindungen**

Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann mithilfe von folgendem Code für die gesamte Anwendung festgelegt werden:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Es gibt einen separaten Grenzwert für Verbindungen, die von HTTP oder HTTPS auf ein anderes Protokoll aktualisiert wurden (z.B. auf eine WebSockets-Anforderung). Nachdem eine Verbindung aktualisiert wurde, zählt diese nicht mehr für den `MaxConcurrentConnections`-Grenzwert. 

Die maximale Anzahl von Verbindungen ist standardmäßig nicht begrenzt (NULL).

**Maximale Größe des Anforderungstexts**

Die maximale Größe des Anforderungstexts beträgt standardmäßig 30.000.000 Byte, also ungefähr 28,6 MB. 

Die empfohlene Methode zur Außerkraftsetzung des Grenzwerts in einer ASP.NET Core-MVC-App besteht im Verwenden des [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)-Attributs in einer Aktionsmethode:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Im folgenden Beispiel wird veranschaulicht, wie die Einschränkung, die für die gesamte Anwendung und sämltiche Anforderungen gültig ist, konfiguriert wird.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Sie können diese Einstellung für eine bestimmte Anforderung in Middleware außer Kraft setzen:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert einer Anforderung zu konfigurieren, nachdem die Anwendung bereits mit dem Lesen der Anforderung begonnen hat. Es gibt eine `IsReadOnly`-Eigenschaft, die Ihnen mitteilt, wenn sich die `MaxRequestBodySize`-Eigenschaft im schreibgeschützten Zustand befindet, also wenn der Grenzwert nicht mehr konfiguriert werden kann.

**Minimale Datenrate des Anforderungstexts**

Kestrel überprüft sekündlich, ob Daten mit der angegebenen Rate in Bytes/Sekunde eingehen. Wenn die Rate den mindestens erforderlichen Wert unterschreitet, wird für die Verbindung wegen Timeout getrennt. Bei der Toleranzperiode handelt es sich um die Zeitspanne, die Kestrel dem Client gewährt, um die Senderate auf den mindestens erforderlichen Wert zu erhöhen. Die Rate wird währenddessen nicht überprüft. Diese Toleranzperiode beugt dem Trennen von Verbindungen vor, die Daten aufgrund eines langsamen TCP-Starts anfänglich mit einer niedrigen Rate senden.

Die mindestens erforderliche Rate beträgt standardmäßig 240 Bytes/Sekunde mit einer Toleranzperiode von 5 Sekunden.

Für die Antwort gilt ebenfalls eine mindestens erforderliche Rate. Der Code zum Festlegen des Grenzwerts für Anforderung und Antwort ist abgesehen von `RequestBody` oder `Response` in den Namen von Eigenschaften und Schnittstelle identisch. 

In diesem Beispiel wird veranschaulicht, wie Sie die mindestens erforderlichen Datenraten in *Program.cs* konfigurieren:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Sie können die Raten pro Anforderung in Middleware konfigurieren:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Weitere Informationen zu anderen Kestrel-Optionen finden Sie in folgenden Klassen:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Weitere Informationen zu Kestrel-Optionen finden Sie unter [KestrelServerOptions class (KestrelServerOptions-Klasse)](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Endpunktkonfiguration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden. Sie können URL-Präfixe und Ports konfigurieren, denen Kestrel lauschen soll, indem Sie die `Listen`- oder `ListenUnixSocket`-Methode unter `KestrelServerOptions` aufrufen. (`UseUrls`, das Befehlszeilenargument `urls` und die Umgebungsvariable „ASPNETCORE_URLS“ funktionieren ebenfalls, verfügen jedoch über Einschränkungen, die [im Verlauf dieses Artikels](#useurls-limitations) erläutert werden.)

**Binden an einen TCP-Socket**

Durch die `Listen`-Methode können Sie eine Bindung an einen TCP-Socket erstellen, und durch den Lambdaausdruck einer Option können Sie ein SSL-Zertifikat konfigurieren:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Beachten Sie, wie SSL in diesem Beispiel mithilfe von [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) für einen bestimmten Endpunkt konfiguriert wird. Sie können die gleiche API verwenden, um andere Kestrel-Einstellungen für bestimmte Endpunkte zu konfigurieren.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Binden an einen Unix-Socket**

Wie in diesem Beispiel dargestellt können Sie einem Unix-Socket lauschen, um eine verbesserte Leistung mit Nginx zu erzielen:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Wenn Sie die Portnummer 0 angeben, erstellt Kestrel eine dynamische Bindung an einen verfügbaren Port. Das folgende Beispiel veranschaulicht, wie bestimmt werden kann, für welchen Port Kestrel zur Laufzeit eine Bindung erstellt hat:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls-Einschränkungen**

Sie können Endpunkte konfigurieren, indem Sie die `UseUrls`-Methode aufrufen oder das Befehlszeilenargument `urls` oder die Umgebungsvariable „ASPNETCORE_URLS“ verwenden. Diese Methoden sind nützlich, wenn Ihr Code mit anderen Servern als Kestrel funktionieren soll. Beachten Sie jedoch folgende Einschränkungen:

* Sie können SSL mit diesen Methoden nicht verwenden.
* Wenn Sie die `Listen`-Methode und `UseUrls` verwenden, setzt der `Listen`-Endpunkt die `UseUrls`-Endpunkte außer Kraft.

**Endpunktkonfiguration für IIS**

Wenn Sie IIS verwenden, setzen die URL-Bindungen für IIS alle Bindungen außer Kraft, die Sie durch Aufrufen von `Listen` oder `UseUrls` festlegen. Weitere Informationen finden Sie unter [Introduction to ASP.NET Core Module (Einführung in ASP.NET Core-Module)](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden. Sie können URL-Präfixe und Ports konfigurieren, denen Kestrel lauschen soll, indem Sie die Erweiterungsmethode `UseUrls`, das Befehlszeilenargument `urls` oder das ASP.NET Core-Konfigurationssystem verwenden. Weitere Informationen zu diesen Methoden finden Sie unter [Hosting](../../fundamentals/hosting.md). Weitere Informationen zur Funktionsweise der URL-Bindung bei der Verwendung von IIS als Reverseproxy finden Sie unter [ASP.NET Core-Module](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>URL-Präfixe

Wenn Sie `UseUrls` aufrufen oder das Befehlszeilenargument `urls` oder die Umgebungsvariable „ASPNETCORE_URLS“ verwenden, kann das URL-Präfix in folgenden Formaten vorliegen. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Nur HTTP-URL-Präfixe sind gültig. Kestrel unterstützt SSL nicht, wenn Sie URL-Bindungen mithilfe von `UseUrls` konfigurieren.

* IPv4-Adresse mit Portnummer

  ```
  http://65.55.39.10:80/
  ```

  Bei 0.0.0.0 handelt es sich um einen Sonderfall, für den eine Bindung an alle IPv4-Adressen erfolgt.


* IPv6-Adresse mit Portnummer

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] stellt das Äquivalent von IPv6 zu 0.0.0.0 für IPv4 dar.


* Hostname mit Portnummer

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Hostnamen, * und + sind nicht spezifisch. Für alle Elemente, die nicht als IP-Adresse oder „localhost“ erkannt werden, wird eine Bindung an alle IPv4- und IPv6-IP-Adressen erstellt. Wenn Sie verschiedene Hostnamen an verschiedene ASP.NET Core-Anwendung auf demselben Port binden, verwenden Sie [HTTP.sys](httpsys.md) oder einen Reverseproxyserver wie IIS, Nginx oder Apache.

* „localhost“ als Name mit Portnummer oder Loopback-IP mit Portnummer

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Wenn `localhost` angegeben ist, versucht Kestrel, eine Bindung zu IPv4- und IPv6-Loopback-Schnittstellen zu erstellen. Wenn der erforderliche Port von einem anderen Dienst auf einer der Loopback-Schnittstellen verwendet wird, tritt beim Starten von Kestrel ein Fehler auf. Wenn eine der Loopback-Schnittstellen aus anderen Gründen nicht verfügbar ist (meistens durch die fehlende Unterstützung von IPv6), protokolliert Kestrel eine Warnung. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4-Adresse mit Portnummer

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  Bei 0.0.0.0 handelt es sich um einen Sonderfall, für den eine Bindung an alle IPv4-Adressen erfolgt.


* IPv6-Adresse mit Portnummer

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] stellt das Äquivalent von IPv6 zu 0.0.0.0 für IPv4 dar.


* Hostname mit Portnummer

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Hostnamen, \* und + sind nicht spezifisch. Für alle Elemente, die nicht als IP-Adresse oder „localhost“ erkannt werden, wird eine Bindung an alle IPv4- und IPv6-IP-Adressen erstellt. Wenn Sie verschiedene Hostnamen an verschiedene ASP.NET Core-Anwendung auf demselben Port binden, verwenden Sie [WebListener](weblistener.md) oder einen Reverseproxyserver wie IIS, Nginx oder Apache.

* „localhost“ als Name mit Portnummer oder Loopback-IP mit Portnummer

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Wenn `localhost` angegeben ist, versucht Kestrel, eine Bindung zu IPv4- und IPv6-Loopback-Schnittstellen zu erstellen. Wenn der erforderliche Port von einem anderen Dienst auf einer der Loopback-Schnittstellen verwendet wird, tritt beim Starten von Kestrel ein Fehler auf. Wenn eine der Loopback-Schnittstellen aus anderen Gründen nicht verfügbar ist (meistens durch die fehlende Unterstützung von IPv6), protokolliert Kestrel eine Warnung. 

* Unix-Socket

  ```
  http://unix:/run/dan-live.sock  
  ```

**Port 0**

Wenn Sie die Portnummer 0 angeben, erstellt Kestrel eine dynamische Bindung an einen verfügbaren Port. Die Bindung an Port 0 ist für jeden Hostnamen oder jede IP-Adresse zulässig, außer für solche mit dem Namen `localhost`.

Das folgende Beispiel veranschaulicht, wie bestimmt werden kann, für welchen Port Kestrel zur Laufzeit eine Bindung erstellt hat:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**URL-Präfixe für SSL**

Achten Sie wie im Folgenden dargestellt darauf, URL-Präfixe mit `https:` einzuschließen, wenn Sie die Erweiterungsmethode `UseHttps` aufrufen.

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> HTTPS und HTTP können nicht auf demselben Port gehostet werden.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Beispiel-App für 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel-Quellcode](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Beispiel-App für 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel-Quellcode](https://github.com/aspnet/KestrelHttpServer)

---
