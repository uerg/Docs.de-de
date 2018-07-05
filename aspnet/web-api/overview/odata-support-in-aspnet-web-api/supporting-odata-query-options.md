---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Unterstützung von OData-in ASP.NET-Web-API 2 Abfrageoptionen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 029a791074a5ab88f1c6d83311e88fc9f726fe6f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814940"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Unterstützung von OData-Abfrageoptionen in der ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

OData definiert die Parameter, die zum Ändern einer OData-Abfrage verwendet werden können. Der Client sendet diese Parameter in der Abfragezeichenfolge der Anforderungs-URI. Um die Ergebnisse zu sortieren, verwendet z. B. ein Client den $orderby-Parameter an:

`http://localhost/Products?$orderby=Name`

Die OData-Spezifikation ruft diese Parameter *Abfrageoptionen*. Sie können OData-Abfrageoptionen für alle Web-API-Controller in Ihrem Projekt &#8212; Controller muss nicht auf einen OData-Endpunkt handeln. Dies bietet Ihnen eine bequeme Möglichkeit zum Hinzufügen von Features wie beispielsweise Filterung und Sortierung für jede Web-API-Anwendung.

Vor dem Aktivieren der Abfrageoptionen, lesen Sie bitte das Thema [OData-Sicherheitsleitfaden](odata-security-guidance.md).

- [Aktivieren der OData-Abfrageoptionen](#enable)
- [Beispiele für Abfragen](#examples)
- [Servergesteuertes Paging](#server-paging)
- [Beschränken die Abfrageoptionen](#limiting_query_options)
- [Abfrageoptionen direkt aufrufen](#ODataQueryOptions)
- [Abfragevalidierung](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Aktivieren der OData-Abfrageoptionen

Web-API unterstützt die folgenden OData-Abfrageoptionen:

| Option | Beschreibung |
| --- | --- |
| der $expand- | Wird die verknüpfte Entitäten Inline erweitert. |
| $filter | Filtert die Ergebnisse, die auf Basis einer booleschen Bedingung. |
| $inlinecount | Weist den Server, der die Gesamtzahl der übereinstimmenden Entitäten in der Antwort enthalten. (Hilfreich für die serverseitige Auslagerung.) |
| $orderby | Sortiert die Ergebnisse. |
| $select | Wählt die Eigenschaften an, in der Antwort enthalten. |
| $skip | Überspringt die ersten n Ergebnisse. |
| $top | Gibt nur der ersten n Ergebnisse zurück. |

Um die OData-Abfrageoptionen verwenden zu können, müssen Sie sie explizit aktivieren. Sie können global für die gesamte Anwendung zu aktivieren, oder für bestimmte Domänencontroller oder bestimmte Aktionen zu aktivieren.

Um OData-Abfrageoptionen global zu aktivieren, rufen **EnableQuerySupport** auf die **HttpConfiguration** Klasse beim Start:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

Die **EnableQuerySupport** Methode können Sie Abfrageoptionen global für alle Controlleraktion, die zurückgibt ein **"IQueryable"** Typ. Wenn Sie nicht, dass die Abfrageoptionen für die gesamte Anwendung aktiviert möchten, können Sie sie für bestimmte Controlleraktionen aktivieren, durch das Hinzufügen der **[Queryable]** Attribut zur Aktionsmethode.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Beispiele für Abfragen

Dieser Abschnitt zeigt die Typen von Abfragen, die mit der OData-Abfrageoptionen möglich sind. Spezifische Details zu den Abfrageoptionen, finden Sie in der OData-Dokumentation unter [www.odata.org](http://www.odata.org/).

Informationen zu $erweitern und $select, finden Sie unter [verwenden $select, $expand, und $value in OData der ASP.NET Web API](using-select-expand-and-value.md).

**Client-gesteuerte Paging**

Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu beschränken. Beispielsweise kann ein Client 10 Einträge jeweils mit "Weiter" Links zum Abrufen der nächsten Seite der Ergebnisse zeigen. Hierzu verwendet der Client die $top und $skip-Optionen.

`http://localhost/Products?$top=10&$skip=20`

$Top können die maximale Anzahl von Einträgen zurückgegeben, und die Option $skip gibt die Anzahl der zu überspringenden Einträge. Im vorherige Beispiel ruft Einträge 21 bis 30.

**Filtern**

Die $filter-Option ermöglicht einen Client, der die Ergebnisse zu filtern, indem Sie einen booleschen Ausdruck anwenden. Die Filterausdrücke sind sehr leistungsstark; Sie enthalten, logische und arithmetische Operatoren, Zeichenfolgenfunktionen und Date-Funktionen.

| Zurückgeben Sie aller Produkte, mit der Kategorie "Toys" gleich. | `http://localhost/Products?$filter=Category` EQ "Toys" |
| --- | --- |
| Geben Sie alle Produkte mit weniger als 10 Preis zurück. | `http://localhost/Products?$filter=Price` Lt 10 |
| Logische Operatoren: alle Produkte zurück, in dem Preis > = 5 und Preis < = 15. | `http://localhost/Products?$filter=Price` ge 5 und Preis le 15 |
| Zeichenfolgenfunktionen: alle Produkte mit "Zz" im Namen zurück. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Datumsfunktionen: alle Produkte mit ReleaseDate nach 2005 zurückgegeben. | `http://localhost/Products?$filter=year(ReleaseDate)` Gt 2005 |

**Sortieren**

Verwenden Sie zum Sortieren der Ergebnisse der $orderby Filter ein.

| Sortieren Sie nach Preis. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Sortieren Sie nach Preis in absteigender Reihenfolge (höchster zu niedrigster Priorität). | `http://localhost/Products?$orderby=Price desc` |
| Sortieren Sie nach Kategorie und dann nach Preis in absteigender Reihenfolge in Kategorien zu sortieren. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Servergesteuertes Paging

Wenn die Datenbank von Millionen von Zeilen enthält, möchten Sie nicht senden sie alle in einer Nutzlast. Um dies zu verhindern, kann der Server die Anzahl der Einträge begrenzen, den sie in einer einzelnen Antwort sendet. Legen Sie zum Aktivieren von Serverpaging der **PageSize** -Eigenschaft in der **Queryable** Attribut. Der Wert ist die maximale Anzahl von Einträgen zurückgegeben.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Wenn Ihr Controller OData-Format zurückgibt, enthält der Antworttext einen Link zur nächsten Seite der Daten:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Der Client kann auf diesen Link verwenden, zum Abrufen der nächsten Seite. Die Gesamtanzahl von Einträgen im Resultset zu finden, kann der Client die Abfrageoption $inlinecount, mit dem Wert "Allpages" festlegen.

`http://localhost/Products?$inlinecount=allpages`

Der Wert weist "Allpages" den Server an der Gesamtanzahl in der Antwort enthalten:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Nächste Seite Links und Inlineanzahl jeweils der OData-Format erforderlich. Der Grund ist, dass OData spezielle Felder im Antworttext zurückgeben, um dem Link und die Anzahl von aufzunehmen definiert.


Für nicht-OData-Formate, ist es weiterhin möglich, Anzahl der Wechsel zur nächsten Seite Links und Inline, zu unterstützen, indem Sie umschließen die Ergebnisse der Abfrage in einem **PageResult&lt;T&gt;**  Objekt. Allerdings ist es ein wenig mehr Code erforderlich. Im Folgenden ein Beispiel:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Hier ist ein Beispiel für JSON-Antwort:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Beschränken die Abfrageoptionen

Die Abfrageoptionen, den der Client einen Großteil der Kontrolle über die Abfrage, die auf dem Server ausgeführt wird. In einigen Fällen empfiehlt es sich um die verfügbaren Optionen aus Gründen der Sicherheit oder Leistung zu beschränken. Die **[Queryable]** Attribut verfügt über einige integrierte Eigenschaften für diese. Im Folgenden finden Sie einige Beispiele.

Zulassen von nur $skip und $top, zur Unterstützung von Paging und nichts anderes:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Lassen Sie die Reihenfolge nur durch bestimmte Eigenschaften, um zu verhindern, Sortieren nach Eigenschaften, die in der Datenbank nicht indiziert werden:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Zulassen der logischen "Eq"-Funktion, aber keine anderen logischen Funktionen:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Lassen Sie keine arithmetischen Operatoren:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Sie können Optionen global einschränken, durch das Erstellen einer **queryableattribute überprüft** -Instanz und die Übergabe an die **EnableQuerySupport** Funktion:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Abfrageoptionen direkt aufrufen

Anstatt die **[Queryable]** -Attribut, Sie können die Abfrageoptionen direkt in Ihrem Controller aufrufen. Fügen Sie zu diesem Zweck eine **ODataQueryOptions** Parameter für die Controllermethode. In diesem Fall müssen nicht die **[Queryable]** Attribut.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web-API füllt die **ODataQueryOptions** aus dem URI-Abfragezeichenfolge. Übergeben Sie zum Anwenden der Abfrage eine **"IQueryable"** auf die **ApplyTo** Methode. Die Methode wird ein anderes **"IQueryable"**.

Für erweiterte Szenarien, wenn Sie kein **"IQueryable"** -Abfrageanbieter, die Sie untersuchen die **ODataQueryOptions** und die Abfrageoptionen in eine andere Form übersetzen. (Z. B. finden Sie in RaghuRam Nadimintis Blogbeitrag [Übersetzen von OData-Abfragen in HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), darunter auch eine [Beispiel](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Abfragevalidierung

Die **[Queryable]** Attribut überprüft, ob die Abfrage vor der Ausführung. Der Überprüfungsschritt erfolgt in der **QueryableAttribute.ValidateQuery** Methode. Sie können auch den Überprüfungsprozess anpassen.

Siehe auch [OData-Sicherheitsleitfaden](odata-security-guidance.md).

Zunächst außer Kraft setzen, die eine für das Validierungssteuerelement, Klassen definiert, der **Web.Http.OData.Query.Validators** Namespace. Beispielsweise werden die folgenden Validator-Klasse die Option 'Desc' für die $orderby-Option deaktiviert.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Unterklasse der **[Queryable]** Attribut zum Überschreiben der **ValidateQuery** Methode.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Legen Sie dann das benutzerdefinierte Attribut entweder global oder pro Controller:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Bei Verwendung von **ODataQueryOptions** direkt festgelegt werden, das Validierungssteuerelement zu den Optionen:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
