---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Unterstützung von OData-, in der ASP.NET Web API 2 Abfrageoptionen | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Unterstützen von OData-Abfrageoptionen in ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

OData definiert die Parameter, die zum Ändern einer OData-Abfrage verwendet werden können. Der Client sendet diese Parameter in der Abfragezeichenfolge der Anforderungs-URI. Beispielsweise verwendet ein Client zum Sortieren der Ergebnisse den $orderby-Parameter:

`http://localhost/Products?$orderby=Name`

Die OData-Spezifikation ruft diese Parameter *Abfrageoptionen*. Sie können OData-Abfrageoptionen für alle Web-API-Controller in Ihrem Projekt & #8212 aktivieren. der Controller muss kein OData-Endpunkt sein. Dies bietet Ihnen eine einfache Möglichkeit zum Hinzufügen von Features wie z. B. Filter- und Sortiereigenschaften für jede Web-API-Anwendung.

Vor der Aktivierung von Abfrageoptionen, lesen Sie bitte das Thema [OData-Sicherheitsrichtlinien](odata-security-guidance.md).

- [Aktivieren der OData-Abfrageoptionen](#enable)
- [Beispiele für Abfragen](#examples)
- [Servergesteuertes Paging](#server-paging)
- [Beschränken die Abfrageoptionen](#limiting_query_options)
- [Abfrageoptionen direkt aufrufen](#ODataQueryOptions)
- [Abfrage-Überprüfung](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Aktivieren der OData-Abfrageoptionen

Web-API unterstützt die folgenden OData-Abfrageoptionen:

| Option | Beschreibung |
| --- | --- |
| $expand- | Wird Inline verknüpfte Entitäten erweitert. |
| $filter | Filtert die Ergebnisse auf Grundlage einer booleschen Bedingung. |
| $inlinecount | Weist den Server die Gesamtzahl der übereinstimmenden Entitäten in die Antwort eingeschlossen werden sollen. (Dies ist nützlich für serverseitiges Paging.) |
| $orderby | Sortiert die Ergebnisse. |
| $select | Wählt die Eigenschaften in die Antwort eingeschlossen werden sollen. |
| $skip | Überspringt die ersten n Ergebnisse. |
| $top | Gibt nur der ersten n Ergebnisse zurück. |

Um die OData-Abfrageoptionen verwenden zu können, müssen Sie sie explizit aktivieren. Sie können global für die gesamte Anwendung zu aktivieren, oder für bestimmte Domänencontroller oder bestimmte Aktionen zu aktivieren.

Um OData-Abfrageoptionen global zu aktivieren, rufen **EnableQuerySupport** auf die **HttpConfiguration** Klasse beim Start:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

Die **EnableQuerySupport** Methode ermöglicht Abfrageoptionen global für alle Controlleraktion, die zurückgibt ein **IQueryable** Typ. Wenn Sie nicht, dass die Abfrageoptionen, die für die gesamte Anwendung aktiviert möchten, können Sie sie für bestimmte Controlleraktionen aktivieren, durch Hinzufügen der **[Queryable]** Attribut zur Aktionsmethode.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Beispiele für Abfragen

Dieser Abschnitt zeigt die Typen von Abfragen, die mit der OData-Abfrageoptionen möglich sind. Spezifische Details zu den Abfrageoptionen finden Sie in der OData-Dokumentation unter [www.odata.org](http://www.odata.org/).

Erweitern Sie für Informationen zu $ und $select, finden Sie unter [mit $select, $expand, und $value in OData der ASP.NET Web API](using-select-expand-and-value.md).

**Client-Driven Paging**

Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu begrenzen. Ein Client kann z. B. 10 Einträge blockweise mit "Weiter" Links zum Abrufen der nächsten Seite der Ergebnisse angezeigt. Zu diesem Zweck verwendet der Client die Optionen $top und $skip.

`http://localhost/Products?$top=10&$skip=20`

Die Option $top gibt die maximale Anzahl von Einträgen zurückgegeben, und die Option $skip gibt die Anzahl der zu überspringenden Einträge. Im vorherige Beispiel werden die Einträge 21 bis 30 abgerufen.

**Filtern**

Die Option $filter kann einen Client, der die Ergebnisse zu filtern, indem Sie einen booleschen Ausdruck angewendet wird. Die Filterausdrücke sind sehr leistungsstark. Dazu zählen logische und arithmetische Operatoren, Zeichenfolgenfunktionen und Datumsfunktionen.

| Zurückgeben Sie aller Produkte mit der Kategorie "Toys" gleich. | `http://localhost/Products?$filter=Category`EQ "Toys" |
| --- | --- |
| Gibt alle Produkte mit weniger als 10 Preis zurück. | `http://localhost/Products?$filter=Price`Lt 10 |
| Logische Operatoren: alle Produkte zurück, in dem Preis > = 5 und Preis < = 15. | `http://localhost/Products?$filter=Price`Jede Seite 5 und Preis le 15 |
| Zeichenfolgenfunktionen: alle Produkte mit "Zz" im Namen zurück. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Datumsfunktionen: alle Produkte mit ReleaseDate nach 2005 zurück. | `http://localhost/Products?$filter=year(ReleaseDate)`Gt 2005 |

**Sortieren**

Verwenden Sie zum Sortieren der Ergebnisse des $orderby Filters ein.

| Sortieren Sie nach Preis. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Sortieren Sie nach Preis in absteigender Reihenfolge (höchsten zum niedrigsten). | `http://localhost/Products?$orderby=Price desc` |
| Sortieren Sie nach Kategorie und dann nach dem Preis in absteigender Reihenfolge in Kategorien zu sortieren. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Servergesteuertes Paging

Wenn Ihre Datenbank Millionen von Datensätze enthält, möchten Sie nicht senden sie alle in einer Nutzlast. Um dies zu verhindern, kann der Server die Anzahl der Einträge begrenzen, die in einer einzelnen Antwort gesendet. Legen Sie zum Aktivieren von Serverpaging der **PageSize** Eigenschaft in der **Queryable** Attribut. Der Wert ist die maximale Anzahl von Einträgen zurückgegeben.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Wenn Ihr Controller OData-Format zurückgibt, enthält der Antworttext einen Link zur nächsten Seite der Daten:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Der Client kann diesen Link verwenden, beim Abrufen der nächsten Seite. Die Gesamtanzahl von Einträgen im Resultset zu erhalten, kann der Client die Abfrageoption $inlinecount mit dem Wert "Allpages" festlegen.

`http://localhost/Products?$inlinecount=allpages`

Der Wert weist "Allpages" den Server die Gesamtzahl in die Antwort eingeschlossen werden sollen:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Nächste Seite Links und Inlineanzahl erfordern OData-Format. Der Grund ist, dass OData spezielle Felder im Antworttext für den Link und Anzahl definiert.


Für nicht-OData-Formate, ist weiterhin möglich, zur nächsten Seite Links und Inline-Anzahl, Unterstützung durch wrapping in die Ergebnisse der Abfrage eine **PageResult&lt;T&gt;**  Objekt. Dies erfordert allerdings mehr Code. Im Folgenden ein Beispiel:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Hier ist ein Beispiel für JSON-Antwort:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Beschränken die Abfrageoptionen

Die Abfrageoptionen, den der Client einen hohen Steuerungsgrad die Abfrage, die auf dem Server ausgeführt wird. In einigen Fällen empfiehlt es sich um die verfügbaren Optionen für Sicherheit oder leistungverlusten Gründe zu beschränken. Die **[Queryable]** Attribut verfügt über einige integrierte Eigenschaften dafür. Im Folgenden finden Sie einige Beispiele.

Ermöglichen Sie nur "$skip" und $top, zur Unterstützung von Paging und keine anderen Aufgaben an:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Ermöglichen Sie die Sortierung nur durch bestimmte Eigenschaften, um zu vermeiden, Sortieren nach Eigenschaften, die in der Datenbank nicht indiziert werden:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Ermöglichen Sie die logische Funktion "Eq", aber keine anderen logischen Funktionen:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Lassen Sie keine arithmetischen Operatoren:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Sie können Optionen global einschränken, durch das Erstellen einer **queryableattribute überprüft** Instanz und die Übergabe an die **EnableQuerySupport** Funktion:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Abfrageoptionen direkt aufrufen

Anstatt die **[Queryable]** -Attribut, Sie können die Abfrageoptionen direkt in Ihr Controller aufrufen. Fügen Sie zu diesem Zweck ein **ODataQueryOptions** Parameter für die Controllermethode. Müssen Sie in diesem Fall nicht die **[Queryable]** Attribut.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web-API füllt die **ODataQueryOptions** aus dem URI-Abfragezeichenfolge. Zum Anwenden von der Abfrage übergeben ein **IQueryable** auf die **"ApplyTo" darf** Methode. Die Methode gibt ein anderes **IQueryable**.

Für erweiterte Szenarien, wenn Sie kein **IQueryable** Abfrageanbieter, untersuchen Sie die **ODataQueryOptions** und übersetzt die Abfrageoptionen in eine andere Form. (Z. B. finden Sie RaghuRam Nadimintis Blogbeitrag [Übersetzen von OData-Abfragen in HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), darunter auch eine [Beispiel](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Abfrage-Überprüfung

Die **[Queryable]** Attribut überprüft die Abfrage vor der Ausführung. Der Überprüfungsschritt erfolgt in der **QueryableAttribute.ValidateQuery** Methode. Sie können auch den Überprüfungsprozess anpassen.

Siehe auch [OData-Sicherheitsrichtlinien](odata-security-guidance.md).

Überschreiben eines das Validierungssteuerelement also Klassen zuerst definiert, der **Web.Http.OData.Query.Validators** Namespace. Beispielsweise werden die folgenden Validator-Klasse die Option "Desc" für die Option $orderby deaktiviert.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Unterklasse der **[Queryable]** Attribut überschreiben die **ValidateQuery** Methode.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Legen Sie dann das benutzerdefinierte Attribut entweder global oder pro Controller:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Bei Verwendung von **ODataQueryOptions** direkt das Validierungssteuerelement zu den Optionen festlegen:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
