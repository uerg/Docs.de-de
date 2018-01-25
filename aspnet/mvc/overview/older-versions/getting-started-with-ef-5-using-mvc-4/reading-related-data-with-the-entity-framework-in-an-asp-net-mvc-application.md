---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Lesen von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (5 10) | Microsoft Docs"
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9093fb90a52b297f173c5cddb6f332d2d1a25135
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lesen-bezogene Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (5 10)
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm abgeschlossen Sie das Datenmodell "School". In diesem Lernprogramm werden Sie lesen und Anzeigen verknüpfter Daten – d. h. die Daten, die das Entity Framework in Navigationseigenschaften lädt.

Die folgenden Abbildungen zeigen den Seiten, arbeiten Sie mit.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Verzögerte Eager und explizite Laden von verknüpften Daten

Es gibt mehrere Möglichkeiten, das Entity Framework in die Navigationseigenschaften einer Entität verknüpfte Daten geladen werden können:

- *Verzögertes Laden*. Wenn die Entität zuerst gelesen wird, ist nicht verbundene Daten abgerufen. Allerdings werden beim ersten Versuch, auf eine Navigationseigenschaft, für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt zu mehreren Abfragen an die Datenbank gesendet – eine für die Entität selbst und eine jedes Mal, die Daten, die für die Entität verknüpft abgerufen werden muss. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Unverzüglichem Laden*. Wenn die Entität gelesen wird, werden darin verknüpfte Daten abgerufen. Dies führt normalerweise zu einer einzelnen Join-Abfrage, die alle Daten abruft, die erforderlich ist. Geben Sie unverzüglichem Laden mithilfe der `Include` Methode.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explizites Laden*. Dies gleicht dem verzögerten Laden, mit dem Unterschied, dass Sie explizit die verknüpften Daten im Code abrufen; Es ist nicht automatisch ausgeführt, wenn Sie Zugriff auf eine Navigationseigenschaft. Sie laden verknüpfte Daten manuell, indem beim Abrufen der Objekt-Eintrag für die Status-Manager für eine Entität und der Aufruf der `Collection.Load` Methode für Sammlungen oder `Reference.Load` Methode für die Eigenschaften, die eine einzelne Entität enthalten. (Im folgenden Beispiel, mussten Sie zum Laden der Navigationseigenschaft Administrator ersetzen `Collection(x => x.Courses)` mit `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Da sie nicht die Eigenschaftswerte sofort abrufen, verzögertes Laden und explizites Laden sind auch beide genannt *verzögertes Laden*.

Im Allgemeinen sollten Sie benötigen Sie verknüpfte Daten für jede Entität, die abgerufen werden, unverzüglichem Laden die beste Leistung bietet, da eine einzelne Abfrage, die an die Datenbank gesendet, in der Regel effizienter als separate Abfragen für jede Entität abgerufen wird. Angenommen Sie in den obigen Beispielen wird, dass jede Abteilung zehn Verwandte Kurse gelten. Im Beispiel unverzüglichem Laden führt in nur einer einzigen (Join)-Abfrage und einem einzelnen Roundtrip mit der Datenbank. Das verzögerte Laden und explizites Laden Beispiele für ergibt beide elf Abfragen und elf Roundtrips zur Datenbank. Zusätzliche Roundtrips zur Datenbank sind vor allem auf die Leistung beeinträchtigen, wenn Latenz hoch ist.

In einigen Szenarien ist das verzögertes Laden andererseits, effizienter. Unverzüglichem Laden kann es sich um eine sehr komplexe Verknüpfung erzeugt werden kann, führen, der SQL Server effizient verarbeiten kann. Oder wenn Sie den Zugriff auf eine Entität Navigationseigenschaften nur für einen Teil einer Reihe von Entitäten müssen Sie verarbeiten möchten, verzögertes Laden möglicherweise eine bessere Leistung, da unverzüglichem Laden mehr Daten als benötigt abrufen würde. Wenn die Leistung kritisch ist, empfiehlt es sich zum Testen von Leistung beides Möglichkeiten, um die beste Wahl vornehmen zu können.

In der Regel verwenden Sie explizite Laden nur, wenn Sie lazy loading-aktiviert haben. Ein Szenario, wenn Sie lazy loading-aktivieren, sollten ist während der Serialisierung. Verzögertes Laden und Serialisierung nicht auch kombinieren, und Laden ist aktiviert, wenn Sie nicht sorgfältig, dass Sie Abfragen deutlich mehr Daten als beabsichtigt beim verzögerten ergeben können. Serialisierung funktioniert im Allgemeinen durch den Zugriff auf jede Eigenschaft in einer Instanz eines Typs. Eigenschaftenzugriff löst das verzögertes Laden, und die verzögerten geladenen Entitäten serialisiert werden. Der Serialisierungsprozess greift dann auf jede Eigenschaft des verzögerten geladene Entitäten verursacht möglicherweise noch mehr verzögertes Laden und Serialisierung. Um diese runaway Kettenreaktion zu verhindern, aktivieren Sie lazy loading-deaktivieren, bevor Sie eine Entität serialisieren.

Die Kontext Datenbankklasse führt verzögertes Laden standardmäßig. Es gibt zwei Möglichkeiten, deaktivieren Sie das verzögertes Laden:

- Lassen Sie für bestimmte Navigationseigenschaften, die `virtual` -Schlüsselwort, wenn Sie die Eigenschaft deklarieren.
- Legen Sie für alle Navigationseigenschaften `LazyLoadingEnabled` auf `false`. Beispielsweise können Sie den folgenden Code im Konstruktor der Context-Klasse einfügen: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Verzögertes Laden kann Code maskieren, die Leistungsprobleme verursacht. Beispielsweise ist die Code, der eager oder expliziten Laden nicht angeben, aber eine große Anzahl von Entitäten verarbeitet und verwendet mehrere Navigationseigenschaften in jeder Iteration (aufgrund von viele Roundtrips zur Datenbank) möglicherweise sehr ineffizient. Eine Anwendung, die auch in der Entwicklung mit einer lokalen SQL Servers führt möglicherweise Leistungsprobleme, wenn aufgrund der Latenzzeit und lazy Loading zu Azure SQL-Datenbank verschoben. Profilerstellung für die Datenbankabfragen mit einer realistischen testladevorgang hilft zu bestimmen, ob das verzögertes Laden geeignet ist. Weitere Informationen finden Sie unter [Demystifying Entity Framework-Strategien: Laden von verknüpften Daten](https://msdn.microsoft.com/magazine/hh205756.aspx) und [mit dem Entity Framework zum Verringern der Netzwerklatenz in SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Erstellen einer Indexseite Kurse, zeigt Abteilungsnamen

Die `Course` Entität enthält, eine Navigationseigenschaft, die enthält die `Department` Entität der Abteilung, die der Kurs zugewiesen ist. Um den Namen der zugewiesenen Abteilung in einer Liste von Kurse anzuzeigen, müssen Sie die `Name` Eigenschaft aus der `Department` Entität, die in der `Course.Department` Navigationseigenschaft.

Erstellen Sie einen Controller mit dem Namen `CourseController` für die `Course` Entitätstyp mit denselben Optionen, die Sie zuvor für die `Student` Controller, wie in der folgenden Abbildung gezeigt (außer im Gegensatz zu dem Bild Context-Klasse in der DAL-Namespace ist keine Modelle Namespace):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Open *Controllers\CourseController.cs* und sehen Sie sich die `Index` Methode:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der automatische Gerüstbau angegeben unverzüglichem Laden für die `Department` Navigationseigenschaft mithilfe der `Include` Methode.

Open *Views\Course\Index.cshtml* und Ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Sie haben die folgenden Änderungen an der scaffolded Code vorgenommen:

- Die Überschrift aus geändert **Index** auf **Kurse**.
- Die Links der Zeile verschoben nach links.
- Eine Spalte unter der Überschrift hinzugefügt **Anzahl** zeigt, dass die `CourseID` Eigenschaftswert. (Standardmäßig sind nicht Primärschlüssel Gerüstbau da normalerweise sie Endbenutzern bedeutungslos sind. Allerdings in diesem Fall der Primärschlüssel sinnvoll ist und Sie sie anzeigen möchten.)
- Die letzte Spaltenüberschrift aus geändert **"DepartmentID"** (der Name des Fremdschlüssels an die `Department` Entität) zu **Abteilung**.

Beachten Sie, dass für die letzte Spalte der scaffolded Code zeigt die `Name` Eigenschaft von der `Department` Entität, die geladen wird die `Department` Navigationseigenschaft:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Führen Sie die Seite (Wählen Sie die **Kurse** Registerkarte auf der Startseite des Contoso-University) um die Liste mit den Abteilungsnamen anzuzeigen.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Erstellen Sie eine Lehrkräfte Indexseite, die Kurse und Registrierung anzeigt

In diesem Abschnitt Sie erstellen einen Controller und zeigen Sie für die `Instructor` Entität, um die Indexseite Lehrkräfte anzuzeigen:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Auf dieser Seite liest und zeigt verknüpfte Daten auf folgende Weise:

- Die Liste der Lehrkräfte zeigt verknüpfte Daten aus der `OfficeAssignment` Entität. Die `Instructor` und `OfficeAssignment` Entitäten sind in einer 1: 0 (null)-oder-1-Beziehung. Verwenden Sie unverzüglichem Laden für die `OfficeAssignment` Entitäten. Wie zuvor erläutert, ist die unverzüglichem Laden in der Regel effizienter, wenn Sie die verknüpften Daten für alle abgerufenen Zeilen von der primären Tabelle benötigen. In diesem Fall möchten Sie Office-Zuweisungen für alle angezeigten Lehrkräfte anzuzeigen.
- Wenn der Benutzer wählt einen Kursleiter, die im Zusammenhang `Course` Entitäten werden angezeigt. Die `Instructor` und `Course` Entitäten sind in einer m: n-Beziehung. Verwenden Sie unverzüglichem Laden für die `Course` Entitäten und die zugehörigen `Department` Entitäten. In diesem Fall kann das verzögertes Laden effizienter sein, da Sie nur für den ausgewählten Dozenten Kurse benötigen. Dieses Beispiel zeigt jedoch mit unverzüglichem Laden für Navigationseigenschaften innerhalb von Entitäten, die selbst in den Navigationseigenschaften werden.
- Wenn der Benutzer einen Kurs auswählt, beziehen Sie Daten aus der `Enrollments` Entitätenmenge wird angezeigt. Die `Course` und `Enrollment` Entitäten sind in einer 1: n-Beziehung. Fügen Sie das explizite Laden für `Enrollment` Entitäten und die zugehörigen `Student` Entitäten. (Explizites Laden nicht erforderlich, da verzögertes Laden ist aktiviert, aber dies veranschaulicht das explizite laden.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Erstellen Sie ein Ansichtsmodell für die Indexansicht Dozenten

Die Instructor-Indexseite zeigt drei verschiedene Tabellen. Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält jede mit den Daten für eine der Tabellen.

In der *ViewModels* Ordner erstellen *InstructorIndexData.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Hinzufügen eines Formats für die ausgewählten Zeilen

Um die ausgewählte Zeilen zu kennzeichnen benötigen Sie eine andere Hintergrundfarbe. Um einen Stil für diese Benutzeroberfläche bereitzustellen, fügen Sie folgenden hervorgehobenen Code hinzu, mit dem Abschnitt `/* info and errors */` in *Content\Site.css*, wie unten dargestellt:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Die Instructor-Controller und Ansichten werden erstellt

Erstellen einer `InstructorController` Controller wie in der folgenden Abbildung gezeigt:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Open *Controllers\InstructorController.cs* und Hinzufügen einer `using` -Anweisung für die `ViewModels` Namespace:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Der scaffolded Code in der `Index` Methode gibt unverzüglichem Laden nur für die `OfficeAssignment` Navigationseigenschaft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ersetzen Sie die `Index` Methode durch den folgenden Code aus, um die zusätzliche Last-bezogene Daten, und fügen Sie ihn in das Modell anzeigen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Die Methode akzeptiert optionale Routendaten (`id`) und einen Abfragezeichenfolgenparameter (`courseID`), geben Sie die ID-Werte der ausgewählten Kursleiter und ausgewählten Kurs, und übergibt alle erforderlichen Daten an die Ansicht. Die Parameter bereitgestellt werden, indem die **wählen** links auf der Seite.

> [!TIP] 
> 
> **Weiterleitung von Daten**
> 
> Routendaten sind Daten, die der Modellbinder in ein URL-Segment angegeben, in der Routingtabelle gefunden. Die Standardroute gibt z. B. `controller`, `action`, und `id` Segmente:
> 
> routes.MapRoute(  
>  Name: "Default"  
>  URL: "{Controller} / {Aktion} / {Id}",  
>  Standardwerte: new {Controller = "Home", Aktion = "Index", Id = UrlParameter.Optional}  
> );
> 
> In der folgenden URL ordnet die Standardroute `Instructor` als die `controller`, `Index` als die `action` und 1 der `id`; Hierbei handelt es sich um Datenwerte für die Route.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? CourseID 2021 =" ist ein Wert der Abfragezeichenfolge. Der Modellbinder funktioniert auch, wenn Sie übergeben die `id` als Abfragezeichenfolgenwert:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Die URLs werden erstellt, indem `ActionLink` Anweisungen in der Razor-Ansicht. Im folgenden Code wird die `id` Parameter entspricht der Standardroute daher `id` die Routendaten hinzugefügt wird.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Im folgenden Code `courseID` einen Parameter in die Standardroute stimmt nicht überein, damit sie als eine Abfragezeichenfolge hinzugefügt wird.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Der Code wird zuerst Erstellen einer Instanz des Modells anzeigen und darin die Liste der Lehrkräfte einfügen. Der Code gibt unverzüglichem Laden für die `Instructor.OfficeAssignment` und `Instructor.Courses` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Die zweite `Include` Methode lädt Kurse und für jeden Kurs, die geladen wird, ist dies der Fall ist unverzüglichem Laden für die `Course.Department` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Wie bereits erwähnt, unverzüglichem Laden ist nicht erforderlich, jedoch wird durchgeführt, um die Leistung zu verbessern. Da die Sicht muss die `OfficeAssignment` Entität, es ist jedoch effizienter, die in derselben Abfrage abgerufen. `Course`Entitäten sind erforderlich, wenn auf der Webseite ein Kursleiter ausgewählt ist, damit unverzüglichem Laden ist besser als lazy loading, nur dann, wenn die Seite mit einem Kurs ausgewählt als ohne häufiger angezeigt wird.

Instructor-ID wurde ausgewählt, wird der ausgewählte Kursleiter aus der Liste der Kursleiter das Ansichtsmodell abgerufen. Des Ansichtsmodells `Courses` Eigenschaft wird dann geladen, mit der `Course` Entitäten aus dieser Dozenten `Courses` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Die `Where` -Methode gibt eine Auflistung, aber in diesem Fall die Kriterien, die nur eine einzelne Methode zu übergeben `Instructor` Entität zurückgegeben wird. Die `Single` -Methode konvertiert die Auflistung in einem einzelnen `Instructor` Entität, die Sie Zugriff auf diese Entität gewährt `Courses` Eigenschaft.

Verwenden Sie die [einzelne](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) Methode auf eine Auflistung, wenn Sie wissen, dass die Auflistung wird nur ein Element verfügen. Die `Single` Methode löst eine Ausnahme aus, wenn die übergebene Auflistung leer ist oder wenn mehr als ein Element vorhanden ist. Ist eine Alternative [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), dem einen Standardwert zurückgegeben (`null` in diesem Fall), wenn die Auflistung leer ist. Jedoch in diesem Fall, da immer noch ansonsten eine Ausnahme (aus beim Suchen nach einer `Courses` Eigenschaft auf einen `null` Verweis), und die Ausnahmemeldung würde weniger deutlich die Ursache des Problems angeben. Beim Aufrufen der `Single` -Methode, können Sie auch übergeben der `Where` Bedingung statt der `Where` Methode getrennt:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

anstelle von:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Als Nächstes wird ein Kurs ausgewählt wurde, der ausgewählte Kurs aus der Liste der Kurse in das Ansichtsmodell abgerufen. Klicken Sie dann die des Ansichtsmodells `Enrollments` Eigenschaft geladen wird, mit der `Enrollment` Entitäten aus dieser Kurs `Enrollments` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Ändern die Indexansicht Dozenten

In *Views\Instructor\Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Sie haben die folgenden Änderungen an den vorhandenen Code vorgenommen:

- Die Modellklasse, um geändert `InstructorIndexData`.
- Der Seitenname von geändert **Index** auf **Lehrkräfte**.
- Der Link Zeilenspalten verschoben nach links.
- Entfernt die **FullName** Spalte.
- Hinzugefügt ein **Office** Spalte `item.OfficeAssignment.Location` nur, wenn `item.OfficeAssignment` ist ungleich null. (Da dies eine 1: 0 (null)-oder-1-Beziehung ist, gibt es möglicherweise keiner verknüpften `OfficeAssignment` Entität.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Code hinzugefügt, die dynamisch hinzufügen `class="selectedrow"` auf die `tr` Element des ausgewählten Kursleiter. Hiermit wird eine Hintergrundfarbe für die ausgewählte Zeile mithilfe der CSS-Klasse, die Sie zuvor erstellt haben. (Die `valign` Attribut wird im folgenden Lernprogramm nützlich sein, wenn Sie eine Spalte mit mehreren Zeile der Tabelle hinzufügen.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Ein neues hinzugefügt `ActionLink` mit der Bezeichnung **wählen** unmittelbar vor der Links von anderen in jeder Zeile, die die ausgewählte Instructor-ID, die zu sendende verursacht die `Index` Methode.

Führen Sie die Anwendung, und wählen Sie die **Lehrkräfte** Registerkarte. Die Seite zeigt die `Location` Eigenschaft verwandter `OfficeAssignment` Entitäten und eine leere Tabelle Zelle bei Nein, die im Zusammenhang `OfficeAssignment` Entität.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

In der *Views\Instructor\Index.cshtml* Datei, nach dem schließenden `table` Element (am Ende der Datei), fügen Sie folgenden hervorgehobenen Code hinzu. Dadurch wird es sich um eine Liste der Kurse, die im Zusammenhang mit einen Kursleiter beim ein Kursleiter ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Dieser Code liest die `Courses` Eigenschaft des Modells anzeigen, das eine Liste der Kurse anzeigen. Sie bietet außerdem eine `Select` Link, der die ID der ausgewählten Kurs, sendet der `Index` Aktionsmethode.

> [!NOTE]
> Die *CSS* Datei wird vom Browser zwischengespeichert. Wenn die Änderungen beim Ausführen der Anwendung nicht angezeigt wird, führen Sie eine feste Aktualisierung (halten Sie STRG gedrückt, während Sie auf die **aktualisieren** klicken, oder drücken Sie STRG + F5).


Führen Sie die Seite, und wählen Sie einen Kursleiter. Jetzt sehen Sie ein Raster mit den für den ausgewählten Kursleiter zugewiesene Kurse zeigt an, und jeder Kurs Sie finden Sie unter den Namen der zugewiesenen Abteilung.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Fügen Sie nachdem der Codeblock, den Sie gerade hinzugefügt haben den folgenden Code ein. Dadurch wird eine Liste der Studenten, die registriert werden in einen Kurs, wenn dieser Kurs ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Dieser Code liest die `Enrollments` Eigenschaft des Modells Ansicht um eine Liste der Schüler anzuzeigen, die in den Kurs registriert sind.

Führen Sie die Seite, und wählen Sie einen Kursleiter. Wählen Sie dann einen Kurs, finden in der Liste der registrierten Studenten und deren Qualitäten.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Explizites Laden hinzufügen

Open *InstructorController.cs* und suchen Sie nach, wie die `Index` Methode ruft die Liste der Registrierung für einen ausgewählten Kurs:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Wenn Sie die Liste der Lehrkräfte abgerufen, angegebene unverzüglichem Laden für die `Courses` Navigationseigenschaft und für die `Department` Eigenschaft jeder natürlich. Sie Sie platzieren der `Courses` Sammlung das Ansichtsmodell und nachdem Sie zugreifen, die `Enrollments` Navigationseigenschaft von einer Entität in der Auflistung. Da Sie nicht unverzüglichem Laden für angegeben haben die `Course.Enrollments` Navigationseigenschaft, die Daten aus dieser Eigenschaft auf der Seite aufgrund einer verzögerten Laden angezeigt werden.

Wenn Sie das verzögertes Laden deaktiviert, ohne den Code auf andere Weise ändern die `Enrollments` Eigenschaft würde unabhängig davon, wie viele Registrierungen in Kurs Wirklichkeit null sein. In diesem Fall wird beim Laden der `Enrollments` -Eigenschaft, müssten Sie unverzüglichem Laden oder explizites Laden angeben. Sie haben bereits gesehen, wie unverzüglichem Laden ausgeführt werden. Um ein Beispiel für explizite Laden angezeigt wird, ersetzen die `Index` Methode durch den folgenden Code, die explizit lädt die `Enrollments` Eigenschaft. Der Code geändert werden hervorgehoben.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Nach dem Abrufen der ausgewählten `Course` Entität, der neue Code lädt explizit des Kurses `Enrollments` Navigationseigenschaft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Anschließend er explizit jedes lädt `Enrollment` Entität die verbundene `Student` Entität:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Beachten Sie, mit denen Sie die `Collection` -Methode zum Laden von Auflistungseigenschaft, aber für eine Eigenschaft, die nur eine Entität enthält, verwenden Sie die `Reference` Methode. Sie können auch die Instructor-Indexseite ausführen und keinen Unterschied in der Anzeige auf der Seite wird angezeigt, obwohl Sie geändert haben, wie die Daten abgerufen werden.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle drei Arten (lazy eager und explizite) zum Laden von verknüpfter Daten in den Navigationseigenschaften verwendet werden. In den nächsten Lernprogrammen erfahren Sie, wie verknüpfte Daten aktualisiert werden.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Weiter](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
