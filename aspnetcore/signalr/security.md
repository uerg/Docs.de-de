---
title: Überlegungen zur Sicherheit in ASP.NET Core SignalR
author: rachelappel
description: Erfahren Sie, wie Sie Authentifizierung und Autorisierung in ASP.NET Core SignalR verwenden.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: eff4542b88f24dd6c1c0675f56874e368d441fdd
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028482"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Überlegungen zur Sicherheit in ASP.NET Core SignalR

Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)

## <a name="overview"></a>Übersicht

SignalR stellt eine Anzahl von Sicherheitsmaßnahmen standardmäßig bereit. Es ist wichtig zu verstehen, wie diese Schutzmaßnahmen zu konfigurieren.

### <a name="cross-origin-resource-sharing"></a>Cross-Origin Resource sharing

[Cross-Origin Resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) können verwendet werden, um die SignalR-Verbindungen zwischen verschiedenen Ursprüngen im Browser zulassen. Wenn Ihr JavaScript-Code auf einem anderen Domänennamen aus der SignalR-app gehostet wird, müssen Sie aktivieren die [ASP.NET Core-CORS-Middleware](xref:security/cors) um die Verbindung zu ermöglichen. Im Allgemeinen können Sie Cross-Origin-Anforderungen nur von Domänen, die Sie steuern. Wenn auf Ihrer Website gehostet wird z. B. `http://www.example.com` und Ihre SignalR-app gehostet wird `http://signalr.example.com`, sollten Sie CORS konfigurieren, in der SignalR-app, um nur den Ursprung zuzulassen `www.example.com`.

Weitere Informationen zum Konfigurieren von CORS finden Sie unter [der Dokumentation zu ASP.NET Core CORS](xref:security/cors). SignalR erforderlich, dass die folgenden CORS-Richtlinien ordnungsgemäß ausgeführt wird:

* Die Richtlinie muss die bestimmten Ursprünge zulassen, die Sie erwarten, die oder Sie können einen beliebigen Ursprung (nicht empfohlen).
* HTTP-Methoden `GET` und `POST` müssen zulässig sein.
* Anmeldeinformationen müssen aktiviert sein, selbst wenn Sie die Authentifizierung nicht verwenden.

Beispielsweise kann die folgende CORS-Richtlinie einer SignalR-Browser-Client auf gehosteten `http://example.com` auf Ihre SignalR-app zugreifen:

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR ist nicht kompatibel mit dem integrierten CORS-Feature in Azure App Service.

### <a name="access-token-logging"></a>Access-token-Protokollierung

Bei der Verwendung von WebSockets oder Server-Sent Ereignisse sendet der Browserclient das Zugriffstoken in der Abfragezeichenfolge an. Dies ist im Allgemeinen so sicher wie die Verwendung des Standards `Authorization` -Header, aber viele Webserver die URL für jede Anforderung protokollieren, einschließlich der Abfragezeichenfolge. Dies bedeutet, dass das Zugriffstoken in Protokollen enthalten sein kann. Sollten Sie die protokollierungseinstellungen des Webservers, um zu vermeiden, diese Informationen protokolliert.

### <a name="exceptions"></a>Ausnahmen

Ausnahmemeldungen gelten im Allgemeinen sensible Daten, die für einen Client offen gelegt werden sollte nicht. In der Standardeinstellung senden nicht SignalR die Details der Ausnahme, die von einer hubmethode ausgelöst wird, an dem Client. Stattdessen erhält der Client eine generische Meldung angezeigt, dass ein Fehler aufgetreten ist. Sie können dieses Verhalten überschreiben, indem Sie die Einstellung der [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) festlegen.

### <a name="buffer-management"></a>Pufferverwaltung

SignalR verwendet pro Verbindung Puffer, um eingehende und ausgehende Nachrichten zu verwalten. Standardmäßig beschränkt SignalR diese Puffer auf 32KB. Dies bedeutet, dass die größte mögliche Nachricht, die einem Client oder Server senden kann 32 KB ist. Dies bedeutet auch, dass die maximale Arbeitsspeichermenge, die von einer Verbindung für Nachrichten verwendet 32KB ist. Wenn Sie, dass die Nachrichten immer kleiner als dieser Grenzwert sind wissen, können Sie diese Größe aus, um zu verhindern, dass einen Client die Möglichkeit zum Senden einer Nachricht größer und zwingt den Server, Arbeitsspeicher zuordnen, um es annehmen, reduzieren. Auf ähnliche Weise, wenn Sie, dass Ihre Nachrichten größer als dieser Grenzwert sind wissen, können Sie ihn erhöhen. Bedenken Sie jedoch, dass mit diesen Grenzwert zu erhöhen, dass der Client dazu führen, dass den Server zusätzlichen Arbeitsspeicher reserviert werden kann und möglicherweise Reduzieren der Anzahl von gleichzeitigen Verbindungen, die Ihre app behandeln kann.

Es gibt separate Grenzwerte für eingehende und ausgehende Nachrichten, sowohl die konfiguriert werden können, auf die [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) Objekt im konfigurierten `MapHub`:

* `ApplicationMaxBufferSize` die maximale Anzahl von Bytes aus dem Client darstellt, die die Server-Puffer. Wenn der Client versucht, eine Nachricht zu senden, die diesen Wert überschreitet, kann die Verbindung geschlossen.
* `TransportMaxBufferSize` Stellt die maximale Anzahl von Bytes, die der Server senden kann. Wenn der Server versucht, eine Nachricht zu senden (einschließlich der Rückgabewerte von Hub-Methoden) größer als diese Einschränkung wird eine Ausnahme ausgelöst werden.

Festlegen der Beschränkung auf `0` die Begrenzung ganz deaktiviert. Dies sollte jedoch mit äußerster Sorgfalt erfolgen. Entfernen das Limit kann ein Client zum Senden einer Nachricht beliebiger Größe. Dies konnte von überschüssiger Arbeitsspeicher zugeordnet werden, dazu führen, dass ein böswilliger Client verwendet werden, erheblich die Anzahl von gleichzeitigen Verbindungen verringert werden kann, die die app zu unterstützen.
