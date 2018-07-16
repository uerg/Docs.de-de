---
title: Implementierung des Webservers Kestrel in ASP.NET Core
author: rick-anderson
description: Einführung in Kestrel, dem plattformübergreifenden Webserver für ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126942"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implementierung des Webservers Kestrel in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) und [Stephen Halter](https://twitter.com/halter73)

Einführung in Kestrel, dem plattformübergreifenden [Webserver für ASP.NET Core](xref:fundamentals/servers/index). Kestrel ist der Webserver, der standardmäßig in ASP.NET Core-Projektvorlagen enthalten ist.

Kestrel unterstützt die folgenden Features:

* HTTPS
* Nicht transparente Upgrades, die zum Aktivieren von [WebSockets](https://github.com/aspnet/websockets) verwendet werden
* Unix-Sockets für eine hohe Leistung im Hintergrund von Nginx

Kestrel wird auf allen Plattformen und für alle Versionen unterstützt, die .NET Core unterstützt.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Verwenden von Kestrel mit einem Reverseproxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Sie können Kestrel als eigenständigen Webserver oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn eine App Anforderungen nur aus einem internen Netzwerk akzeptiert, kann Kestrel direkt als Server der App verwendet werden.

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

Wenn Sie Ihre App im Internet zur Verfügung stellen, verwenden Sie IIS, Nginx oder Apache als *Reverseproxyserver*. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Ein Reverseproxy ist aus Sicherheitsgründen für Edge-Bereitstellungen erforderlich, die Datenverkehr aus dem Internet ausgesetzt sind. Die 1.x Versionen von Kestrel besitzen keine umfassenden Schutzmaßnahmen gegenüber Angriffen, z.B. entsprechende Timeouts, Größenbeschränkungen und Beschränkungen gleichzeitiger Verbindungen.

---

Ein Reverseproxy ist erforderlich, wenn mehrere Apps mit der gleichen IP und dem gleichen Port auf einem einzelnen Server ausgeführt werden. Kestrel unterstützt dieses Szenario nicht, da Kestrel nicht das Freigeben der gleichen IP und des gleichen Ports für mehrere Prozesse unterstützt. Wenn Kestrel dafür konfiguriert ist, einen Port zu überwachen, verarbeitet Kestrel den gesamten Datenverkehr von diesem Port unabhängig vom Hostheader der Anforderungen. Ein Reverseproxy, der Ports freigeben kann, kann Anforderungen an Kestrel über eine eindeutige IP und einen eindeutigen Port weiterleiten.

Auch wenn kein Reverseproxyserver erforderlich ist, kann die Verwendung eines solchen aus anderen Gründen empfehlenswert sein:

* Er kann die zur Verfügung gestellten öffentlichen Oberflächen der von ihm gehosteten Apps einschränken.
* Er stellt eine zusätzliche Ebene für Konfiguration und Schutz bereit.
* Er kann sich besser in die vorhandene Infrastruktur integrieren.
* Er vereinfacht den Lastenausgleich und die Konfiguration von SSL. Nur der Reverseproxyserver erfordert ein SSL-Zertifikat. Dieser Server kann mithilfe von HTTP mit Ihren Anwendungsservern auf dem internen Netzwerk kommunizieren.

> [!WARNING]
> Wenn Sie keinen Reverseproxy mit aktivierter Host-Filterung verwenden, müssen Sie die [Host-Filterung](#host-filtering) aktivieren.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Verwenden von Kestrel in ASP.NET Core-Apps

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Das Paket [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) ist im [Metapaket Microsoft.AspNetCore.App] (xref:fundamentals/metapackage-app) enthalten (ASP.NET Core 2.1 oder höher).

ASP.NET Core-Projektvorlagen verwenden Kestrel standardmäßig. Der Vorlagencode in *Program.cs* ruft [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) auf, wodurch [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) im Hintergrund aufgerufen wird.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Rufen Sie die Erweiterungsmethode [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) unter [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in der `Main`-Methode auf, und geben Sie dabei wie im folgenden Abschnitt dargestellt alle erforderlichen [Kestrel-Optionen](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) an.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel-Optionen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Der Kestrel-Webserver verfügt über einschränkende Konfigurationsoptionen, die besonders nützlich bei Bereitstellungen mit Internetzugriff sind. Einige der wichtigen Einschränkungen können angepasst werden:

* Maximale Anzahl der Clientverbindungen
* Maximale Größe des Anforderungstexts
* Minimale Datenrate des Anforderungstexts

Legen Sie diese oder andere Einschränkungen in der Eigenschaft [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) der Klasse [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) fest. Die `Limits`-Eigenschaft enthält eine Instanz der [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)-Klasse.

**Maximale Anzahl der Clientverbindungen**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann mithilfe von folgendem Code für die gesamte App festgelegt werden:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

Es gibt einen separaten Grenzwert für Verbindungen, die von HTTP oder HTTPS auf ein anderes Protokoll aktualisiert wurden (z.B. auf eine WebSockets-Anforderung). Nachdem eine Verbindung aktualisiert wurde, zählt diese nicht mehr für den `MaxConcurrentConnections`-Grenzwert.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

Die maximale Anzahl von Verbindungen ist standardmäßig nicht begrenzt (NULL).

**Maximale Größe des Anforderungstexts**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

Die maximale Größe des Anforderungstexts beträgt standardmäßig 30.000.000 Byte, also ungefähr 28,6 MB.

Die empfohlene Methode zur Außerkraftsetzung des Grenzwerts in einer ASP.NET Core-MVC-App besteht im Verwenden des [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute)-Attributs in einer Aktionsmethode:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Im folgenden Beispiel wird veranschaulicht, wie die Einschränkungen auf jeder Anforderung für die App konfiguriert werden:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

Sie können diese Einstellung für eine bestimmte Anforderung in Middleware außer Kraft setzen:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert einer Anforderung zu konfigurieren, nachdem die App bereits mit dem Lesen der Anforderung begonnen hat. Es gibt eine `IsReadOnly`-Eigenschaft, die angibt, wenn sich die `MaxRequestBodySize`-Eigenschaft im schreibgeschützten Zustand befindet, also wenn der Grenzwert nicht mehr konfiguriert werden kann.

**Minimale Datenrate des Anforderungstexts**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel überprüft sekündlich, ob Daten mit der angegebenen Rate in Bytes/Sekunde eingehen. Wenn die Rate den mindestens erforderlichen Wert unterschreitet, wird für die Verbindung wegen Timeout getrennt. Bei der Toleranzperiode handelt es sich um die Zeitspanne, die Kestrel dem Client gewährt, um die Senderate auf den mindestens erforderlichen Wert zu erhöhen. Die Rate wird währenddessen nicht überprüft. Diese Toleranzperiode beugt dem Trennen von Verbindungen vor, die Daten aufgrund eines langsamen TCP-Starts anfänglich mit einer niedrigen Rate senden.

Die mindestens erforderliche Rate beträgt standardmäßig 240 Bytes/Sekunde mit einer Toleranzperiode von 5 Sekunden.

Für die Antwort gilt ebenfalls eine mindestens erforderliche Rate. Der Code zum Festlegen des Grenzwerts für Anforderung und Antwort ist abgesehen von `RequestBody` oder `Response` in den Namen von Eigenschaften und Schnittstelle identisch.

In diesem Beispiel wird veranschaulicht, wie Sie die mindestens erforderlichen Datenraten in *Program.cs* konfigurieren:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

Sie können die Raten pro Anforderung in Middleware konfigurieren:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

Weitere Informationen zu anderen Kestrel-Optionen und -Einschränkungen finden Sie unter:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Weitere Informationen zu Kestrel-Optionen und -Einschränkungen finden Sie unter:

* [KestrelServerOptions class (KestrelServerOptions-Klasse)](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>Endpunktkonfiguration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden. Rufen Sie die Methoden [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oder [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) in [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) auf, um URL-Präfixe und Ports für Kestrel zu konfigurieren. `UseUrls`, das Befehlszeilenargument `--urls`, der Hostkonfigurationsschlüssel `urls` und die Umgebungsvariable `ASPNETCORE_URLS` funktionieren ebenfalls, verfügen jedoch über Einschränkungen, die im Verlauf dieses Abschnitts erläutert werden.

Der Hostkonfigurationsschlüssel `urls` muss von der Hostkonfiguration stammen, nicht von der App-Konfiguration. Das Hinzufügen eines `urls`-Schlüssels und -Werts in *appsettings.json* hat keinen Einfluss auf die Hostkonfiguration, da der Host bereits vollständig initialisiert ist, wenn die Konfiguration aus der Konfigurationsdatei gelesen wird. Allerdings kann ein `urls`-Schlüssel in *appsettings.json* mit [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) auf dem Hostbuilder verwendet werden, um den Host zu konfigurieren:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Standardmäßig wird ASP.NET Core an Folgendes gebunden:

* `http://localhost:5000`
* `https://localhost:5001` (wenn ein lokales Entwicklungszertifikat vorhanden ist)

Ein Entwicklungszertifikat wird erstellt:

* Wenn das [.NET Core SDK](/dotnet/core/sdk) installiert wird.
* Das [dev-cert-Tool](xref:aspnetcore-2.1#https) wird zum Erstellen eines Zertifikats verwendet.

Einige Browser erfordern, dass Sie dem Browser die explizite Berechtigung erteilen, dem lokalen Entwicklungszertifikat zu vertrauen.

ASP.NET Core 2.1 und spätere Projektvorlagen konfigurieren Apps, damit sie standardmäßig auf HTTPS ausgeführt werden und die [HTTPS-Umleitung und HSTS-Unterstützung](xref:security/enforcing-ssl) enthalten.

Rufen Sie die Methoden [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oder [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) in [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) auf, um URL-Präfixe und Ports für Kestrel zu konfigurieren.

`UseUrls`, das Befehlszeilenargument `--urls`, der Hostkonfigurationsschlüssel `urls` und die Umgebungsvariable `ASPNETCORE_URLS` funktionieren ebenfalls, verfügen jedoch über Einschränkungen, die im Verlauf dieses Abschnitts erläutert werden (Ein Standardzertifikat muss für die HTTPS-Endpunktkonfiguration verfügbar sein).

ASP.NET Core 2.1 `KestrelServerOptions`-Konfiguration:

**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**  
Gibt die Konfiguration von `Action` zum Ausführen von jedem angegebenen Endpunkt an. Mehrmalige Aufrufe von `ConfigureEndpointDefaults` ersetzen vorherige Instanzen von `Action` mit der zuletzt angegebenen `Action`.

**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**  
Gibt die Konfiguration von `Action` zum Ausführen von jedem HTTPS-Endpunkt an. Mehrmalige Aufrufe von `ConfigureHttpsDefaults` ersetzen vorherige Instanzen von `Action` mit der zuletzt angegebenen `Action`.

**Configure(IConfiguration)**  
Erstellt einen Konfigurationsladeprogramm für das Einrichten von Kestrel, was [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) als Eingabe erfordert. Die Konfiguration muss auf den Konfigurationsabschnitt für Kestrel festgelegt werden.

**ListenOptions.UseHttps**  
Konfiguriert Kestrel zur Verwendung von HTTPS.

`ListenOptions.UseHttps`-Erweiterungen:

* `UseHttps`: Konfiguriert Kestrel zur Verwendung von HTTPS mit dem Standardzertifikat. Löst eine Ausnahme aus, wenn kein Standardzertifikat konfiguriert ist.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps`-Parameter:

* `filename` entspricht dem Pfad und Dateinamen einer Zertifikatdatei relativ zu dem Verzeichnis, das die Inhaltsdateien der App enthält.
* `password` ist das für den Zugriff auf die X.509-Zertifikatsdaten erforderliche Kennwort.
* `configureOptions` ist eine `Action` zum Konfigurieren von `HttpsConnectionAdapterOptions`. Gibt `ListenOptions` zurück.
* `storeName` ist der Zertifikatspeicher, aus dem das Zertifikat geladen wird.
* `subject` ist der Name des Antragstellers für das Zertifikat.
* `allowInvalid` gibt an, ob ungültige Zertifikate berücksichtigt werden sollten, z.B. selbstsignierte Zertifikate.
* `location` ist der Speicherort, aus dem das Zertifikat geladen wird.
* `serverCertificate` ist das X.509-Zertifikat.

In der Produktion muss HTTPS explizit konfiguriert sein. Zumindest muss ein Standardzertifikat angegeben werden.

Die im Folgenden beschriebenen unterstützten Konfigurationen:

* Keine Konfiguration
* Ersetzen des Standardzertifikats aus der Konfiguration
* Ändern des Standards im Code

*Keine Konfiguration*

Kestrel überwacht `http://localhost:5000` und `https://localhost:5001` (wenn ein Standardzertifikat verfügbar ist).

Verwenden Sie Folgendes zum Angeben der URLs:

* Die Umgebungsvariable `ASPNETCORE_URLS`
* Das Befehlszeilenargument `--urls`
* Den Hostkonfigurationsschlüssel `urls`
* Die Erweiterungsmethode `UseUrls`

Weitere Informationen finden Sie unter [Server-URLs](xref:fundamentals/host/web-host#server-urls) und [Außerkraftsetzen der Konfiguration](xref:fundamentals/host/web-host#override-configuration).

Der Wert, der mit diesen Ansätzen angegeben wird, kann mindestens ein HTTP- oder HTTPS-Endpunkt sein (HTTPS wenn ein Standardzertifikat verfügbar ist). Konfigurieren Sie den Wert als eine durch Semikolons getrennte Liste (z.B. `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Ersetzen des Standardzertifikats aus der Konfiguration*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ruft standardmäßig `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` zum Laden der Kestrel-Konfiguration auf. Ein Standardkonfigurationsschema für HTTPS-App-Einstellungen ist für Kestrel verfügbar. Konfigurieren Sie mehrere Endpunkte, einschließlich der zu verwendenden URLs und Zertifikate aus einer Datei auf dem Datenträger oder einem Zertifikatspeicher.

Die Vorgehensweise im folgenden *appsettings.json*-Beispiel:

* Legen Sie **AllowInvalid** auf `true` fest, um die Verwendung von ungültigen Zertifikaten zu erlauben (z.B. selbstsignierte Zertifikate).
* Jeder HTTPS-Endpunkt, der kein Zertifikat angibt (im folgenden Beispiel der Endpunkt **HttpsDefaultCert**), greift auf das unter **Zertifikate**>**Standard** festgelegte Zertifikat oder das Entwicklungszertifikat zurück.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Alternativ zur Verwendung von **Pfad** und **Kennwort** für alle Zertifikatknoten können Sie das Zertifikat mithilfe von Zertifikatspeicherfeldern angeben. Das Zertifikat unter **Zertifikate** > **Standard** kann beispielweise wie folgt angegeben werden:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Schema-Hinweise:

* Bei den Namen der Endpunkte wird die Groß-/Kleinschreibung nicht beachtet. Zum Beispiel sind `HTTPS` und `Https` gültig.
* Der Parameter `Url` ist für jeden Endpunkt erforderlich. Das Format für diesen Parameter ist identisch mit dem allgemeinen Konfigurationsparameter `Urls`, mit der Ausnahme, dass er auf einen einzelnen Wert begrenzt ist.
* Diese Endpunkte ersetzen die in der allgemeinen `Urls`-Konfiguration festgelegten Endpunkte, anstatt zu ihnen hinzuzufügen. Endpunkte, die über `Listen` in Code definiert werden, werden den im Konfigurationsabschnitt definierten Endpunkten hinzugefügt.
* Der `Certificate`-Abschnitt ist optional. Wenn der `Certificate`-Abschnitt nicht angegeben ist, werden die in früheren Szenarios definierten Standardwerte verwendet. Wenn keine Standardwerte verfügbar sind, löst der Server eine Ausnahme aus und startet nicht.
* Der `Certificate`-Abschnitt unterstützt die Zertifikate **Pfad**&ndash;**Kennwort** und **Subject**&ndash;**Store** (Antragsteller–Speicher).
* Auf diese Weise kann eine beliebige Anzahl von Endpunkten definiert werden, solange Sie keine Portkonflikte verursachen.
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` gibt `KestrelConfigurationLoader` mit der Methode `.Endpoint(string name, options => { })` zurück, die dazu verwendet werden kann, die Einstellungen eines Endpunkts zu ergänzen:

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Außerdem können Sie auch direkt auf `KestrelServerOptions.ConfigurationLoader` zugreifen, um ein vorhandenes Ladeprogramm zu durchlaufen, z.B. das von [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bereitgestellten Ladeprogramm.

* Der Konfigurationsabschnitt ist für jeden Endpunkt in den Optionen der Methode `Endpoint` verfügbar, sodass benutzerdefinierte Einstellungen gelesen werden können.
* Mehrere Konfigurationen können durch erneutes Aufrufen von `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` mit einem anderen Abschnitt geladen werden. Sofern `Load` nicht in einer vorherigen Instanz explizit aufgerufen wird, wird nur die letzte Konfiguration verwendet. Das Metapaket ruft `Load` nicht auf, sodass der Abschnitt mit der Standardkonfiguration ersetzt werden kann.
* `KestrelConfigurationLoader` spiegelt die API-Familie `Listen` von `KestrelServerOptions` als `Endpoint`-Überladungen, weshalb Code und Konfigurationsendpunkte am selben Ort konfiguriert werden können. Diese Überladungen verwenden keine Namen und nutzen nur Standardeinstellungen aus der Konfiguration.

*Ändern des Standards im Code*

`ConfigureEndpointDefaults` und `ConfigureHttpsDefaults` können zum Ändern der Standardeinstellungen für `ListenOptions` und `HttpsConnectionAdapterOptions` verwendet werden, einschließlich der Standardzertifikate, die im vorherigen Szenario festgelegt wurden. `ConfigureEndpointDefaults` und `ConfigureHttpsDefaults` sollten aufgerufen werden, bevor Endpunkte konfiguriert werden.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Kestrel-Unterstützung für SNI*

Die [Servernamensanzeige (SNI)](https://tools.ietf.org/html/rfc6066#section-3) kann zum Hosten mehrerer Domänen auf der gleichen IP-Adresse und dem gleichen Port verwendet werden. Damit die Servernamensanzeige funktioniert, sendet der Client während des TLS-Handshakes den Hostnamen für die sichere Sitzung an den Server, sodass der Server das richtige Zertifikat bereitstellen kann. Der Client verwendet das beigestellte Zertifikat für die verschlüsselte Kommunikation mit dem Server während der sicheren Sitzung nach dem TLS-Handshake.

Kestrel unterstützt die Servernamensanzeige über den `ServerCertificateSelector`-Rückruf. Der Rückruf wird für jede Verbindung einmal aufgerufen, um der App zu ermöglichen, den Hostnamen zu überprüfen und das entsprechende Zertifikat auszuwählen.

Für die Unterstützung der Servernamensanzeige benötigen Sie Folgendes:

* Wird auf dem Zielframework `netcoreapp2.1` ausgeführt. Der Rückruf wird auf `netcoreapp2.0` und `net461` aufgerufen, `name` ist jedoch immer `null`. `name` ist auch `null`, wenn der Client den Hostnamenparameter nicht im TLS-Handshake angibt.
* Alle Websites werden in derselben Kestrel-Instanz ausgeführt. Kestrel unterstützt ohne Reverseproxy keine gemeinsame IP-Adresse und keinen gemeinsamen Port für mehrere Instanzen.

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

**Binden an einen TCP-Socket**

Die [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen)-Methode wird an ein TCP-Socket gebunden, und ein Lambdaausdruck einer Option lässt die Konfiguration des SSL-Zertifikats zu:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

Im Beispiel wird SSL für einen Endpunkt mit [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) konfiguriert. Verwenden Sie die gleiche API zum Konfigurieren anderer Kestrel-Einstellungen für bestimmte Endpunkte.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Binden an einen Unix-Socket**

Lauschen Sie einem Unix-Socket mithilfe von [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket), um wie im folgenden Beispiel eine bessere Leistung mit Nginx zu erzielen:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Wenn die Portnummer `0` angegeben wird, wird Kestrel dynamisch an einen verfügbaren Port gebunden. Im folgenden Beispiel wird veranschaulicht, wie bestimmt werden kann, für welchen Port Kestrel zur Laufzeit eine Bindung erstellt hat:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Wenn die App ausgeführt wird, gibt das Ausgabefenster der Konsole den dynamischen Port an, über den die App erreicht werden kann:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

**UseUrls, Befehlszeilenargument „--urls“, Hostkonfigurationsschlüssel „urls“und die Einschränkungen der Umgebungsvariable ASPNETCORE_URLS**

Konfigurieren Sie Endpunkte mithilfe der folgenden Ansätze:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* Befehlszeilenargument `--urls`
* Hostkonfigurationsschlüssel `urls`
* Umgebungsvariable `ASPNETCORE_URLS`

Diese Methoden sind nützlich, wenn Ihr Code mit anderen Servern als Kestrel funktionieren soll. Beachten Sie jedoch folgende Einschränkungen:

* SSL kann nicht mit diesen Ansätzen verwendet werden, außer ein Standardzertifikat wird in der HTTPS-Endpunktkonfiguration angegeben (z.B. wenn Sie wie zuvor in diesem Artikel gezeigt die `KestrelServerOptions`-Konfiguration oder eine Konfigurationsdatei verwenden).
* Wenn die Ansätze `Listen` und `UseUrls` gleichzeitig verwendet werden, überschreiben die `Listen`-Endpunkte die `UseUrls`-Endpunkte.

**IIS-Endpunktkonfiguration**

Bei der Verwendung von IIS werden die URL-Bindungen für IIS-Überschreibungsbindungen durch `Listen` oder `UseUrls` festgelegt. Weitere Informationen finden Sie im Artikel [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden. Konfigurieren Sie URL-Präfixe und Ports für Kestrel durch Verwendung von Folgendem:

* Erweiterungsmethode [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)
* Befehlszeilenargument `--urls`
* Hostkonfigurationsschlüssel `urls`
* ASP.NET Core-Konfigurationssystem, einschließlich der Umgebungsvariable `ASPNETCORE_URLS`

Weitere Informationen zu diesen Methoden finden Sie unter [Hosting](xref:fundamentals/host/index).

**IIS-Endpunktkonfiguration**

Bei der Verwendung von IIS werden die URL-Bindungen für IIS-Überschreibungsbindungen durch `UseUrls` festgelegt. Weitere Informationen finden Sie im Artikel [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Transportkonfiguration

Mit dem Release von ASP.NET Core 2.1 basiert der Standardtransport von Kestrel nicht mehr auf Libuv, sondern auf verwalteten Sockets. Dies stellt einen Breaking Change für ASP.NET Core 2.0-Apps dar, die auf Version 2.1 upgraden, [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) aufrufen und von einem der folgenden Pakete abhängig sind:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direkter Paketverweis)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Gehen Sie bei Projekten mit ASP.NET Core 2.1 oder höher, die das [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) verwenden und für die die Verwendung von Libuv erforderlich ist, wie folgt vor:

* Fügen Sie für das [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/)-Paket eine Abhängigkeit auf die Projektdatei der App hinzu:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* Rufen Sie [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) auf:

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>URL-Präfixe

Bei der Verwendung von `UseUrls`, dem Befehlszeilenargument `--urls`, dem Hostkonfigurationsschlüssel `urls` oder der Umgebungsvariable `ASPNETCORE_URLS` können die URL-Präfixe in den folgenden Formaten vorliegen.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Nur HTTP-URL-Präfixe sind gültig. Kestrel unterstützt SSL nicht beim Konfigurieren von URL-Bindungen mit `UseUrls`.

* IPv4-Adresse mit Portnummer

  ```
  http://65.55.39.10:80/
  ```

  Bei `0.0.0.0` handelt es sich um einen Sonderfall, für den eine Bindung an alle IPv4-Adressen erfolgt.

* IPv6-Adresse mit Portnummer

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` stellt das Äquivalent von IPv6 zu `0.0.0.0` für IPv4 dar.

* Hostname mit Portnummer

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Hostnamen, `*` und `+` sind nicht spezifisch. Alle Elemente, die nicht als gültige IP-Adresse oder `localhost` erkannt werden, werden an alle IPv4- und IPv6-IP-Adressen gebunden. Verwenden Sie [HTTP.sys](xref:fundamentals/servers/httpsys) oder einen Reverseproxyserver zum Binden verschiedener Hostnamen an verschiedene ASP.NET Core-Apps auf demselben Port, z.B. IIS, Nginx oder Apache.

  > [!WARNING]
  > Wenn Sie keinen Reverseproxy mit aktivierter Host-Filterung verwenden, aktivieren Sie die [Host-Filterung](#host-filtering).

* `localhost`-Hostname mit Portnummer oder Loopback-IP mit Portnummer

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Wenn `localhost` angegeben ist, versucht Kestrel, eine Bindung zu IPv4- und IPv6-Loopback-Schnittstellen zu erstellen. Wenn der erforderliche Port von einem anderen Dienst auf einer der Loopback-Schnittstellen verwendet wird, tritt beim Starten von Kestrel ein Fehler auf. Wenn eine der Loopback-Schnittstellen aus anderen Gründen nicht verfügbar ist (meistens durch die fehlende Unterstützung von IPv6), protokolliert Kestrel eine Warnung.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* IPv4-Adresse mit Portnummer

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  Bei `0.0.0.0` handelt es sich um einen Sonderfall, für den eine Bindung an alle IPv4-Adressen erfolgt.

* IPv6-Adresse mit Portnummer

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` stellt das Äquivalent von IPv6 zu `0.0.0.0` für IPv4 dar.

* Hostname mit Portnummer

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Hostnamen, `*` und `+` sind nicht spezifisch. Alle Elemente, die keine bekannte IP-Adresse oder `localhost` sind, werden an alle IPv4- und IPv6-IP-Adressen gebunden. Verwenden Sie [WebListener](xref:fundamentals/servers/weblistener) oder einen Reverseproxyserver zum Binden verschiedener Hostnamen an verschiedene ASP.NET Core-Apps auf demselben Port, z.B. IIS, Nginx oder Apache.

* `localhost`-Hostname mit Portnummer oder Loopback-IP mit Portnummer

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

Wenn die Portnummer `0` angegeben wird, wird Kestrel dynamisch an einen verfügbaren Port gebunden. Die Bindung an Port `0` ist für jeden Hostnamen oder jede IP-Adresse zulässig, abgesehen von `localhost`.

Wenn die App ausgeführt wird, gibt das Ausgabefenster der Konsole den dynamischen Port an, über den die App erreicht werden kann:

```console
Now listening on: http://127.0.0.1:48508
```

**URL-Präfixe für SSL**

Achten Sie beim Aufrufen der Erweiterungsmethode `UseHttps` darauf, dass Sie URL-Präfixe mit `https:` einschließen:

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

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a>Host-Filterung

Obwohl Kestrel die Konfiguration basierend auf Präfixe wie `http://example.com:5000` unterstützt, ignoriert Kestrel den Hostnamen weitgehend. Der Host `localhost` ist ein Sonderfall, der für die Bindung an Loopback-Adressen verwendet wird. Jeder Host, der keine explizite IP-Adresse ist, wird an alle öffentlichen IP-Adressen gebunden. Keine dieser Informationen wird zum Überprüfen der `Host`-Abfrageheader verwendet.

::: moniker range="< aspnetcore-2.0"

Sie können das Problem umgehen, indem Sie hinter einem Reverseproxy mit Filtern von Hostheadern hosten. In ASP.NET Core 1.x ist dies das einzige unterstützte Szenario für Kestrel.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Verwenden Sie zur Umgehung des Problems eine Middleware zum Filtern von Anforderungen vom `Host`-Header:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Registrieren Sie die vorherige `HostFilteringMiddleware` in `Startup.Configure`. Beachten Sie, dass die [Sortierung der Middleware-Registrierung](xref:fundamentals/middleware/index#ordering) wichtig ist. Die Registrierung sollte unmittelbar nach der Registrierung der diagnostischen Middleware stattfinden (z.B. `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Die Middleware erwartet einen `AllowedHosts`-Schlüssel in *appsettings.json*/*appsettings.\<Umgebungsname>.json*. Der Wert ist eine durch Semikolons getrennte Liste von Hostnamen ohne Portnummern:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Verwenden Sie Middleware zum Filtern von Hosts, um dieses Problem zu umgehen. Middleware zum Filtern von Hosts wird im Paket [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) bereitgestellt, das im [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten ist (ASP.NET Core 2.1 oder höher). Die Middleware wird von [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) hinzugefügt, das [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering) aufruft:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Die Middleware zum Filtern von Hosts ist standardmäßig deaktiviert. Wenn Sie die Middleware aktivieren möchten, definieren Sie einen `AllowedHosts`-Schlüssel in *appsettings.json*/*appsettings.\<Umgebungsname>.json*. Der Wert ist eine durch Semikolons getrennte Liste von Hostnamen ohne Portnummern:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Middleware für weitergeleitete Header](xref:host-and-deploy/proxy-load-balancer) verfügt auch über die Option [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts). Middleware für weitergeleitete Header und Middleware zum Filtern von Hosts besitzen ähnliche Funktionen für unterschiedliche Szenarios. Legen Sie `AllowedHosts` bei Middleware für weitergeleitete Header fest, wenn der Hostheader beim Weiterleiten von Anforderungen mit einem Reverseproxyserver oder Lastenausgleich nicht beibehalten wird. Legen Sie `AllowedHosts` bei Middleware zum Filtern von Hosts fest, wenn Kestrel als Edgeserver verwendet wird oder wenn der Hostheader direkt weitergeleitet wird.
>
> Weitere Informationen zu Middleware für weitergeleitete Header finden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Erzwingen von HTTPS](xref:security/enforcing-ssl)
* [Kestrel-Quellcode](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Message Syntax und Routing (Abschnitt 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)
* [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer)
