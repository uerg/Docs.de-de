---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Sicherheitsempfehlungen für ASP.NET Web-API 2 OData | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 4ba53e15dab83368097a58ba4d0d2e46d113d1d2
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325717"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Sicherheitsempfehlungen für ASP.NET Web-API 2 OData
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieses Thema beschreibt einige der Sicherheitsprobleme, die Sie berücksichtigen sollten, wenn Sie ein Dataset über OData verfügbar zu machen.

## <a name="edm-security"></a>EDM-Sicherheit

Die Semantik der Abfrage basiert auf Entity Data Model (EDM) gibt nicht die zugrunde liegenden Modelltypen. Sie können eine Eigenschaft ausschließen, aus dem EDM und wird nicht für die Abfrage sichtbar sein. Nehmen wir beispielsweise an, dass Ihr Modell einen Employee-Typ mit einem Gehalt-Eigenschaft enthält. Sie sollten diese Eigenschaft des EDM, um es von Clients ausblenden ausgeschlossen werden.

Es gibt zwei Möglichkeiten, schließt eine Eigenschaft aus dem EDM. Sie können festlegen, die **[IgnoreDataMember]** Attribut für die Eigenschaft in der Modellklasse:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Sie können auch programmgesteuert die-Eigenschaft von EDM entfernen:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Abfragesicherheit

Ein böswilliger oder naive-Client möglicherweise zum Erstellen einer Abfrage, die zum Ausführen sehr lange dauert. Im schlimmsten Fall kann dies den Zugriff auf den Dienst unterbrechen.

Die **[Queryable]** -Attribut ist ein Aktionsfilter, der analysiert, überprüft und wendet die Abfrage. Der Filter konvertiert die Abfrageoptionen in einem LINQ-Ausdruck. Wenn der OData-Controller gibt eine **"IQueryable"** Typ, der **"IQueryable"** LINQ-Anbieter konvertiert den LINQ-Ausdruck in einer Abfrage. Aus diesem Grund hängt die Leistung auf der LINQ-Anbieter, der verwendet wird, sowie auf den bestimmten Merkmalen Ihres Schemas Dataset oder eine Datenbank.

Weitere Informationen zur Verwendung von OData-Abfrageoptionen in ASP.NET Web-API finden Sie unter [unterstützt OData-Abfrageoptionen](supporting-odata-query-options.md).

Wenn Sie wissen, dass alle Clients (z. B. in einer unternehmensumgebung) als vertrauenswürdig eingestuft werden, oder wenn Ihr Dataset klein ist, kann die abfrageleistung kein Problem dar. Andernfalls sollten Sie die folgenden Empfehlungen.

- Testen Sie des Diensts mit verschiedenen Abfragen aus, und Stufen Sie die Datenbank.
- Aktivieren Sie servergesteuertes Paging, um zu vermeiden, ein großes Dataset in einer Abfrage zurückgegeben. Weitere Informationen finden Sie unter [Server-Driven Paging](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Erforderlich $filter und $orderby? Bei einigen Anwendungen können paging, mithilfe von $top und $skip zulassen, aber der anderen Abfrageoptionen deaktivieren. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Beachten Sie, $orderby auf Eigenschaften in einem gruppierten Index zu beschränken. Sortieren von großen Daten ohne gruppierten Index ist langsam. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Maximale Knotenanzahl: die **MaxNodeCount** Eigenschaft **[Queryable]** legt die maximale Anzahl Knoten, die in der Syntaxstruktur $filter erlaubt. Der Standardwert ist 100, aber Sie möchten ein niedrigerer Wert, da eine große Anzahl von Knoten zum Kompilieren langsam sein kann. Dies gilt insbesondere bei Verwendung von LINQ to Objects (z. B. LINQ-Abfragen in einer Auflistung im Arbeitsspeicher, ohne einen zwischengeschalteten LINQ-Anbieter). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Deaktivieren Sie ggf. die Funktionen any() und all(), da diese langsam sein können. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Wenn alle Zeichenfolgeneigenschaften große Zeichenfolgen enthalten&#8212;z. B. eine produktbeschreibung oder einem Blogeintrag&#8212;sollten Sie die Zeichenfolgenfunktionen deaktivieren. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Beachten Sie, untersagen von Filtern auf Navigationseigenschaften. Filterung für Navigationseigenschaften kann in einem Join führen, die je nach Schema Ihrer Datenbank langsam sein kann. Der folgende Code zeigt ein Abfrage-Validierungssteuerelement, das verhindert, dass für Navigationseigenschaften des filtern. Weitere Informationen zu Abfrage-Validierungssteuerelemente, finden Sie unter [Abfragevalidierung](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Beachten Sie, Einschränken von $filter Abfragen durch Schreiben ein Validierungssteuerelement, das für Ihre Datenbank angepasst wird. Betrachten Sie beispielsweise diese beiden Abfragen aus: 

  - Alle Filme mit Actors, deren Nachname mit "A" beginnt.
  - Alle Filme im 1994 veröffentlicht wurde.

    Es sei denn, Filme Akteure indiziert sind, müssen möglicherweise die erste Abfrage der Datenbank-Engine, um die gesamte Liste von Filmen zu scannen. Während die zweite Abfrage zulässig ist, werden die Annahme, dass Filme von Veröffentlichungsjahr indiziert.

    Der folgende Code zeigt ein Validierungssteuerelement, das ermöglicht das Filtern auf die Eigenschaften "ReleaseYear" und "Title", aber keine anderen Eigenschaften.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Im Allgemeinen sollten Sie die $filter-Funktionen, die Sie benötigen. Wenn Ihre Clients die vollständige ausdrucksfähigkeit der $filter nicht benötigen, können Sie die zulässigen Funktionen begrenzen.
