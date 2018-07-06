---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Was ist neu in ASP.NET Web-API 2.1 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838402"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="a7002-102">Neuerungen in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="a7002-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="a7002-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a7002-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="a7002-104">Dieses Thema beschreibt, welche neuerungen für ASP.NET Web-API 2.1.</span><span class="sxs-lookup"><span data-stu-id="a7002-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="a7002-105">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="a7002-105">Download</span></span>](#download)
- [<span data-ttu-id="a7002-106">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="a7002-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="a7002-107">Neue Features in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="a7002-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="a7002-108">Globale Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="a7002-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="a7002-109">Attribut-Routing-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="a7002-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="a7002-110">Verbesserungen der Hilfe auf der Eigenschaftenseite</span><span class="sxs-lookup"><span data-stu-id="a7002-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="a7002-111">IgnoreRoute-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="a7002-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="a7002-112">BSON Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="a7002-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="a7002-113">Bessere Unterstützung für Async-Filter</span><span class="sxs-lookup"><span data-stu-id="a7002-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="a7002-114">Abfrageanalyse, die für den Client, die Formatierung der Bibliothek</span><span class="sxs-lookup"><span data-stu-id="a7002-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="a7002-115">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="a7002-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="a7002-116">Fehlerbehebungen</span><span class="sxs-lookup"><span data-stu-id="a7002-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="a7002-117">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="a7002-117">Download</span></span>

<span data-ttu-id="a7002-118">Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="a7002-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="a7002-119">Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="a7002-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="a7002-120">Das neueste Paket mit ASP.NET Web-API 2.1 RTM hat die folgende Version: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="a7002-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="a7002-121">Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="a7002-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="a7002-122">Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.</span><span class="sxs-lookup"><span data-stu-id="a7002-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="a7002-123">Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="a7002-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="a7002-124">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="a7002-124">Documentation</span></span>

<span data-ttu-id="a7002-125">Lernprogramme und Weitere Informationen zu ASP.NET Web-API 2.1 RTM stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="a7002-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="a7002-126">Neue Features in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="a7002-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="a7002-127">Globale Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="a7002-127">Global Error Handling</span></span>

<span data-ttu-id="a7002-128">Alle nicht behandelte Ausnahmen können jetzt über eine zentrale Mechanismus protokolliert werden, und das Verhalten für nicht behandelte Ausnahmen kann angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="a7002-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="a7002-129">Das Framework unterstützt mehrere Protokollierungen Ausnahmen, die alle finden Sie unter der nicht behandelten Ausnahme und Informationen über den Kontext, in dem es aufgetreten ist, z. B. die Anforderung, die zum Zeitpunkt verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="a7002-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="a7002-130">Der folgende Code verwendet beispielsweise System.Diagnostics.TraceSource, um alle nicht behandelten Ausnahmen zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="a7002-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="a7002-131">Sie können auch der Standardausnahmehandler ersetzen, damit Sie die HTTP-Antwortnachricht vollständig anpassen können, die gesendet wird, wenn eine nicht behandelte Ausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="a7002-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="a7002-132">Wir haben bereitgestellt, eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , protokolliert alle Ausnahmefehler, über das beliebte ELMAH-Framework.</span><span class="sxs-lookup"><span data-stu-id="a7002-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="a7002-133">Attribut-Routing-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="a7002-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="a7002-134">Attributrouting nun unterstützt Einschränkungen, versionsverwaltung und Header-basierte Routenauswahl aktiviert.</span><span class="sxs-lookup"><span data-stu-id="a7002-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="a7002-135">Darüber hinaus sind viele Aspekte der attributrouten jetzt über anpassbare der **IDirectRouteFactory** Schnittstelle und **RouteFactoryAttribute** Klasse.</span><span class="sxs-lookup"><span data-stu-id="a7002-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="a7002-136">Das Routenpräfix ist nun erweiterbar, über die **IRoutePrefix** Schnittstelle und **RoutePrefixAttribute** Klasse.</span><span class="sxs-lookup"><span data-stu-id="a7002-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="a7002-137">Wir haben bereitgestellt, eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , Einschränkungen, dynamisch Filtern von Controller über eine HTTP-Header "api-Version" verwendet.</span><span class="sxs-lookup"><span data-stu-id="a7002-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="a7002-138">Verbesserungen der Hilfe auf der Eigenschaftenseite</span><span class="sxs-lookup"><span data-stu-id="a7002-138">Help Page Improvements</span></span>

<span data-ttu-id="a7002-139">Web-API 2.1 umfasst die folgenden Erweiterungen [-API-Hilfeseiten](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="a7002-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="a7002-140">Dokumentation der einzelnen Eigenschaften von Parametern oder Rückgabetypen von Aktionen.</span><span class="sxs-lookup"><span data-stu-id="a7002-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="a7002-141">Die Dokumentation von datenmodellanmerkungen.</span><span class="sxs-lookup"><span data-stu-id="a7002-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="a7002-142">Das Design der Benutzeroberfläche von Hilfeseiten wurde ebenfalls aktualisiert, um diese Änderungen zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="a7002-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="a7002-143">IgnoreRoute-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="a7002-143">IgnoreRoute Support</span></span>

<span data-ttu-id="a7002-144">Die Web-API 2.1 unterstützt, wird ignoriert, URL-Mustern im Web-API-routing, über einen Satz von **IgnoreRoute** Erweiterungsmethoden **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="a7002-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="a7002-145">Diese Methoden dazu führen, dass Web-API, um alle URLs zu ignorieren, die eine angegebene Vorlage entsprechen, und der Host gelten zusätzliche Verarbeitung aus, falls zutreffend.</span><span class="sxs-lookup"><span data-stu-id="a7002-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="a7002-146">Im folgenden Beispiel wird ignoriert, URIs, die mit einem &quot;Inhalt&quot; Segment:</span><span class="sxs-lookup"><span data-stu-id="a7002-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="a7002-147">BSON Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="a7002-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="a7002-148">Web-API jetzt unterstützt die [BSON](http://bsonspec.org/) Wire-Format, sowohl auf dem Client als auch auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="a7002-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="a7002-149">Um BSON auf der Serverseite zu aktivieren, fügen Sie der **BsonMediaTypeFormatter** der Formatierer Auflistung:</span><span class="sxs-lookup"><span data-stu-id="a7002-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="a7002-150">Hier ist, wie ein .NET Client BSON-Format nutzen kann:</span><span class="sxs-lookup"><span data-stu-id="a7002-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="a7002-151">Wir haben bereitgestellt, eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) zeigt, dass sowohl die Client- und Serverseite.</span><span class="sxs-lookup"><span data-stu-id="a7002-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="a7002-152">Weitere Informationen finden Sie unter [BSON-Unterstützung in Web-API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="a7002-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="a7002-153">Bessere Unterstützung für Async-Filter</span><span class="sxs-lookup"><span data-stu-id="a7002-153">Better Support for Async Filters</span></span>

<span data-ttu-id="a7002-154">Web-API unterstützt jetzt eine einfache Möglichkeit zum Erstellen von Filtern, die asynchron ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="a7002-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="a7002-155">Diese Funktion ist nützlich, wird der Filter muss eine Async-Aktion, wie Access eine Datenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="a7002-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="a7002-156">Zuvor musste einen asynchroner Filter zu erstellen, die filterschnittstelle selbst implementiert, da die Filter-Basisklassen nur synchrone Methoden verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="a7002-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="a7002-157">Nachdem Sie das virtuelle außer Kraft setzen können `On*Async` Methoden des Filters-Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="a7002-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="a7002-158">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a7002-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="a7002-159">Die **AuthorizationFilterAttribute**, **ActionFilterAttribute**, und **ExceptionFilterAttribute** Klassen, die alle unterstützen Async in Web-API 2.1.</span><span class="sxs-lookup"><span data-stu-id="a7002-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="a7002-160">Abfrageanalyse, die für den Client, die Formatierung der Bibliothek</span><span class="sxs-lookup"><span data-stu-id="a7002-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="a7002-161">Zuvor **System.Net.Http.Formatting** analysieren und Aktualisieren von URI-Abfragen für serverseitigen Code unterstützt, aber die entsprechende portable Bibliothek wurde dieses Feature fehlt.</span><span class="sxs-lookup"><span data-stu-id="a7002-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="a7002-162">In Web-API 2.1 kann eine Clientanwendung jetzt leicht analysieren und eine Abfragezeichenfolge zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a7002-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="a7002-163">Die folgenden Beispiele zeigen, wie Sie analysieren, ändern und URI-Abfragen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="a7002-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="a7002-164">(Die Beispiele zeigen eine Konsolenanwendung aus Gründen der Einfachheit.)</span><span class="sxs-lookup"><span data-stu-id="a7002-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="a7002-165">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="a7002-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="a7002-166">Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET Web-API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="a7002-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="a7002-167">Attribut-Routing</span><span class="sxs-lookup"><span data-stu-id="a7002-167">Attribute Routing</span></span>

<span data-ttu-id="a7002-168">Allerdings haben Mehrdeutigkeiten in Attribut-routing-Übereinstimmungen meldet jetzt eine Fehlermeldung statt die erste Übereinstimmung auswählen.</span><span class="sxs-lookup"><span data-stu-id="a7002-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="a7002-169">Attributrouten untersagt die *{Controller}* Parameter, und mit der *{Action}* Parameter auf Routen für Aktionen platziert.</span><span class="sxs-lookup"><span data-stu-id="a7002-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="a7002-170">Diese Parameter würde wahrscheinlich zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="a7002-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="a7002-171">Gerüstbau für MVC-Web-API mit 5.1 Pakete führt 5.0-Pakete in ein Projekt für diejenigen, die nicht bereits im Projekt vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="a7002-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="a7002-172">Aktualisieren von NuGet-Pakete für ASP.NET Web-API 2.1 RTM wird nicht der Visual Studio-Tools, z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="a7002-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="a7002-173">Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="a7002-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="a7002-174">Daher wird das Gerüst ASP.NET die vorherige Version (5.0.0.0) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="a7002-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="a7002-175">Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten.</span><span class="sxs-lookup"><span data-stu-id="a7002-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="a7002-176">Wenn Sie ASP.NET-Gerüstbau nach der Aktualisierung der Pakete auf Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen von Web-API und MVC konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="a7002-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="a7002-177">Typ umbenennen</span><span class="sxs-lookup"><span data-stu-id="a7002-177">Type Renames</span></span>

<span data-ttu-id="a7002-178">Einige der Typen für die Attribut-routing-Erweiterbarkeit verwendet wurden über die RC-Version in der RTM-Version 2.1 umbenannt.</span><span class="sxs-lookup"><span data-stu-id="a7002-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="a7002-179">Alter Typname (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="a7002-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="a7002-180">Neuer Typname (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="a7002-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="a7002-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="a7002-181">IDirectRouteProvider</span></span> | <span data-ttu-id="a7002-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="a7002-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="a7002-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="a7002-183">RouteProviderAttribute</span></span> | <span data-ttu-id="a7002-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="a7002-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="a7002-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="a7002-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="a7002-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="a7002-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="a7002-187">Ausnahmefilter sind nicht Aggregieren von Ausnahmen in Async-Aktionen ausgelöst entpacken.</span><span class="sxs-lookup"><span data-stu-id="a7002-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="a7002-188">Zuvor, wenn eine asynchrone Aktion ausgelöst hat eine **"AggregateException"**, Ausnahmefilter würde die Ausnahme, entpacken und **OnException** die Basisausnahme erhalten.</span><span class="sxs-lookup"><span data-stu-id="a7002-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="a7002-189">2.1, Ausnahmefilter ist nicht entpacken, und **OnException** Ruft die ursprüngliche **"AggregateException"**.</span><span class="sxs-lookup"><span data-stu-id="a7002-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="a7002-190">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="a7002-190">Bug Fixes</span></span>

<span data-ttu-id="a7002-191">Diese Version enthält auch mehrere Fehler behoben.</span><span class="sxs-lookup"><span data-stu-id="a7002-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="a7002-192">Sie können hier die vollständige Liste finden:</span><span class="sxs-lookup"><span data-stu-id="a7002-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="a7002-193">5.1.0-Paket</span><span class="sxs-lookup"><span data-stu-id="a7002-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="a7002-194">5.1.1-Paket</span><span class="sxs-lookup"><span data-stu-id="a7002-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="a7002-195">Die 5.1.2 Paket enthält die IntelliSense-Updates, aber keine Programmfehlerbehebungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a7002-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
