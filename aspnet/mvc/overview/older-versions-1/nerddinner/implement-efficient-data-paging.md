---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementieren von effizienten Datenauslagerung | Microsoft-Dokumentation
author: microsoft
description: Schritt 8 zeigt, wie die URL "/ dinners" pagingunterstützung hinzugefügt werden, sodass anstatt 1000 s von Dinner auf einmal, wir nur 10 bevorstehende Dinner unter wird angezeigt...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: bcd7fdf59fac8328752aa2ebab61c1d50a8b6b0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842142"
---
<a name="implement-efficient-data-paging"></a>Implementieren von effizienten Datenauslagerung
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 8 von einem kostenlosen ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 8 zeigt, wie die URL "/ dinners" pagingunterstützung hinzugefügt werden, sodass anstatt von 1000 s von Dinner gleichzeitig wir nur 10 bevorstehende Dinner zu einem Zeitpunkt - anzeigen und Benutzern die Möglichkeit zum Seite wieder vorwärts und rückwärts durchlaufen die gesamte Liste, in einer geeigneten SEO-Form.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner Schritt 8: Unterstützung der Paginierung

Wenn unsere Website erfolgreich ist, müssen sie Tausende von anstehenden Dinner. Wir müssen sicherstellen, dass unsere-Benutzeroberfläche wird skaliert, um alle diese Dinner behandeln und ermöglicht es Benutzern, die sie durchsuchen. Um dies zu ermöglichen, fügen wir Unterstützung der Paginierung, unsere *"/ dinners"* URL so, dass anstelle der Anzeige von 1000 s von Dinner auf einmal, wir werden nur 10 bevorstehende Dinner zu einem Zeitpunkt - angezeigt und Benutzern die Möglichkeit zum Back Seite vorwärts und rückwärts durchlaufen die gesamte Liste in eine benutzerfreundliche SEO-Methode.

### <a name="index-action-method-recap"></a>Eine Erörterung von Index()-Klausel Aktion-Methode

Die Index() Aktionsmethode in unsere Klasse "dinnerscontroller" wie derzeit sieht die folgende:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Wenn eine Anforderung wird, auf die *"/ dinners"* -URL, er ruft eine Liste mit allen zukünftigen Dinner und rendert dann eine Liste aller Tabellen:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Grundlegendes zu IQuerable&lt;T&gt;

*"IQueryable"&lt;T&gt;*  ist eine Schnittstelle, die mit LINQ als Teil von .NET 3.5 eingeführt wurde. Leistungsstarke "verzögerte Ausführung" Szenarien, denen wir nutzen können, die Unterstützung der Paginierung implementieren können.

In unserem "dinnerrepository" werden wir ein "IQueryable" zurückgeben&lt;Dinner&gt; Folge aus unserer FindUpcomingDinners()-Methode:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Das IQueryable-Objekt&lt;Dinner&gt; von unserem FindUpcomingDinners()-Methode zurückgegebene Objekt kapselt eine Abfrage zum Abrufen von Dinner-Objekten aus unserer Datenbank mit LINQ to SQL. Es wird nicht wichtig ist, dass die Abfrage für die Datenbank ausgeführt, bis wir versuchen, Zugriff/die Daten in der Abfrage durchlaufen oder Aufrufen der Methode ToList() darauf. Der Code die FindUpcomingDinners()-Methode aufrufen kann optional auch die "IQueryable" zusätzliche "verketteten"-Vorgänge/Filter hinzufügen&lt;Dinner&gt; Objekt vor dem Ausführen der Abfrage. LINQ to SQL ist intelligent genug, um die kombinierten Abfrage für die Datenbank ausgeführt, wenn die Daten angefordert werden.

Paginglogik implementieren können wir unsere "dinnerscontroller" Index()-Aktionsmethode, damit sie zusätzliche "Skip" und "Take"-Operatoren für das zurückgegebene IQueryable-Objekt gilt aktualisieren&lt;Dinner&gt; Sequenz vor dem Aufrufen von ToList() darauf:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Der obige Code überspringt die ersten 10 bevorstehende Dinner in der Datenbank, und dann 20 Dinner zurückgibt. LINQ to SQL ist intelligent genug, um eine optimierte SQL-Abfrage zu erstellen, die dies ausführt, wird die Logik in der SQL-Datenbank – und nicht in der Web-Server übersprungen. Dies bedeutet, dass auch wenn wir von Millionen von zukünftigen Dinner in der Datenbank verfügen, wird nur die 10 gewünschten als Teil dieser Anforderung (wodurch sie effizienter und skalierbarer) abgerufen werden.

### <a name="adding-a-page-value-to-the-url"></a>Hinzufügen eines Werts für die "Page" zu der URL

Anstelle eines bestimmten Seitenbereichs, sollten wir unsere URLs "Seite" Parameter enthalten, die die Dinner-Bereichs gibt an, ein Benutzer angefordert wird.

#### <a name="using-a-querystring-value"></a>Verwenden einen Querystring-Wert

Der folgende Code veranschaulicht, wie wir unsere Index() Aktionsmethode zur Unterstützung von Querystring-Parameter, und aktivieren die URLs, wie aktualisieren können *"/ dinners"? Seite = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Die oben genannten Index() Aktionsmethode hat einen Parameter namens "Page". Der Parameter als auf NULL festlegbare Ganzzahl deklariert ist (d.h. welche Int? gibt). Dies bedeutet, dass die *"/ dinners"? Seite = 2* URL bewirkt, dass der Wert "2" als Wert des Parameters übergeben werden. Die *"/ dinners"* URL (ohne ein Querystring-Wert) führt dazu, dass einen null-Wert übergeben werden.

Wir sind der Wert durch die Seitengröße (in diesem Fall 10 Zeilen) multipliziert, um zu bestimmen, wie viele Dinner zu überspringen. Wir verwenden die ["C#-null COALESCE"-Operator (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) was nützlich ist, beim Arbeiten mit nullable-Typen. Der obige Code weist Seite den Wert 0, wenn die Seite "-Parameter null ist.

#### <a name="using-embedded-url-values"></a>Verwenden die eingebettete URL-Werte

Eine Alternative zur Verwendung eines Querystring-Wert wäre den Seitenparameter "in der richtigen URL selbst einbetten. Zum Beispiel: */Dinners/Page/2* oder */Dinners/2*. ASP.NET MVC umfasst eine leistungsstarke URL routing-Engine, die zur Unterstützung von Szenarios wie folgt vereinfacht werden.

Wir können benutzerdefinierte Regeln für die nachrichtenweiterleitung registrieren, die alle eingehenden URL oder die URL-format auf die gewünschte Methode alle Controller-Klasse oder eine Aktionsmethode zugeordnet werden. Müssen wir nur die Aufgabe besteht darin, die Datei "Global.asax" in unserem Projekt zu öffnen:

![](implement-efficient-data-paging/_static/image2.png)

Ein, und klicken Sie dann eine neue Zuordnungsregel, die mit der Hilfsmethode MapRoute(), wie der erste Aufruf von Routen zu registrieren. MapRoute() unten:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Wir werden über eine neue Routingregel, die mit dem Namen "UpcomingDinners" registrieren. Wir sind, der angibt, das URL-Format kann "Dinner/Seite / {Seiten}", in denen {Page} einen Parameterwert in der URL eingebettet ist. Der dritte Parameter der Methode MapRoute() gibt an, dass wir URLs zugeordnet werden sollen, die mit diesem Format an die Aktionsmethode Index(), in der Klasse "dinnerscontroller" übereinstimmen.

Wir können genauen den gleichen Index() Code verwenden, wie mit diesem Querystring-Szenario – vorher mit der Ausnahme nun unsere "Page"-Parameter aus der URL und nicht in der Abfragezeichenfolge stammen:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Und jetzt Wenn wir die Anwendung auszuführen und geben Sie in *"/ dinners"* sehen, dass die ersten 10 bevorstehende Dinner:

![](implement-efficient-data-paging/_static/image3.png)

Und wenn wir geben im */Dinners/Page/1* sehen, dass die nächste Seite der Dinner:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Hinzufügen von UI-Seitennavigation

Zum Implementieren von "Weiter" und "früher" Navigationsbenutzeroberfläche in unsere Vorlage anzeigen, damit Benutzer auf die Dinner-Daten ganz einfach überspringen können, ist der letzte Schritt in diesem Szenario für die Paginierung abgeschlossen werden.

Um dies ordnungsgemäß zu implementieren, müssen wir wissen, dass in der Datenbank, die Gesamtanzahl der Dinner auch wie viele Seiten mit Daten in diese übersetzt. Wir müssen so berechnen, ob die gerade angeforderte "Page"-am Anfang oder Ende der Daten Wert, und blenden Sie oder der Benutzeroberflächenautomatisierungs "früheren" und "Weiter" entsprechend. Wir könnten diese Logik innerhalb der Aktionsmethode unsere Index() implementieren. Alternativ können wir eine Hilfsklasse unser Projekt hinzufügen, die diese Logik in einer Weise mehr wieder verwendbare kapselt.

Im folgenden finden Sie eine einfache "PaginatedList"-Hilfsklasse, die aus der Liste abgeleitet wird&lt;T&gt; Auflistungsklasse integriert in .NET Framework. Sie implementiert eine wieder verwendbare Auflistungsklasse, die für eine beliebige Sequenz von "IQueryable" Daten die Paginierung verwendet werden kann. In unserer NerdDinner-Anwendung haben wir dann über "IQueryable" Funktionsweise&lt;Dinner&gt; Ergebnisse, aber es ebenso einfach missbraucht werden für die "IQueryable"&lt;Produkt&gt; oder IQueryable&lt;Kunden&gt;führt andere Anwendungsszenarien:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Beachten Sie, dass oben wie berechnet, und klicken Sie dann wie macht Eigenschaften verfügbar, "PageIndex", "PageSize", "TotalCount" und "TotalPages". Er macht auch zwei Hilfseigenschaften "HasPreviousPage" und "HasNextPage", die angeben, ob die Seite der Daten in der Auflistung am Anfang oder Ende der ursprünglichen Sequenz ist verfügbar. Der obige Code führt dazu, dass zwei SQL-Abfragen ausgeführt werden – wird die erste zum Abrufen der Gesamtzahl der Dinner-Objekte (dadurch nicht zurückgegeben, die Objekte – stattdessen wird eine "SELECT COUNT"-Anweisung, die eine ganze Zahl zurückgegeben), und das zweite nur die Zeilen abgerufen Daten, die wir aus unserer Datenbank für die aktuelle Seite der Daten benötigen.

Wir können dann aktualisieren, unsere DinnersController.Index()-Hilfsmethode zum Erstellen einer PaginatedList&lt;Dinner&gt; aus unserer DinnerRepository.FindUpcomingDinners() dazu führen, und übergeben Sie es an unsere Vorlage anzeigen:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Wir können dann aktualisieren, die ansichtsvorlage \Views\Dinners\Index.aspx ViewPage erben&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; statt ViewPage&lt;"IEnumerable"&lt;Dinner&gt;&gt;, und klicken Sie dann den folgenden Code am unteren Rand unserer Ansicht-Vorlage anzeigen oder Ausblenden der vorherigen und nächsten Navigationsbenutzeroberfläche hinzufügen:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Beachten Sie, oben, wie wir die Hilfsmethode Html.RouteLink() verwenden werden, um unsere Links zu generieren. Diese Methode ähnelt die Html.ActionLink()-Hilfsmethode, die wir zuvor verwendet haben. Der Unterschied besteht darin, dass wir die URL der routing-Regel, dass wir in unserer Datei "Global.asax" setup "UpcomingDinners" generieren. Dadurch wird sichergestellt, dass wir URLs generiert wird, auf unsere Index()-Aktionsmethode, die das Format auf: *"/ dinners" Seite "/ {Page}* : die {}-Wert einer Variablen ist wir bieten sind oben basierend auf der aktuellen PageIndex.

Und nun, wenn ich die Anwendung ausführe erneut wir sehen 10 Dinner zu einem Zeitpunkt in unserem Browser:

![](implement-efficient-data-paging/_static/image5.png)

Wir haben auch &lt; &lt; &lt; und &gt; &gt; &gt; Navigationsbenutzeroberfläche am unteren Rand der Seite, mit der wir leitet überspringen, und suchen Abwärtskompatibilität über unsere Daten mit Suchmaschinen-URLs-zugegriffen werden kann:

![](implement-efficient-data-paging/_static/image6.png)

| **Seite-Thema: Kenntnis der Auswirkungen von "IQueryable"&lt;T&gt;** |
| --- |
| "IQueryable"&lt;T&gt; ist eine sehr leistungsstarke Funktion, die eine Vielzahl von Szenarien für interessante verzögerte Ausführung ermöglicht (wie Paging und Komposition basierend auf Abfragen). Als mit allen leistungsstarke Features, Sie möchten, achten Sie darauf, dass Sie mit der Verwendung und stellen Sie sicher, dass es nicht missbraucht wird. Es ist wichtig zu erkennen, ein "IQueryable" zurückgeben&lt;T&gt; Ergebnis aus dem Repository kann aufrufende Code auf verketteten Standardabfrageoperator-Methoden, Anfügen und daher in der ultimate abfrageausführung teilnehmen. Wenn Sie keine aufrufenden Code diese Möglichkeit geben möchten, sollten Sie zurückgeben sichern IList&lt;T&gt; oder IEnumerable&lt;T&gt; Ergebnisse – die die Ergebnisse einer Abfrage enthalten, die bereits ausgeführt wurde. Für paginierungsszenarien würde dies müssen Sie die tatsächlichen Daten Paginierung Logik in die repositorymethode aufgerufen wird mithilfe von Push übertragen. In diesem Szenario können wir aktualisieren unsere FindUpcomingDinners() Finder-Methode, um eine Signatur verfügen, dass entweder ein PaginatedList zurückgegeben: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (PageIndex von Int, Int PageSize) {} oder zurückzugeben IList &lt;Dinner&gt;, und verwenden Sie eine "TotalCount" out-Parameter, um die Gesamtzahl der Dinner zurückzugeben: IList&lt;Dinner&gt; FindUpcomingDinners (PageIndex von Int, Int, Int TotalCount PageSize) {} |

### <a name="next-step"></a>Nächster Schritt

Jetzt sehen wir uns wie wir hinzufügen können, dass die Authentifizierung und Autorisierung bei unserer Anwendung unterstützen.

> [!div class="step-by-step"]
> [Zurück](re-use-ui-using-master-pages-and-partials.md)
> [Weiter](secure-applications-using-authentication-and-authorization.md)
