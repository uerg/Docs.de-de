---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Neuigkeiten in der ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 89b065fccd0e4864f4a24c37b4caa29a1e127840
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961298"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Neuigkeiten in der ASP.NET Web API 2.2
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt die Neuigkeiten für ASP.NET Web API 2.2.

- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [Neue Funktionen in ASP.NET Web-API 2.2](#newf)

    - [OData v4](#OData)
    - [-Attribut Routing Verbesserungen](#ARI)
    - [Web-API-Client-Unterstützung für Windows Phone 8.1](#phone)
- [Bekannte Probleme und aktueller Änderungen](#known-issues)
- [Fehlerkorrekturen](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung. Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation. Das aktuellste ASP.NET Web API-2.2-Paket ist die folgende Version: "5.2.0". Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.

Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET Web API 2.2 stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Neue Funktionen in ASP.NET Web-API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Diese Version bietet Unterstützung für OData v4-Protokoll. Weitere Informationen finden Sie unter der [Web API OData v4-Dokumentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Hier sind einige der wichtigsten Funktionen und Änderungen für OData v4:

- [Unterstützung für Aliasing-Eigenschaften in OData-Modell](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Unterstützung für ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute und ConcurrencyCheckAttribute in ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Geben Sie die angezeigten Titel für Aktionen ermöglichen](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL-UriParser integriert
- Unterstützung für [Enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [Containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) und [Singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Unterstützung für primitive Typen umwandeln
- [Unterstützung für OData-Funktion](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Parameteraliase für Funktionsaufrufe zu unterstützen.](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Camel-Case Benennungskonvention im Modell unterstützen](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Unterstützung für cast() in $filter
- Unterstützung für offenen komplexer Typ
- Entfernte EntitySetController und AsyncEntitySetController
- [Geänderte $link zu $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Hinzugefügte Attribut routing-Unterstützung](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- OData-Core-Bibliotheken 6.4.0 verwendet

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>-Attribut Routing Verbesserungen

Routing-Attribut jetzt bietet einen Erweiterungspunkt aufgerufen IDirectRouteProvider, die vollständige Kontrolle über wie attributenrouten erkannt und konfiguriert werden kann. Ein IDirectRouteProvider ist verantwortlich für die Bereitstellung einer Liste von Aktionen und Controller zusammen mit zugehörigen Routeninformationen an genau welche Routingkonfiguration für diese Aktionen gewünscht wird. Eine Implementierung IDirectRouteProvider kann angegeben werden, wenn MapAttributes/MapHttpAttributeRoutes aufrufen.

Anpassen von IDirectRouteProvider werden einfachste durch unsere Standardimplementierung DefaultDirectRouteProvider erweitern. Diese Klasse stellt separate überschreibbare virtuelle Methoden, um die Logik zum Erkennen von Attribute, routeneinträge erstellen und durch die Ermittlung Routenpräfix und Bereichspräfix zu ändern.

Es folgen einige Beispiele für mit dieser neuen Erweiterungspunkt zugesagten:

1. Unterstützung von Vererbung Route-Attribute

    Beispiel:

    Hier würden like "/ api/Werte/10" erfolgreich "Erfolg: 10" zurück

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Geben Sie einen Standardnamen für die Route für die attributenrouten anhand einiger Konvention, die Ihnen gefallen. Standardmäßig erstellt routing-Attribut nicht automatisch Namen für die attributenrouten.
3. Ändern Sie attributenrouten routenvorlage an einer zentralen Stelle aus, bevor sie in der Routingtabelle enden.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Web-API-Client-Unterstützung für Windows Phone 8.1

Sie können nun die Web-API-Client-NuGet-Paket verwenden, Ihre Web-API-Client-Logik implementieren, Windows Phone 8.1 abzielt oder vom in eine universelle App.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und neueste Änderungen in der ASP.NET Web API-2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Modellgenerator

Problem: Überladene Funktionen konnte nicht als FunctionImport verfügbar gemacht werden

Wenn 2 überladene Funktionen vorhanden sind, und sie unterscheiden sich auch um FunctionImport wie unten Anfordern von ~/GetAllConventionCustomers(CustomerName={customerName}) führt System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Problemumgehung: Die problemumgehung für dieses Problem ist sowohl die funktionsüberladungen als FunctionImports hinzugefügt.

#### <a name="odata-routing"></a>OData-Routing

Zeichenfolgenliterale, die die URL-codierte Schrägstrich (% 2F) und backslash(%5C) 404-Fehler verursachen, wenn sie in den Ressourcenpfaden OData verwendet werden.

Zeichenfolgenliterale können z. B. als Parameter von Funktionen oder Schlüsselwerte der angegebenen Entitätenmengen in den Ressourcenpfaden OData verwendet werden.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Erhalt Dienste solche Anforderungen Hosts wird Escapezeichen Escapesequenzen die fest, bevor sie an die Web-API-Runtime übergeben. Dies schützt vor Angriffen wie folgt:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Dies bewirkt, dass der Stapel Web API OData-Fehler 404 (Nichtgefunden) zurückgegeben. Um diesen Fehler zu verhindern, sollte der Client verwenden, die doppelte Escapesequenzen für Schrägstrich ("% 252F") und umgekehrter Schrägstrich ("%" 255 C). Dies geschieht nicht für Abfragezeichenfolgen, z. B. /Employees? $filter = Name Eq "Namen % 2F"

**Notieren Sie sich ohne Escapezeichen Schrägstriche ('/'), und umgekehrte Schrägstriche (") sind nicht zulässig in OData-Ressource Pfad Zeichenfolgenliteralen. Schrägstriche sollte nur als Pfadtrennzeichen angezeigt werden und umgekehrte Schrägstriche sollte nicht auf allen in den Pfad der OData-Ressource angezeigt. (Beide sind in einigen Teilen einer OData-Abfragezeichenfolge verwendet werden kann.)**

Problemumgehung: Sie könnten die Parse-Methode der DefaultODataPathHandler mit dem Schrägstrich und den umgekehrten Schrägstrich in Zeichenfolgenliteralen Escapezeichen vor der Analyse tatsächlich diese überschreiben. Sie finden ein Beispiel dieses Ansatzes besteht.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Abfragbare]

Das Attribut [Queryable] ist veraltet. Neue OData v3-Anwendungen die zu verwendende **System.Web.Http.OData.EnableQueryAttribute**.

Die **ODataHttpConfigurationExtensions.EnableQuerySupport** Erweiterungsmethode fügt nun ein **EnableQueryAttribute** auf die globale filterauflistung. Wenn alle Domänencontroller haben die **[Queryable]** Attribut aufrufen `config.EnableQuerySupport()` führt dazu, dass die **[Queryable]** Attribut fehlschlägt

Die empfohlene Methode zum Beheben dieses Problems ist, zum Ersetzen aller Instanzen des **queryableattribute überprüft** mit **System.Web.Http.OData.EnableQueryAttribute**.

Eine alternative problemumgehung besteht darin, den folgenden Code in Ihrer Web-API-Konfiguration verwenden:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Routing-Attribut

Problem: Wurden die modellbindung des komplexen Typs ab, mit FromUri-Attribut ergänzt wird, verhält sich anders bei Verwendung der Routing-Attribut.

Folgenden Link besteht im Nachverfolgen des Problems auch und Details für dieses Problem zu umgehen.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problem: Gerüstbau MVC/Web-API in einem Projekt mit 5.2.0 Pakete führt 5.1.2 Pakete für Argumente, die nicht bereits im Projekt vorhanden sind

Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.2 aktualisiert nicht Visual Studio-Tools wie z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung. Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (z. B. 5.1.2 in Update 2). Daher wird das Gerüst ASP.NET die Vorgängerversion (z. B. 5.1.2 in Update 2) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind. Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben. Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren die Pakete der Projekte auf Web-API 2.2 oder ASP.NET MVC 5.2 verwenden, stellen Sie sicher, dass die Versionen der Web-API und ASP.NET MVC konsistent sind.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Fehlerbehebungen und kleinere Feature-Updates

Diese Version enthält auch mehrere Programmfehlerbehebungen und kleinere Funktion Updates. Sie können die vollständige Liste hier finden:

- [5.2-Paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Das Microsoft.AspNet.OData 5.2.1-Paket enthält, aber keine Fehlerkorrekturen NuGet-Abhängigkeit Updates. Mit diesem Update können auf Microsoft.OData.Core 6.4.0 mehr eine strikte Abhängigkeit vorhanden ist, aber eine kann auf eine beliebige Version zwischen 6.4.0 und 7.0.0 aktualisieren.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

In dieser Version haben wir eine Abhängigkeit für ändern `Json.Net 6.0.4`. Weitere Informationen zu in dieser Version von Neuigkeiten `Json.NET`, finden Sie unter [Json.NET 6.0 Version 4: Zusammenführen von JSON-Abhängigkeitsinjektion](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Diese Version keine anderen neuen Features oder Fehlerkorrekturen in der Web-API. Wir haben anschließend alle anderen abhängigen Pakete aktualisiert, die wir besitzen, um diese neue Version von Web-API abhängig.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Informieren Sie sich über die Version [hier](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Diese Version enthält nur Fehlerkorrekturen. Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) auf die Liste der in dieser Version behobene Probleme anzuzeigen.
