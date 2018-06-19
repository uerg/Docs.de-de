---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Neuigkeiten in der ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508169"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="74dea-102">Neuigkeiten in der ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="74dea-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="74dea-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="74dea-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="74dea-104">Dieses Thema beschreibt die Neuigkeiten für ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="74dea-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="74dea-105">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="74dea-105">Download</span></span>](#download)
- [<span data-ttu-id="74dea-106">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="74dea-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="74dea-107">Neue Funktionen in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="74dea-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="74dea-108">Globale Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="74dea-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="74dea-109">-Attribut Routing Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="74dea-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="74dea-110">Hilfe zur Seite Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="74dea-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="74dea-111">IgnoreRoute-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="74dea-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="74dea-112">BSON Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="74dea-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="74dea-113">Bessere Unterstützung für asynchrone Filter</span><span class="sxs-lookup"><span data-stu-id="74dea-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="74dea-114">Abfrage für den Client Formatierung Bibliothek analysieren</span><span class="sxs-lookup"><span data-stu-id="74dea-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="74dea-115">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="74dea-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="74dea-116">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="74dea-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="74dea-117">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="74dea-117">Download</span></span>

<span data-ttu-id="74dea-118">Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="74dea-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="74dea-119">Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="74dea-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="74dea-120">Das aktuellste Paket von ASP.NET Web API 2.1 RTM wurde die folgende Version: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="74dea-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="74dea-121">Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="74dea-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="74dea-122">Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.</span><span class="sxs-lookup"><span data-stu-id="74dea-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="74dea-123">Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:</span><span class="sxs-lookup"><span data-stu-id="74dea-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="74dea-124">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="74dea-124">Documentation</span></span>

<span data-ttu-id="74dea-125">Lernprogramme und Weitere Informationen zu ASP.NET Web API 2.1 RTM stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="74dea-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="74dea-126">Neue Funktionen in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="74dea-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="74dea-127">Globale Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="74dea-127">Global Error Handling</span></span>

<span data-ttu-id="74dea-128">Alle nicht behandelte Ausnahmen können jetzt über eine zentrale Mechanismus protokolliert werden, und das Verhalten für nicht behandelte Ausnahmen kann angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="74dea-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="74dea-129">Das Framework unterstützt mehrere Protokollierungen Ausnahmen, die alle finden Sie unter der nicht behandelten Ausnahme und Informationen über den Kontext, in dem es aufgetreten ist, z. B. die Anforderung, die zum Zeitpunkt verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="74dea-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="74dea-130">Der folgende Code verwendet beispielsweise System.Diagnostics.TraceSource, um alle nicht behandelten Ausnahmen zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="74dea-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="74dea-131">Sie können auch der Standardausnahmehandler ersetzen, damit Sie vollständig auf die HTTP-Antwortnachricht anpassen können, die gesendet wird, wenn eine nicht behandelte Ausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="74dea-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="74dea-132">Enthalten eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , die nicht behandelte Ausnahmen über den gängigen ELMAH Framework protokolliert.</span><span class="sxs-lookup"><span data-stu-id="74dea-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="74dea-133">-Attribut Routing Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="74dea-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="74dea-134">Jetzt routing-Attribut unterstützt Einschränkungen, versionsverwaltung und Header basierende Routenauswahl aktivieren.</span><span class="sxs-lookup"><span data-stu-id="74dea-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="74dea-135">Darüber hinaus sind zahlreiche Aspekte der attributenrouten jetzt anpassbare über die **IDirectRouteFactory** Schnittstelle und **RouteFactoryAttribute** Klasse.</span><span class="sxs-lookup"><span data-stu-id="74dea-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="74dea-136">Das Routenpräfix ist jetzt erweiterbar ist, über die **IRoutePrefix** Schnittstelle und **RoutePrefixAttribute** Klasse.</span><span class="sxs-lookup"><span data-stu-id="74dea-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="74dea-137">Wir haben bereitgestellt, die eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , Einschränkungen zum dynamischen Filtern von Domänencontrollern über einen "api-Version" HTTP-Header verwendet.</span><span class="sxs-lookup"><span data-stu-id="74dea-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="74dea-138">Hilfe zur Seite Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="74dea-138">Help Page Improvements</span></span>

<span data-ttu-id="74dea-139">Web-API 2.1 umfasst die folgenden Verbesserungen auf [-API-Hilfeseiten](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="74dea-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="74dea-140">Dokumentation der einzelnen Eigenschaften von Parametern oder Rückgabetypen von Aktionen.</span><span class="sxs-lookup"><span data-stu-id="74dea-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="74dea-141">Die Dokumentation von datenanmerkungen-Modell.</span><span class="sxs-lookup"><span data-stu-id="74dea-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="74dea-142">Das Design der Benutzeroberfläche von Hilfeseiten wurde ebenfalls aktualisiert, um diese Änderungen zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="74dea-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="74dea-143">IgnoreRoute-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="74dea-143">IgnoreRoute Support</span></span>

<span data-ttu-id="74dea-144">Web-API 2.1 unterstützt das Ignorieren von URL-Mustern in Web-API-routing durch eine Reihe von **IgnoreRoute** Erweiterungsmethoden in **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="74dea-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="74dea-145">Diese Methoden dazu führen, dass Web-API, um alle URLs zu ignorieren, die eine angegebene Vorlage überein und ermöglichen dem Host bei Bedarf zusätzliche Verarbeitung angewendet.</span><span class="sxs-lookup"><span data-stu-id="74dea-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="74dea-146">Im folgenden Beispiel wird ignoriert, URIs, die mit beginnt ein &quot;Inhalt&quot; Segment:</span><span class="sxs-lookup"><span data-stu-id="74dea-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="74dea-147">BSON Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="74dea-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="74dea-148">Web-API unterstützt jetzt auch die [BSON](http://bsonspec.org/) Übertragungsformat, sowohl auf dem Client als auch auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="74dea-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="74dea-149">Um BSON auf dem Server zu aktivieren, fügen Sie der **BsonMediaTypeFormatter** der Formatierer Auflistung:</span><span class="sxs-lookup"><span data-stu-id="74dea-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="74dea-150">So sieht wie ein Client .NET BSON Format nutzen kann:</span><span class="sxs-lookup"><span data-stu-id="74dea-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="74dea-151">Wir haben bereitgestellt, die eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) zeigt, dass sowohl die Client-als auch die Seite.</span><span class="sxs-lookup"><span data-stu-id="74dea-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="74dea-152">Weitere Informationen finden Sie unter [BSON-Unterstützung in Web-API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="74dea-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="74dea-153">Bessere Unterstützung für asynchrone Filter</span><span class="sxs-lookup"><span data-stu-id="74dea-153">Better Support for Async Filters</span></span>

<span data-ttu-id="74dea-154">Web-API unterstützt nun eine einfache Möglichkeit zum Erstellen von Filtern, die asynchron ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="74dea-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="74dea-155">Diese Funktion ist hilfreich, der Filter für eine asynchrone Aktion, z. B. den Zugriff eine Datenbank benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="74dea-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="74dea-156">Bisher mussten, zum Erstellen einer Async-Filters können Sie die Filter-Schnittstelle selbst implementiert, da die Filter-Basisklassen nur synchrone Methoden verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="74dea-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="74dea-157">Nachdem Sie die virtuellen überschreiben können `On*Async` Methoden des Filters Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="74dea-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="74dea-158">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="74dea-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="74dea-159">Die **AuthorizationFilterAttribute**, **ActionFilterAttribute**, und **ExceptionFilterAttribute** alle Klassen unterstützt asynchrone in Web-API 2.1.</span><span class="sxs-lookup"><span data-stu-id="74dea-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="74dea-160">Abfrage für den Client Formatierung Bibliothek analysieren</span><span class="sxs-lookup"><span data-stu-id="74dea-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="74dea-161">Zuvor **System.Net.Http.Formatting** analysieren und Aktualisieren von URI-Abfragen für serverseitigen Code unterstützt, aber die entsprechende portable Bibliothek wurde dieses Feature fehlt.</span><span class="sxs-lookup"><span data-stu-id="74dea-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="74dea-162">Im Web-API 2.1 kann eine Clientanwendung jetzt problemlos analysieren und aktualisieren eine Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="74dea-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="74dea-163">Die folgenden Beispiele zeigen, wie zum Analysieren, ändern und URI-Abfragen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="74dea-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="74dea-164">(Die Beispiele zeigen eine Konsolenanwendung aus Gründen der Einfachheit.)</span><span class="sxs-lookup"><span data-stu-id="74dea-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="74dea-165">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="74dea-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="74dea-166">Dieser Abschnitt beschreibt bekannte Probleme und neueste Änderungen in der ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="74dea-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="74dea-167">Routing-Attribut</span><span class="sxs-lookup"><span data-stu-id="74dea-167">Attribute Routing</span></span>

<span data-ttu-id="74dea-168">Mehrdeutigkeiten im Attribut routing Übereinstimmungen Bericht jetzt eine Fehlermeldung statt die erste Übereinstimmung auswählen.</span><span class="sxs-lookup"><span data-stu-id="74dea-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="74dea-169">Attributenrouten sind nicht zulässig, von der Verwendung der *{Controller}* Parameter, und von der Verwendung der *{Aktion}* Parameter auf die Routen zu Aktionen platziert.</span><span class="sxs-lookup"><span data-stu-id="74dea-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="74dea-170">Diese Parameter würde sehr wahrscheinlich zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="74dea-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="74dea-171">Gerüstbau MVC/Web-API in einem Projekt mit 5.1 Pakete-Ergebnissen in 5.0-Paketen für Argumente, die nicht bereits im Projekt vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="74dea-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="74dea-172">Aktualisieren von NuGet-Pakete für ASP.NET Web API 2.1 RTM wird nicht Visual Studio-Tools, z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="74dea-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="74dea-173">Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="74dea-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="74dea-174">Daher wird das Gerüst ASP.NET die Vorgängerversion (5.0.0.0) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="74dea-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="74dea-175">Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben.</span><span class="sxs-lookup"><span data-stu-id="74dea-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="74dea-176">Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren der Pakete auf Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen der Web-API und MVC konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="74dea-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="74dea-177">Umbenennen von Typ</span><span class="sxs-lookup"><span data-stu-id="74dea-177">Type Renames</span></span>

<span data-ttu-id="74dea-178">Einige der Typen für die Attribut-routing-Erweiterbarkeit verwendet wurden, die RTM-Version 2.1 von der RC-umbenannt.</span><span class="sxs-lookup"><span data-stu-id="74dea-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="74dea-179">Alter Typname (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="74dea-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="74dea-180">Neuer Typname (RTM 2.1)</span><span class="sxs-lookup"><span data-stu-id="74dea-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="74dea-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="74dea-181">IDirectRouteProvider</span></span> | <span data-ttu-id="74dea-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="74dea-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="74dea-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="74dea-183">RouteProviderAttribute</span></span> | <span data-ttu-id="74dea-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="74dea-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="74dea-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="74dea-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="74dea-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="74dea-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="74dea-187">Ausnahmefilter nicht aggregierte Ausnahmen in Async-Aktionen entpacken.</span><span class="sxs-lookup"><span data-stu-id="74dea-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="74dea-188">Zuvor, wenn eine asynchrone Aktion ausgelöst hat eine **AggregateException**, ein Ausnahmefilter würde die Ausnahme entpacken und **OnException** erhalten die Basis-Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="74dea-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="74dea-189">2.1, der Ausnahmefilter ist nicht entpacken, und **OnException** Ruft die ursprüngliche **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="74dea-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="74dea-190">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="74dea-190">Bug Fixes</span></span>

<span data-ttu-id="74dea-191">Diese Version enthält auch verschiedene Fehlerbehebungen.</span><span class="sxs-lookup"><span data-stu-id="74dea-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="74dea-192">Sie können die vollständige Liste hier finden:</span><span class="sxs-lookup"><span data-stu-id="74dea-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="74dea-193">5.1.0-Paket</span><span class="sxs-lookup"><span data-stu-id="74dea-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="74dea-194">5.1.1-Paket</span><span class="sxs-lookup"><span data-stu-id="74dea-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="74dea-195">Die 5.1.2 Paket enthält, aber keine Fehlerkorrekturen IntelliSense-Updates.</span><span class="sxs-lookup"><span data-stu-id="74dea-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
