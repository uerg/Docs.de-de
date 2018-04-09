---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementieren von Paging effiziente | Microsoft Docs
author: microsoft
description: Schritt 8 wird gezeigt, wie pagingunterstützung für unsere /Dinners-URL hinzufügen, sodass anstatt 1000 s von Abendessen gleichzeitig wir nur 10 bevorstehende Abendessen am darstellen wird...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="implement-efficient-data-paging"></a>Implementieren von Paging effiziente Nutzung von Daten
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 8 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 8 wird die Unterstützung der Paginierung unsere /Dinners-URL hinzufügen, damit anstelle von 1000 s von Abendessen gleichzeitig wir nur 10 bevorstehende Abendessen jeweils - anzeigen und Benutzern die Möglichkeit zum Seite zurück und Vorwärts durch die gesamte Liste in eine friendly SEO-Methode veranschaulicht.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner Schritt 8: Unterstützung der Paginierung

Wenn unsere Website erfolgreich ist, benötigen sie Tausende von bevorstehenden Abendessen angezeigt. Wir müssen sicherstellen, dass unsere UI wird skaliert, um alle von dieser Abendessen behandeln, und ermöglicht es Benutzern, die sie durchsuchen. Um dies zu ermöglichen, fügen wir pagingunterstützung zu unserem */Dinners* URL damit Anzeigen eines 1000 s von Abendessen auf einmal, es werden nur 10 bevorstehende Abendessen jeweils - anzeigen und Benutzern die Möglichkeit zum Seite zurück und Vorwärts durch die gesamte Liste in eine benutzerfreundliche SEO-Methode.

### <a name="index-action-method-recap"></a>Zusammenfassung von Index() Aktion-Methode

Die Aktionsmethode Index() innerhalb der Klasse DinnersController wie derzeit sieht unten:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Wenn eine Anforderung gestellt wird, die */Dinners* URL, er ruft eine Liste mit allen zukünftigen Abendessen und rendert dann eine Liste aller sie:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Grundlegendes zu IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  ist eine Schnittstelle, die mit LINQ als Bestandteil von .NET 3.5 eingeführt wurde. Dadurch werden leistungsfähige "verzögerte Ausführung" Szenarien, denen wir zum Implementieren der Unterstützung der Paginierung nutzen kann.

In unserem DinnerRepository sind wir eine IQueryable zurückgeben&lt;Dinner&gt; Sequenz von unseren FindUpcomingDinners()-Methode:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Das IQueryable-Objekt&lt;Dinner&gt; von unseren FindUpcomingDinners()-Methode zurückgegebene Objekt kapselt eine Abfrage zum Abrufen von Dinner Objekten aus der Datenbank mit LINQ to SQL. Sie wird nicht entscheidend ist dabei, die Abfrage für die Datenbank ausgeführt, bis wir versuchen, Zugriff/die Daten in der Abfrage durchlaufen, oder wir die ToList()-Methode für ihn aufgerufen werden. Der Code, der unsere FindUpcomingDinners()-Methode aufrufen können optional zusätzliche "verketteten" Vorgänge/Filter auf die IQueryable-Objekt hinzufügen&lt;Dinner&gt; Objekt vor dem Ausführen der Abfrage. LINQ to SQL ist intelligent genug, um die kombinierten Abfrage für die Datenbank ausgeführt, wenn die Daten angefordert werden.

Paginglogik implementieren können wir unsere DinnersController Index() Aktionsmethode aktualisieren, damit sie zusätzliche "Überspringen" und "Aufzeichnen" Operatoren für das zurückgegebene IQueryable-Objekt gilt&lt;Dinner&gt; Sequenz vor dem Aufrufen von ToList() darauf:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Der obige Code überspringt die ersten 10 bevorstehenden Abendessen in der Datenbank und versetzt dann wieder 20 Abendessen angezeigt. LINQ to SQL ist intelligent genug, um eine optimierte SQL-Abfrage erstellen, die dies überspringen Logik in der SQL-Datenbank – und nicht in der Web-Server ausführt. Dies bedeutet, dass selbst wenn es in der Datenbank Millionen von bevorstehenden Abendessen haben, nur die 10 gewünschte als Teil dieser Anforderung (wodurch sie effiziente und skalierbare) abgerufen werden.

### <a name="adding-a-page-value-to-the-url"></a>Hinzufügen eines Werts für die "Seite" zu URL

Anstatt eine feste Programmierung eines bestimmten Seitenbereichs, möchten wir unsere URLs "Seite" Parameter enthalten, die den Bereich Dinner gibt an, ein Benutzer angefordert wird.

#### <a name="using-a-querystring-value"></a>Verwenden einen Querystring-Wert

Der folgende Code zeigt, wie aktualisieren wir können unsere Index() Aktionsmethode zur Unterstützung von Querystring-Parameter, und aktivieren die URLs wie */Dinners? Seite 2 =*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Die oben genannten Index() Aktionsmethode hat einen Parameter namens "Seite". Der Parameter deklariert wurde, als ganze Zahl Null-Werte zulässt (also, welche Int? angibt). Dies bedeutet, dass die */Dinners? Seite 2 =* URL führt dazu, dass der Wert "2" als Parameterwert übergeben werden. Die */Dinners* URL (ohne eine Querystring-Wert) führt dazu, dass einen null-Wert übergeben werden.

Wir sind den Seitenwert an die Seitengröße (in diesem Fall 10 Zeilen) multipliziert, um zu bestimmen, wie viele Abendessen zu überspringen. Wir verwenden die ["C#-null COALESCE"-Operator (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) was nützlich ist, beim Umgang mit auf NULL festlegbare Typen. Der obige Code weist Seite den Wert 0, wenn die Seite "-Parameter null ist.

#### <a name="using-embedded-url-values"></a>Verwenden von eingebetteten URL-Werte

Eine Alternative zur Verwendung eines Querystring-Wert wäre, der dieser Parameter innerhalb der tatsächliche URL selbst einbetten. Zum Beispiel: */Dinners/Page/2* oder */Abendessen/2*. ASP.NET MVC umfasst ein leistungsfähiges Modul zum Routingconnectors der URL, das Unterstützung von Szenarien wie folgt vereinfacht.

Benutzer können benutzerdefinierte Routingregeln registrieren, die alle Controllermethode Klasse oder eine Aktion, die wir möchten alle eingehenden oder URL format zugeordnet. Müssen wir nur die Aufgabe besteht darin, die Datei "Global.asax" in Projekt öffnen:

![](implement-efficient-data-paging/_static/image2.png)

Und registrieren Sie eine neue Zuordnungsregel die Hilfsmethode MapRoute() wie der erste Aufruf von Routen verwenden. MapRoute() unten:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Wir werden über eine neue Routingregel, die mit dem Namen "UpcomingDinners" registrieren. Wir sind, der angibt, er hat das Format der URL "Abendessen/Seite" / {Seite} "– wobei {Page} ein Parameterwert, der in die URL eingebettet ist. Der dritte Parameter der Methode MapRoute() gibt an, dass wir URLs zugeordnet werden sollen, die mit diesem Format an die Aktionsmethode Index() für die Klasse DinnersController übereinstimmen.

Wir können denselben Index() Code wie, den wir vor mit unserem Szenario Querystring – verfügen, mit der Ausnahme nun unsere "Seite"-Parameter der URL und nicht die Abfragezeichenfolge stammen:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Und wenn wir führen Sie die Anwendung und geben Sie in */Dinners* sehen wir die ersten 10 bevorstehende Abendessen angezeigt:

![](implement-efficient-data-paging/_static/image3.png)

Und wenn wir eingeben */Dinners/Page/1* sehen wir die nächste Seite der Abendessen angezeigt:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Hinzufügen von UI-Seitennavigation

Der letzte Schritt zum Abschließen der vorstehenden Szenarios Paging wird zum Implementieren von "Weiter" und "früher" Navigationsbenutzeroberfläche in unserer Vorlage anzeigen, Benutzern die Dinner Daten problemlos überspringen aktiviert sein.

Um dies ordnungsgemäß zu implementieren, müssen wir die Gesamtzahl der Abendessen in der Datenbank zu kennen sowie wie viele Seiten mit Daten in diese übersetzt. Wir müssen so berechnen, ob der aktuell angeforderte "Seite" am Anfang oder Ende der Daten ist, und blenden Sie oder die Benutzeroberfläche "zurück" und "Weiter" entsprechend. Wir konnten diese Logik in unserer Index() Aktionsmethode implementieren. Alternativ können wir eine Hilfsklasse unsere Projekt hinzufügen, die diese Logik auf eine Weise mehr Schutzverfahren kapselt.

Im folgenden finden Sie eine einfache "PaginatedList" Hilfsklasse, die aus der Liste abgeleitete&lt;T&gt; Auflistungsklasse erstellt in .NET Framework. Implementiert eine wieder verwendbare Auflistungsklasse, die zum Paginieren einer beliebigen Folge von IQueryable-Daten verwendet werden kann. In der vorliegenden Anwendung NerdDinner lassen wir, es die Arbeit mit IQueryable&lt;Dinner&gt; Ergebnisse, aber es ebenso einfach missbraucht werden gegen IQueryable&lt;Produkt&gt; oder IQueryable&lt;Kunden&gt;in anderen Szenarien der Anwendung führt:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Beachten Sie, oben, wie er berechnet und dann wie macht Eigenschaften verfügbar, "PageIndex", "PageSize", "TotalCount" und "TotalPages". Macht auch dann zwei Helper Eigenschaften "HasPreviousPage" und "HasNextPage", die angeben, ob die Seite der Daten in der Auflistung am Anfang oder Ende der ursprünglichen Sequenz ist. Der obige Code bewirkt, dass zwei SQL-Abfragen ausgeführt werden – wird die erste zum Abrufen der Gesamtanzahl von Objekten Dinner (Dies gibt nicht die Objekte zurück – vielmehr ausgeführt, die eine "COUNT wählen Sie"-Anweisung, die eine ganze Zahl zurückgegeben), und der zweiten Seite wird nur die Zeilen der abrufen Daten, die wir aus unserer Datenbank für die aktuelle Seite der Daten benötigen.

Wir können unsere DinnersController.Index() Hilfsmethode zum Erstellen einer PaginatedList dann aktualisieren&lt;Dinner&gt; aus unserem DinnerRepository.FindUpcomingDinners() führen, sowie deren Übergabe an unsere Vorlage anzeigen:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Wir aktualisieren Sie dann die Vorlage der \Views\Dinners\Index.aspx anzeigen von ViewPage erben&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; statt ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;, und fügen Sie folgenden Code am Ende unserer anzeigen oder Ausblenden der vorherigen und nächsten Navigationsbenutzeroberfläche Vorlage anzeigen:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Beachten Sie, über wie wir die Hilfsmethode Html.RouteLink() verwenden werden, um unsere Hyperlinks zu generieren. Diese Methode ähnelt der Html.ActionLink()-Hilfsmethode, die wir zuvor verwendet haben. Der Unterschied besteht darin, dass wir die URL mit dem routing der Regel, dass wir in unserer Datei "Global.asax" setup "UpcomingDinners" generieren. Dadurch wird sichergestellt, dass wir URLs generieren müssen, zu unseren Index()-Aktionsmethode, die das Format aufweisen: */Dinners/Seite / {Page}* –, in dem die {Seite ""}-Wert einer Variablen wir bieten ist oben basierend auf der aktuellen PageIndex.

Und nun bei Ausführung die Anwendung erneut sehen wir 10 Abendessen zu einem Zeitpunkt unsere Browser:

![](implement-efficient-data-paging/_static/image5.png)

Wir haben auch &lt; &lt; &lt; und &gt; &gt; &gt; Navigationsbenutzeroberfläche am unteren Rand der Seite, die uns ermöglicht, leitet überspringen, und suchen Sie Abwärtskompatibilität über unsere Daten mit Datenbankmodul zugänglich URLs:

![](implement-efficient-data-paging/_static/image6.png)

| **Seite Thema: Verstehen der Auswirkungen der IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; ist eine sehr leistungsstarke Funktion, die eine Vielzahl von Szenarien für interessante verzögerte Ausführung ermöglicht (z. B. Paginierung und dem Kompositionsthread basierend auf Abfragen). Als mit allen zusätzliche leistungsstarke Features, Sie möchten, achten Sie darauf, dass Sie mit einsetzen und stellen Sie sicher, dass es nicht missbraucht wird. Es ist wichtig zu wissen, eine IQueryable zurückgeben&lt;T&gt; Ergebnis aus dem Repository ermöglicht Aufrufcode auf verketteten Standardabfrageoperator-Methoden, Anfügen und daher in der endgültigen abfrageausführung teilnehmen. Wenn Sie nicht aufrufenden Code diese Möglichkeit bieten möchten, sollten Sie zurückgeben sichern IList&lt;T&gt; oder IEnumerable&lt;T&gt; Ergebnissatz – die Ergebnisse einer Abfrage enthalten, die bereits ausgeführt wurde. Für paginierungsszenarien würde dies müssen Sie die tatsächlichen Daten Paginierung Logik aufgerufenen Repository-Methode abzulegen. In diesem Szenario kann wir aktualisieren unsere FindUpcomingDinners() Finder-Methode, um eine Signatur verfügen, dass entweder ein PaginatedList zurückgegeben: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (PageIndex Int, Int PageSize) {} oder wieder zurück in den IList &lt;Dinner&gt;, und verwenden Sie "TotalCount" out-Parameter, um die Gesamtzahl der Abendessen zurückzugeben: IList&lt;Dinner&gt; FindUpcomingDinners (PageIndex Int, Int, Int TotalCount PageSize) {} |

### <a name="next-step"></a>Nächster Schritt

Jetzt betrachten wie wir hinzufügen können, dass die Authentifizierung und Autorisierung für unsere Anwendung unterstützen.

> [!div class="step-by-step"]
> [Zurück](re-use-ui-using-master-pages-and-partials.md)
> [Weiter](secure-applications-using-authentication-and-authorization.md)
