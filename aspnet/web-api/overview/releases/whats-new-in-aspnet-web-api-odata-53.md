---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Neues in ASP.NET Web API OData 5.3 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Was ist neu in ASP.NET Web API OData 5.3
====================
durch [Microsoft](https://github.com/microsoft)

Dieses Thema beschreibt ASP.NET Web API OData 5.3 Neuigkeiten.

- [Herunterladen](#download)
- [Dokumentation](#documentation)
- [OData-Core-Bibliotheken](#corelib)
- [Neue Funktionen](#newf)
- [Bekannte Probleme und aktueller Änderungen](#known-issues)
- [Fehlerkorrekturen](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Herunterladen

Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung. Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Finden Sie Lernprogramme und Weitere Dokumentationen zu ASP.NET Web API OData auf der [ASP.NET-Website](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData-Core-Bibliotheken

Für OData v4 verwendet Web-API jetzt ODataLib Version 6.5.0 ist

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Neue Funktionen in ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Unterstützung für $levels in $erweitern

Sie können die $levels Abfrage, die Option in der $ Abfragen zu erweitern. Zum Beispiel:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Diese Abfrage ist äquivalent zu:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Unterstützung für Entitätstypen öffnen

Ein *öffnen geben* ist ein Stuctured-Typ, die dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Typdefinition deklariert werden. Offene Typen können Sie Ihre Datenmodelle Flexibilität hinzufügen. Weitere Informationen finden Sie unter "Xxxx".

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Unterstützung für dynamische Sammlungseigenschaften in offenen Typen

Bisher mussten eine dynamische Eigenschaft, einen einzelnen Wert sein. In 5.3 kann über die dynamischen Eigenschaften ausflistungswerten verfügen. In der folgenden JSON-Nutzlast, z. B. die `Emails` -Eigenschaft ist eine dynamische Eigenschaft und der Auflistung der Zeichenfolgentyp ist:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Unterstützung von Vererbung für komplexe Typen

Komplexe Typen können jetzt von einem Basistyp erben. Ein OData-Dienst kann z. B. folgenden komplexen Typen definieren:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Dies ist das EDM für dieses Beispiel aus:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Weitere Informationen finden Sie unter [OData-Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

Dieser Abschnitt beschreibt bekannte Probleme und neueste Änderungen in der ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Abfrageoptionen

Problem: Verwenden von geschachtelten $erweitern mit $levels = max führt zu einer falschen erweiterungstiefe.

Betrachten Sie z. B. die folgende Anforderung:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Wenn `MaxExpansionDepth` 5 ist, diese Abfrage würde eine erweiterungstiefe 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Fehlerbehebungen und kleinere Feature-Updates

Diese Version enthält auch mehrere Programmfehlerbehebungen und kleinere Funktion Updates. Sie können die vollständige Liste hier finden:

- [Fehlerkorrekturen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

In dieser Version erzielt wir eine [Fehlerkorrektur](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) einiger AllowedFunctions-Enumerationen. Diese Version keine keine weiteren Fehlerkorrekturen oder neue Features.
