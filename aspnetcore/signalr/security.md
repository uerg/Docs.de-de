---
title: Überlegungen zur Sicherheit in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie Authentifizierung und Autorisierung in ASP.NET Core SignalR verwenden.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225368"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Überlegungen zur Sicherheit in ASP.NET Core SignalR

Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)

Dieser Artikel enthält Informationen zum Sichern von SignalR.

## <a name="cross-origin-resource-sharing"></a>Cross-Origin Resource sharing

[Cross-Origin Resource sharing (CORS)](https://www.w3.org/TR/cors/) können verwendet werden, um die SignalR-Verbindungen zwischen verschiedenen Ursprüngen im Browser zulassen. Wenn JavaScript-Code auf einer anderen Domäne als der SignalR-app gehostet wird [CORS-Middleware](xref:security/cors) muss aktiviert sein, um die JavaScript für die Verbindung zur SignalR-app zu ermöglichen. Zulassen, dass Cross-Origin-Anforderungen nur von Domänen, denen, die Sie vertrauen, oder Steuerelement. Zum Beispiel:

* Ihre Website wird auf gehostet. `http://www.example.com`
* Der SignalR-app wird auf gehostet. `http://signalr.example.com`

CORS konfiguriert werden sollte, in der SignalR-app, um nur den Ursprung zuzulassen `www.example.com`.

Weitere Informationen zum Konfigurieren von CORS finden Sie unter [aktivieren Ursprungsübergreifender Anforderungen (CORS)](xref:security/cors). SignalR **erfordert** die folgenden CORS-Richtlinien:

* Ermöglichen Sie die bestimmten erwarteten Ursprünge. Ermöglicht einem beliebigen Ursprung ist möglich ist aber **nicht** sichere oder empfohlen.
* HTTP-Methoden `GET` und `POST` müssen zulässig sein.
* Anmeldeinformationen müssen aktiviert werden, auch wenn keine Authentifizierung verwendet wird.

Beispielsweise kann die folgende CORS-Richtlinie einer SignalR-Browser-Client auf gehosteten `http://example.com` Zugriff auf die SignalR-app, die auf gehosteten `http://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR ist nicht kompatibel mit dem integrierten CORS-Feature in Azure App Service.

## <a name="websocket-origin-restriction"></a>Ursprung der WebSocket-Einschränkung

::: moniker range=">= aspnetcore-2.2"

Der Schutz von CORS erhalten, anwenden nicht auf WebSockets. Origin-Einschränkung für WebSockets, finden Sie in [WebSockets Ursprung Einschränkung](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Der Schutz von CORS erhalten, anwenden nicht auf WebSockets. Führen Sie den Browser **nicht**:

* Führen Sie die CORS-Preflight-Prüfliste-Anforderungen.
* Berücksichtigen Sie die Einschränkungen, die im angegebenen `Access-Control` Header bei WebSocket-Anforderungen.

Allerdings Browser Sende die `Origin` Header, wenn die WebSocket-Anforderungen ausgeben. Anwendungen sollten konfiguriert werden, um zu überprüfen diese Header, um sicherzustellen, dass nur WebSockets, die von der erwarteten Ursprünge zulässig sind.

In ASP.NET Core 2.1 und höher headerüberprüfung erfolgt über eine benutzerdefinierte Middleware platziert **vor `UseSignalR`, und die authentifizierungsmiddleware** in `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> Die `Origin` Header wird gesteuert, durch den Client und, wie die `Referer` -Header, überlistet werden kann. Dieser Header sollte **nicht** als Authentifizierungsmechanismus verwendet werden.

::: moniker-end

## <a name="access-token-logging"></a>Access-token-Protokollierung

Bei der Verwendung von WebSockets oder Server-Sent Ereignisse sendet der Browserclient das Zugriffstoken in der Abfragezeichenfolge an. Empfangen des Zugriffstokens über die Abfragezeichenfolge in der Regel so sicher wie die Verwendung des Standards ist `Authorization` Header. Allerdings melden vielen Webservern an die URL für jede Anforderung, einschließlich der Abfragezeichenfolge. Protokollieren die URLs möglicherweise melden Sie sich das Zugriffstoken. Eine bewährte Methode besteht darin im Web protokolleinstellungen des Servers zu verhindern, dass bei der Protokollierung Zugriffstoken festzulegen.

## <a name="exceptions"></a>Ausnahmen

Ausnahmemeldungen gelten im Allgemeinen sensible Daten, die für einen Client offen gelegt werden sollte nicht. In der Standardeinstellung senden nicht SignalR die Details der Ausnahme, die von einer hubmethode ausgelöst wird, an dem Client. Stattdessen erhält der Client eine generische Meldung angezeigt, dass ein Fehler aufgetreten ist. Mit Ausnahme der Nachrichtenübermittlung an den Client (z. B. in Entwicklungs- oder testumgebung) überschrieben werden kann [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). Ausnahmemeldungen sollte nicht an den Client in Produktions-apps verfügbar gemacht werden.

## <a name="buffer-management"></a>Pufferverwaltung

SignalR verwendet pro Verbindung Puffer zum Verwalten von eingehender und ausgehender Nachrichten. Standardmäßig beschränkt SignalR diese Puffer auf 32 KB. Die größte Nachricht, die einem Client oder Server senden kann, beträgt 32 KB. Der maximale Arbeitsspeicher genutzt werden, indem Sie eine Verbindung für Nachrichten beträgt 32 KB. Wenn Ihre Nachrichten immer kleiner als 32 KB sind, können Sie die maximale Anzahl reduzieren die:

* Verhindert, dass einen Client eine größere Nachricht senden.
* Der Server muss nie zuweisen großer Puffer Nachrichten zu akzeptieren.

Wenn Ihre Nachrichten größer als 32 KB sind, können Sie den Grenzwert erhöhen. Erhöhen diese Beschränkung bedeutet:

* Der Client kann den Server an große Speicherpuffer zuzuordnen.
* Serverzuordnung großen Puffern kann die Anzahl von gleichzeitigen Verbindungen reduzieren.

Es sind die Grenzwerte für eingehende und ausgehende Nachrichten, sowohl die konfiguriert werden können, auf die [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) Objekt im konfigurierten `MapHub`:

* `ApplicationMaxBufferSize` die maximale Anzahl von Bytes aus dem Client darstellt, die die Server-Puffer. Wenn der Client versucht, eine Nachricht zu senden, die diesen Wert überschreitet, kann die Verbindung geschlossen.
* `TransportMaxBufferSize` Stellt die maximale Anzahl von Bytes, die der Server senden kann. Wenn der Server versucht, die eine Nachricht (einschließlich Rückgabewerte von Hub-Methoden) zu senden, die diesen Wert überschreitet, wird eine Ausnahme ausgelöst werden.

Festlegen der Beschränkung auf `0` deaktiviert den Grenzwert. Entfernen das Limit kann ein Client zum Senden einer Nachricht beliebiger Größe. Böswillige Clients Senden großer Nachrichten können dazu führen, dass überschüssigen Arbeitsspeicher zugeordnet werden. Übermäßige speicherauslastung kann die Anzahl von gleichzeitigen Verbindungen erheblich reduzieren.
