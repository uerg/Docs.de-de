---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortieren, Filtern und Paging mit Entitätsframework in einer ASP.NET MVC-Anwendung | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 3cd7d5e7ea97dd4defa5e609de70beda7dfccf77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375411"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Sortieren, Filtern und Paging mit Entitätsframework in einer ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Tutorial, die Sie implementiert einer Reihe von Webseiten für grundlegende CRUD-Vorgänge für `Student` Entitäten. In diesem Tutorial fügen Sie sortieren, Filtern und Pagingfunktionen für die **Schüler/Studenten** Indexseite. Sie werden auch eine Seite erstellen, auf der einfache Gruppierungsvorgänge ausgeführt werden.

Die folgende Abbildung zeigt, wie die Seite am Ende aussehen wird. Die Spaltenüberschriften sind Links, auf die der Benutzer klicken kann, um die Spalte zu sortieren. Wiederholtes Klicken auf eine Spaltenüberschrift schaltet zwischen aufsteigender und absteigender Sortierreihenfolge um.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Hinzufügen von Spaltensortierungslinks zur Studentenindexseite

Um der Indexseite die Sortierfunktion hinzuzufügen, ändern Sie die `Index` Methode der `Student` Controller und fügen Sie Code die `Student` indizieren Sie die Sicht.

### <a name="add-sorting-functionality-to-the-index-method"></a>Hinzufügen der Sortierfunktion zur Indexmethode

In *Controllers\StudentController.cs*, ersetzen Sie die `Index` Methode durch den folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code empfängt einen `sortOrder`-Parameter aus der Abfragezeichenfolge in der URL. Den Wert der Abfragezeichenfolge wird von ASP.NET MVC als Parameter an die Aktionsmethode bereitgestellt. Der Parameter ist eine Zeichenfolge, entweder „Name“ oder „Date“, optional gefolgt von einem Unterstrich und der Zeichenfolge „desc“, die die absteigende Reihenfolge angibt. Standardmäßig wird eine aufsteigende Sortierreihenfolge verwendet.

Bei der ersten Anforderung der Indexseite gibt es keine Abfragezeichenfolge. Die Studenten werden angezeigt, in aufsteigender Reihenfolge von `LastName`, dies ist die Standardeinstellung, wie das Fall-through-Fall in die `switch` Anweisung. Wenn der Benutzer auf den Link einer Spaltenüberschrift klickt, wird der entsprechende `sortOrder`-Wert in der Abfragezeichenfolge bereitgestellt.

Die beiden `ViewBag` Variablen werden verwendet, sodass die Ansicht die Links der Spaltenüberschriften mit den entsprechenden Abfragezeichenfolgenwerten konfigurieren kann:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Hierbei handelt es sich um ternäre Anweisungen. Die erste gibt an, dass bei der `sortOrder` -Parameter ist null oder leer ist, `ViewBag.NameSortParm` sollte so festgelegt werden "Namen\_Desc" ist, andernfalls sollte Sie auf eine leere Zeichenfolge festgelegt werden. Durch diese beiden Anweisungen können in der Ansicht die Hyperlinks in den Spaltenüberschriften wie folgt festgelegt werden:

| Aktuelle Sortierreihenfolge | Hyperlink „Nachname“ | Hyperlink „Datum“ |
| --- | --- | --- |
| Nachname (aufsteigend) | descending | ascending |
| Nachname (absteigend) | ascending | ascending |
| Datum (aufsteigend) | ascending | descending |
| Datum (absteigend) | ascending | ascending |

Die Methode verwendet [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) an die Spalte für die Sortierung. Der Code erstellt ein ["IQueryable"](https://msdn.microsoft.com/library/bb351562.aspx) Variable vor dem die `switch` -Anweisung ändert in der `switch` -Anweisung und ruft die `ToList` Methode nach der `switch` Anweisung. Es wir keine Abfrage an die Datenbank gesendet, wenn Sie die `IQueryable`-Variablen erstellen und ändern. Die Abfrage wird nicht ausgeführt, bis Sie konvertieren die `IQueryable` Objekt in einer Auflistung durch Aufrufen einer Methode wie z. B. `ToList`. Aus diesem Grund führt dieser Code zu einer einzelnen Abfrage, die nicht, bis ausgeführt wird die `return View` Anweisung.

Als Alternative zum Schreiben von verschiedenen LINQ-Anweisungen für jede Sortierreihenfolge können Sie dynamisch eine LINQ-Anweisung erstellen. Weitere Informationen zu dynamischen LINQ finden Sie unter [dynamische LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Fügen Sie Links zur Indexansicht für Schüler und Studenten für die Spaltenüberschrift hinzu

In *Views\Student\Index.cshtml*, ersetzen Sie die `<tr>` und `<th>` Elemente für die Zeile mit der Überschrift mit den hervorgehobenen Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Dieser Code verwendet die Informationen in den `ViewBag` Eigenschaften zum Einrichten von Hyperlinks mit der entsprechenden Abfrage-Zeichenfolgenwerte.

Führen Sie die Seite, und klicken Sie auf die **Nachname** und **Registrierungsdatum** funktioniert Spaltenüberschriften, um diese Sortierung zu überprüfen.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Nachdem Sie auf die **Nachname** Überschrift werden Schüler/Studenten in absteigender Reihenfolge der letzten Namen angezeigt.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Hinzufügen eines Suchfelds auf der Indexseite "Studenten"

Wenn Sie eine Filterfunktion zur Studentenindexseite hinzufügen möchten, dann fügen Sie ein Textfeld und die Schaltfläche „Senden“ zur Ansicht hinzu, und führen Sie die entsprechenden Änderungen in der `Index`-Methode aus. Sie können eine Zeichenfolge in das Textfeld für Vor- und Nachnamen eingeben, um eine Suche zu starten.

### <a name="add-filtering-functionality-to-the-index-method"></a>Hinzufügen der Filterfunktion zur Indexmethode

In *Controllers\StudentController.cs*, ersetzen Sie die `Index` Methode durch den folgenden Code (die Änderungen sind hervorgehoben):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Sie haben einen `searchString`-Parameter zur `Index`-Methode hinzugefügt. Der Zeichenfolgenwert für die Suche wird aus einem Textfeld empfangen, das Sie zur Indexansicht hinzufügen. Sie haben auch die LINQ-Anweisung hinzugefügt eine `where` -Klausel, die nur Studenten auswählt, deren vor- oder Nachname die Suchzeichenfolge enthält. Die Anweisung, die fügt die [, in denen](https://msdn.microsoft.com/library/bb535040.aspx) -Klausel ausgeführt wird, nur bei ein zu suchenden Wert an.

> [!NOTE]
> In vielen Fällen können Sie die gleiche Methode aufrufen, an einer Entity Framework-Entitätenmenge oder als eine Erweiterungsmethode in einer in-Memory-Sammlung. Die Ergebnisse sind normalerweise identisch, aber in einigen Fällen können abweichen.
> 
> Z. B. die Implementierung von .NET Framework die `Contains` Methode gibt alle Zeilen zurück, wenn Sie eine leere Zeichenfolge an sie übergeben, aber die Entity Framework-Anbieter für SQL Server Compact 4.0 gibt 0 (null) Zeilen auf leere Zeichenfolgen zurück. Aus diesem Grund wird der Code im Beispiel (Einfügen der `Where` -Anweisung innerhalb einer `if` Anweisung) stellt sicher, dass Sie die gleichen Ergebnisse für alle Versionen von SQL Server erhalten. Darüber hinaus die Implementierung von .NET Framework die `Contains` -Methode führt einen Vergleich Groß-/Kleinschreibung standardmäßig, aber die Entity Framework SQL Server-Ressourcenanbieter führen die Groß-/Kleinschreibung Vergleiche standardmäßig. Aus diesem Grund Aufrufen der `ToUpper` Methode, um den Test explizit Groß-/Kleinschreibung, wird sichergestellt, dass die Ergebnisse nicht ändern, wenn Sie ändern den Code später ein Repository verwenden, die zurückgibt, wird ein `IEnumerable` -Sammlung anstelle einer `IQueryable` Objekt. (Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.)
> 
> NULL-Behandlung für unterschiedliche Datenbankanbieter oder wenn Sie möglicherweise auch eine `IQueryable` Objekt im Vergleich zu, bei der Verwendung einer `IEnumerable` Auflistung. Beispielsweise in einigen Szenarien eine `Where` wie z. B. für die erste Bedingung `table.Column != 0` Spalten, die möglicherweise nicht zurück `null` als Wert. Weitere Informationen finden Sie unter [fehlerhafte Behandlung von null-Variablen in der where-Klausel](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Hinzufügen eines Suchfelds zur Studentenindexansicht

In *Views\Student\Index.cshtml*, fügen Sie den hervorgehobenen Code unmittelbar vor dem Öffnen `table` Tag, um eine Beschriftung, ein Textfeld, erstellen und eine **Suche** Schaltfläche.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Führen Sie die Seite, geben Sie eine Suchzeichenfolge ein, und klicken Sie auf **Suche** um sicherzustellen, dass die Funktionsweise des Filters.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Beachten Sie, dass die URL nicht die Suchzeichenfolge "an" enthält, was bedeutet, dass wenn Sie diese Seite kennzeichnen, die gefilterte Liste erhalten Sie wird nicht bei der Verwendung von Lesezeichen. Dies gilt auch für die spaltensortierungslinks, wie sie die gesamte Liste sortiert werden sollen. Ändern Sie die **Suche** Schaltfläche, um später in diesem Tutorial verwenden Sie Abfragezeichenfolgen für Filterkriterien.

## <a name="add-paging-to-the-students-index-page"></a>Hinzufügen von Paging zu der Indexseite "Studenten"

Beginnen Sie zur Indexseite "Studenten" Paging hinzugefügt haben, durch die Installation von der **PagedList.Mvc** NuGet-Paket. Sie zusätzliche Änderungen vornehmen, werden die `Index` Methode und Hinzufügen von paginglinks in der `Index` anzeigen. **PagedList.Mvc** ist einer der vielen guten Paginierung und Sortierung von Paketen für ASP.NET MVC und dessen Verwendung hier dient nur als Beispiel, nicht als eine Empfehlung für die sie über weitere Optionen. Die folgende Abbildung zeigt den Paging-Links.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installieren Sie das PagedList.MVC NuGet-Paket

NuGet **PagedList.Mvc** Paketinstallationen automatisch die **PagedList** Paket als Abhängigkeit. Die **PagedList** Paket installiert einen `PagedList` Sammlungsmethoden Typ und die Erweiterung für `IQueryable` und `IEnumerable` Sammlungen. Die Erweiterungsmethoden erstellen Sie eine einzelne Seite mit Daten in eine `PagedList` Auflistung von Ihrer `IQueryable` oder `IEnumerable`, und die `PagedList` enthält verschiedene Eigenschaften und Methoden, die das Paging zu erleichtern. Die **PagedList.Mvc** Paketinstallationen eine paging-Hilfsprogramm, das die Pagingschaltflächen anzeigt.

Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager** und dann **-Paket-Manager-Konsole**.

In der **-Paket-Manager-Konsole** Fenster, stellen Sie sicher, dass die **Paketquelle** ist **nuget.org** und die **Standardprojekt** ist**ContosoUniversity**, und geben Sie dann den folgenden Befehl aus:

`Install-Package PagedList.Mvc`

![Installieren Sie PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Erstellen Sie das Projekt. 

### <a name="add-paging-functionality-to-the-index-method"></a>Hinzufügen von Pagingfunktionen zur Indexmethode

In *Controllers\StudentController.cs*, Hinzufügen einer `using` -Anweisung für die `PagedList` Namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Ersetzen Sie die `Index`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Dieser Code Fügt eine `page` -Parameter, einen aktuellen Sortierparameter Reihenfolge und einen aktuellen Filterparameter zur Methodensignatur:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging- oder Sortierlink geklickt hat, werden alle Parameter gleich 0 (null) sein. Wenn auf ein paginglink geklickt wird, die `page` Variable enthält die anzuzeigende Seitenzahl.

Ein `ViewBag` Eigenschaft stellt die Ansicht mit der aktuellen Sortierreihenfolge bereit, da dies in den paginglinks enthalten sein muss, um der Sortierreihenfolge der während des Pagings beibehalten:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Eine andere Eigenschaft, `ViewBag.CurrentFilter`, bietet eine Ansicht mit der aktuellen Filterzeichenfolge. Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird. Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann. Die Suchzeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben, und die Schaltfläche "Senden" geklickt wird. In diesem Fall die `searchString` -Parameter ist nicht null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Am Ende der Methode die `ToPagedList` Erweiterungsmethode für die Schüler/Studenten `IQueryable` Objekt konvertiert die studentenabfrage in eine einzelne Seite in einem Sammlungstyp, der Pagingvorgänge unterstützt. Diese einzelnen Seite mit Studentendaten wird dann an die Ansicht übergeben:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `ToPagedList`-Methode nimmt eine Seitenanzahl. Die zwei Fragezeichen darstellen der [Null-Sammeloperator](https://msdn.microsoft.com/library/ms173224.aspx). Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.

### <a name="add-paging-links-to-the-student-index-view"></a>Hinzufügen eines Paginglinks zur Indexansicht für Schüler und Studenten

In *Views\Student\Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PagedList`-Objekt anstelle eines `List`-Objekts aufruft.

Die `using` -Anweisung für `PagedList.Mvc` bietet Zugriff auf das MVC-Hilfsprogramm für die Pagingschaltflächen.

Der Code verwendet eine Überladung der [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , ermöglicht es an [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Der Standardwert [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) sendet Formulardaten mit einem POST, was bedeutet, dass Parameter als Abfragezeichenfolgen im Hauptteil HTTP-Nachricht und nicht in der URL übergeben werden. Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen. Die [W3C-Richtlinien für die Verwendung von HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) wird empfohlen, dass Sie GET verwenden sollten, wenn die Aktion nicht in einem Update führt.

Das Textfeld wird mit die aktuelle Suchzeichenfolge initialisiert, wenn Sie eine neue Seite klicken Sie auf die aktuelle Suchzeichenfolge zu sehen.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Die Spaltenüberschriftenlinks verwenden die Abfragezeichenfolge, um die aktuelle Suchzeichenfolge an den Controller zu übergeben, damit Benutzer Filterergebnisse sortieren können:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Die aktuelle Seite und die Summe der Anzahl der Seiten werden angezeigt.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Wenn keine Seiten zum Anzeigen vorhanden sind, wird "Seite 0 0" angezeigt. (In diesem Fall ist die Nummer der Seite größer als die Anzahl der Seiten da `Model.PageNumber` 1 ist, und `Model.PageCount` ist 0.)

Die Pagingschaltflächen werden angezeigt, durch die `PagedListPager` Hilfsprogramm:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Die `PagedListPager` Hilfsprogramm stellt eine Reihe von Optionen, die Sie anpassen können, einschließlich URLs und formatieren,. Weitere Informationen finden Sie unter [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) auf der GitHub-Website.

Führen Sie die Seite.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert. Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Erstellen Sie eine Infoseite, die Statistiken der Studentendaten anzeigt

Zeigen Sie für der Contoso University-Website die Infoseite wie viele Studenten jedes Anmeldedatum angemeldet haben. Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen. Um dies zu erreichen, ist Folgendes erforderlich:

- Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.
- Ändern der `About` -Methode in der die `Home` Controller.
- Ändern der `About` anzeigen.

### <a name="create-the-view-model"></a>Erstellen des Ansichtsmodells

Erstellen Sie eine *ViewModels* Ordner im Projektordner. Fügen Sie in diesem Ordner die Klassendatei *EnrollmentDateGroup.cs* , und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Ändern des Home-Controllers

In *"HomeController.cs"*, fügen Sie die folgenden `using` Anweisungen am Anfang der Datei:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Fügen Sie unmittelbar nach der öffnenden geschweiften Klammer für die Klasse eine Klassenvariable für den Datenbankkontext hinzu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Ersetzen Sie die `About`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Die LINQ-Anweisung gruppiert die Studentenentitäten nach Anmeldedatum, berechnet die Anzahl der Entitäten in jeder Gruppe und speichert die Ergebnisse in einer Sammlung von `EnrollmentDateGroup`-Ansichtsmodellobjekten.

Hinzufügen einer `Dispose` Methode:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Ändern der Infoansicht

Ersetzen Sie den Code in die *Views\Home\About.cshtml* -Datei mit den folgenden Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Führen Sie die app, und klicken Sie auf die **zu** Link. Die Anzahl der Studenten für die jeweiligen Anmeldedatumswerte wird in einer Tabelle angezeigt.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gesehen, wie ein Datenmodell erstellen und grundlegende CRUD, sortieren, filtern, paging und Gruppieren von Funktionen zu implementieren. Im nächsten Tutorial beginnen Sie Themen für fortgeschrittenere Benutzer ansehen, indem Sie das Datenmodell erweitern.

Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können. Sie können auch neue Themen anfordern [anzeigen Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Ressourcen des Entity Framework finden Sie im [ASP.NET-Datenzugriff – empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Weiter](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
