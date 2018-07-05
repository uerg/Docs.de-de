---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Was ist neu in ASP.NET Web-API 2.1 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385699"
---
<a name="whats-new-in-aspnet-web-api-21"></a>Neuerungen in ASP.NET Web-API 2.1
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt, welche neuerungen für ASP.NET Web-API 2.1.

- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Features in ASP.NET Web-API 2.1](#new-features)

    - [Globale Fehlerbehandlung](#global-error)
    - [Attribut-Routing-Verbesserungen](#attribute-routing)
    - [Verbesserungen der Hilfe auf der Eigenschaftenseite](#help-page)
    - [IgnoreRoute-Unterstützung](#ignoreroute)
    - [BSON Medientypformatierer](#bson)
    - [Bessere Unterstützung für Async-Filter](#async-filters)
    - [Abfrageanalyse, die für den Client, die Formatierung der Bibliothek](#query-parsing)
- [Bekannte Probleme und aktueller Änderungen](#known-issues)
- [Fehlerbehebungen](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht. Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation. Das neueste Paket mit ASP.NET Web-API 2.1 RTM hat die folgende Version: "5.1.2". Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.

Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET Web-API 2.1 RTM stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Neue Features in ASP.NET Web-API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Globale Fehlerbehandlung

Alle nicht behandelte Ausnahmen können jetzt über eine zentrale Mechanismus protokolliert werden, und das Verhalten für nicht behandelte Ausnahmen kann angepasst werden.

Das Framework unterstützt mehrere Protokollierungen Ausnahmen, die alle finden Sie unter der nicht behandelten Ausnahme und Informationen über den Kontext, in dem es aufgetreten ist, z. B. die Anforderung, die zum Zeitpunkt verarbeitet werden.

Der folgende Code verwendet beispielsweise System.Diagnostics.TraceSource, um alle nicht behandelten Ausnahmen zu protokollieren:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Sie können auch der Standardausnahmehandler ersetzen, damit Sie die HTTP-Antwortnachricht vollständig anpassen können, die gesendet wird, wenn eine nicht behandelte Ausnahme auftritt.

Wir haben bereitgestellt, eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , protokolliert alle Ausnahmefehler, über das beliebte ELMAH-Framework.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Attribut-Routing-Verbesserungen

Attributrouting nun unterstützt Einschränkungen, versionsverwaltung und Header-basierte Routenauswahl aktiviert. Darüber hinaus sind viele Aspekte der attributrouten jetzt über anpassbare der **IDirectRouteFactory** Schnittstelle und **RouteFactoryAttribute** Klasse. Das Routenpräfix ist nun erweiterbar, über die **IRoutePrefix** Schnittstelle und **RoutePrefixAttribute** Klasse.

Wir haben bereitgestellt, eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) , Einschränkungen, dynamisch Filtern von Controller über eine HTTP-Header "api-Version" verwendet.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Verbesserungen der Hilfe auf der Eigenschaftenseite

Web-API 2.1 umfasst die folgenden Erweiterungen [-API-Hilfeseiten](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Dokumentation der einzelnen Eigenschaften von Parametern oder Rückgabetypen von Aktionen.
- Die Dokumentation von datenmodellanmerkungen.

Das Design der Benutzeroberfläche von Hilfeseiten wurde ebenfalls aktualisiert, um diese Änderungen zu berücksichtigen.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute-Unterstützung

Die Web-API 2.1 unterstützt, wird ignoriert, URL-Mustern im Web-API-routing, über einen Satz von **IgnoreRoute** Erweiterungsmethoden **HttpRouteCollection**. Diese Methoden dazu führen, dass Web-API, um alle URLs zu ignorieren, die eine angegebene Vorlage entsprechen, und der Host gelten zusätzliche Verarbeitung aus, falls zutreffend.

Im folgenden Beispiel wird ignoriert, URIs, die mit einem &quot;Inhalt&quot; Segment:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON Medientypformatierer

Web-API jetzt unterstützt die [BSON](http://bsonspec.org/) Wire-Format, sowohl auf dem Client als auch auf dem Server.

Um BSON auf der Serverseite zu aktivieren, fügen Sie der **BsonMediaTypeFormatter** der Formatierer Auflistung:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Hier ist, wie ein .NET Client BSON-Format nutzen kann:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Wir haben bereitgestellt, eine [Beispiel](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) zeigt, dass sowohl die Client- und Serverseite.

Weitere Informationen finden Sie unter [BSON-Unterstützung in Web-API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Bessere Unterstützung für Async-Filter

Web-API unterstützt jetzt eine einfache Möglichkeit zum Erstellen von Filtern, die asynchron ausgeführt werden soll. Diese Funktion ist nützlich, wird der Filter muss eine Async-Aktion, wie Access eine Datenbank ausführen. Zuvor musste einen asynchroner Filter zu erstellen, die filterschnittstelle selbst implementiert, da die Filter-Basisklassen nur synchrone Methoden verfügbar gemacht. Nachdem Sie das virtuelle außer Kraft setzen können `On*Async` Methoden des Filters-Basisklasse.

Zum Beispiel:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

Die **AuthorizationFilterAttribute**, **ActionFilterAttribute**, und **ExceptionFilterAttribute** Klassen, die alle unterstützen Async in Web-API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Abfrageanalyse, die für den Client, die Formatierung der Bibliothek

Zuvor **System.Net.Http.Formatting** analysieren und Aktualisieren von URI-Abfragen für serverseitigen Code unterstützt, aber die entsprechende portable Bibliothek wurde dieses Feature fehlt. In Web-API 2.1 kann eine Clientanwendung jetzt leicht analysieren und eine Abfragezeichenfolge zu aktualisieren.

Die folgenden Beispiele zeigen, wie Sie analysieren, ändern und URI-Abfragen zu generieren. (Die Beispiele zeigen eine Konsolenanwendung aus Gründen der Einfachheit.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET Web-API 2.1 RTM.

### <a name="attribute-routing"></a>Attribut-Routing

Allerdings haben Mehrdeutigkeiten in Attribut-routing-Übereinstimmungen meldet jetzt eine Fehlermeldung statt die erste Übereinstimmung auswählen.

Attributrouten untersagt die *{Controller}* Parameter, und mit der *{Action}* Parameter auf Routen für Aktionen platziert. Diese Parameter würde wahrscheinlich zu Mehrdeutigkeiten führen.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Gerüstbau für MVC-Web-API mit 5.1 Pakete führt 5.0-Pakete in ein Projekt für diejenigen, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET Web-API 2.1 RTM wird nicht der Visual Studio-Tools, z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung aktualisiert. Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (5.0.0.0). Daher wird das Gerüst ASP.NET die vorherige Version (5.0.0.0) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind. Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten.

Wenn Sie ASP.NET-Gerüstbau nach der Aktualisierung der Pakete auf Web-API 2.1 oder ASP.NET MVC 5.1 verwenden, stellen Sie sicher, dass die Versionen von Web-API und MVC konsistent sind.

### <a name="type-renames"></a>Typ umbenennen

Einige der Typen für die Attribut-routing-Erweiterbarkeit verwendet wurden über die RC-Version in der RTM-Version 2.1 umbenannt.

| Alter Typname (2.1 RC) | Neuer Typname (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Ausnahmefilter sind nicht Aggregieren von Ausnahmen in Async-Aktionen ausgelöst entpacken.

Zuvor, wenn eine asynchrone Aktion ausgelöst hat eine **"AggregateException"**, Ausnahmefilter würde die Ausnahme, entpacken und **OnException** die Basisausnahme erhalten. 2.1, Ausnahmefilter ist nicht entpacken, und **OnException** Ruft die ursprüngliche **"AggregateException"**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Fehlerkorrekturen

Diese Version enthält auch mehrere Fehler behoben. Sie können hier die vollständige Liste finden:

- [5.1.0-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

Die 5.1.2 Paket enthält die IntelliSense-Updates, aber keine Programmfehlerbehebungen bereitgestellt.
