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
ms.openlocfilehash: f0e371f5ea6c6688ef32adcacf667a412e4625e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="a3bec-103">Anfordern von Funktionen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3bec-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="a3bec-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a3bec-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a3bec-105">Ausführliche Informationen zur Webserverimplementierung, die mit HTTP-Anforderungen und -antworten in Verbindung stehen, werden in Schnittstellen definiert.</span><span class="sxs-lookup"><span data-stu-id="a3bec-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="a3bec-106">Diese Schnittstellen werden von serverimplementierungen und Middleware verwendet, hosting-Pipeline der Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3bec-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="a3bec-107">Feature-Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="a3bec-107">Feature interfaces</span></span>

<span data-ttu-id="a3bec-108">ASP.NET Core definiert eine Reihe von HTTP-Funktion Schnittstellen in `Microsoft.AspNetCore.Http.Features` der werden von Servern verwendet, um die Funktionen zu identifizieren, sie unterstützen.</span><span class="sxs-lookup"><span data-stu-id="a3bec-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="a3bec-109">Die folgenden Feature-Schnittstellen Behandeln von Anforderungen und Antworten zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="a3bec-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="a3bec-110">`IHttpRequestFeature`Definiert die Struktur einer HTTP-Anforderung, einschließlich Protokoll, Pfad, einer Abfragezeichenfolge, Header und Text.</span><span class="sxs-lookup"><span data-stu-id="a3bec-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="a3bec-111">`IHttpResponseFeature`Definiert die Struktur einer HTTP-Antwort, einschließlich der Statuscode, Header und Nachrichtentext der Antwort.</span><span class="sxs-lookup"><span data-stu-id="a3bec-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="a3bec-112">`IHttpAuthenticationFeature`Definiert Unterstützung zur Identifizierung von Benutzern, die auf der Grundlage einer `ClaimsPrincipal` und einen authentifizierungshandler angeben.</span><span class="sxs-lookup"><span data-stu-id="a3bec-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="a3bec-113">`IHttpUpgradeFeature`Definiert Unterstützung für [HTTP-Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), dem kann auf dem Client, um anzugeben, welche zusätzlichen sie Protokolle verwenden, wenn der Server Protokolle wechseln möchte möchten.</span><span class="sxs-lookup"><span data-stu-id="a3bec-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="a3bec-114">`IHttpBufferingFeature`Definiert Methoden für das Deaktivieren der Pufferung der Anforderungen und/oder Antworten.</span><span class="sxs-lookup"><span data-stu-id="a3bec-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="a3bec-115">`IHttpConnectionFeature`Definiert Eigenschaften für lokale und remote-Adressen und Ports.</span><span class="sxs-lookup"><span data-stu-id="a3bec-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="a3bec-116">`IHttpRequestLifetimeFeature`Definiert die Unterstützung für das Abbrechen von Verbindungen oder erkennen, wenn eine Anforderung vorzeitig, z. B. von einem Client trennen beendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a3bec-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="a3bec-117">`IHttpSendFileFeature`Definiert eine Methode zum asynchronen Senden von Dateien.</span><span class="sxs-lookup"><span data-stu-id="a3bec-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="a3bec-118">`IHttpWebSocketFeature`Definiert eine API für die Unterstützung von WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a3bec-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="a3bec-119">`IHttpRequestIdentifierFeature`Fügt eine Eigenschaft, die zur eindeutigen Identifizierung von Anforderungen implementiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a3bec-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="a3bec-120">`ISessionFeature`Definiert `ISessionFactory` und `ISession` Abstraktionen für die Unterstützung von benutzersitzungen.</span><span class="sxs-lookup"><span data-stu-id="a3bec-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="a3bec-121">`ITlsConnectionFeature`Definiert eine API zum Abrufen von Clientzertifikaten.</span><span class="sxs-lookup"><span data-stu-id="a3bec-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="a3bec-122">`ITlsTokenBindingFeature`Definiert Methoden zum Arbeiten mit TLS-tokenbindung-Parameter.</span><span class="sxs-lookup"><span data-stu-id="a3bec-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="a3bec-123">`ISessionFeature`eine Serverfunktion nicht, jedoch wird implementiert, indem die `SessionMiddleware` (finden Sie unter [Anwendungszustand verwalten](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="a3bec-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="a3bec-124">Feature-Auflistungen</span><span class="sxs-lookup"><span data-stu-id="a3bec-124">Feature collections</span></span>

<span data-ttu-id="a3bec-125">Die `Features` Eigenschaft `HttpContext` stellt eine Schnittstelle für das Abrufen und Festlegen der verfügbaren HTTP-Features für die aktuelle Anforderung.</span><span class="sxs-lookup"><span data-stu-id="a3bec-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="a3bec-126">Da die Auflistung der Funktion selbst innerhalb des Kontexts einer Anforderung änderbar ist, kann Middleware Sammlung ändern und Hinzufügen von Unterstützung für zusätzliche Funktionen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a3bec-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="a3bec-127">Middleware und Anforderung-Funktionen</span><span class="sxs-lookup"><span data-stu-id="a3bec-127">Middleware and request features</span></span>

<span data-ttu-id="a3bec-128">Während der Server für das Erstellen der Funktion Auflistung zuständig sind, kann Middleware sowohl zur Auflistung hinzufügen, und Nutzen von Funktionen aus der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="a3bec-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="a3bec-129">Z. B. die `StaticFileMiddleware` greift auf die `IHttpSendFileFeature` Funktion.</span><span class="sxs-lookup"><span data-stu-id="a3bec-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="a3bec-130">Wenn die Funktion vorhanden ist, wird es verwendet die angeforderte statische Datei von der physikalische Pfad gesendet.</span><span class="sxs-lookup"><span data-stu-id="a3bec-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="a3bec-131">Andernfalls wird eine langsamere alternative Methode zum Senden der Datei verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3bec-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="a3bec-132">Sofern verfügbar, die `IHttpSendFileFeature` zum Öffnen der das, und führen eine Kopie des direkten Kernel-Modus für die Netzwerkkarte dem Betriebssystem ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="a3bec-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="a3bec-133">Darüber hinaus kann die Feature-Auflistung, die vom Server eingerichtet Middleware hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a3bec-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="a3bec-134">Vorhandene Funktionen können auch von Middleware, die die Middleware die Funktionalität des Servers erweitern, sodass ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="a3bec-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="a3bec-135">Funktionen, die der Auflistung hinzugefügt sind sofort für anderer Middleware oder die zugrunde liegenden Anwendung selbst weiter unten in der Anforderungspipeline verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a3bec-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="a3bec-136">Durch Kombinieren von benutzerdefinierten serverimplementierungen und bestimmten Middleware-Erweiterungen, kann die genaue Satz von Funktionen eine Anwendung benötigten erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="a3bec-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="a3bec-137">Dadurch fehlende Funktionen hinzugefügt werden, ohne dass eine Änderung auf Server und wird sichergestellt, dass nur die Mindestanzahl an Funktionen verfügbar, daher beschränken Angriff surface Area und Verbessern der Leistung.</span><span class="sxs-lookup"><span data-stu-id="a3bec-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="a3bec-138">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a3bec-138">Summary</span></span>

<span data-ttu-id="a3bec-139">Feature-Schnittstellen definieren bestimmte HTTP-Features, die eine bestimmte Anforderung unterstützen kann.</span><span class="sxs-lookup"><span data-stu-id="a3bec-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="a3bec-140">Server zu definieren, Sammlungen von Funktionen und den anfänglichen Satz von Funktionen, die von diesem Server unterstützt, aber Middleware kann verwendet werden, um diese Funktionen zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="a3bec-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3bec-141">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a3bec-141">Additional Resources</span></span>

* [<span data-ttu-id="a3bec-142">Server</span><span class="sxs-lookup"><span data-stu-id="a3bec-142">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="a3bec-143">Middleware</span><span class="sxs-lookup"><span data-stu-id="a3bec-143">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="a3bec-144">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="a3bec-144">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
