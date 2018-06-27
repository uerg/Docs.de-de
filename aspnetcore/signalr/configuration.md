---
title: Konfiguration für ASP.NET SignalR Core
author: rachelappel
description: Informationen Sie zum Konfigurieren von ASP.NET Core SignalR-apps.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961982"
---
# <a name="aspnet-core-signalr-configuration"></a>Konfiguration für ASP.NET SignalR Core

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack Serialisierungsoptionen

ASP.NET Core SignalR unterstützt zwei Protokolle zum Codieren von Nachrichten: [JSON](https://www.json.org/) und [MessagePack](https://msgpack.org/index.html). Jedes Protokoll verfügt über die Konfigurationsoptionen für die Serialisierung.

JSON-Serialisierung kann konfiguriert werden, auf dem Server mithilfe der [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) Erweiterungsmethode, die nach hinzugefügt werden kann [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in Ihrer `Startup.ConfigureServices` Methode. Die `AddJsonProtocol` Methode nimmt ein Delegat, empfängt ein `options` Objekt. Die [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Eigenschaft für dieses Objekt ist ein JSON.NET `JsonSerializerSettings` -Objekt, das zum Konfigurieren der Serialisierung der Argumente und Rückgabewerte verwendet werden kann. Finden Sie unter der [JSON.NET Dokumentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) Weitere Details.

Verwenden Sie beispielsweise zum Konfigurieren von des Serialisierungsprogramms um "PascalCase" Eigenschaftennamen, statt die Standardnamen "camelCase ändern möchten", verwenden den folgenden Code ein:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

In der .NET Client `AddJsonHubProtocol` Erweiterungsmethode vorhanden ist, auf [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Die `Microsoft.Extensions.DependencyInjection` Namespace muss importiert werden, um die Erweiterungsmethode zu beheben:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> Es ist nicht möglich, die JSON-Serialisierung zu diesem Zeitpunkt in der JavaScript-Client zu konfigurieren.

### <a name="messagepack-serialization-options"></a>Serialisierungsoptionen MessagePack

MessagePack Serialisierung kann konfigurieren, indem ein Delegat zum Bereitstellen der [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) aufrufen. Finden Sie unter [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) Weitere Details.

> [!NOTE]
> Es ist nicht möglich, MessagePack Serialisierung zu diesem Zeitpunkt in der JavaScript-Client zu konfigurieren.

## <a name="configure-server-options"></a>Serveroptionen konfigurieren

Die folgende Tabelle beschreibt die Optionen zum Konfigurieren von SignalR-Hubs:

| Option | Beschreibung |
| ------ | ----------- |
| `HandshakeTimeout` | Wenn der Client eine Nachricht für die anfängliche Handshake innerhalb dieses Zeitintervalls senden nicht, wird die Verbindung geschlossen. |
| `KeepAliveInterval` | Wenn der Server eine Nachricht nicht innerhalb dieses Intervalls gesendet noch nicht, wird automatisch eine Ping-Nachricht gesendet, die Verbindung geöffnet zu lassen. |
| `SupportedProtocols` | Von diesem Hub unterstützten Protokolle. Standardmäßig alle Protokolle auf dem Server registriert sind zulässig, Protokolle können jedoch entfernt werden aus der Liste, um die spezifischen Protokolle für einzelne Hubs zu deaktivieren. |
| `EnableDetailedErrors` | Wenn `true`, detaillierte Fehlermeldungen werden an Clients zurückgegeben, wenn in einer hubmethode eine Ausnahme ausgelöst wird. Die Standardeinstellung ist `false`, wie diese ausnahmemeldungen können vertraulichen Informationen enthalten. |

Optionen können für sämtliche Hubs im aktuellen durch Bereitstellen ein Delegaten Optionen zum Konfigurieren der `AddSignalR` Aufrufen in `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Optionen für einen einzelnen Hub überschreiben die globalen Optionen im `AddSignalR` und kann konfiguriert werden, mithilfe von [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Verwendung `HttpConnectionDispatcherOptions` konfigurieren Erweiterte Einstellungen im Zusammenhang mit Transporte und Verwaltung des Arbeitsspeichers Puffer. Diese Optionen werden durch einen Delegaten übergeben konfiguriert [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Option | Beschreibung |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | Die maximale Anzahl von Bytes, die vom Client empfangen, die die Server-Puffer. Durch Erhöhen dieses Wertes ermöglicht es den Server, umfangreiche Nachrichten empfangen, aber den Arbeitsspeicherverbrauch negativ beeinträchtigt werden kann. Der Standardwert beträgt 32KB. |
| `AuthorizationData` | Eine Liste der [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) Objekte verwendet, um zu bestimmen, ob ein Client autorisiert ist, mit dem Hub herzustellen. Standardmäßig ist dies mit Werten aus aufgefüllt, die `Authorize` Attribute auf die hubklasse angewendet werden. |
| `TransportMaxBufferSize` | Die maximale Anzahl der gesendeten Bytes nach der app, die die Server-Puffer. Durch Erhöhen dieses Wertes können den Server zum Senden von Nachrichten größer, aber Speicherverbrauch negativ beeinträchtigt werden kann. Der Standardwert beträgt 32KB. |
| `Transports` | Eine Bitmaske der `HttpTransportType` Werte, die die Transporte einschränken können ein Client kann die Verbindung verwenden. Alle Transporte sind standardmäßig aktiviert. |
| `LongPolling` | Zusätzliche Optionen, die spezifisch für den Transport lange abrufen. |
| `WebSockets` | Zusätzliche Optionen, die spezifisch für den Transport WebSockets. |

Abrufen von Long-Transport hat zusätzliche Optionen, die mit konfiguriert werden können die `LongPolling` Eigenschaft:

| Option | Beschreibung |
| ------ | ----------- |
| `PollTimeout` | Die maximale Zeitdauer eine Nachricht an den Client zu senden, bevor eine Anforderung Einzelabfrage beendet der Server wartet. Durch senken dieses Werts bewirkt, dass der Client neue abrufanforderungen häufiger ausgibt. Der Standardwert ist 90 Sekunden. |

Der WebSocket-Transport hat zusätzliche Optionen, die mit konfiguriert werden können die `WebSockets` Eigenschaft:

| Option | Beschreibung |
| ------ | ----------- |
| `CloseTimeout` | Nachdem der Server beendet, wenn der Client nicht innerhalb dieses Zeitintervalls schließen, wird die Verbindung beendet. |
| `SubProtocolSelector` | Ein Delegat, der verwendet werden kann, zum Festlegen der `Sec-WebSocket-Protocol` -Header auf einen benutzerdefinierten Wert. Der Delegat empfängt die Werte, die vom Client als Eingabe angefordert werden und den gewünschten Wert zurückgeben soll. |

## <a name="configure-client-options"></a>Konfigurieren Sie Clientoptionen

Clientoptionen konfiguriert werden können, auf die `HubConnectionBuilder` Typ (in .NET und JavaScript-Clients verfügbar gemacht wird), als auch auf die `HubConnection` selbst.

### <a name="configure-logging"></a>Konfigurieren der Protokollierung

Protokollierung wurde so konfiguriert, in der .NET Client mithilfe der `ConfigureLogging` Methode. Protokollierung von Anbietern und Filter kann auf die gleiche Weise registriert werden, wie sie auf dem Server sind. Finden Sie unter der [Protokollierung in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Dokumentation weitere Informationen.

> [!NOTE]
> Um die Protokollierung zu registrieren, müssen Sie die erforderlichen Pakete installieren. Finden Sie unter der [integrierten Protokollanbieter](xref:fundamentals/logging/index#built-in-logging-providers) Abschnitt der Dokumentation für eine vollständige Liste.

Beispielsweise um konsolenprotokollierung zu aktivieren, installieren die `Microsoft.Extensions.Logging.Console` NuGet-Paket. Rufen Sie die `AddConsole` Erweiterungsmethode:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

In der JavaScript-Client ein ähnliches `configureLogging` Methode vorhanden ist. Geben Sie einen `LogLevel` Wert, der angibt, der minimalen Stufe der protokollmeldungen zu erzeugen. Protokolle werden im Browser Konsolenfenster geschrieben.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Geben Sie zum Deaktivieren der Protokollierung vollständig `signalR.LogLevel.None` in die `configureLogging` Methode.

Protokollebenen für den JavaScript-Client verfügbar sind unten aufgeführt. Die Protokollebene auf einen der folgenden Werte festlegen aktiviert die Protokollierung von Nachrichten in **oder höher** dieser Ebene.

| Ebene | Beschreibung |
| ----- | ----------- |
| `None` | Es werden keine Meldungen protokolliert. |
| `Critical` | Nachrichten, die einen Fehler in der gesamten app anzeigen. |
| `Error` | Nachrichten, die einen Fehler in den aktuellen Vorgang anzeigen. |
| `Warning` | Nachrichten, die ein nicht schwerwiegender Fehler aufgetreten. |
| `Information` | Informationsmeldungen. |
| `Debug` | Diagnosemeldungen beim Debuggen nützlich. |
| `Trace` | Sehr detaillierte diagnosemeldungen konzipiert, die für das Diagnostizieren von Problemen. |

### <a name="configure-allowed-transports"></a>Konfigurieren von zulässigen Transporte

Die von SignalR verwendete Transporte können konfiguriert werden, der `WithUrl` aufrufen (`withUrl` in JavaScript). Eine bitweise OR-Operation für die Werte der `HttpTransportType` kann verwendet werden, um den Client, um nur die angegebenen Transporte verwenden beschränken. Alle Transporte sind standardmäßig aktiviert.

Zulassen Sie angenommen, um den Transport Server-Sent Ereignisse zu deaktivieren, aber WebSockets und lange Abfragen Verbindungen:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

In der JavaScript-Client Transporte konfiguriert werden, indem die `transport` Feld für die Options-Objekt, das bereitgestellte `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Konfigurieren der trägerauthentifizierung

Um Authentifizierungsdaten zusammen mit Anforderungen für SignalR bereitzustellen, verwenden Sie die `AccessTokenProvider` Option (`accessTokenFactory` in JavaScript) an eine Funktion, die das gewünschte Zugriffstoken zurückgibt. In der .NET Client das Zugriffstoken übergeben als eine HTTP-"Authentifizierung Träger" token (mithilfe der `Authorization` Header mit einem `Bearer`). In der JavaScript-Client dient als ein trägertoken, das Zugriffstoken **außer** in einigen Fällen, in denen Browser APIs eingeschränkt die Möglichkeit, Header (insbesondere bei Server-Sent Ereignisse und WebSockets-Anforderungen) anzuwenden. In diesen Fällen das Zugriffstoken bereitgestellt wird, als ein Wert der Abfragezeichenfolge `access_token`.

In der .NET Client die `AccessTokenProvider` Option kann angegeben werden, mithilfe der Optionen Delegaten in `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

In der JavaScript-Client das Zugriffstoken konfiguriert, indem die `accessTokenFactory` auf das Objekt im Feld `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Timeout und Keep-alive-Optionen konfigurieren

Zusätzliche Optionen zum Konfigurieren von Timeouts und Keep-alive-Verhalten befinden sich die `HubConnection` Objekt selbst:

| .NET-option | JavaScript-Option | Beschreibung |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Timeout für die Serveraktivität. Wenn der Server alle Nachrichten in diesem Intervall gesendet noch nicht, betrachtet der Client den Server getrennt und die Trigger der `Closed` Ereignis (`onclose` in JavaScript). |
| `HandshakeTimeout` | Kann nicht konfiguriert | Timeout für den ersten Server-Handshake. Wenn der Server eine Antwort Handshake in diesem Intervall senden nicht, wird vom Client abgebrochen, die Handshakes und der Trigger die `Closed` Ereignis (`onclose` in JavaScript). |

In der .NET Client Timeoutwerte angegeben sind, als `TimeSpan` Werte. In der JavaScript-Client werden die Timeoutwerte als Zahlen angegeben. Die Zahlen stellen Zeitwerte in Millisekunden dar.

### <a name="configure-additional-options"></a>Konfigurieren zusätzlicher Optionen

Zusätzliche Optionen können konfiguriert werden, der `WithUrl` (`withUrl` in JavaScript)-Methode `HubConnectionBuilder`:

| .NET-option | JavaScript-Option | Beschreibung |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Eine Funktion, eine Zeichenfolge, die als ein Authentifizierungstoken Trägertoken in HTTP-Anforderungen bereitgestellt wird. |
| `SkipNegotiation` | `skipNegotiation` | Legen Sie `true` um die Aushandlung Schritt zu überspringen. **Nur unterstützt, wenn der Transport WebSockets der nur aktivierte Transport ist**. Diese Einstellung kann nicht aktiviert werden, wenn der Azure-SignalR-Dienst verwendet. |
| `ClientCertificates` | Nicht konfigurierbar * | Eine Auflistung von TLS-Zertifikate zum Authentifizieren von Anforderungen zu senden. |
| `Cookies` | Nicht konfigurierbar * | Eine Auflistung von HTTP-Cookies mit jeder HTTP-Anforderung zu senden. |
| `Credentials` | Nicht konfigurierbar * | Anmeldeinformationen mit jeder HTTP-Anforderung zu senden. |
| `CloseTimeout` | Nicht konfigurierbar * | WebSockets. Die maximale Zeitdauer wartet der Client nach dem Schließen für den Server die schließende-Anforderung zu bestätigen. Wenn der Server das Schließen nicht innerhalb dieses Zeitraums bestätigt nicht, wird der Client getrennt. |
| `Headers` | Nicht konfigurierbar * | Ein Wörterbuch von zusätzlichen HTTP-Header mit jeder HTTP-Anforderung zu senden. |
| `HttpMessageHandlerFactory` | Nicht konfigurierbar * | Ein Delegat, der verwendet werden kann, um zu konfigurieren, oder Ersetzen Sie die `HttpMessageHandler` zum Senden von HTTP-Anforderungen verwendet. Nicht verwendet für WebSocket-Verbindungen. Dieser Delegat muss einen Wert ungleich Null zurückgeben, und den Standardwert als Parameter erhält. Ändern Sie die Einstellungen auf dieser Standardwert und zurückgeben oder ein vollständig neues zurückgeben `HttpMessageHandler` Instanz. |
| `Proxy` | Nicht konfigurierbar * | Ein HTTP-Proxy zum Senden von HTTP-Anforderungen verwendet. |
| `UseDefaultCredentials` | Nicht konfigurierbar * | Legen Sie diesen Boolean-Wert der Standardanmeldeinformationen für HTTP und WebSockets-Anforderungen zu senden. Dadurch wird die Verwendung der Windows-Authentifizierung. |
| `WebSocketConfiguration` | Nicht konfigurierbar * | Ein Delegat, der zum Konfigurieren der zusätzlicher "WebSocket"-Optionen verwendet werden kann. Empfängt eine Instanz von [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , die verwendet werden kann, um Optionen zu konfigurieren. |

Optionen, die mit einem Sternchen (*) gekennzeichnet sind, nicht in der JavaScript-Client aufgrund der Einschränkungen im Browser APIs konfigurierbar.

In der .NET Client können durch die Optionen Delegaten bereitgestellt, um diese Optionen geändert werden `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

In der JavaScript-Client können in einem JavaScript-Objekt bereitgestellt, um diese Optionen bereitgestellt werden `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
