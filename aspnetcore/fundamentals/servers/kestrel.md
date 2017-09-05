---
title: Kestrel webserverimplementierung in ASP.NET Core
author: tdykstra
description: "Führt ein Kestrel, die plattformübergreifende Webserver für ASP.NET Core auf Libuv basieren."
keywords: "ASP.NET Core, Kestrel, Libuv, Url-Präfixe"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451a548403c8fa0ed2befeb6969a3ee28fe34790
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Einführung in die Kestrel webserverimplementierung in ASP.NET Core

Durch [Tom Dykstra](http://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), und [Stephen Halter](https://twitter.com/halter73)

Kestrel ist eine plattformübergreifende [Webserver für ASP.NET Core](index.md) basierend auf [Libuv](https://github.com/libuv/libuv), eine plattformübergreifende asynchrone e/a-Bibliothek. Kestrel ist der Webserver, der standardmäßig in ASP.NET Core-Projektvorlagen enthalten ist. 

Kestrel unterstützt die folgenden Funktionen:

  * HTTPS
  * Nicht transparente Upgrade zur Aktivierung [WebSockets](https://github.com/aspnet/websockets)
  * UNIX-Sockets für hohe Leistung hinter Nginx 

Kestrel wird unterstützt, auf allen Plattformen und Versionen, die .NET Core unterstützt.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Anzeigen oder Herunterladen von Beispielcode für 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Anzeigen oder Herunterladen von Beispielcode für 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Verwendung von Kestrel mit einer reverse-proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel können allein oder mit einem *Reverseproxyserver*, z. B. IIS, Nginx oder Apache. Ein reverse-Proxy-Server empfängt HTTP-Anforderungen aus dem Internet und an Kestrel weiterleitet, nachdem einige vorläufige Behandlung.

![Kestrel kommuniziert direkt mit dem Internet ohne einen reverse-Proxy-server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen reverse-Proxy-Server, z. B. IIS, Nginx oder Apache.](kestrel/_static/kestrel-to-internet.png)

Konfiguration &mdash; mit oder ohne einen reverse-Proxy-Server &mdash; kann auch verwendet werden, wenn Kestrel nur mit einem internen Netzwerk bereitgestellt wird.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn Ihre Anwendung nur Anforderungen von einem internen Netzwerk akzeptiert, können Sie Kestrel allein verwenden.

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

Wenn Sie Ihre Anwendung mit dem Internet verfügbar machen, müssen Sie IIS, Nginx oder Apache als verwenden eine *Reverseproxyserver*. Ein reverse-Proxy-Server empfängt HTTP-Anforderungen aus dem Internet und an Kestrel weiterleitet, nachdem einige vorläufige Behandlung.

![Kestrel kommuniziert indirekt mit dem Internet über einen reverse-Proxy-Server, z. B. IIS, Nginx oder Apache.](kestrel/_static/kestrel-to-internet.png)

Reverse-Proxy ist für Edge-Bereitstellungen (für den Datenverkehr aus dem Internet ausgesetzt) aus Sicherheitsgründen erforderlich. Die Versionen 1.x Kestrel besitzen keine vollständige Ergänzung der entsprechende Schutzmaßnahmen gegen Angriffe. Dies schließt jedoch nicht auf die entsprechenden Timeouts, Grenzwerte für sammlungsgröße und Limits für gleichzeitige Verbindungen beschränkt.

---

Reverseproxy erforderlich ist, wenn Sie mehrere Anwendungen verfügen, die gemeinsam dieselbe IP und Ports auf einem einzelnen Server ausführen. Die nicht direkt mit Kestrel arbeiten, da Kestrel Freigeben von derselben IP- und Port zwischen mehreren Prozessen nicht unterstützt. Wenn Sie Kestrel zum Lauschen an einem Port konfigurieren, verarbeitet sie Datenverkehr für diesen Port unabhängig von der Hostheader. Ein reverse-Proxy, der Ports gemeinsam verwenden, kann muss dann an Kestrel auf eine eindeutige IP- und -Port weiterleiten.

Auch wenn ein reverse-Proxy-Server nicht erforderlich ist, kann mithilfe einer eine gute Wahl aus anderen Gründen sein:

* Sie können die verfügbar gemachten Oberfläche einschränken.
* Es bietet eine optionale zusätzliche Ebene der Konfiguration und den gründlichen Schutz.
* Es kann eine bessere Leistung in die vorhandene Infrastruktur integrieren.
* Sie vereinfacht den Lastenausgleich und SSL-Einrichtung. Nur der reverse-Proxy-Server erfordert ein SSL-Zertifikat, und dieser Server mit Ihrer Anwendungsserver, auf dem internen Netzwerk über einfaches HTTP kommunizieren kann.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Gewusst wie: Kestrel in ASP.NET Core-apps verwendet wird.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) Paket enthalten ist, der [Microsoft.AspNetCore.All Metapackage](xref:fundamentals/metapackage).

ASP.NET Core-Projektvorlagen Kestrel wird standardmäßig verwendet. In *"Program.cs"*, ruft die Vorlage `CreateDefaultBuilder`, welche Aufrufe [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) im Hintergrund.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Wenn Sie Optionen für die Kestrel konfigurieren müssen, rufen Sie `UseKestrel` in *"Program.cs"* wie im folgenden Beispiel gezeigt:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installieren der [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet-Paket.

Rufen Sie die [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) Erweiterungsmethode auf `WebHostBuilder` in Ihrer `Main` Methode, dabei werden alle [Kestrel Optionen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , die Sie benötigen, wie im nächsten Abschnitt gezeigt.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel-Optionen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Der Webserver Kestrel hat Einschränkung Konfigurationsoptionen, die vor allem hilfreich bei Bereitstellungen mit Internetzugriff sind. Hier sind einige der Grenzen, die Sie festlegen können:

- Maximaler Client-Verbindungen
- Maximale Anforderungsgröße-Text
- Minimale Text Data-Anforderungsrate

Sie legen diese Einschränkungen und andere in der `Limits` Eigenschaft von der [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) Klasse. Die `Limits` Eigenschaft enthält eine Instanz der dem [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) Klasse. 

**Maximaler Client-Verbindungen**

Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann für die gesamte Anwendung durch den folgenden Code festgelegt werden:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Es wird eine separate Anzahl an Verbindungen, die auf ein anderes Protokoll (z. B. auf eine Anforderung WebSockets) über HTTP oder HTTPS aktualisiert wurden. Nachdem eine Verbindung aktualisiert wird, wird nicht gezählt gegen die `MaxConcurrentConnections` Grenzwert. 

Die maximale Anzahl von Verbindungen ist standardmäßig unbegrenzt (Null).

**Maximale Anforderungsgröße-Text**

Die Standardgröße der Höchstwert für die Anforderung Text ist 30,000,000 Bytes, also ungefähr 28.6 MB. 

Die empfohlene Methode zum Überschreiben Sie des Grenzwert von in einer ASP.NET-MVC-Anwendung Core ist die Verwendung der [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) Attribut auf eine Aktionsmethode:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Hier ist ein Beispiel, das veranschaulicht, wie die Einschränkung für die gesamte Anwendung, jede Anforderung zu konfigurieren:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Sie können die Einstellung für eine bestimmte Anforderung in Middleware überschreiben:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert für eine Anforderung zu konfigurieren, nach dem Start der Anwendung Lesen der Anforderung. Ist ein `IsReadOnly` -Eigenschaft, die Aufschluss darüber gibt Wenn die `MaxRequestBodySize` Eigenschaft befindet sich im schreibgeschützten Zustand, d. h., es ist zu spät so konfigurieren Sie den Grenzwert.

**Minimale Text Data-Anforderungsrate**

Kestrel überprüft pro Sekunde an, wenn Daten mit der angegebenen Rate in Bytes/Sekunde in stammt. Wenn die Rate der Minimum unterschreitet, wird die Verbindung ein Timeout. Der Toleranzperiode ist die Zeitspanne, Kestrel den Client seine Senderate bis zu mindestens erhöhen kann. die Rate wird während dieses Zeitraums nicht überprüft. Die Toleranzperiode wird vermieden, Verbindungen, die anfänglich für das Senden von Daten mit einer langsamen Rate aufgrund TCP langsam gestartet.

Der minimale Standardwert ist 240 Bytes/Sekunde, mit der eine Toleranzperiode von fünf Sekunden.

Ein Mindestprozentsatz gilt auch für die Antwort. Der Code aus, um die "Anforderungslimit" und die Obergrenze für ist identisch, außer dass `RequestBody` oder `Response` im Namen Eigenschaft und -Schnittstelle. 

Hier ist ein Beispiel, das zeigt, wie so konfigurieren Sie die minimale Datenraten in *"Program.cs"*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Sie können die Sätze pro Anforderung in Middleware konfigurieren:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Informationen zu den weiteren Optionen Kestrel finden Sie die folgenden Klassen:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Informationen zu Kestrel-Optionen finden Sie unter [KestrelServerOptions Klasse](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Endpunktkonfiguration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Standardmäßig ASP.NET Core bindet an `http://localhost:5000`. Konfigurieren von URL-Präfixe und Ports für Kestrel Abhören durch Aufrufen von `Listen` oder `ListenUnixSocket` Methoden auf `KestrelServerOptions`. (`UseUrls`, `urls` Befehlszeilenargument und die Umgebungsvariable ASPNETCORE_URLS funktionieren auch bieten jedoch die angegeben Einschränkungen [weiter unten in diesem Artikel](#useurls-limitations).)

**Binden Sie an einen TCP-socket**

Die `Listen` -Methode bindet ein TCP-Socket, und das Lambda einer Optionen können Sie ein SSL-Zertifikat zu konfigurieren:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Beachten Sie, wie in diesem Beispiel wird mithilfe von SSL für einen bestimmten Endpunkt konfiguriert [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Sie können die gleiche API verwenden, andere Kestrel für bestimmte Endpunkte konfigurieren.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Binden Sie an ein Unix-socket**

Sie können auf einem Unix-Socket für verbesserte Leistung mit Nginx, Lauschen wie im folgenden Beispiel gezeigt:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Wenn Sie die Portnummer 0 angeben, wird die Kestrel dynamisch auf einen verfügbaren Port gebunden. Das folgende Beispiel zeigt, wie Sie den Port ermittelt haben Kestrel tatsächlich zur Laufzeit gebunden:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls Einschränkungen**

Konfigurieren Sie Endpunkte durch Aufrufen der `UseUrls` -Methode oder mithilfe der `urls` Befehlszeilenargument oder die Umgebungsvariable ASPNETCORE_URLS. Diese Methoden sind nützlich, wenn Sie Code mit anderen Servern als Kestrel arbeiten möchten. Bedenken Sie jedoch, die diese Einschränkungen:

* Sie können keine SSL mit diesen Methoden.
* Wenn Sie beides verwenden die `Listen` Methode und `UseUrls`, die `Listen` Endpunkte überschreiben die `UseUrls` Endpunkte.

**Dienstendpunkt-Konfiguration für IIS**

Wenn Sie IIS verwenden, wird die URL-Bindungen für IIS außer Kraft setzen alle Bindungen, die Sie festgelegt werden, indem entweder die `Listen` oder `UseUrls`. Weitere Informationen finden Sie unter [Einführung in ASP.NET Core-Modul](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Standardmäßig ASP.NET Core bindet an `http://localhost:5000`. Sie können konfigurieren, URL-Präfixe und Ports für Kestrel Abhören mithilfe der `UseUrls` Erweiterungsmethode, die `urls` Befehlszeilenargument oder das Konfigurationssystem ASP.NET Core. Weitere Informationen zu diesen Methoden finden Sie unter [Hosting](../../fundamentals/hosting.md). Informationen zur Funktionsweise der URL-Bindung bei der Verwendung von IIS als Reverseproxy finden Sie unter [ASP.NET Core-Modul](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>URL-Präfixe

Beim Aufrufen `UseUrls` , oder verwenden Sie die `urls` Befehlszeilenargument oder ASPNETCORE_URLS-Umgebungsvariablen angegeben, können die URL-Präfixe in einem der folgenden Formate sein. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Nur HTTP-URL-Präfixe sind gültig. Kestrel unterstützt keine SSL beim Konfigurieren von URL-Bindungen mit `UseUrls`.

* IPv4-Adresse mit Port-Nummer

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 ist ein Sonderfall, der an alle IPv4-Adressen gebunden.


* IPv6-Adresse mit Port-Nummer

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] ist das Äquivalent IPv6 IPv4 0.0.0.0.


* Hostname und Portnummer

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Hostnamen, *, und + sind nicht spezifisch. Elemente, die keinen erkannten IP-Adresse oder "Localhost" wird für alle IPv4- und IPv6-IP-Adressen gebunden. Wenn Sie unterschiedliche Hostnamen an verschiedenen ASP.NET Core-Anwendungen auf demselben Port binden, verwenden [HTTP.sys](httpsys.md) oder einen reverse-Proxy-Server wie IIS, Nginx oder Apache.

* Name von "Localhost" mit Portnummer Port Nummer oder Loopback-IP

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Wenn `localhost` angegeben wird, Kestrel versucht, IPv4 und IPv6-Loopback-Schnittstellen zu binden. Wenn es sich bei der angeforderte Port verwendet, die von einem anderen Dienst entweder Loopback-Schnittstelle wird, nicht Kestrel gestartet. Wenn der Loopback-Schnittstellen für andere Zwecke nicht verfügbar ist (die meisten häufig verwendet werden, weil IPv6 nicht unterstützt wird), Kestrel protokolliert eine Warnung. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4-Adresse mit Port-Nummer

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 ist ein Sonderfall, der an alle IPv4-Adressen gebunden.


* IPv6-Adresse mit Port-Nummer

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] ist das Äquivalent IPv6 IPv4 0.0.0.0.


* Hostname und Portnummer

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Hostnamen, \*, und + sind spezielle nicht. Einen erkannten IP-Adresse oder "Localhost" nicht für alle IPv4- und IPv6-IP-Adressen gebunden. Wenn Sie unterschiedliche Hostnamen an verschiedenen ASP.NET Core-Anwendungen auf demselben Port binden, verwenden [WebListener](weblistener.md) oder einen reverse-Proxy-Server wie IIS, Nginx oder Apache.

* Name von "Localhost" mit Portnummer Port Nummer oder Loopback-IP

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Wenn `localhost` angegeben wird, Kestrel versucht, IPv4 und IPv6-Loopback-Schnittstellen zu binden. Wenn es sich bei der angeforderte Port verwendet, die von einem anderen Dienst entweder Loopback-Schnittstelle wird, nicht Kestrel gestartet. Wenn der Loopback-Schnittstellen für andere Zwecke nicht verfügbar ist (die meisten häufig verwendet werden, weil IPv6 nicht unterstützt wird), Kestrel protokolliert eine Warnung. 

* UNIX-socket

  ```
  http://unix:/run/dan-live.sock  
  ```

**Port 0**

Wenn Sie die Portnummer 0 angeben, wird die Kestrel dynamisch auf einen verfügbaren Port gebunden. Bindung an port 0 kann für alle Hostnamen oder die IP-Adresse außer für `localhost` Name.

Das folgende Beispiel zeigt, wie Sie den Port ermittelt haben Kestrel tatsächlich zur Laufzeit gebunden:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**URL-Präfixe für SSL**

Achten Sie darauf, dass Sie mit URL-Präfixe enthalten `https:` beim Aufrufen der `UseHttps` Erweiterungsmethode, wie unten dargestellt.

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
> HTTPS und HTTP darf nicht auf demselben Port gehostet werden.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Beispiel-app für 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel-Quellcode](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Beispiel-app für 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel-Quellcode](https://github.com/aspnet/KestrelHttpServer)

---
