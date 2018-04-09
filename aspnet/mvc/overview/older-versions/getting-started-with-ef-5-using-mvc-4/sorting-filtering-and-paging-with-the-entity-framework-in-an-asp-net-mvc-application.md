---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortieren, Filtern und Paging mit Entity Framework in einer ASP.NET MVC-Anwendung (3 von 10) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sortieren, Filtern und Paging mit Entity Framework in einer ASP.NET MVC-Anwendung (3 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Sie haben einen `searchString`-Parameter zur `Index`-Methode hinzugefügt. Sie haben auch die LINQ-Anweisung hinzugefügt eine `where` Clausethat wählt nur die Studenten, deren vor- oder Nachnamen die zu suchende Zeichenfolge enthält. Der Zeichenfolgenwert für die Suche wird aus einem Textfeld empfangen, die Sie die Indexansicht hinzufügen. Die Anweisung, die fügt der [, in dem](https://msdn.microsoft.com/library/bb535040.aspx) -Klausel nur, wenn ein Wert für die Suche ausgeführt wird.

> [!NOTE]
> In vielen Fällen können Sie die gleiche Methode aufrufen, bei einer Entity Framework-Entitätenmenge oder als eine Erweiterungsmethode für eine in-Memory-Auflistung. Die Ergebnisse sind normalerweise identisch, aber in einigen Fällen unterscheiden. Angenommen, die .NET Framework-Implementierung von der `Contains` Methode werden alle Zeilen zurückgegeben, wenn Sie eine leere Zeichenfolge an sie übergeben, aber der Entity Framework-Datenanbieter für SQL Server Compact 4.0 0 Zeilen für leere Zeichenfolgen zurück. Daher den Code im Beispiel (Einfügen der `Where` -Anweisung innerhalb einer `if` Anweisung) wird sichergestellt, dass Sie die gleichen Ergebnisse für alle Versionen von SQL Server erhalten. Darüber hinaus die .NET Framework-Implementierung von der `Contains` -Methode führt einen Vergleich Groß-/Kleinschreibung beachtet, standardmäßig aber Entity Framework SQL Server-Anbietern für Vergleiche Groß-/Kleinschreibung standardmäßig. Aus diesem Grund Aufrufen der `ToUpper` Methode, um den Test explizit Groß-/Kleinschreibung, wird sichergestellt, dass die Ergebnisse nicht verändern, wenn Sie ändern den Code später, um ein Repository zu verwenden, die zurückgibt, wird ein `IEnumerable` Auflistung anstelle von einer `IQueryable` Objekt. (Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Hinzufügen eines Suchfelds zur Studentenindexansicht

In *Views\Student\Index.cshtml*, fügen Sie den hervorgehobenen Code unmittelbar vor dem Öffnen `table` Tag, um eine Beschriftung, einem Textfeld zu erstellen und eine **Suche** Schaltfläche.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Führen Sie die Seite, geben Sie eine Suchzeichenfolge ein, und klicken Sie auf **Suche** um sicherzustellen, dass die Filterung arbeitet.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Beachten Sie, dass die URL die Suchzeichenfolge "an" enthält, d. h. Wenn Sie diese Seite Lesezeichen, die gefilterte Liste erhalten Sie wird nicht bei der Verwendung von Lesezeichen. Ändern Sie die **Suche** Schaltfläche Abfragezeichenfolgen für Filterkriterien angeben, die später in diesem Lernprogramm verwenden.

## <a name="add-paging-to-the-students-index-page"></a>Den Studenten Indexseite Paging hinzufügen

Um die Studenten Indexseite Paging hinzugefügt haben, beginnen Sie durch die Installation von der **PagedList.Mvc** NuGet-Paket. Dann Sie zusätzliche Änderungen in machen der `Index` Methode und Paginierungslinks zum Hinzufügen der `Index` anzeigen. **PagedList.Mvc** ist einer der vielen gute Paging und Sortieren von Paketen für ASP.NET MVC und Verwendungsmöglichkeiten hier dient nur als Beispiel nicht als eine Empfehlung für ihn gegenüber anderen Optionen. Die folgende Abbildung zeigt die Auslagerung Links.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installieren Sie das PagedList.MVC NuGet-Paket

Die NuGet **PagedList.Mvc** Paketinstallationen automatisch die **PagedList** Paket als Abhängigkeit. Die **PagedList** Paket installiert eine `PagedList` Sammlungsmethoden Typ und die Erweiterung für `IQueryable` und `IEnumerable` Sammlungen. Die Erweiterungsmethoden [c#], erstellen eine einzelne Seite der Daten in eine `PagedList` Auflistung von Ihrem `IQueryable` oder `IEnumerable`, und die `PagedList` enthält verschiedene Eigenschaften und Methoden, die Auslagerungsdatei zu erleichtern. Die **PagedList.Mvc** Paketinstallationen eine Auslagerungsdatei-Hilfsprogramm, das die Auslagerung Schaltflächen angezeigt.

Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten**.

In der **NuGet-Pakete verwalten** (Dialogfeld), klicken Sie auf die **Online** links auf die Registerkarte, und klicken Sie dann in das Suchfeld eingeben "ausgelagerten". Wenn Sie sehen die **PagedList.Mvc** Paket, klicken Sie auf **installieren**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

In der **Projekte auswählen** auf **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Fügen Sie Pagingfunktionen zur Index-Methode

In *Controllers\StudentController.cs*, Hinzufügen einer `using` -Anweisung für die `PagedList` Namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Ersetzen Sie die `Index`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Dieser Code Fügt eine `page` Parameter, eine aktuelle sortieren Order-Parameter und eine aktuelle Filter-Parameter auf die Methodensignatur, wie hier gezeigt:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging- oder Sortierlink geklickt hat, werden alle Parameter gleich 0 (null) sein. Wenn ein Auslagerung Link geklickt wird, die `page` Variable enthält die Seitenzahl angezeigt.

`A ViewBag` Eigenschaft ermöglicht die Ansicht mit der aktuellen Sortierung, da dies in die Auslagerungsdatei Links enthalten sein muss, um der Sortierreihenfolge beim Paging beibehalten:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Eine andere Eigenschaft `ViewBag.CurrentFilter`, erhält die Ansicht mit der aktuellen Filterzeichenfolge. Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird. Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann. Die Suchzeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben wird und die Schaltfläche "Absenden" gedrückt wird. In diesem Fall die `searchString` Parameter nicht null ist.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Am Ende der Methode die `ToPagedList` Erweiterungsmethode für Studenten `IQueryable` Objekt konvertiert die Student-Abfrage in einer einzelnen Seite der Schüler in einen Auflistungstyp, die Paging unterstützt. Diese einzelnen Studenten-Seite wird dann an die Ansicht übergeben:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `ToPagedList`-Methode nimmt eine Seitenanzahl. Die zwei Fragezeichen darstellen der [Null-Sammeloperator](https://msdn.microsoft.com/library/ms173224.aspx). Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.

### <a name="add-paging-links-to-the-student-index-view"></a>Können die Indexansicht Student Paginierungslinks hinzufügen

In *Views\Student\Index.cshtml*, ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PagedList`-Objekt anstelle eines `List`-Objekts aufruft.

Die `using` -Anweisung für `PagedList.Mvc` bietet Zugriff auf das MVC-Hilfsprogramm für die Auslagerung Schaltflächen.

Der Code verwendet eine Überladung der [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ermöglicht es an [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Die Standardeinstellung [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) übermittelt Formulardaten mit einer POST, was bedeutet, dass der Parameter als Abfragezeichenfolgen in den Hauptteil der HTTP-Nachricht und nicht in der URL übergeben werden. Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen. Die [W3C-Richtlinien für die Verwendung von HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) angeben, dass Sie GET verwenden soll, wenn die Aktion nicht in einem Update führt.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert. Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Erstellen Sie eine über eine Berichtsseite mit Student Statistiken

Zeigen Sie für die Contoso-University Website zur Seite wie viele Studenten für jedes Registrierungsdatum registriert haben. Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen. Um dies zu erreichen, ist Folgendes erforderlich:

- Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.
- Ändern der `About` Methode in der `Home` Controller.
- Ändern der `About` anzeigen.

### <a name="create-the-view-model"></a>Erstellen des Modells anzeigen

Erstellen einer *ViewModels* Ordner. Fügen Sie eine Klassendatei in diesem Ordner *EnrollmentDateGroup.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Optional: Bereitstellen der app für Windows Azure

Bisher wurde die Anwendung lokal in IIS Express auf dem Entwicklungscomputer ausgeführt. Um ihn für andere Benutzer über das Internet verfügbar machen, müssen Sie es auf einen Webhostinganbieter bereitstellen. In diesem optionalen Abschnitt des Lernprogramms stellen Sie es auf einer Windows Azure-Website bereit.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Mithilfe von Code First-Migrationen zum Bereitstellen der Datenbank

Um die Datenbank bereitzustellen, verwenden Sie Code First-Migrationen. Wenn Sie das Veröffentlichungsprofil, mit denen Sie Einstellungen erstellen für die Bereitstellung von Visual Studio konfigurieren, wählen Sie ein Kontrollkästchen mit der Bezeichnung **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**. Diese Einstellung bewirkt, dass der Bereitstellungsprozess so konfigurieren Sie die Anwendung automatisch *"Web.config"* Datei auf dem Zielserver, sodass der Code First verwendet die `MigrateDatabaseToLatestVersion` Initialisiererklasse.

Visual Studio ist nicht mit der Datenbank während des Bereitstellungsvorgangs keine Wirkung. Wenn die bereitgestellte Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift, Code First automatisch erstellt die Datenbank oder das Datenbankschema auf die neueste Version aktualisiert. Wenn die Anwendung eine Migrationen implementiert `Seed` -Methode, die Methode ausgeführt wird, nachdem die Datenbank erstellt wird, oder das Schema aktualisiert.

Migrationen `Seed` Methode fügt Testdaten. Wenn Sie in einer produktionsumgebung bereitgestellt wurden, müssten Sie ändern die `Seed` Methode, sodass die It nur Daten einfügt, die in der-Datenbank eingefügt werden sollen. Angenommen, in Ihrem aktuellen Datenmodell empfiehlt real Kurse aber fiktive Studenten in der Entwicklungsdatenbank verfügen. Sie schreiben eine `Seed` Methode zum Laden sowohl in der Entwicklung, und klicken Sie dann die fiktiven Studenten kommentieren Sie vor der Bereitstellung bis hin zur Produktion. Oder Sie können Schreiben einer `Seed` Methode, um nur Kurse zu laden, und geben die fiktiven Studenten in der Testdatenbank manuell mithilfe der Benutzeroberfläche der Anwendung.

### <a name="get-a-windows-azure-account"></a>Richten Sie ein Windows Azure-Konto

Sie benötigen ein Windows Azure-Konto. Wenn Sie einen noch nicht haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Windows Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Erstellen Sie eine Website und eine SQL-Datenbank in Windows Azure

Ihr Windows Azure-Website wird in einer freigegebenen Hostingumgebung ausgeführt, dies bedeutet, dass es auf virtuellen Computern (VMs) ausgeführt wird, die mit anderen Windows Azure-Clients gemeinsam genutzt werden. Eine freigegebene Hostingumgebung ist eine kostengünstige Möglichkeit, in der Cloud zu beginnen. Ihres Webdatenverkehrs zunimmt, kann später die Anwendung skalieren, um die Anforderungen zu erfüllen, indem auf dedizierten virtuellen Computern ausführen. Wenn Sie eine komplexere Architektur benötigen, können Sie zu einem Windows Azure-Cloud-Dienst migrieren. Clouddienste ausgeführt werden auf dedizierten virtuellen Computern, die Sie entsprechend Ihren Anforderungen konfigurieren können.

Windows Azure SQL-Datenbank ist eine cloudbasierte relationale Datenbankdienst, der auf SQL Server-Technologien integriert ist. Tools und Anwendungen, die Arbeit mit SQL Server funktionieren auch mit SQL-Datenbank.

1. In der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/), klicken Sie auf **Websites** in der linken Registerkarte, und klicken Sie dann auf **neu**.

    ![Schaltfläche "Neu" im Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Klicken Sie auf **Benutzerdefiniert erstellen**.

    ![Mit Datenbank-Link im Verwaltungsportal erstellen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Die **neue Website - Benutzerdefiniert erstellen** -Assistent wird geöffnet.
3. In der **neue Website** Schritt des Assistenten geben Sie eine Zeichenfolge in der **URL** Feld eindeutige URL für Ihre Anwendung verwenden. Die vollständige URL besteht aus Ihnen hier eingegebenen plus das Suffix, das neben dem Textfeld angezeigt. Die Abbildung zeigt "ConU", aber diese URL stammt wahrscheinlich, so dass Sie einen anderen Drucker auswählen.

    ![Mit Datenbank-Link im Verwaltungsportal erstellen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. In der **Region** Dropdown-Liste, wählen Sie eine Region nahe Sie. Diese Einstellung gibt an, das Rechenzentrum, in der Website ausgeführt werden.
5. In der **Datenbank** Dropdown- Liste **erstellen Sie eine kostenlose 20 MB-SQL-Datenbank**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. In der **NAME der DATENBANKVERBINDUNGSZEICHENFOLGE**, geben Sie *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Klicken Sie auf den Pfeil nach rechts am unteren Rand der Box. Der Assistent springt zu der **Datenbankeinstellungen** Schritt.
8. In der **Namen** geben *ContosoUniversityDB*.
9. In der **Server** wählen Sie im **neue SQL-Datenbankserver**. Wenn Sie einen Server zuvor erstellt haben, können Sie Alternativ können Sie diesen Server aus der Dropdown-Liste auswählen.
10. Geben Sie einen Administrator **ANMELDENAME** und **Kennwort**. Wenn Sie ausgewählt haben **neue SQL-Datenbankserver** Sie sind nicht Sie einen vorhandenen Namen und Kennwort hier eingeben, die Sie einen neuen Namen und ein Kennwort, die Sie jetzt definieren, zur späteren Verwendung beim Zugriff auf die Datenbank eingeben. Wenn Sie einen Server ausgewählt haben, den Sie zuvor erstellt haben, müssen Sie Anmeldeinformationen für diesen Server eingeben. Sie wird nicht für dieses Lernprogramm wählen die ***erweitert*** Kontrollkästchen. Die ***erweitert*** Optionen ermöglichen es Ihnen, legen Sie die Datenbank [Sortierung](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Wählen Sie die gleiche **Region** , die Sie ausgewählt haben, für die Website.
12. Klicken Sie auf das Häkchen unten rechts neben dem Feld, um anzugeben, dass Sie fertig sind.   
  
    ![Schritt für Datenbank-Einstellungen der neuen Website - mit Datenbank-Assistenten erstellen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Die folgende Abbildung zeigt die mit einem vorhandenen SQL Server und dem Anmeldenamen.   
  
    ![Schritt für Datenbank-Einstellungen der neuen Website - mit Datenbank-Assistenten erstellen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Das Verwaltungsportal auf der Seite "Websites" zurückgibt und die **Status** Spalte zeigt, dass die Website erstellt wird. Nach einer Weile (in der Regel weniger als eine Minute) die **Status** Spalte zeigt, dass die Website erfolgreich erstellt wurde. In der Navigationsleiste auf der linken Seite die Anzahl der Standorte, die Sie in Ihrem Konto haben wird neben der **Websites** Symbol und die Anzahl von Datenbanken wird neben der **SQL-Datenbanken** Symbol.

## <a name="deploy-the-application-to-windows-azure"></a>Bereitstellen der Anwendung in Windows Azure

1. In Visual Studio mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.  
  
    ![Im Projektkontextmenü veröffentlichen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. In der **Profil** auf der Registerkarte die **Web veröffentlichen** -Assistenten klicken Sie auf **Import**.  
  
    ![Veröffentlichungseinstellungen importieren](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Wenn Sie Ihr Windows Azure-Abonnement in Visual Studio zuvor nicht hinzugefügt haben, führen Sie die folgenden Schritte aus. In den folgenden Schritten fügen Sie Ihr Abonnement, sodass der Dropdown-Liste unter **aus einer Windows Azure-Website importieren** Ihrer Website enthalten.

    a. In der **Veröffentlichungsprofil importieren** (Dialogfeld), klicken Sie auf **aus einer Windows Azure-Website importieren**, und klicken Sie dann auf **Hinzufügen von Windows Azure-Abonnement**.

    ![Windows Azure-Abonnement hinzufügen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. In der **Windows Azure-Abonnements importieren** (Dialogfeld), klicken Sie auf **Download Abonnementdatei**.

    ![Abonnementdatei herunterzuladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Speichern Sie in Ihrem Browserfenster die *publishsettings* Datei.

    ![Herunterladen der Datei "publishsettings"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Sicherheit – die *Publishsettings* -Datei enthält die (nicht codierten) Anmeldeinformationen, die zur Verwaltung der Windows Azure-Abonnements und-Dienste verwendet werden. Die bewährte Sicherheitsmethode für diese Datei besteht darin, sie vorübergehend außerhalb von Quellverzeichnissen speichern (z. B. in der *Libraries\Documents* Ordner), und es dann zu löschen, nachdem der Import abgeschlossen ist. Ein böswilliger Benutzer, die erhält Zugriff auf die `.publishsettings` Datei bearbeiten, erstellen und löschen Sie Ihre Windows Azure-Dienste kann.

    d. In der **Windows Azure-Abonnements importieren** (Dialogfeld), klicken Sie auf **Durchsuchen** und navigieren Sie zu der *publishsettings* Datei.

    ![Sub herunterladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Klicken Sie auf **Import**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. In der **Veröffentlichungsprofil importieren** wählen Sie im Dialogfeld **aus einer Windows Azure-Website importieren**, wählen Sie Ihre Website aus der Dropdown Liste, und klicken Sie dann auf **OK**.  
  
    ![Importieren Sie das Veröffentlichungsprofil](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. In der **Verbindung** auf **Validate Connection** um sicherzustellen, dass die Einstellungen richtig sind.  
  
    ![Überprüfen der Verbindung](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Wenn die Verbindung erfolgreich überprüft wurde, wird ein grünes Häkchen neben gezeigt die **Validate Connection** Schaltfläche. Klicken Sie auf **Weiter**.  
  
    ![Erfolgreich überprüfte Verbindung](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Öffnen der **Remote Verbindungszeichenfolge** Dropdown-Liste unter **SchoolContext** , und wählen Sie die Verbindungszeichenfolge für die Datenbank, die Sie erstellt haben.
8. Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.
9. Deaktivieren Sie **verwenden diese Verbindungszeichenfolge zur Laufzeit** für die **UserContext (DefaultConnection)**, da diese Anwendung nicht die Mitgliedschaftsdatenbank verwendet wird.   
  
    ![Registerkarte "Einstellungen"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Klicken Sie auf **Weiter**.
11. In der **Vorschau** auf **starten Preview**.  
  
    ![Schaltfläche "StartPreview" auf der Registerkarte "Vorschau"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Die Registerkarte zeigt eine Liste der Dateien, die an den Server kopiert werden. Anzeigen der Vorschau ist nicht erforderlich, um die Anwendung zu veröffentlichen, aber es ist eine nützliche Funktion zu berücksichtigen. In diesem Fall müssen Sie mit der Liste der Dateien keine Wirkung, die angezeigt wird. Das nächste Mal mit das Sie diese Anwendung bereitstellen, werden in dieser Liste nur die Dateien, die geändert wurden.  
  
    ![Dateiausgabe StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Klicken Sie auf **Veröffentlichen**.  
    Visual Studio startet den Prozess für das Kopieren der Dateien an den Windows Azure-Server.
13. Die **Ausgabe** Fenster zeigt, welche Bereitstellungsaktionen ausgeführt wurden, und gibt erfolgreichen Abschluss der Bereitstellung.  
  
    ![Fenster "Ausgabe" erfolgreich berichtsbereitstellung](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Nach der erfolgreichen Bereitstellung wird der Standardbrowser automatisch an die URL der bereitgestellten-Website geöffnet.  
    Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt. Klicken Sie auf der Registerkarte "Students".  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

Zu diesem Zeitpunkt Ihrer *SchoolContext* Datenbank wurde in der Windows Azure SQL-Datenbank erstellt, da Sie ausgewählte **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**. Die *"Web.config"* -Datei in die bereitgestellte Website geändert wurde, damit die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer führen zum ersten Mal Code liest oder schreibt Daten in der Datenbank (die geschah, als Sie ausgewählt haben die **Studenten** Registerkarte "):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge *(SchoolContext\_DatabasePublish*) für die Code First-Migrationen zum Aktualisieren des Datenbankschemas und das seeding der Datenbank verwendet werden soll.

![Database_Publish-Verbindungszeichenfolge](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

Die *DefaultConnection* Verbindungszeichenfolge bezieht sich auf die Mitgliedschaftsdatenbank (die wir in diesem Lernprogramm nicht verwenden). Die *SchoolContext* Verbindungszeichenfolge bezieht sich auf die ContosoUniversity-Datenbank.

Sie finden die bereitgestellte Version der Datei "Web.config" auf dem lokalen Computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Sie erreichen die bereitgestellte *"Web.config"* Datei selbst unter Verwendung von FTP. Anweisungen hierzu finden Sie unter [ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen eines Updates Code](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Befolgen Sie die Anweisungen, die beginnen mit "um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort."

> [!NOTE]
> Die Web-app implementieren nicht Sicherheit, damit Personen die URL findet die Daten ändern kann. Informationen zum Sichern der Website finden Sie unter [bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Sie können verhindern, dass andere Personen mithilfe der Website mithilfe der Windows Azure-Verwaltungsportal oder **Server-Explorer** in Visual Studio zum Beenden der Website.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Erste Initialisierer Code

Im Abschnitt Bereitstellung Sie gesehen haben die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer verwendet wird. Code enthält zunächst auch andere Initialisierer, die Sie verwenden können, einschließlich, [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (Standard), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) und [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Die `DropCreateAlways` Initialisierer kann nützlich zum Einrichten von Bedingungen für die Komponententests werden. Sie können auch eigene Initialisierer schreiben, und Sie können einen Initialisierer explizit aufrufen, wenn Sie nicht warten, bis die Anwendung liest aus oder in die Datenbank geschrieben werden sollen. Eine umfassende Erläuterung der Initialisierer, finden Sie in Kapitel 6 des Buchs [Entity Framework-Programmierung: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman und Rowan Miller.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen, wie ein Datenmodell erstellen und implementieren grundlegende CRUD, sortieren, filtern, paging und Funktionalität zur Gruppierung. In den nächsten Lernprogrammen beginnen Sie erweiterte Themen ansehen, erweitern Sie das Datenmodell.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Weiter](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
