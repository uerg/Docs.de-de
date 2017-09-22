---
title: ASP.NET Core MVC mit EF-Kern - Sortierung, Filter, Paging - 3 von 10
author: tdykstra
description: "In diesem Lernprogramm fügen Sie sortieren, Filtern und paging Funktionen zur Seite mit ASP.NET Core und Entity Framework Core."
keywords: ASP.NET Core Entity Framework Core "," Sortieren "," Filter "," Paging, gruppieren
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 59fff4dbf4736f0776aac4072f3f4e2d40119842
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>Sortieren, filtern, paging und Gruppierung - EF-Core mit ASP.NET Core MVC-Lernprogramm (3 von 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

Im vorherigen Lernprogramm implementiert Sie eine Reihe von Webseiten für grundlegende CRUD-Vorgänge für Student-Entitäten. In diesem Lernprogramm fügen Sie sortieren, Filtern und Pagingfunktionalität zur Seite Studenten Index. Erstellen Sie auch eine Seite, die einfache Gruppierung ausführt.

Die folgende Abbildung zeigt, wie die Seite aussehen wird, wenn Sie fertig sind. Die Spaltenüberschriften sind Links, die der Benutzer klicken kann, um nach dieser Spalte zu sortieren. Auf eine Spaltenüberschrift wiederholt Schaltet zwischen aufsteigender und absteigender Sortierreihenfolge.

![Indexseite für Studenten](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Den Studenten Indexseite Spalte sortieren Links hinzufügen

Um die Sortierung für die Student Indexseite hinzufügen, ändern Sie die `Index` -Methode des Controllers Studenten und fügen Sie Code hinzu, um die Student Indexansicht.

### <a name="add-sorting-functionality-to-the-index-method"></a>Fügen Sie die Sortierfunktionen für die Index-Methode

In *StudentsController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Dieser Code empfängt eine `sortOrder` Parameter aus der Abfragezeichenfolge in der URL. Der Wert der Abfragezeichenfolge wird als Parameter an die Aktionsmethode durch ASP.NET Core MVC bereitgestellt. Der Parameter wird eine Zeichenfolge sein, der entweder "Name" oder "Date", optional gefolgt von einem Unterstrich und die Zeichenfolge "Desc" in absteigender Reihenfolge anzugeben. Standardmäßig wird eine aufsteigende Sortierreihenfolge verwendet.

Die Indexseite angefordert wird, das zum ersten Mal ist keine Abfragezeichenfolge vorhanden. Den Studenten werden in aufsteigender Reihenfolge angezeigt, nach dem Nachnamen, die standardmäßig aktiviert ist, wie in der Fall fallen über die `switch` Anweisung. Wenn der Benutzer einen Spaltenüberschrift Hyperlink der entsprechenden klickt `sortOrder` Wert in der Abfragezeichenfolge angegeben ist.

Die beiden `ViewData` Elemente (NameSortParm und DateSortParm) werden von der Sicht verwendet, so konfigurieren Sie die Spaltenüberschrift Hyperlinks mit den entsprechenden Abfragezeichenfolgen-Werte.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Hierbei handelt es sich um ternäre-Anweisungen. Das erste Schema gibt an, dass bei der `sortOrder` -Parameter ist null oder leer ist, NameSortParm sollte auf "Name_desc" festgelegt werden; andernfalls eine leere Zeichenfolge festgelegt werden sollte. Diese beiden Anweisungen aktivieren die Sicht die Spalte Überschrift Hyperlinks wie folgt festgelegt:

|  Aktuelle Sortierreihenfolge  | Letzte Name Hyperlink | Datum-Link |
|:--------------------:|:-------------------:|:--------------:|
| Letzte aufsteigend  | descending          | ascending      |
| Letzte Name absteigend | ascending           | ascending      |
| Datum aufsteigend       | ascending           | descending     |
| Absteigend nach Datum      | ascending           | ascending      |

Die Methode verwendet LINQ to Entities an die Spalte zu sortieren. Der Code erstellt ein `IQueryable` Variable vor der Switch-Anweisung ändert sie in der Switch-Anweisung und die Aufrufe der `ToListAsync` Methode nach der `switch` Anweisung. Beim Erstellen und ändern Sie `IQueryable` Variablen, wird keine Abfrage an die Datenbank gesendet. Die Abfrage wird nicht ausgeführt, bis Sie konvertieren die `IQueryable` Objekt in eine Auflistung durch Aufrufen einer Methode wie z. B. `ToListAsync`. Aus diesem Grund dieser Code führt zu einer einzelnen Abfrage, die nicht, bis ausgeführt wird die `return View` Anweisung.

Dieser Code konnte ausführliche mit einer großen Anzahl von Spalten abgerufen werden. [Im letzten Lernprogramm dieser Reihe](advanced.md#dynamic-linq) wird gezeigt, wie Sie Code schreiben, mit dem Sie den Namen eines übergeben der `OrderBy` Spalte in einer Zeichenfolgenvariablen.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Die Indexansicht Student Spaltenüberschrift Links hinzufügen

Ersetzen Sie den Code in *Views/Students/Index.cshtml*, durch den folgenden Code Spaltenüberschrift Hyperlinks hinzufügen. Die geänderten Zeilen werden hervorgehoben.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Dieser Code verwendet die Informationen in `ViewData` Eigenschaften zum Einrichten von Hyperlinks mit der entsprechenden Abfrage Zeichenfolgenwerte.

Die app auszuführen, wählen Sie die **Studenten** Registerkarte, und klicken Sie auf die **Nachname** und **Registrierungsdatum** funktioniert Spaltenüberschriften, um diese Sortierung zu überprüfen.

![Studenten Indexseite im Namensreihenfolge](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Den Studenten Indexseite ein Suchfeld hinzufügen

Zum Hinzufügen der Studenten Indexseite filtern, Sie fügen einem Textfeld und einer Schaltfläche "Absenden" zur Ansicht und entsprechende Änderungen an der `Index` Methode. Das Textfeld können Sie geben eine Zeichenfolge, die in den Vornamen und den letzten Namensfelder gesucht werden soll.

### <a name="add-filtering-functionality-to-the-index-method"></a>Hinzufügen von Filterfunktionen zur Verfügung, die Index-Methode

In *StudentsController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code (die Änderungen werden hervorgehoben).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Sie hinzugefügt haben eine `searchString` Parameter an die `Index` Methode. Der Zeichenfolgenwert für die Suche wird aus einem Textfeld empfangen, die Sie die Indexansicht hinzufügen. Sie haben ebenfalls hinzugefügt, die LINQ-Anweisung eine Where-Klausel, die nur Studenten auswählt, deren vor- oder Nachnamen die zu suchende Zeichenfolge enthält. Die Anweisung, die fügt die Where-Klausel ist nur dann, wenn es ein Wert für die Suche wird ausgeführt.

> [!NOTE]
> Sie werden hier Aufrufen der `Where` Methode auf eine `IQueryable` Objekt und der Filter wird auf dem Server verarbeitet werden. In einigen Szenarien Sie aufgerufen werden möglicherweise die `Where` -Methode eine Erweiterungsmethode für eine in-Memory-Auflistung. (Z. B. Angenommen, Sie ändern, dass den Verweis auf `_context.Students` also, sondern von einer EF `DbSet` er verweist auf eine Repository-Methode, die zurückgibt eine `IEnumerable` Auflistung.) Das Ergebnis wäre normalerweise identisch, aber in einigen Fällen unterscheiden.
>
>Angenommen, die .NET Framework-Implementierung von der `Contains` -Methode führt einen Vergleich Groß-/Kleinschreibung standardmäßig, jedoch in SQL Server wird dies durch die sortierungseinstellung der SQL Server-Instanz bestimmt. Diese Einstellung wird standardmäßig auf Groß-/Kleinschreibung. Rufen Sie die `ToUpper` Methode zum Erstellen von Tests explizit Groß-/Kleinschreibung: *, in denen (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Würde sicherzustellen, dass Ergebnisse gleich bleiben, wenn Sie ändern den Code weiter unten, um ein Repository verwenden die zurückgibt, eine `IEnumerable` Auflistung anstelle von einer `IQueryable` Objekt. (Beim Aufrufen der `Contains` Methode auf eine `IEnumerable` -Auflistung, erhalten Sie die .NET Framework-Implementierung; Wenn Sie es auf Aufrufen einer `IQueryable` -Objekt erhalten Sie die Implementierung des Anbieters.) Es ist jedoch eine Leistungseinbuße für diese Lösung. Die `ToUpper` Code eine Funktion in der WHERE-Klausel der t-SQL SELECT-Anweisung setzen würden. Die würde verhindern, dass den Optimierer einen Index verwenden. Davon ausgehend, dass SQL hauptsächlich als Groß-/Kleinschreibung installiert ist, es wird empfohlen, vermeiden Sie die `ToUpper` code, bis die Migration zu einem Datenspeicher für die Groß-/Kleinschreibung beachtet.

### <a name="add-a-search-box-to-the-student-index-view"></a>Fügen Sie ein Suchfeld auf die Indexansicht Student

In *Views/Student/Index.cshtml*, fügen Sie der hervorgehobene Code hinzu, unmittelbar bevor das öffnende Tag um erstellen eine Beschriftung, ein Textfeld, Tabelle und eine **Suche** Schaltfläche.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Dieser Code verwendet die `<form>` [tag Helper](xref:mvc/views/tag-helpers/intro) Suchtextfeld und Schaltfläche hinzufügen. Wird standardmäßig der `<form>` Tag Hilfsprogramm sendet Formulardaten mit einer POST, was bedeutet, dass der Parameter als Abfragezeichenfolgen in den Hauptteil der HTTP-Nachricht und nicht in der URL übergeben werden. Bei der Angabe von HTTP GET die Formulardaten übergeben die URL als Abfragezeichenfolgen, dadurch können sich Benutzer auf die URL von Lesezeichen. Das W3C Richtlinien wird empfohlen, das zu verwendende erhalten, wenn die Aktion nicht in einem Update führt.

Die app auszuführen, wählen Sie die **Studenten** Registerkarte, geben Sie eine Suchzeichenfolge ein, und klicken Sie auf Suchen, um sicherzustellen, dass die Filterung arbeitet.

![Studenten Indexseite mit Filtern](sort-filter-page/_static/filtering.png)

Beachten Sie, dass die URL der zu suchende Zeichenfolge enthält.

```html
http://localhost:5813/Students?SearchString=an
```

Wenn Sie diese Seite Lesezeichen, erhalten Sie die gefilterte Liste, bei der Verwendung von Lesezeichen. Hinzufügen von `method="get"` auf die `form` Tag ist die Ursache der Abfragezeichenfolge an, die generiert werden.

In dieser Phase, wenn Sie einen Spaltenüberschrift sortieren Link klicken Sie verlieren die Filterwert, der im eingegebenen der **Suche** Feld. Sie beheben, die im nächsten Abschnitt.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Den Studenten Indexseite Pagingfunktionen hinzufügen

Um die Studenten Indexseite Paging hinzugefügt haben, erstellen Sie eine `PaginatedList` -Klasse, verwendet `Skip` und `Take` Anweisungen zum Filtern von Daten auf dem Server statt immer alle Zeilen der Tabelle abzurufen. Dann Sie zusätzliche Änderungen in machen der `Index` Methode und Pagingschaltflächen zum Hinzufügen der `Index` anzeigen. Die folgende Abbildung zeigt die Pagingschaltflächen.

![Studenten Indexseite mit Paginierungslinks](sort-filter-page/_static/paging.png)

Erstellen Sie in den Projektordner `PaginatedList.cs`, und Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

Die `CreateAsync` Methode in diesem Code akzeptiert Seitengröße und die Seitenzahl und wendet die entsprechenden `Skip` und `Take` Anweisungen, die die `IQueryable`. Wenn `ToListAsync` aufgerufen wird, auf die `IQueryable`, wird eine Liste mit nur die angeforderte Seite zurückgegeben. Die Eigenschaften `HasPreviousPage` und `HasNextPage` dienen zum Aktivieren oder deaktivieren Sie **vorherige** und **Weiter** paging Schaltflächen.

Ein `CreateAsync` Methode wird verwendet, statt einen Konstruktor zum Erstellen der `PaginatedList<T>` Objekt, da der Konstruktoren asynchronen Code ausgeführt werden können.

## <a name="add-paging-functionality-to-the-index-method"></a>Fügen Sie Pagingfunktionen zur Index-Methode

In *StudentsController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Dieser Code Fügt eine Seite-Number-Parameter, eine aktuelle sortieren Reihenfolge Parameter- und eine aktuelle Filter-Parameter auf die Methodensignatur an.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

Ersten Mal die Seite wird angezeigt, oder wenn der Benutzer eine Auslagerung oder Sortierung von Link geklickt hat noch nicht, werden alle Parameter null sein.  Wenn ein Auslagerung Link geklickt wird, enthält die Seitenvariable die Seitenzahl angezeigt.

Die `ViewData` Element mit dem Namen CurrentSort erhält die Ansicht mit der aktuellen Sortierung aus, da dies in die Auslagerungsdatei Links enthalten sein muss, um der Sortierreihenfolge beim Paging identisch zu halten.

Die `ViewData` Element mit dem Namen "aktuelle Filter" enthält die Ansicht mit der aktuellen Filterzeichenfolge. Dieser Wert muss in die Auslagerungsdatei Links enthalten sein, um die filtereinstellungen während der Auslagerung zu gewährleisten, und es muss in das Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird.

Wenn die Suchzeichenfolge während Paging geändert wird, verfügt über die Seite auf 1 zurückgesetzt werden sollen, der neue Filter kann in verschiedenen anzuzeigenden ergeben. Die Suchzeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben wird und die Schaltfläche "Absenden" gedrückt wird. In diesem Fall die `searchString` Parameter nicht null ist.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

Am Ende der `Index` -Methode, die `PaginatedList.CreateAsync` Methode konvertiert die Student-Abfrage in einer einzelnen Seite der Schüler in einen Auflistungstyp, die Paging unterstützt. Diese einzelnen Studenten-Seite wird dann an die Ansicht übergeben.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

Die `PaginatedList.CreateAsync` Methode nimmt eine Seitenzahl an. Die zwei Fragezeichen darstellen, die Null-Sammeloperator. Der Null-Sammeloperator definiert einen Standardwert für einen NULL-Werte zulässt. der Ausdruck `(page ?? 1)` bedeutet, dass der Rückgabewert von `page` , wenn es einen Wert, oder 1 zurück, wenn `page` ist null.

## <a name="add-paging-links-to-the-student-index-view"></a>Können die Indexansicht Student Paginierungslinks hinzufügen

In *Views/Students/Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

Die `@model` Anweisung am oberen Rand der Seite "gibt an, dass die Sicht nun Ruft eine `PaginatedList<T>` -Objekt anstelle einer `List<T>` Objekt.

Die Spalte Header Links verwenden die Abfragezeichenfolge an die aktuelle Suchzeichenfolge an den Controller übergeben, damit der Benutzer in Filterergebnisse sortieren kann:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Tag-Hilfsprogramme sind die Auslagerung Schaltflächen angezeigt:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Führen Sie die app, und wechseln Sie zu der Seite "Students".

![Studenten Indexseite mit Paginierungslinks](sort-filter-page/_static/paging.png)

Klicken Sie auf die Auslagerung Links in verschiedenen Sortierreihenfolgen, stellen Sie sicher, dass Paging funktioniert. Klicken Sie dann geben Sie eine Suchzeichenfolge ein, und versuchen Sie es erneut aus, um sicherzustellen, dass Paging auch ordnungsgemäß mit Sortier- und Filtervorgänge Paging.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Erstellen Sie eine Informationsseite, die Student Statistiken

Für der Contoso-University Website **zu** Seite müssen Sie anzeigen, wie viele Studenten für jedes Registrierungsdatum registriert haben. Dazu gruppieren und einfache Berechnungen für Gruppen. Um dies zu erreichen, müssen Sie Folgendes ausführen:

* Erstellen Sie eine Modellklasse Ansicht für die Daten, die an die Ansicht ьbergeben werden sollen.

* Ändern Sie die Info-Methode im Home-Controller.

* Ändern Sie die Info-Sicht.

### <a name="create-the-view-model"></a>Erstellen des Modells anzeigen

Erstellen einer *SchoolViewModels* Ordner in der *Modelle* Ordner.

Fügen Sie eine Klassendatei in den neuen Ordner *EnrollmentDateGroup.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Ändern Sie den Home-Controller

In *HomeController.cs*, fügen Sie die folgenden using-Anweisungen am Anfang der Datei:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Fügen Sie eine Klassenvariable für den Datenbankkontext unmittelbar nach der öffnenden geschweiften Klammer für die Klasse, und rufen Sie eine Instanz des Kontexts von ASP.NET Core DI:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Ersetzen Sie die `About`-Methode durch folgenden Code:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

Registrierungsdatum Student-Entität gruppiert, berechnet die Anzahl der Entitäten in jeder Gruppe und speichert die Ergebnisse in einer Auflistung von LINQ-Anweisung `EnrollmentDateGroup` Model-Objekte anzeigen.
> [!NOTE] 
> Klicken Sie in der Version 1.0 von Entity Framework Core das gesamte Resultset an den Client zurückgegeben wird, und Gruppierung erfolgt auf dem Client. In einigen Szenarien konnte das Leistungsproblemen erstellt werden. Achten Sie darauf, dass zum Testen der Leistung mit Produktion Datenmengen und bei Bedarf unformatierten SQL verwenden, um die Gruppierung auf dem Server ausführen. Informationen zur Verwendung von unformatierten SQL finden Sie unter [im letzten Lernprogramm dieser Reihe](advanced.md).

### <a name="modify-the-about-view"></a>Ändern Sie die Informationen zur Ansicht

Ersetzen Sie den Code in der *Views/Home/About.cshtml* Datei durch den folgenden Code:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Führen Sie die app, und wechseln Sie zu der Seite "Info". Die Anzahl der Schüler für jedes Registrierungsdatum wird in einer Tabelle angezeigt.

![Zu den Seiten](sort-filter-page/_static/about.png)

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen, wie sortieren, filtern, Paging und gruppieren ausführen. In den nächsten Lernprogrammen erfahren Sie, wie datenmodelländerungen zu behandeln, indem Sie Migrationen.

>[!div class="step-by-step"]
[Zurück](crud.md)
[Weiter](migrations.md)  
