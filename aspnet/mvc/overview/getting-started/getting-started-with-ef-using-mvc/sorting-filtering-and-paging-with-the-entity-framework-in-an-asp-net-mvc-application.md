---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortieren, Filtern und Paging mit Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b7d988202966dc0011eeed32cd632c6e0565b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Sortieren, Filtern und Paging mit Entity Framework in einer ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm, die Sie implementiert einer Reihe von Webseiten für grundlegende CRUD-Vorgänge für `Student` Entitäten. In diesem Lernprogramm fügen Sie sortieren, Filtern und Pagingfunktionen für die **Studenten** Indexseite. Sie werden auch eine Seite erstellen, auf der einfache Gruppierungsvorgänge ausgeführt werden.

Die folgende Abbildung zeigt, wie die Seite am Ende aussehen wird. Die Spaltenüberschriften sind Links, auf die der Benutzer klicken kann, um die Spalte zu sortieren. Wiederholtes Klicken auf eine Spaltenüberschrift schaltet zwischen aufsteigender und absteigender Sortierreihenfolge um.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Hinzufügen von Spaltensortierungslinks zur Studentenindexseite

Um die Sortierung für die Student Indexseite hinzufügen, ändern Sie die `Index` Methode der `Student` Controller und Code Hinzufügen der `Student` indizieren Sie die Sicht.

### <a name="add-sorting-functionality-to-the-index-method"></a>Fügen Sie die Sortierfunktionen für die Index-Methode

In *Controllers\StudentController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code empfängt einen `sortOrder`-Parameter aus der Abfragezeichenfolge in der URL. Der Wert der Abfragezeichenfolge wird als Parameter an die Aktionsmethode durch ASP.NET MVC bereitgestellt. Der Parameter ist eine Zeichenfolge, entweder „Name“ oder „Date“, optional gefolgt von einem Unterstrich und der Zeichenfolge „desc“, die die absteigende Reihenfolge angibt. Standardmäßig wird eine aufsteigende Sortierreihenfolge verwendet.

Bei der ersten Anforderung der Indexseite gibt es keine Abfragezeichenfolge. Den Studenten werden angezeigt, in aufsteigender Reihenfolge von `LastName`, Hierbei handelt es sich um die Standardeinstellung, wie in der Fall fallen durch die `switch` Anweisung. Wenn der Benutzer auf den Link einer Spaltenüberschrift klickt, wird der entsprechende `sortOrder`-Wert in der Abfragezeichenfolge bereitgestellt.

Die beiden `ViewBag` Variablen werden verwendet, sodass die Ansicht Hyperlinks Überschrift der Spalte mit den entsprechenden Abfragezeichenfolgenwerten konfigurieren kann:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Hierbei handelt es sich um ternäre Anweisungen. Das erste Schema gibt an, dass bei der `sortOrder` ist null oder leer ist, `ViewBag.NameSortParm` sollte festgelegt werden, um "Name\_" DESC "" ist, andernfalls sollte auf eine leere Zeichenfolge festgelegt werden. Durch diese beiden Anweisungen können in der Ansicht die Hyperlinks in den Spaltenüberschriften wie folgt festgelegt werden:

| Aktuelle Sortierreihenfolge | Hyperlink „Nachname“ | Hyperlink „Datum“ |
| --- | --- | --- |
| Nachname (aufsteigend) | descending | ascending |
| Nachname (absteigend) | ascending | ascending |
| Datum (aufsteigend) | ascending | descending |
| Datum (absteigend) | ascending | ascending |

Die Methode verwendet [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) an die Spalte zu sortieren. Der Code erstellt ein [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) Variable vor der `switch` -Anweisung ändert sie in der `switch` -Anweisung, und ruft die `ToList` Methode nach der `switch` Anweisung. Es wir keine Abfrage an die Datenbank gesendet, wenn Sie die `IQueryable`-Variablen erstellen und ändern. Die Abfrage wird nicht ausgeführt, bis Sie konvertieren die `IQueryable` Objekt in eine Auflistung durch Aufrufen einer Methode wie z. B. `ToList`. Aus diesem Grund dieser Code führt zu einer einzelnen Abfrage, die nicht, bis ausgeführt wird die `return View` Anweisung.

Als Alternative zum Schreiben von verschiedenen LINQ-Anweisungen für jede Sortierreihenfolge können Sie dynamisch eine LINQ-Anweisung erstellen. Informationen über dynamische LINQ finden Sie unter [dynamische LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Hinzufügen von links, um die Indexansicht Student für den Spaltenüberschrift

In *Views\Student\Index.cshtml*, ersetzen Sie die `<tr>` und `<th>` Elemente für die Zeile mit der Überschrift, mit der hervorgehobene Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Dieser Code verwendet die Informationen in den `ViewBag` Eigenschaften zum Einrichten von Hyperlinks mit der entsprechenden Abfrage Zeichenfolgenwerte.

Führen Sie die Seite, und klicken Sie auf die **Nachname** und **Registrierungsdatum** funktioniert Spaltenüberschriften, um diese Sortierung zu überprüfen.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Nachdem Sie auf die **Nachname** Überschrift Studenten in absteigender Namensreihenfolge der letzte angezeigt.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Die Indexseite Studenten ein Suchfeld hinzufügen

Wenn Sie eine Filterfunktion zur Studentenindexseite hinzufügen möchten, dann fügen Sie ein Textfeld und die Schaltfläche „Senden“ zur Ansicht hinzu, und führen Sie die entsprechenden Änderungen in der `Index`-Methode aus. Sie können eine Zeichenfolge in das Textfeld für Vor- und Nachnamen eingeben, um eine Suche zu starten.

### <a name="add-filtering-functionality-to-the-index-method"></a>Hinzufügen von Filterfunktionen zur Verfügung, die Index-Methode

In *Controllers\StudentController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code (die Änderungen werden hervorgehoben):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Sie haben einen `searchString`-Parameter zur `Index`-Methode hinzugefügt. Der Zeichenfolgenwert für die Suche wird aus einem Textfeld empfangen, das Sie zur Indexansicht hinzufügen. Sie haben auch die LINQ-Anweisung hinzugefügt eine `where` -Klausel, die nur Studenten auswählt, deren vor- oder Nachnamen die zu suchende Zeichenfolge enthält. Die Anweisung, die fügt der [, in dem](https://msdn.microsoft.com/library/bb535040.aspx) -Klausel nur, wenn ein Wert für die Suche ausgeführt wird.

> [!NOTE]
> In vielen Fällen können Sie die gleiche Methode aufrufen, bei einer Entity Framework-Entitätenmenge oder als eine Erweiterungsmethode für eine in-Memory-Auflistung. Die Ergebnisse sind normalerweise identisch, aber in einigen Fällen unterscheiden.
> 
> Angenommen, die .NET Framework-Implementierung von der `Contains` Methode werden alle Zeilen zurückgegeben, wenn Sie eine leere Zeichenfolge an sie übergeben, aber der Entity Framework-Datenanbieter für SQL Server Compact 4.0 0 Zeilen für leere Zeichenfolgen zurück. Daher den Code im Beispiel (Einfügen der `Where` -Anweisung innerhalb einer `if` Anweisung) wird sichergestellt, dass Sie die gleichen Ergebnisse für alle Versionen von SQL Server erhalten. Darüber hinaus die .NET Framework-Implementierung von der `Contains` -Methode führt einen Vergleich Groß-/Kleinschreibung beachtet, standardmäßig aber Entity Framework SQL Server-Anbietern für Vergleiche Groß-/Kleinschreibung standardmäßig. Aus diesem Grund Aufrufen der `ToUpper` Methode, um den Test explizit Groß-/Kleinschreibung, wird sichergestellt, dass die Ergebnisse nicht verändern, wenn Sie ändern den Code später, um ein Repository zu verwenden, die zurückgibt, wird ein `IEnumerable` Auflistung anstelle von einer `IQueryable` Objekt. (Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.)
> 
> NULL-Behandlung möglicherweise auch für andere Datenbank-Anbieter oder bei Verwendung der verschiedenen ein `IQueryable` Objekt im Vergleich zu, bei der Verwendung einer `IEnumerable` Auflistung. Beispielsweise ist in einigen Szenarien eine `Where` Bedingung wie z. B. `table.Column != 0` gelegten keine Spalten mit `null` als Wert. Weitere Informationen finden Sie unter [fehlerhafte Behandlung von null-Variablen in "where"-Klausel](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Hinzufügen eines Suchfelds zur Studentenindexansicht

In *Views\Student\Index.cshtml*, fügen Sie den hervorgehobenen Code unmittelbar vor dem Öffnen `table` Tag, um eine Beschriftung, einem Textfeld zu erstellen und eine **Suche** Schaltfläche.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Führen Sie die Seite, geben Sie eine Suchzeichenfolge ein, und klicken Sie auf **Suche** um sicherzustellen, dass die Filterung arbeitet.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Beachten Sie, dass die URL die Suchzeichenfolge "an" enthält, d. h. Wenn Sie diese Seite Lesezeichen, die gefilterte Liste erhalten Sie wird nicht bei der Verwendung von Lesezeichen. Dies gilt auch für die Spalte sortieren Links, wie sie die gesamte Liste sortiert werden sollen. Ändern Sie die **Suche** Schaltfläche Abfragezeichenfolgen für Filterkriterien angeben, die später in diesem Lernprogramm verwenden.

## <a name="add-paging-to-the-students-index-page"></a>Den Studenten Indexseite Paging hinzufügen

Um die Studenten Indexseite Paging hinzugefügt haben, beginnen Sie durch die Installation von der **PagedList.Mvc** NuGet-Paket. Dann Sie zusätzliche Änderungen in machen der `Index` Methode und Paginierungslinks zum Hinzufügen der `Index` anzeigen. **PagedList.Mvc** ist einer der vielen gute Paging und Sortieren von Paketen für ASP.NET MVC und Verwendungsmöglichkeiten hier dient nur als Beispiel nicht als eine Empfehlung für ihn gegenüber anderen Optionen. Die folgende Abbildung zeigt die Auslagerung Links.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installieren Sie das PagedList.MVC NuGet-Paket

Die NuGet **PagedList.Mvc** Paketinstallationen automatisch die **PagedList** Paket als Abhängigkeit. Die **PagedList** Paket installiert eine `PagedList` Sammlungsmethoden Typ und die Erweiterung für `IQueryable` und `IEnumerable` Sammlungen. Die Erweiterungsmethoden [c#], erstellen eine einzelne Seite der Daten in eine `PagedList` Auflistung von Ihrem `IQueryable` oder `IEnumerable`, und die `PagedList` enthält verschiedene Eigenschaften und Methoden, die Auslagerungsdatei zu erleichtern. Die **PagedList.Mvc** Paketinstallationen eine Auslagerungsdatei-Hilfsprogramm, das die Auslagerung Schaltflächen angezeigt.

Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager** und dann **Package Manager Console**.

In der **Package Manager Console** Fenster, stellen Sie sicher, dass die **Paketquelle** ist **nuget.org** und die **Standardprojekt** ist**ContosoUniversity**, und geben Sie dann den folgenden Befehl aus:

`Install-Package PagedList.Mvc`

![Installieren von PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Erstellen Sie das Projekt. 

### <a name="add-paging-functionality-to-the-index-method"></a>Fügen Sie Pagingfunktionen zur Index-Methode

In *Controllers\StudentController.cs*, Hinzufügen einer `using` -Anweisung für die `PagedList` Namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Ersetzen Sie die `Index`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Dieser Code Fügt eine `page` Parameter, eine aktuelle sortieren Order-Parameter und eine aktuelle Filter-Parameter auf die Methodensignatur:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging- oder Sortierlink geklickt hat, werden alle Parameter gleich 0 (null) sein. Wenn ein Auslagerung Link geklickt wird, die `page` Variable enthält die Seitenzahl angezeigt.

Ein `ViewBag` Eigenschaft ermöglicht die Ansicht mit der aktuellen Sortierung aus, da dies in die Auslagerungsdatei Links enthalten sein muss, um der Sortierreihenfolge beim Paging beibehalten:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Eine andere Eigenschaft `ViewBag.CurrentFilter`, erhält die Ansicht mit der aktuellen Filterzeichenfolge. Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird. Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann. Die Suchzeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben wird und die Schaltfläche "Absenden" gedrückt wird. In diesem Fall die `searchString` Parameter nicht null ist.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Am Ende der Methode die `ToPagedList` Erweiterungsmethode für Studenten `IQueryable` Objekt konvertiert die Student-Abfrage in einer einzelnen Seite der Schüler in einen Auflistungstyp, die Paging unterstützt. Diese einzelnen Studenten-Seite wird dann an die Ansicht übergeben:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `ToPagedList`-Methode nimmt eine Seitenanzahl. Die zwei Fragezeichen darstellen der [Null-Sammeloperator](https://msdn.microsoft.com/library/ms173224.aspx). Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.

### <a name="add-paging-links-to-the-student-index-view"></a>Können die Indexansicht Student Paginierungslinks hinzufügen

In *Views\Student\Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PagedList`-Objekt anstelle eines `List`-Objekts aufruft.

Die `using` -Anweisung für `PagedList.Mvc` bietet Zugriff auf das MVC-Hilfsprogramm für die Auslagerung Schaltflächen.

Der Code verwendet eine Überladung der [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ermöglicht es an [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Die Standardeinstellung [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) übermittelt Formulardaten mit einer POST, was bedeutet, dass der Parameter als Abfragezeichenfolgen in den Hauptteil der HTTP-Nachricht und nicht in der URL übergeben werden. Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen. Die [W3C-Richtlinien für die Verwendung von HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) wird empfohlen, dass Sie GET verwenden soll, wenn die Aktion nicht in einem Update führt.

Im Textfeld wird mit der aktuellen Suchzeichenfolge initialisiert, wenn Sie eine neue Seite klicken Sie auf die aktuelle Suchzeichenfolge zu sehen.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Die Spaltenüberschriftenlinks verwenden die Abfragezeichenfolge, um die aktuelle Suchzeichenfolge an den Controller zu übergeben, damit Benutzer Filterergebnisse sortieren können:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Die aktuelle Seite und die Summe der Anzahl der Seiten werden angezeigt.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Wenn keine Seiten zum Anzeigen vorhanden sind, wird "Seite 0 0" angezeigt. (In diesem Fall wird die Seitenzahl größer als die Anzahl der Seiten da `Model.PageNumber` 1 ist, und `Model.PageCount` ist 0.)

Die Auslagerung Schaltflächen werden angezeigt, indem die `PagedListPager` Hilfsprogramm:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Die `PagedListPager` -Hilfsprogramm stellt eine Reihe von Optionen, die Sie anpassen können, einschließlich URLs und Formate,. Weitere Informationen finden Sie unter [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) auf der GitHub-Website.

Führen Sie die Seite.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert. Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Erstellen Sie eine über eine Berichtsseite mit Student Statistiken

Zeigen Sie für die Contoso-University Website zur Seite wie viele Studenten für jedes Registrierungsdatum registriert haben. Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen. Um dies zu erreichen, ist Folgendes erforderlich:

- Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.
- Ändern der `About` Methode in der `Home` Controller.
- Ändern der `About` anzeigen.

### <a name="create-the-view-model"></a>Erstellen des Modells anzeigen

Erstellen einer *ViewModels* Ordner im Projektordner. Fügen Sie eine Klassendatei in diesem Ordner *EnrollmentDateGroup.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Ändern des Home-Controllers

In *HomeController.cs*, fügen Sie die folgenden `using` Anweisungen am Anfang der Datei:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Fügen Sie eine Klassenvariable für den Datenbankkontext unmittelbar nach der öffnenden geschweiften Klammer für die Klasse hinzu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Ersetzen Sie die `About`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Die LINQ-Anweisung gruppiert die Studentenentitäten nach Anmeldedatum, berechnet die Anzahl der Entitäten in jeder Gruppe und speichert die Ergebnisse in einer Sammlung von `EnrollmentDateGroup`-Ansichtsmodellobjekten.

Hinzufügen einer `Dispose` Methode:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Ändern der Infoansicht

Ersetzen Sie den Code in der *Views\Home\About.cshtml* Datei durch den folgenden Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Die app auszuführen, und klicken Sie auf die **zu** Link. Die Anzahl der Studenten für die jeweiligen Anmeldedatumswerte wird in einer Tabelle angezeigt.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen, wie ein Datenmodell erstellen und implementieren grundlegende CRUD, sortieren, filtern, paging und Funktionalität zur Gruppierung. In den nächsten Lernprogrammen beginnen Sie erweiterte Themen ansehen, erweitern Sie das Datenmodell.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie im [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Weiter](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
