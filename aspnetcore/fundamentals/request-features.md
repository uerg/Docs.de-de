---
title: Anfordern von Funktionen in ASP.NET Core
author: ardalis
description: "Informationen Sie zur Web-Server-Implementierungsdetails, die im Zusammenhang mit HTTP-Anforderungen und Antworten, die in Schnittstellen für ASP.NET Core definiert sind."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: 42e2959aefef98ce7289e50b6f72bd23eaed38bc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="request-features-in-aspnet-core"></a>Anfordern von Funktionen in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/)

Ausführliche Informationen zur Webserverimplementierung, die mit HTTP-Anforderungen und -antworten in Verbindung stehen, werden in Schnittstellen definiert. Diese Schnittstellen werden von serverimplementierungen und Middleware verwendet, hosting-Pipeline der Anwendung zu erstellen.

## <a name="feature-interfaces"></a>Feature-Schnittstellen

ASP.NET Core definiert eine Reihe von HTTP-Funktion Schnittstellen in `Microsoft.AspNetCore.Http.Features` der werden von Servern verwendet, um die Funktionen zu identifizieren, sie unterstützen. Die folgenden Feature-Schnittstellen Behandeln von Anforderungen und Antworten zurückgeben:

`IHttpRequestFeature`Definiert die Struktur einer HTTP-Anforderung, einschließlich Protokoll, Pfad, einer Abfragezeichenfolge, Header und Text.

`IHttpResponseFeature`Definiert die Struktur einer HTTP-Antwort, einschließlich der Statuscode, Header und Nachrichtentext der Antwort.

`IHttpAuthenticationFeature`Definiert Unterstützung zur Identifizierung von Benutzern, die auf der Grundlage einer `ClaimsPrincipal` und einen authentifizierungshandler angeben.

`IHttpUpgradeFeature`Definiert Unterstützung für [HTTP-Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), dem kann auf dem Client, um anzugeben, welche zusätzlichen sie Protokolle verwenden, wenn der Server Protokolle wechseln möchte möchten.

`IHttpBufferingFeature`Definiert Methoden für das Deaktivieren der Pufferung der Anforderungen und/oder Antworten.

`IHttpConnectionFeature`Definiert Eigenschaften für lokale und remote-Adressen und Ports.

`IHttpRequestLifetimeFeature`Definiert die Unterstützung für das Abbrechen von Verbindungen oder erkennen, wenn eine Anforderung vorzeitig, z. B. von einem Client trennen beendet wurde.

`IHttpSendFileFeature`Definiert eine Methode zum asynchronen Senden von Dateien.

`IHttpWebSocketFeature`Definiert eine API für die Unterstützung von WebSockets.

`IHttpRequestIdentifierFeature`Fügt eine Eigenschaft, die zur eindeutigen Identifizierung von Anforderungen implementiert werden kann.

`ISessionFeature`Definiert `ISessionFactory` und `ISession` Abstraktionen für die Unterstützung von benutzersitzungen.

`ITlsConnectionFeature`Definiert eine API zum Abrufen von Clientzertifikaten.

`ITlsTokenBindingFeature`Definiert Methoden zum Arbeiten mit TLS-tokenbindung-Parameter.

> [!NOTE]
> `ISessionFeature`ist keine Serverfunktion, jedoch wird implementiert, indem die `SessionMiddleware` (finden Sie unter [Anwendungszustand verwalten](app-state.md)).

## <a name="feature-collections"></a>Feature-Auflistungen

Die `Features` Eigenschaft `HttpContext` stellt eine Schnittstelle für das Abrufen und Festlegen der verfügbaren HTTP-Features für die aktuelle Anforderung. Da die Auflistung der Funktion selbst innerhalb des Kontexts einer Anforderung änderbar ist, kann Middleware Sammlung ändern und Hinzufügen von Unterstützung für zusätzliche Funktionen verwendet werden.

## <a name="middleware-and-request-features"></a>Middleware und Anforderung-Funktionen

Während der Server für das Erstellen der Funktion Auflistung zuständig sind, kann Middleware sowohl zur Auflistung hinzufügen, und Nutzen von Funktionen aus der Auflistung. Z. B. die `StaticFileMiddleware` greift auf die `IHttpSendFileFeature` Funktion. Wenn die Funktion vorhanden ist, wird es verwendet die angeforderte statische Datei von der physikalische Pfad gesendet. Andernfalls wird eine langsamere alternative Methode zum Senden der Datei verwendet. Sofern verfügbar, die `IHttpSendFileFeature` zum Öffnen der das, und führen eine Kopie des direkten Kernel-Modus für die Netzwerkkarte dem Betriebssystem ermöglicht.

Darüber hinaus kann die Feature-Auflistung, die vom Server eingerichtet Middleware hinzufügen. Vorhandene Funktionen können auch von Middleware, die die Middleware die Funktionalität des Servers erweitern, sodass ersetzt werden. Funktionen, die der Auflistung hinzugefügt sind sofort für anderer Middleware oder die zugrunde liegenden Anwendung selbst weiter unten in der Anforderungspipeline verfügbar.

Durch Kombinieren von benutzerdefinierten serverimplementierungen und bestimmten Middleware-Erweiterungen, kann die genaue Satz von Funktionen eine Anwendung benötigten erstellt werden. Dadurch fehlende Funktionen hinzugefügt werden, ohne dass eine Änderung auf Server und wird sichergestellt, dass nur die Mindestanzahl an Funktionen verfügbar, daher beschränken Angriff surface Area und Verbessern der Leistung.

## <a name="summary"></a>Zusammenfassung

Feature-Schnittstellen definieren bestimmte HTTP-Features, die eine bestimmte Anforderung unterstützen kann. Server zu definieren, Sammlungen von Funktionen und den anfänglichen Satz von Funktionen, die von diesem Server unterstützt, aber Middleware kann verwendet werden, um diese Funktionen zu verbessern.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Server](servers/index.md)

* [Middleware](middleware.md)

* [Open Web Interface for .NET (OWIN)](owin.md)
