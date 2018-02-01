---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Neuigkeiten in der ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: 
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
---
<a name="whats-new-in-aspnet-web-api-21"></a>Neuigkeiten in der ASP.NET Web API 2.1
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt die Neuigkeiten für ASP.NET Web API 2.1.

- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Funktionen in ASP.NET Web-API 2.1](#new-features)

    - [Globale Fehlerbehandlung](#global-error)
    - [-Attribut Routing Verbesserungen](#attribute-routing)
    - [Hilfe zur Seite Verbesserungen](#help-page)
    - [IgnoreRoute-Unterstützung](#ignoreroute)
    - [BSON Medientypformatierer](#bson)
    - [Bessere Unterstützung für asynchrone Filter](#async-filters)
    - [Abfrage für den Client Formatierung Bibliothek analysieren](#query-parsing)
- [Bekannte Probleme und aktueller Änderungen](#known-issues)
- [Fehlerkorrekturen](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung. Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation. Das aktuellste Paket von ASP.NET Web API 2.1 RTM wurde die folgende Version: "5.1.2". Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.

Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET Web API 2.1 RTM stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Neue Funktionen in ASP.NET Web-API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Globale Fehlerbehandlung

Alle nicht behandelte Ausnahmen können jetzt über eine zentrale Mechanismus protokolliert werden, und das Verhalten für nicht behandelte Ausnahmen kann angepasst werden.

Das Framework unterstützt mehrere Protokollierungen Ausnahmen, die alle finden Sie unter der nicht behandelten Ausnahme und Informationen über den Kontext, in dem es aufgetreten ist, z. B. die Anforderung, die zum Zeitpunkt verarbeitet werden.

Der folgende Code verwendet beispielsweise System.Diagnostics.TraceSource, um alle nicht behandelten Ausnahmen zu protokollieren:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Sie können auch der Standardausnahmehandler ersetzen, damit Sie vollständig auf die HTTP-Antwortnachricht anpassen können, die gesendet wird, wenn eine nicht behandelte Ausnahme auftritt.

Enthalten eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , die nicht behandelte Ausnahmen über den gängigen ELMAH Framework protokolliert.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>-Attribut Routing Verbesserungen

Jetzt routing-Attribut unterstützt Einschränkungen, versionsverwaltung und Header basierende Routenauswahl aktivieren. Darüber hinaus sind zahlreiche Aspekte der attributenrouten jetzt anpassbare über die **IDirectRouteFactory** Schnittstelle und **RouteFactoryAttribute** Klasse. Das Routenpräfix ist jetzt erweiterbar ist, über die **IRoutePrefix** Schnittstelle und **RoutePrefixAttribute** Klasse.

Wir haben bereitgestellt, die eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , Einschränkungen zum dynamischen Filtern von Domänencontrollern über einen "api-Version" HTTP-Header verwendet.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Hilfe zur Seite Verbesserungen

Web-API 2.1 umfasst die folgenden Verbesserungen auf [-API-Hilfeseiten](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Dokumentation der einzelnen Eigenschaften von Parametern oder Rückgabetypen von Aktionen.
- Die Dokumentation von datenanmerkungen-Modell.

Das Design der Benutzeroberfläche von Hilfeseiten wurde ebenfalls aktualisiert, um diese Änderungen zu berücksichtigen.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute-Unterstützung

Web-API 2.1 unterstützt das Ignorieren von URL-Mustern in Web-API-routing durch eine Reihe von **IgnoreRoute** Erweiterungsmethoden in **HttpRouteCollection**. Diese Methoden dazu führen, dass Web-API, um alle URLs zu ignorieren, die eine angegebene Vorlage überein und ermöglichen dem Host bei Bedarf zusätzliche Verarbeitung angewendet.

Im folgenden Beispiel wird ignoriert, URIs, die mit beginnt ein &quot;Inhalt&quot; Segment:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON Medientypformatierer

Web-API unterstützt jetzt auch die [BSON](http://bsonspec.org/) Übertragungsformat, sowohl auf dem Client als auch auf dem Server.

Um BSON auf dem Server zu aktivieren, fügen Sie der **BsonMediaTypeFormatter** der Formatierer Auflistung:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

So sieht wie ein Client .NET BSON Format nutzen kann:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Wir haben bereitgestellt, die eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) zeigt, dass sowohl die Client-als auch die Seite.

Weitere Informationen finden Sie unter [BSON-Unterstützung in Web-API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Bessere Unterstützung für asynchrone Filter

Web-API unterstützt nun eine einfache Möglichkeit zum Erstellen von Filtern, die asynchron ausgeführt werden soll. Diese Funktion ist hilfreich, der Filter für eine asynchrone Aktion, z. B. den Zugriff eine Datenbank benötigt wird. Bisher mussten, zum Erstellen einer Async-Filters können Sie die Filter-Schnittstelle selbst implementiert, da die Filter-Basisklassen nur synchrone Methoden verfügbar gemacht. Nachdem Sie die virtuellen überschreiben können `On*Async` Methoden des Filters Basisklasse.

Zum Beispiel:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

Die **AuthorizationFilterAttribute**, **ActionFilterAttribute**, und **ExceptionFilterAttribute** alle Klassen unterstützt asynchrone in Web-API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Abfrage für den Client Formatierung Bibliothek analysieren

Zuvor **System.Net.Http.Formatting** analysieren und Aktualisieren von URI-Abfragen für serverseitigen Code unterstützt, aber die entsprechende portable Bibliothek wurde dieses Feature fehlt. Im Web-API 2.1 kann eine Clientanwendung jetzt problemlos analysieren und aktualisieren eine Abfragezeichenfolge.

Die folgenden Beispiele zeigen, wie zum Analysieren, ändern und URI-Abfragen zu generieren. (Die Beispiele zeigen eine Konsolenanwendung aus Gründen der Einfachheit.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und neueste Änderungen in der ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Routing-Attribut

Mehrdeutigkeiten im Attribut routing Übereinstimmungen Bericht jetzt eine Fehlermeldung statt die erste Übereinstimmung auswählen.

Attributenrouten sind nicht zulässig, von der Verwendung der *{Controller}* Parameter, und von der Verwendung der *{Aktion}* Parameter auf die Routen zu Aktionen platziert. Diese Parameter würde sehr wahrscheinlich zu Mehrdeutigkeiten führen.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Gerüstbau MVC/Web-API in einem Projekt mit 5.1 Pakete-Ergebnissen in 5.0-Paketen für Argumente, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET Web API 2.1 RTM wird nicht Visual Studio-Tools, z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung aktualisiert. Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (5.0.0.0). Daher wird das Gerüst ASP.NET die Vorgängerversion (5.0.0.0) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind. Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben.

Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren der Pakete auf Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen der Web-API und MVC konsistent sind.

### <a name="type-renames"></a>Umbenennen von Typ

Einige der Typen für die Attribut-routing-Erweiterbarkeit verwendet wurden, die RTM-Version 2.1 von der RC-umbenannt.

| Alter Typname (2.1 RC) | Neuer Typname (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Ausnahmefilter nicht aggregierte Ausnahmen in Async-Aktionen entpacken.

Zuvor, wenn eine asynchrone Aktion ausgelöst hat eine **AggregateException**, ein Ausnahmefilter würde die Ausnahme entpacken und **OnException** erhalten die Basis-Ausnahme. 2.1, der Ausnahmefilter ist nicht entpacken, und **OnException** Ruft die ursprüngliche **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Fehlerkorrekturen

Diese Version enthält auch verschiedene Fehlerbehebungen. Sie können die vollständige Liste hier finden:

- [5.1.0-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

Die 5.1.2 Paket enthält, aber keine Fehlerkorrekturen IntelliSense-Updates.
