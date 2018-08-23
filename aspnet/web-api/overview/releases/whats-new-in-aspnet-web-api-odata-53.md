---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Was ist neu in ASP.NET Web API OData 5.3 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838057"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Neuerungen in ASP.NET Web API OData 5.3
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt, was neu in ASP.NET Web API OData 5.3 ist.

- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [OData-Kernbibliotheken](#corelib)
- [Neue Features](#newf)
- [Bekannte Probleme und aktueller Änderungen](#known-issues)
- [Fehlerbehebungen](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht. Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Finden Sie Tutorials und anderem Dokumentationsmaterial zu ASP.NET Web API OData auf der [ASP.NET-Website](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData-Kernbibliotheken

Für OData v4 und verwendet Web-API jetzt ODataLib Version 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Neue Funktionen in ASP.NET Web-API OData 5.3

### <a name="support-for-levels-in-expand"></a>Unterstützung für $levels in $erweitern

Sie können die $levels Abfrage, die Option in der $ Abfragen zu erweitern. Zum Beispiel:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Diese Abfrage entspricht:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Unterstützung für öffnen Entitätstypen

Ein *offener Typ* ist ein Stuctured, die dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Definition eines Typs deklariert werden. Offene Typen können Sie die Flexibilität mit Ihren Datenmodellen hinzufügen. Weitere Informationen finden Sie unter Xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Unterstützung für dynamische Sammlung von Eigenschaften in offenen Typen

Zuvor mussten eine dynamische Eigenschaft ein einzelner Wert sein. In 5.3 haben dynamische Eigenschaften der Auflistungswerte. Z. B. in der folgenden JSON-Nutzlast die `Emails` -Eigenschaft ist eine dynamische Eigenschaft und die Auflistung von String-Typ ist:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Unterstützung für die Vererbung für komplexe Typen

Komplexe Typen können nun von einem Basistyp erben. Ein OData-Dienst kann z. B. folgenden komplexen Typen definieren:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

So sieht das EDM in diesem Beispiel aus:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Weitere Informationen finden Sie unter [OData Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Abfrageoptionen

Problem: Verwenden von geschachtelten $erweitern mit $levels = maximale Anzahl von Ergebnissen in einer falschen erweiterungstiefe.

Betrachten Sie z. B. die folgende Anforderung:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Wenn `MaxExpansionDepth` 5 ist, diese Abfrage würde eine erweiterungstiefe 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Fehlerbehebungen und kleinere Feature-Updates

Diese Version umfasst auch mehrere Fehlerbehebungen und kleinere Feature Updates. Sie können hier die vollständige Liste finden:

- [Fehlerkorrekturen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

In dieser Version, die wir vorgenommen eine [Programmfehlerbehebung](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) einiger der AllowedFunctions Enumerationen. Diese Version ist keine anderen Fehlerbehebungen oder neuen Features.
