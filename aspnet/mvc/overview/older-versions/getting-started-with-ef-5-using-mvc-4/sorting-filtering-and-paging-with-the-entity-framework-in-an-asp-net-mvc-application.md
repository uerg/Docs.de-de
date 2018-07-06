---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortieren, Filtern und Paging mit Entitätsframework in einer ASP.NET MVC-Anwendung (3 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f5ce5f926021a53a5dd96e578b26b8c186c9a83c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841033"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sortieren, Filtern und Paging mit Entitätsframework in einer ASP.NET MVC-Anwendung (3 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Sie haben einen `searchString`-Parameter zur `Index`-Methode hinzugefügt. Sie haben auch die LINQ-Anweisung hinzugefügt eine `where` Clausethat wählt Schüler/Studenten beschränkt, deren vor- oder Nachname die Suchzeichenfolge enthält. Der suchzeichenfolgenwert wird aus einem Textfeld empfangen, die Sie zur Indexansicht hinzufügen. Die Anweisung, die fügt die [, in denen](https://msdn.microsoft.com/library/bb535040.aspx) -Klausel ausgeführt wird, nur bei ein zu suchenden Wert an.

> [!NOTE]
> In vielen Fällen können Sie die gleiche Methode aufrufen, an einer Entity Framework-Entitätenmenge oder als eine Erweiterungsmethode in einer in-Memory-Sammlung. Die Ergebnisse sind normalerweise identisch, aber in einigen Fällen können abweichen. Z. B. die Implementierung von .NET Framework die `Contains` Methode gibt alle Zeilen zurück, wenn Sie eine leere Zeichenfolge an sie übergeben, aber die Entity Framework-Anbieter für SQL Server Compact 4.0 gibt 0 (null) Zeilen auf leere Zeichenfolgen zurück. Aus diesem Grund wird der Code im Beispiel (Einfügen der `Where` -Anweisung innerhalb einer `if` Anweisung) stellt sicher, dass Sie die gleichen Ergebnisse für alle Versionen von SQL Server erhalten. Darüber hinaus die Implementierung von .NET Framework die `Contains` -Methode führt einen Vergleich Groß-/Kleinschreibung standardmäßig, aber die Entity Framework SQL Server-Ressourcenanbieter führen die Groß-/Kleinschreibung Vergleiche standardmäßig. Aus diesem Grund Aufrufen der `ToUpper` Methode, um den Test explizit Groß-/Kleinschreibung, wird sichergestellt, dass die Ergebnisse nicht ändern, wenn Sie ändern den Code später ein Repository verwenden, die zurückgibt, wird ein `IEnumerable` -Sammlung anstelle einer `IQueryable` Objekt. (Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Hinzufügen eines Suchfelds zur Studentenindexansicht

In *Views\Student\Index.cshtml*, fügen Sie den hervorgehobenen Code unmittelbar vor dem Öffnen `table` Tag, um eine Beschriftung, ein Textfeld, erstellen und eine **Suche** Schaltfläche.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Führen Sie die Seite, geben Sie eine Suchzeichenfolge ein, und klicken Sie auf **Suche** um sicherzustellen, dass die Funktionsweise des Filters.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Beachten Sie, dass die URL nicht die Suchzeichenfolge "an" enthält, was bedeutet, dass wenn Sie diese Seite kennzeichnen, die gefilterte Liste erhalten Sie wird nicht bei der Verwendung von Lesezeichen. Ändern Sie die **Suche** Schaltfläche, um später in diesem Tutorial verwenden Sie Abfragezeichenfolgen für Filterkriterien.

## <a name="add-paging-to-the-students-index-page"></a>Hinzufügen von Paging zu der Indexseite "Studenten"

Beginnen Sie zur Indexseite "Studenten" Paging hinzugefügt haben, durch die Installation von der **PagedList.Mvc** NuGet-Paket. Sie zusätzliche Änderungen vornehmen, werden die `Index` Methode und Hinzufügen von paginglinks in der `Index` anzeigen. **PagedList.Mvc** ist einer der vielen guten Paginierung und Sortierung von Paketen für ASP.NET MVC und dessen Verwendung hier dient nur als Beispiel, nicht als eine Empfehlung für die sie über weitere Optionen. Die folgende Abbildung zeigt den Paging-Links.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installieren Sie das PagedList.MVC NuGet-Paket

NuGet **PagedList.Mvc** Paketinstallationen automatisch die **PagedList** Paket als Abhängigkeit. Die **PagedList** Paket installiert einen `PagedList` Sammlungsmethoden Typ und die Erweiterung für `IQueryable` und `IEnumerable` Sammlungen. Die Erweiterungsmethoden erstellen Sie eine einzelne Seite mit Daten in eine `PagedList` Auflistung von Ihrer `IQueryable` oder `IEnumerable`, und die `PagedList` enthält verschiedene Eigenschaften und Methoden, die das Paging zu erleichtern. Die **PagedList.Mvc** Paketinstallationen eine paging-Hilfsprogramm, das die Pagingschaltflächen anzeigt.

Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten**.

In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf die **Online** links auf die Registerkarte, und geben Sie dann in das Suchfeld "ausgelagert". Wenn Sie sehen die **PagedList.Mvc** -Paket, klicken Sie auf **installieren**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

In der **Projekte auswählen** auf **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Hinzufügen von Pagingfunktionen zur Indexmethode

In *Controllers\StudentController.cs*, Hinzufügen einer `using` -Anweisung für die `PagedList` Namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Ersetzen Sie die `Index`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Dieser Code Fügt eine `page` -Parameter, einen aktuellen Sortierparameter Reihenfolge und eine aktuelle Filter-Parameter, um die Signatur der Methode, wie hier gezeigt:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging- oder Sortierlink geklickt hat, werden alle Parameter gleich 0 (null) sein. Wenn auf ein paginglink geklickt wird, die `page` Variable enthält die anzuzeigende Seitenzahl.

`A ViewBag` Eigenschaft bietet eine Ansicht mit der aktuellen Sortierreihenfolge, da dies in den paginglinks enthalten sein muss, um der Sortierreihenfolge der während des Pagings beibehalten:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Eine andere Eigenschaft, `ViewBag.CurrentFilter`, bietet eine Ansicht mit der aktuellen Filterzeichenfolge. Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird. Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann. Die Suchzeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben, und die Schaltfläche "Senden" geklickt wird. In diesem Fall die `searchString` -Parameter ist nicht null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Am Ende der Methode die `ToPagedList` Erweiterungsmethode für die Schüler/Studenten `IQueryable` Objekt konvertiert die studentenabfrage in eine einzelne Seite in einem Sammlungstyp, der Pagingvorgänge unterstützt. Diese einzelnen Seite mit Studentendaten wird dann an die Ansicht übergeben:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `ToPagedList`-Methode nimmt eine Seitenanzahl. Die zwei Fragezeichen darstellen der [Null-Sammeloperator](https://msdn.microsoft.com/library/ms173224.aspx). Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.

### <a name="add-paging-links-to-the-student-index-view"></a>Hinzufügen eines Paginglinks zur Indexansicht für Schüler und Studenten

In *Views\Student\Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PagedList`-Objekt anstelle eines `List`-Objekts aufruft.

Die `using` -Anweisung für `PagedList.Mvc` bietet Zugriff auf das MVC-Hilfsprogramm für die Pagingschaltflächen.

Der Code verwendet eine Überladung der [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , ermöglicht es an [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Der Standardwert [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) sendet Formulardaten mit einem POST, was bedeutet, dass Parameter als Abfragezeichenfolgen im Hauptteil HTTP-Nachricht und nicht in der URL übergeben werden. Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen. Die [W3C-Richtlinien für die Verwendung von HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) anzugeben, dass Sie GET verwenden sollten, wenn die Aktion nicht in einem Update führt.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert. Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Erstellen Sie eine Infoseite, die Statistiken der Studentendaten anzeigt

Zeigen Sie für der Contoso University-Website die Infoseite wie viele Studenten jedes Anmeldedatum angemeldet haben. Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen. Um dies zu erreichen, ist Folgendes erforderlich:

- Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.
- Ändern der `About` -Methode in der die `Home` Controller.
- Ändern der `About` anzeigen.

### <a name="create-the-view-model"></a>Erstellen des Ansichtsmodells

Erstellen Sie eine *ViewModels* Ordner. Fügen Sie in diesem Ordner die Klassendatei *EnrollmentDateGroup.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Optional: Bereitstellen der app für Windows Azure

Bisher wurde die Anwendung lokal in IIS Express auf Ihrem Entwicklungscomputer ausgeführt wurde. Um es für andere Benutzer über das Internet verfügbar zu machen, müssen Sie es auf einen Webhostinganbieter bereitstellen. In diesem optionalen Abschnitt des Tutorials stellen Sie es auf einem Windows Azure-Website bereit.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Mithilfe von Code First-Migrationen zum Bereitstellen der Datenbank

Um die Datenbank bereitstellen, verwenden Sie Code First-Migrationen. Wenn Sie das Veröffentlichungsprofil, die Sie zum Konfigurieren von Einstellungen erstellen für die Bereitstellung aus Visual Studio verwenden, wählen Sie ein Kontrollkästchen mit der Bezeichnung **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**. Diese Einstellung bewirkt, dass den Bereitstellungsprozess so konfigurieren Sie die Anwendung automatisch *"Web.config"* Datei auf dem Zielserver so, dass Code First verwendet die `MigrateDatabaseToLatestVersion` datenbankinitialisiererklasse.

Visual Studio führt keine Aktion mit der Datenbank während der Bereitstellung aus. Wenn die bereitgestellte Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift, Code First automatisch wird die Datenbank erstellt oder aktualisiert das Datenbankschema auf die neueste Version. Wenn die Anwendung eine Migrationen implementiert `Seed` -Methode, die Methode ausgeführt wird, nachdem die Datenbank wird erstellt, oder das Schema wird aktualisiert.

Ihre Migrationen `Seed` -Methode fügt die Testdaten. Wenn Sie in einer produktionsumgebung bereitstellen, müssten Sie ändern die `Seed` Methode, sodass die It nur Daten einfügt, die in der-Datenbank eingefügt werden sollen. Z. B. in Ihrem aktuellen Datenmodell empfiehlt um echte Kurse, aber fiktive Schüler/Studenten in der Entwicklungsdatenbank zu erhalten. Können Sie schreiben eine `Seed` Methode, um sowohl in der Entwicklung zu laden und kommentieren Sie die fiktiven Schüler/Studenten, bevor Sie in der produktionsumgebung bereitstellen. Oder Sie schreiben eine `Seed` Methode, um nur Kurse zu laden, und geben die fiktiven Schüler/Studenten in der Testdatenbank manuell mithilfe der Benutzeroberfläche der Anwendung.

### <a name="get-a-windows-azure-account"></a>Fordern Sie ein Windows Azure-Konto

Sie benötigen ein Windows Azure-Konto. Wenn Sie noch nicht haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Erstellen Sie eine Website und einer SQL-Datenbank in Windows Azure

Der Windows Azure-Website wird in einer freigegebenen Hostingumgebung ausgeführt, was bedeutet, dass sie auf virtuellen Computern (VMs) ausgeführt wird, die gemeinsam mit anderen Windows Azure-Clients verwendet werden. Eine freigegebene Hostingumgebung ist eine kostengünstige Möglichkeit für den Einstieg in die Cloud. Wenn der Webdatenverkehr zunimmt, kann später die Anwendung skalieren, um die Anforderungen zu erfüllen, indem Sie auf dedizierten virtuellen Computern ausführen. Wenn Sie eine komplexere Architektur benötigen, können Sie in Windows Azure Cloud Services migrieren. Führen Sie Cloud-Dienste auf dedizierten virtuellen Computern, die Sie gemäß Ihren Anforderungen konfigurieren können.

Windows Azure SQL-Datenbank ist eine cloudbasierte relationale Datenbank-Dienst, der auf SQL Server-Technologien basiert. Tools und Anwendungen, die Arbeit mit SQL Server funktionieren auch mit SQL-Datenbank.

1. In der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/), klicken Sie auf **Websites** in der linken Registerkarte, und klicken Sie dann auf **neu**.

    ![Schaltfläche "Neu" im Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Klicken Sie auf **Benutzerdefiniert erstellen**.

    ![Erstellen Sie mit der Datenbank-Link im Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Die **neue Website - Benutzerdefiniert erstellen** -Assistent wird geöffnet.
3. In der **neue Website** Schritt des Assistenten geben Sie in einer Zeichenfolge in die **URL** Feld als eindeutige URL für Ihre Anwendung verwenden. Die vollständige URL besteht aus hier eingegebenen plus das Suffix, das neben dem Textfeld angezeigt. Die Abbildung zeigt "ConU", aber diese URL wird wahrscheinlich erstellt, daher müssen Sie einen anderen Wert auswählen.

    ![Erstellen Sie mit der Datenbank-Link im Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. In der **Region** Dropdown-Liste, wählen Sie eine Region in Ihrer Nähe. Diese Einstellung gibt an, welches Datencenter Ihrer Website ausgeführt wird.
5. In der **Datenbank** Dropdown- Liste **erstellen Sie eine kostenlose 20-MB-SQL-Datenbank**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. In der **NAME der DATENBANKVERBINDUNGSZEICHENFOLGE**, geben Sie *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Klicken Sie auf den Pfeil nach rechts unten auf der das Feld. Der Assistent wechselt zur der **Datenbankeinstellungen** Schritt.
8. In der **Namen** geben *ContosoUniversityDB*.
9. In der **Server** Kontrollkästchen **neue SQL-Datenbankserver**. Wenn Sie einen Server zuvor erstellt haben, können Sie auch diesen Server aus der Dropdown-Liste auswählen.
10. Geben Sie einen Administrator **ANMELDENAME** und **Kennwort**. Wenn Sie ausgewählt haben **neue SQL-Datenbankserver** Sie sind nicht keinen vorhandenen Namen und das Kennwort hier eingeben, geben Sie einen neuen Namen und das Kennwort, das stattdessen Sie jetzt definieren zur späteren Verwendung, wenn Sie die Datenbank zugreifen. Wenn Sie einen Server, den Sie zuvor erstellt haben ausgewählt, müssen Sie Anmeldeinformationen für diesen Server eingeben. Sie wird nicht für dieses Tutorial Auswählen der ***erweitert*** Kontrollkästchen. Die ***erweitert*** Optionen können Sie die Datenbank [Sortierreihenfolge](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Wählen Sie die gleiche **Region** , die Sie für die Website ausgewählt haben.
12. Klicken Sie auf das Häkchen unten rechts neben dem Feld, um anzugeben, dass Sie fertig sind.   
  
    ![Datenbank-Einstellungen für-Schritt der neue Website - mit Datenbank-Assistenten erstellen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Die folgende Abbildung zeigt, verwenden eine vorhandene SQL Server, und melden.   
  
    ![Datenbank-Einstellungen für-Schritt der neue Website - mit Datenbank-Assistenten erstellen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Das Verwaltungsportal auf der Seite "Websites" zurückgibt und die **Status** Spalte zeigt, dass die Website erstellt wird. Nach einer Weile (in der Regel weniger als einer Minute) die **Status** Spalte wird angezeigt, die Website erfolgreich erstellt wurde. In der Navigationsleiste auf der linken Seite, die Anzahl der Standorte, die Sie in Ihrem Konto haben wird neben der **Websites** Symbol und die Anzahl der Datenbanken wird neben der **SQL-Datenbanken** Symbol.

## <a name="deploy-the-application-to-windows-azure"></a>Bereitstellen der Anwendung in Windows Azure

1. In Visual Studio mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.  
  
    ![Veröffentlichen Sie im Kontextmenü "Projekt"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. In der **Profil** Registerkarte die **Webveröffentlichung** -Assistenten klicken Sie auf **Import**.  
  
    ![Importieren der veröffentlichungseinstellungen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Wenn Sie Ihr Windows Azure-Abonnement in Visual Studio zuvor nicht hinzugefügt haben, führen Sie die folgenden Schritte aus. In den folgenden Schritten fügen Sie Ihrem Abonnement, sodass der Dropdown-Liste unter **importieren aus einer Windows Azure-Website** Ihrer Website enthalten.

    a. In der **Veröffentlichungsprofil importieren** Dialogfeld klicken Sie auf **importieren aus einer Windows Azure-Website**, und klicken Sie dann auf **Hinzufügen von Windows Azure-Abonnement**.

    ![Windows Azure-Abonnement hinzufügen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. In der **Windows Azure-Abonnements importieren** Dialogfeld klicken Sie auf **Abonnementdatei herunterladen**.

    ![Abonnementdatei herunterladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Speichern Sie in Ihrem Browser, die *.publishsettings* Datei.

    ![Herunterladen der PUBLISHSETTINGS-Datei](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Sicherheit – die *Publishsettings* -Datei enthält Ihre (unverschlüsselten) Anmeldeinformationen, die zur Verwaltung Ihrer Windows Azure-Abonnements und-Dienste verwendet werden. Die bewährte Sicherheitsmethode für diese Datei ist vorübergehend außerhalb Ihrer quellcodeverzeichnisse speichern (z. B. in der *Libraries\Documents* Ordner), und löschen Sie sie nach Abschluss des Importvorgangs. Ein böswilliger Benutzer, erhält Zugriff auf, die `.publishsettings` Datei bearbeiten, erstellen und Löschen von Windows Azure-Diensten kann.

    d. In der **Windows Azure-Abonnements importieren** Dialogfeld klicken Sie auf **Durchsuchen** und navigieren Sie zu der *.publishsettings* Datei.

    ![Sub herunterladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Klicken Sie auf **Import**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. In der **Veröffentlichungsprofil importieren** wählen Sie im Dialogfeld **importieren aus einer Windows Azure-Website**, wählen Sie Ihre Website aus der Dropdown-Liste, und klicken Sie dann auf **OK**.  
  
    ![Importieren Sie das Veröffentlichungsprofil](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. In der **Verbindung** auf **Verbindung überprüfen** um sicherzustellen, dass die Einstellungen richtig sind.  
  
    ![Verbindung überprüfen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Wenn die Verbindung validiert wurde, wird ein grünes Häkchen neben gezeigt die **Verbindung überprüfen** Schaltfläche. Klicken Sie auf **Weiter**.  
  
    ![Erfolgreich überprüfte Verbindung](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Öffnen der **remoteverbindungszeichenfolge** Dropdown-Liste unter **SchoolContext** , und wählen Sie die Verbindungszeichenfolge für die Datenbank, die Sie erstellt haben.
8. Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**.
9. Deaktivieren Sie **verwenden Sie diese Verbindungszeichenfolge zur Laufzeit** für die **UserContext (DefaultConnection)**, da die Mitgliedschaftsdatenbank nicht von dieser Anwendung verwendet wird.   
  
    ![Registerkarte "Einstellungen"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Klicken Sie auf **Weiter**.
11. In der **Vorschau** auf **Vorschau starten**.  
  
    ![StartPreview-Schaltfläche in der Registerkarte "Vorschau"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Die Registerkarte zeigt eine Liste der Dateien, die an den Server kopiert werden. Anzeigen der Vorschau nicht zum Veröffentlichen der Anwendung erforderlich, aber es ist eine nützliche Funktion berücksichtigen. In diesem Fall müssen Sie mit der Liste der Dateien nichts, das angezeigt wird. Bei der nächsten Bereitstellung dieser Anwendung werden nur die geänderten Dateien in dieser Liste.  
  
    ![StartPreview DateiAusgabe](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Klicken Sie auf **Veröffentlichen**.  
    Visual Studio startet den Prozess des Kopierens von Dateien mit dem Windows Azure-Server.
13. Die **Ausgabe** Fenster zeigt, welche Bereitstellungsaktionen ausgeführt wurden, und meldet die erfolgreiche Durchführung der Bereitstellung.  
  
    ![Fenster "Ausgabe" Meldung zur erfolgreichen Bereitstellung](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Nach der erfolgreichen Bereitstellung wird der Standardbrowser automatisch an die URL der bereitgestellten Website geöffnet.  
    Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt. Klicken Sie auf der Registerkarte "Students".  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

An diesem Punkt Ihrer *SchoolContext* Datenbank wurde in Windows Azure SQL-Datenbank erstellt wurde, da Sie ausgewählt haben **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**. Die *"Web.config"* -Datei in die bereitgestellte Website geändert wurde, damit die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer würde beim ersten Durchlauf des Codes liest oder schreibt Daten in der Datenbank (die passiert, wenn Sie ausgewählt haben die **Schüler/Studenten** Registerkarte "):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge *(SchoolContext\_DatabasePublish*) für Code First-Migrationen zum Aktualisieren des Datenbankschemas und das seeding der Datenbank.

![Database_Publish-Verbindungszeichenfolge](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

Die *DefaultConnection* Verbindungszeichenfolge angegeben wurde, für die Mitgliedschaftsdatenbank (die wir in diesem Lernprogramm nicht verwenden). Die *SchoolContext* Verbindungszeichenfolge, die für die Datenbank ContosoUniversity ist.

Sie finden die bereitgestellte Version der Datei "Web.config" auf dem lokalen Computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Sie können die bereitgestellte zugreifen *"Web.config"* Datei selbst unter Verwendung von FTP. Anweisungen hierzu finden Sie unter [ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Codeupdates](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Befolgen Sie die Anweisungen, die mit beginnen "um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort."

> [!NOTE]
> Die Web-app implementieren nicht Sicherheit, damit jeder, der die URL findet die Daten ändern kann. Anweisungen dazu, wie Sie die Website zu schützen, finden Sie unter [bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einem Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Sie können verhindern, dass andere Personen über die Website mithilfe der Windows Azure-Verwaltungsportal oder **Server-Explorer** in Visual Studio für den Standort zu beenden.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Code First-Initialisierer

In den Abschnitt zur Bereitstellung, in dem Sie gesehen haben die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer, die verwendet wird. Code stellt zunächst auch andere Initialisierer, die Sie verwenden können, einschließlich, ["createDatabaseIfNotExists"](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (Standardeinstellung), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) und [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Die `DropCreateAlways` Initialisierer für das Einrichten von Bedingungen für die Komponententests nützlich sein kann. Sie können auch eigene Initialisierer schreiben, und Sie können keinen Initialisierer explizit aufrufen, wenn Sie nicht warten, bis die Anwendung liest bzw. in die Datenbank geschrieben werden sollen. Eine umfassende Erläuterung der Initialisierer, finden Sie in Kapitel 6 des Buchs [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gesehen, wie ein Datenmodell erstellen und grundlegende CRUD, sortieren, filtern, paging und Gruppieren von Funktionen zu implementieren. Im nächsten Tutorial beginnen Sie Themen für fortgeschrittenere Benutzer ansehen, indem Sie das Datenmodell erweitern.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Weiter](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
