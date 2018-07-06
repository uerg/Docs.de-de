---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Neuigkeiten in der ASP.NET Web API 2.2 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 0f0794988da712897092ab808a08fca5eeebb6d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813277"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Neuerungen in ASP.NET Web API 2.2
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt, welche neuerungen für ASP.NET Web API 2.2.

- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Features in der ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Attribut-Routing-Verbesserungen](#ARI)
    - [Web-API-Client-Unterstützung für Windows Phone 8.1](#phone)
- [Bekannte Probleme und aktueller Änderungen](#known-issues)
- [Fehlerbehebungen](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI vom Typ 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht. Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation. Das neueste Paket mit ASP.NET Web API 2.2 hat die folgende Version: "5.2.0". Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.

Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET Web API 2.2 stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Neue Features in der ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Diese Version bietet Unterstützung für OData v4-Protokolls. Weitere Informationen finden Sie unter den [Web-API OData v4-Dokumentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Hier sind einige der wichtigsten Features und Änderungen für OData v4:

- [Unterstützung von Aliasing-Eigenschaften in der OData-Modell](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Unterstützung für ComplexTypeAttribute "," AssociationAttribute "," TimesTampAttribute "und" ConcurrencyCheckAttribute in ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Geben Sie die Möglichkeit, die benutzerfreundliche Titel für Aktionen angeben](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL-UriParser integrieren
- Unterstützung für [Enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [Kapselung](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) und [Singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Unterstützung für primitive Typen umwandeln
- [Unterstützung für OData-Funktion](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Unterstützen Sie parameteraliase für Funktionsaufrufe](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Camel Case Benennungskonvention im Modell zu unterstützen.](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Unterstützung für cast() in $filter
- Unterstützung für open komplexen Typ
- Entfernte EntitySetController und AsyncEntitySetController
- [Geänderte $link zu $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Hinzugefügte Attribut-routing-Unterstützung](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- OData-Kernbibliotheken 6.4.0 verwendet

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Attribut-Routing-Verbesserungen

Attributrouting jetzt bietet einen Erweiterungspunkt aufgerufen IDirectRouteProvider, die ermöglicht die vollständige Kontrolle darüber, wie attributrouten erkannt und konfiguriert werden. Ein IDirectRouteProvider ist verantwortlich für die Angabe einer Liste von Aktionen und Controller zusammen mit zugehörigen Routeninformationen an genau welche Routingkonfiguration für diese Aktionen erforderlich ist. Eine IDirectRouteProvider-Implementierung kann angegeben werden, wenn MapAttributes/MapHttpAttributeRoutes aufrufen.

Anpassen von IDirectRouteProvider werden am einfachsten durch die Erweiterung unserer Standardimplementierung DefaultDirectRouteProvider. Diese Klasse stellt separate überschreibbare virtuelle Methoden, um die Logik zum Ermitteln von Attributen, routeneinträge erstellen und Ermitteln von Routenpräfix und Bereichspräfix zu ändern.

Es folgen einige Beispiele dazu, wie Sie dieser neue Erweiterungspunkt verwenden können:

1. Unterstützung von Vererbung von routenattribute

    Beispiel:

    Hier würde die Anforderung wie "/ api/Werte/10" erfolgreich "Erfolg: 10" zurückgeben

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Geben Sie einen Standardnamen für die Route für die attributrouten, durch eine Konvention, die Sie wie folgt. In der Standardeinstellung erstellt attributrouting nicht automatisch Namen für die attributrouten.
3. Ändern Sie attributrouten routenvorlage an einem zentralen Ort aus, bevor sie in der Routentabelle enden.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Web-API-Client-Unterstützung für Windows Phone 8.1

Jetzt können Sie das Web-API-Client-NuGet-Paket implementieren Sie Ihre Web-API-Client-Logik beim Windows Phone 8.1 als Ziel oder von einer universellen App.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Modell-Generator

Problem: Überladene Funktionen können nicht als FunctionImport verfügbar gemacht werden

Wenn 2 überladene Funktionen vorhanden sind, und sie auch FunctionImport sind wie unten, klicken Sie dann anfordern ~/GetAllConventionCustomers(CustomerName={customerName}) führt System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Problemumgehung: Die problemumgehung für dieses Problem ist, um sowohl die funktionsüberladungen als FunctionImports hinzuzufügen.

#### <a name="odata-routing"></a>OData-Routing

Zeichenfolgenliterale, die die URL-codierte Schrägstrich (% 2F), und backslash(%5C) verursachen einen 404-Fehler auf, wenn sie in den Pfaden der OData-Ressource verwendet werden.

Zeichenfolgenliterale können z. B. als Parameter von Funktionen oder Schlüsselwerte der Entitätenmengen in den Pfaden der OData-Ressource verwendet werden.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Wenn Dienste diese Anforderungen den Hosts werden Escapezeichen erhalten Escapesequenzen die fest, bevor sie an die Web-API-Laufzeit übergeben. Dies schützt vor Angriffen wie folgt:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Dies bewirkt, dass den Web-API OData-Stapel einen 404-Fehler (nicht gefunden) zurückgegeben. Um diesen Fehler zu vermeiden, sollte der Client die doppelte Escapesequenzen für Schrägstrich (% 252F) und umgekehrter Schrägstrich (% 255-C) verwenden. Dies geschieht nicht für Abfragezeichenfolgen, z. B. /Employees? $filter = Name Eq "Name % 2F"

**Beachten Sie ohne Escapezeichen Schrägstriche ("/"), und umgekehrte Schrägstriche (") sind nicht in OData-Ressource Pfad Zeichenfolgenliterale zulässig. Schrägstriche sollte angezeigt werden, nur als Pfadtrennzeichen und umgekehrte Schrägstriche sollten nicht alle in der OData-Ressourcenpfad angezeigt. (Beide sind in einigen Teilen des OData-Abfragezeichenfolge verwendet werden.)**

Problemumgehung: Sie könnten die Parse-Methode der DefaultODataPathHandler der Schrägstrich und umgekehrte Schrägstriche sind bei Zeichenfolgenliteralen, die vor der Analyse tatsächlich als Escapezeichen überschreiben. Sie finden ein Beispiel dieses Ansatzes besteht.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Queryable]

Das Attribut [Queryable] ist veraltet. Neue OData v3-Anwendungen sollten verwenden **System.Web.Http.OData.EnableQueryAttribute**.

Die **ODataHttpConfigurationExtensions.EnableQuerySupport** Erweiterungsmethode bietet nun eine **EnableQueryAttribute** auf die globale filterauflistung. Wenn alle Controller haben die **[Queryable]** Attribut aufrufen `config.EnableQuerySupport()` führt dazu, dass die **[Queryable]** Attribut fehlschlägt

Die empfohlene Methode zum Beheben dieses Problems ist, zum Ersetzen aller Instanzen des **queryableattribute überprüft** mit **System.Web.Http.OData.EnableQueryAttribute**.

Eine alternative problemumgehung besteht darin, den folgenden Code in Ihrer Web-API-Konfiguration verwenden:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Attribut-Routing

Problem: Modellbindung des komplexen Typs ab, mit FromUri-Attribut versehen ist, verhält sich anders bei Verwendung der Attribut-Routing.

Folgenden Link verfolgt das Problem auch und Details zu der dieses Problem zu umgehen.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problem: Gerüstbau MVC-Web-API in einem Projekt mit 5.2.0 Pakete führt 5.1.2 Pakete, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.2 wird Visual Studio-Tools wie z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung nicht aktualisiert. Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (z. B. 5.1.2 in Update 2). Daher wird das Gerüst ASP.NET die vorherige Version (z. B. 5.1.2 in Update 2) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind. Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten. Bei Verwendung von ASP.NET-Gerüstbau nach der Aktualisierung der Pakete Ihrer Projekte zu Web-API 2.2 oder ASP.NET MVC 5.2 Stellen Sie sicher, dass die Versionen von Web-API und ASP.NET MVC konsistent sind.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Fehlerbehebungen und kleinere Feature-Updates

Diese Version umfasst auch mehrere Fehlerbehebungen und kleinere Feature Updates. Sie können hier die vollständige Liste finden:

- [5.2-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Das Microsoft.AspNet.OData 5.2.1-Paket enthält die Aktualisierung von NuGet-Abhängigkeiten, aber keine Programmfehlerbehebungen bereitgestellt. Mit diesem Update kann steht nicht mehr eine strenge Abhängigkeit auf Microsoft.OData.Core 6.4.0 jedoch ein einem upgrade auf eine beliebige Version 7.0.0 bis 6.4.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

In dieser Version haben wir eine abhängigkeitsänderung für `Json.Net 6.0.4`. Weitere Informationen zu neuerungen in dieser Version von `Json.NET`, finden Sie unter [Json.NET 6.0 Release 4 – JSON-Zusammenführung, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Diese Version keine anderen neuen Features oder Programmfehlerbehebungen in Web-API. Wir haben anschließend alle anderen abhängigen Pakete aktualisiert, die wir besitzen, um diese neue Version von Web-API abhängen.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI vom Typ 5.2.3 Beta

Informieren Sie sich über die Version [hier](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Diese Version enthält nur Fehler behoben. Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) um die Liste der in diesem Release behobene Probleme anzuzeigen.
