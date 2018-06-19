---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Sicherheitshinweise für ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868707"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Sicherheitshinweise für ASP.NET Web API 2 OData
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieses Thema beschreibt einige der Sicherheitsprobleme, die Sie berücksichtigen sollten, wenn Sie ein Dataset über OData verfügbar zu machen.

## <a name="edm-security"></a>EDM-Sicherheit

Die Semantik der Abfrage basieren auf dem Entity Data Model (EDM), nicht die zugrunde liegenden Modelltypen. Sie können eine Eigenschaft aus den EDM ausschließen und wird nicht für die Abfrage sichtbar sein. Nehmen Sie beispielsweise an, dass Ihr Modell Mitarbeitertyp mit Gehalt-Eigenschaft enthält. Möglicherweise möchten diese Eigenschaft von EDM von Clients ausblenden auszuschließen.

Es gibt zwei Möglichkeiten, schließt eine Eigenschaft aus den EDM. Sie können festlegen, die **[IgnoreDataMember]** Attribut für die Eigenschaft in der Modellklasse:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Sie können auch die Eigenschaft EDM programmgesteuert aufheben:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Abfrage-Sicherheit

Ein böswilliger oder Naïve-Client möglicherweise um eine Abfrage erstellen, die zum Ausführen sehr viel Zeit beansprucht. Im schlimmsten Fall kann dies den Zugriff auf den Dienst unterbrechen.

Die **[Queryable]** -Attribut ist ein Aktionsfilter, der analysiert, überprüft und wendet die Abfrage. Der Filter konvertiert die Abfrageoptionen in einem LINQ-Ausdruck. Wenn der OData-Controller gibt eine **IQueryable** Typ, der **IQueryable** LINQ-Anbieter wandelt LINQ-Ausdruck in einer Abfrage. Daher hängen die Leistung auf der LINQ-Anbieter, der verwendet wird, sowie auf den bestimmten Merkmalen Ihres Datasets oder Datenbank-Schemas.

Weitere Informationen zur Verwendung von OData-Abfrageoptionen in ASP.NET Web-API finden Sie unter [OData-Abfrageoptionen unterstützen](supporting-odata-query-options.md).

Wenn Sie wissen, dass alle Clients (z. B. in einer unternehmensumgebung) als vertrauenswürdig eingestuft werden oder wenn Ihr Dataset klein ist, ist möglicherweise die abfrageleistung ein Problem nicht. Andernfalls sollten Sie die folgenden Empfehlungen beachten.

- Testen Sie des Diensts mit verschiedenen Abfragen und erstellen Sie die Datenbank ein Profil.
- Aktivieren Sie servergesteuerte Auslagerung, um zu vermeiden, ein großes Dataset in einer Abfrage zurückgegeben. Weitere Informationen finden Sie unter [Server-Driven Paging](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Erforderlich $filter und $orderby? Einige Anwendungen gewähren, Client paging, $top und $skip verwenden, und deaktivieren die anderen Abfrageoptionen. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Erwägen Sie $orderby einzuschränken, um Eigenschaften in einem gruppierten Index. Sortieren von großen Daten ohne gruppierten Index ist langsam. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Maximale Knotenanzahl: die **MaxNodeCount** Eigenschaft **[Queryable]** legt die maximale Anzahl Knoten, die in der Syntaxstruktur $filter erlaubt. Der Standardwert ist 100, aber Sie können einen niedrigeren Wert festlegen möchten, da eine große Anzahl von Knoten zum Kompilieren langsam sein kann. Dies gilt besonders bei Verwendung von LINQ to Objects (d. h. LINQ-Abfragen in einer Auflistung im Arbeitsspeicher, ohne die Verwendung eines intermediate LINQ-Anbieter). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Deaktivieren Sie ggf. die any() und all()-Funktionen, wie diese langsam sein können. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Wenn alle Zeichenfolgeneigenschaften große Zeichenfolgen #8212for Beispiel eine produktbeschreibung oder einer Blogeintrag &, #8212consider deaktivieren die String-Funktionen enthalten. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Erwägen Sie, untersagen von Filtern für Navigationseigenschaften. Filtern nach Navigationseigenschaften kann in einem Join zurückgeben, was möglicherweise langsam, je nach Datenbankschema. Der folgende Code zeigt eine abfragevalidator, die verhindert, dass für Navigationseigenschaften filtern. Weitere Informationen zu Abfrage Validierungssteuerelemente, finden Sie unter [Abfrage Überprüfung](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Erwägen Sie, das Einschränken des $filter Abfragen nach schreiben ein Validator, das für Ihre Datenbank angepasst wird. Betrachten Sie beispielsweise die beiden Abfragen aus: 

  - Alle Filme mit Akteure, deren Nachname mit "A" beginnt.
  - Alle Filme 1994 freigegeben.

    Wenn Filme von Akteuren, indiziert sind, müssen Sie die erste Abfrage möglicherweise das Datenbankmodul zum Scannen der gesamten Liste von Filmen. Während die zweite Abfrage akzeptabel sein kann, werden die Annahme, dass Filme nach Version Jahr indiziert.

    Der folgende Code zeigt ein Validator, die ermöglicht das Filtern auf die Eigenschaften "ReleaseYear" und "Title", aber keine anderen Eigenschaften.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Im Allgemeinen sollten Sie die $filter-Funktionen, die Sie benötigen. Wenn Ihre Clients nicht der vollständige Ausdruckskraft des $filter benötigen, können Sie die zulässigen Funktionen begrenzen.
