---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lesen von relevanten Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung (5 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 6fa85f561c85d1761ddcbc7167aa4b39a6c6eda3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374324"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lesen verwandter Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung (5 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Tutorial haben Sie das Datenmodell "School". In diesem Tutorial werden Sie lesen und Anzeigen von verknüpften Daten, d. h. Daten, die Entity Framework in Navigationseigenschaften lädt.

Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Verzögerte, Eager und explizite Laden verwandter Daten

Es gibt mehrere Möglichkeiten, Entity Framework zugehörige Daten in die Navigationseigenschaften einer Entität laden kann:

- *Lazy Loading (verzögertes Laden)*. Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen. Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt zu mehreren Abfragen, die an die Datenbank gesendet – eine für die Entität selbst und eine jedes Mal, die Daten für die Entität beziehen abgerufen werden muss. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Eager Loading (vorzeitiges Laden)*. Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen. Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind. Sie geben eager Loading mit der `Include` Methode.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explizites Laden*. Dies ist ähnlich wie beim verzögerten Laden, mit dem Unterschied, dass Sie explizit im Code die verwandten Daten abgerufen werden; Dies erfolgt nicht automatisch beim Zugriff auf eine Navigationseigenschaft. Sie verwandte Daten manuell laden durch Abrufen der Objekt-Status-Manager-Eintrag für eine Entität und das Aufrufen der `Collection.Load` -Methode für Sammlungen oder `Reference.Load` Methode für die Eigenschaften, die eine einzelne Entität enthalten. (Im folgenden Beispiel, wenn Sie die Administrator-Navigationseigenschaft laden möchten ersetzen `Collection(x => x.Courses)` mit `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Da sie sofort der Abrufen von Eigenschaftswerten nicht lazy Loading und explizites Laden werden auch beide genannt *verzögertes Laden*.

Im Allgemeinen, wenn Sie wissen, benötigen Sie verwandte Daten für jede Entität, die abgerufen werden, eager Loading die beste Leistung bietet, da eine einzelne Abfrage, die an die Datenbank gesendet, in der Regel effizienter als separate Abfragen für jede abgerufene Entität ist. Angenommen Sie in den obigen Beispielen wird, dass jede Abteilung zehn Verwandte Kurse hat. Das Beispiel für eager Loading würde nur einen einzelnen (Join)-Abfrage und einem einzelnen Roundtrip zur Datenbank führen. Der lazy Loading und Beispiele für explizites Laden ergibt beide elf Abfragen und elf Roundtrips zur Datenbank. Die zusätzlichen Roundtrips zur Datenbank beeinträchtigen die Leistung besonders bei hoher Latenz.

In einigen Szenarien ist lazy Loading auf der anderen Seite effizienter. Eager Loading möglicherweise eine sehr komplexe Verknüpfung generieren, die SQL Server nicht effizient verarbeiten kann. Oder Sie verarbeiten können, Lazy Load möglicherweise besser ausgeführt werden, da eager Loading mehr Daten als benötigt abrufen würde, wenn Sie Navigationseigenschaften einer Entität, nur für einen Teil einer Reihe von Entitäten zugreifen müssen. Wenn die Leistung wichtig ist, empfiehlt es sich, die Leistung mit beiden Möglichkeiten zu testen, um die beste Wahl treffen zu können.

In der Regel verwenden Sie explizite Laden nur, wenn Sie lazy loading-aktiviert haben. Ein Szenario, wenn Sie lazy loading-aktivieren sollten, ist während der Serialisierung. Lazy Loading und Serialisierung nicht auch kombinieren, und Laden ist aktiviert, wenn Sie nicht sorgfältig, dass Sie letztlich bedeutend mehr Daten als beabsichtigt beim verzögerten Abfragen können. Serialisierung funktioniert im Allgemeinen durch den Zugriff auf jede Eigenschaft in einer Instanz eines Typs. Zugriff auf Eigenschaften wird lazy Loading ausgelöst, und die verzögerte geladenen Entitäten serialisiert werden. Der Serialisierungsprozess greift dann auf jede Eigenschaft der lazy geladenen Entitäten, und zwar mit möglicherweise sogar noch mehr lazy Loading und Serialisierung. Um diese runaway Kettenreaktion zu verhindern, aktivieren Sie lazy loading-deaktivieren, bevor Sie eine Entität zu serialisieren.

Der Datenbankkontext-Klasse führt die Lazy Load standardmäßig. Es gibt zwei Möglichkeiten, um die Deaktivierung von lazy Loading aus:

- Lassen Sie für bestimmte Eigenschaften, die `virtual` -Schlüsselwort, wenn Sie die Eigenschaft deklarieren.
- Legen Sie für alle Navigationseigenschaften `LazyLoadingEnabled` zu `false`. Beispielsweise können Sie den folgenden Code im Konstruktor der Kontextklasse nehmen: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Lazy Loading kann Code maskiert, für die Leistungsprobleme verursacht. Kann z. B., Code, der nicht geben eager oder expliziten Laden jedoch eine große Anzahl von Entitäten verarbeitet und verwendet mehrere Navigationseigenschaften in jeder Iteration sehr ineffizient (Dank der viele Roundtrips zur Datenbank). Eine Anwendung, die sich für die Entwicklung mit einer lokalen SQLServer führt möglicherweise Leistungsprobleme, wenn aufgrund der Latenzzeit und Lazy Load in Azure SQL-Datenbank verschoben. Profilieren von Datenbankabfragen mit einer realistischen testladevorgang können Sie bestimmen, wenn lazy Loading geeignet ist. Weitere Informationen finden Sie unter [die Enträtselung der Entity Framework-Strategien: Laden von verknüpften Daten](https://msdn.microsoft.com/magazine/hh205756.aspx) und [mithilfe von Entity Framework zum Verringern der Netzwerklatenz zu SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Erstellen Sie eine Indexseite "Kurse", zeigt Abteilungsname

Die `Course` Entität enthält eine Navigationseigenschaft, enthält die `Department` Entität des Fachbereichs, die der Kurs zugewiesen ist. Um den Namen der zugewiesenen Abteilung in einer Kursliste anzuzeigen, müssen Sie zum Abrufen der `Name` Eigenschaft aus der `Department` Entität, die in der `Course.Department` Navigationseigenschaft.

Erstellen Sie einen Controller mit dem Namen `CourseController` für die `Course` Entitätstyp mit den gleichen Optionen, die Sie für zuvor die `Student` Controller, wie in der folgenden Abbildung dargestellt (außer im Gegensatz zu der Abbildung ist die Context-Klasse im DAL-Namespace ist keine Modelle Namespace):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Open *Controllers\CourseController.cs* und sehen Sie sich die `Index` Methode:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der automatische Gerüstbau hat mithilfe der `Include`-Methode ein Eager Loading für die `Department`-Navigationseigenschaft angegeben.

Open *Views\Course\Index.cshtml* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Sie haben die folgenden Änderungen am eingerüsteten Code vorgenommen:

- Die Überschrift von geändert **Index** zu **Kurse**.
- Die Links für die Zeile verschoben nach links.
- Eine Spalte unter der Überschrift hinzugefügt **Anzahl** zeigt, dass die `CourseID` -Eigenschaftswert. (Standardmäßig werden nicht primären Schlüssel eingerüstet, da Sie normalerweise sie ohne Bedeutung für Endbenutzer sind. Allerdings in diesem Fall der Primärschlüssel sinnvoll ist und Sie sie anzeigen möchten.)
- Die letzte Spaltenüberschrift aus geändert **"DepartmentID"** (der Name der der Fremdschlüssel für die `Department` Entität) zu **Abteilung**.

Beachten Sie, dass für die letzte Spalte wird der eingerüstete Code zeigt die `Name` Eigenschaft der `Department` Entität, die in den geladen wird die `Department` Navigationseigenschaft:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Führen Sie die Seite (Wählen Sie die **Kurse** Registerkarte auf der Contoso University-Startseite) um die Liste mit den Abteilungsnamen anzuzeigen.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Erstellen Sie eine Indexseite "Dozenten", die Kurse und Registrierungen anzeigt

In diesem Abschnitt erstellen Sie einen Controller und zeigen Sie für die `Instructor` Entität, um die Indexseite für Dozenten anzuzeigen:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Auf dieser Seite werden verwandte Daten auf folgende Weise gelesen und angezeigt:

- Die Liste der Dozenten Zeigt verwandte Daten aus der `OfficeAssignment` Entität. Die `Instructor`- und `OfficeAssignment`-Entitäten stehen in einer 1:0..1-Beziehung zueinander. Verwenden Sie eager Loading für die `OfficeAssignment` Entitäten. Wie zuvor erläutert, ist Eager Loading in der Regel effizienter, wenn Sie die verwandten Daten für alle abgerufenen Zeilen der primären Tabelle benötigen. In diesem Fall sollten Sie die Office-Anweisungen für alle angezeigten Dozenten anzeigen.
- Wenn der Benutzer einen Dozenten, die im Zusammenhang auswählt `Course` Entitäten werden angezeigt. Die `Instructor`- und `Course`-Entitäten stehen in einer m:n-Beziehung zueinander. Verwenden Sie eager Loading für die `Course` Entitäten und den zugehörigen `Department` Entitäten. In diesem Fall kann Lazy Load effizienter sein, da Sie Kurse nur für den ausgewählten Dozenten benötigt. Dieses Beispiel zeigt jedoch, wie Eager Loading für Navigationseigenschaften in Entitäten in den Navigationseigenschaften verwendet wird.
- Wenn der Benutzer einen Kurs auswählt, verwandte Daten aus der `Enrollments` Entitätenmenge wird angezeigt. Die `Course`- und `Enrollment`-Entitäten stehen in einer 1:n-Beziehung zueinander. Fügen Sie das explizite Laden für `Enrollment` Entitäten und den zugehörigen `Student` Entitäten. (Explizites Laden nicht erforderlich, da lazy Loading aktiviert, jedoch wird veranschaulicht, wie Eager Loading.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Erstellen Sie ein Ansichtsmodell für "Instructor" Ansicht "Index"

Die Indexseite für "Instructor" zeigt die drei verschiedene Tabellen. Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält. Jede enthält Daten für eine der Tabellen.

In der *ViewModels* Ordner erstellen *InstructorIndexData.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Hinzufügen eines Stils für ausgewählte Zeilen

Zum Markieren der ausgewählten Zeilen benötigen Sie eine andere Hintergrundfarbe. Um einen Stil für diese Benutzeroberfläche bereitzustellen, fügen Sie folgenden hervorgehobenen Code im Abschnitt `/* info and errors */` in *Content\Site.css*, wie unten dargestellt:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Erstellen der Dozentencontroller und-Ansichten

Erstellen Sie eine `InstructorController` Controller wie in der folgenden Abbildung gezeigt:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Open *Controllers\InstructorController.cs* und Hinzufügen einer `using` -Anweisung für die `ViewModels` Namespace:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Der eingerüstete Code in die `Index` -Methode gibt eager Loading nur für die `OfficeAssignment` Navigationseigenschaft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ersetzen Sie die `Index` -Methode durch den folgenden Code, um weitere laden zugehöriger Daten, und fügen Sie ihn in das Ansichtsmodell:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Die Methode akzeptiert optionale Routendaten (`id`) und einen Abfragezeichenfolgen-Parameter (`courseID`) die ID-Werte, der den ausgewählten Dozenten und Kurses bereitstellen und alle erforderlichen Daten an die Ansicht übergibt. Die Parameter werden durch die **Auswählen**-Links auf der Seite bereitgestellt.

> [!TIP]
> 
> **Weiterleiten von Daten**
> 
> Routendaten sind Daten, die die modellbindung in ein URL-Segment, das in der Routingtabelle angegebene gefunden. Die Standardroute gibt z. B. `controller`, `action`, und `id` Segmente:
> 
> routes.MapRoute(  
>  Name: "Default",  
>  URL: "{Controller} / {Action} / {Id}",  
>  Standardwerte: new {Controller = "Home" Action = "Index", Id = UrlParameter.Optional}  
> );
> 
> In der folgenden URL ordnet die Standardroute `Instructor` als die `controller`, `Index` als die `action` und 1 der `id`; Hierbei handelt es sich um Datenwerte für die Route.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? CourseID = 2021" ist der Wert einer Abfragezeichenfolge. Der Modellbinder funktioniert auch, wenn Sie übergeben die `id` als Wert einer Abfragezeichenfolge:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Die URLs werden erstellt, indem `ActionLink` Anweisungen in der Razor-Ansicht. In den folgenden Code der `id` Parameter entspricht die Standardroute, daher `id` den Routendaten hinzugefügt wird.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> In den folgenden Code `courseID` ein Parameters in der Standardroute, stimmt nicht überein, sodass es als Abfragezeichenfolge hinzugefügt wird.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Der Code erstellt zuerst eine Instanz des Ansichtsmodells und fügt die Dozentenliste ein. Der Code gibt eager Loading für die `Instructor.OfficeAssignment` und `Instructor.Courses` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Die zweite `Include` Methode lädt, Kurse und für jeden Kurs, der geladen wird, ist dies der Fall ist eager Loading für die `Course.Department` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Wie bereits erwähnt, ist Eager Load ist nicht erforderlich, aber erfolgt, um die Leistung zu verbessern. Da die Ansicht immer erforderlich ist der `OfficeAssignment` Entität ist es effizienter, die in der gleichen Abfrage abgerufen. `Course` Entitäten sind erforderlich, wenn auf der Webseite ein Dozent ausgewählt wird, damit Sie eager Loading ist besser als das verzögerte Laden nur dann, wenn die Seite häufiger mit einem ausgewählten Kurs als ohne angezeigt wird.

Wenn eine ID für "Instructor" ausgewählt wurde, wird der ausgewählte Dozent aus der Liste der Dozenten im Ansichtsmodell abgerufen. Des Ansichtsmodells `Courses` Eigenschaft wird dann geladen, mit der `Course` Entitäten aus dieser "Instructor" `Courses` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Die `Where` -Methode gibt eine Auflistung, aber in diesem Fall die Kriterien, die nur eine einzige Methode zu übergeben `Instructor` Entität zurückgegeben wird. Die `Single` Methode konvertiert die Sammlung in eine einzelne `Instructor` -Entität, die Sie Zugriff auf die juristischen Person erhalten `Courses` Eigenschaft.

Sie verwenden die [einzelne](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) Methode für eine Sammlung aus, wenn Sie wissen, dass die Auflistung wird nur ein Element aufweisen. Die `Single` Methode löst eine Ausnahme aus, wenn die Auflistung, die an sie übergebenen leer ist oder wenn mehr als ein Element vorhanden ist. Eine Alternative ist die [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), womit einen Standardwert (`null` in diesem Fall), wenn die Auflistung leer ist. Allerdings in diesem Fall, die immer noch würde zu einer Ausnahme (versucht, finden Sie eine `Courses` Eigenschaft für eine `null` Verweis), und die Ausnahmemeldung würde die Ursache des Problems weniger deutlich angeben. Beim Aufrufen der `Single` -Methode, Sie können auch übergeben die `Where` Bedingung statt der `Where` Methode getrennt:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

anstelle von:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Wenn ein Kurs ausgewählt wurde, wird der ausgewählte Kurs aus der Kursliste im Ansichtsmodell abgerufen. Klicken Sie dann die des Ansichtsmodells `Enrollments` Eigenschaft wird geladen, mit der `Enrollment` Entitäten aus dieser Kurs die `Enrollments` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Ändern der Indexansicht "Instructor"

In *Views\Instructor\Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Sie haben die folgenden Änderungen am bestehenden Code vorgenommen:

- Die Modellklasse wurde zu `InstructorIndexData` geändert.
- Der Seitenname wurde von **Index** in **Dozenten** geändert.
- Die Spalten der Zeile-Link verschoben in der linken Seite.
- Entfernt die **"FullName"** Spalte.
- Hinzugefügt ein **Office** Spalte `item.OfficeAssignment.Location` nur, wenn `item.OfficeAssignment` nicht null ist. (Da dies eine 1: 0 (null)-oder-1-Beziehung ist, gibt es möglicherweise keine verknüpfte `OfficeAssignment` Entität.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Code hinzugefügt, die dynamisch hinzufügen `class="selectedrow"` auf die `tr` Element des ausgewählten Dozenten. Hiermit wird eine Hintergrundfarbe für die ausgewählte Zeile mithilfe der CSS-Klasse, die Sie zuvor erstellt haben. (Die `valign` Attribut wird in dem folgenden Tutorial nützlich sein, wenn Sie eine Spalte mit mehreren Zeile zur Tabelle hinzufügen.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Einen neuen `ActionLink` mit der Bezeichnung **wählen** unmittelbar vor den anderen Links in jeder Zeile, wodurch die ID des ausgewählten Dozenten zu sendende der `Index` Methode.

Führen Sie die Anwendung, und wählen Sie die **Dozenten** Registerkarte. Die Seite zeigt die `Location` Eigenschaft des zugehörigen `OfficeAssignment` Entitäten und eine leere Tabelle Zelle bei Nein, die im Zusammenhang `OfficeAssignment` Entität.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

In der *Views\Instructor\Index.cshtml* -Datei nach dem schließenden `table` Element (am Ende der Datei), fügen Sie folgenden hervorgehobenen Code hinzu. Dies zeigt eine Liste der Kurse, die im Zusammenhang mit einem Dozenten stehen, wenn ein Dozent ausgewählt wird.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Dieser Code liest die `Courses`-Eigenschaft des Ansichtsmodells, um eine Kursliste anzuzeigen. Es bietet auch eine `Select` Hyperlink, der die ID des ausgewählten Kurs, sendet der `Index` Aktionsmethode.

> [!NOTE]
> Die *CSS* Datei wird vom Browser zwischengespeichert. Wenn die Änderungen nicht angezeigt wird, wenn Sie die Anwendung ausgeführt haben, führen Sie eine Aktualisierung schwierige (halten Sie STRG gedrückt, während Sie auf die **aktualisieren** Schaltfläche oder drücken Sie STRG + F5).


Führen Sie die Seite, und wählen Sie einen Dozenten. Jetzt sehen Sie ein Raster, das die dem Dozenten zugewiesenen Kurse anzeigt. Sie sehen auch den Namen der zugewiesenen Abteilung für jeden Kurs.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Fügen Sie den folgenden Code hinzu, nachdem Sie den Codeblock hinzugefügt haben. Dies zeigt eine Liste der Studenten an, die im Kurs registriert sind, wenn dieser Kurs ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Dieser Code liest die `Enrollments` -Eigenschaft des Ansichtsmodells, um eine Liste von Teilnehmern anzuzeigen, die in diesem Kurs registrierten.

Führen Sie die Seite, und wählen Sie einen Dozenten. Wählen Sie dann einen Kurs aus, um die Liste der registrierten Studenten und deren Noten einzusehen.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Hinzufügen von expliziten Laden

Open *InstructorController.cs* und sehen Sie sich wie die `Index` Methode ruft die Liste der Registrierungen für einen ausgewählten Kurs:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Wenn Sie die Liste der Dozenten abgerufen haben, Sie angegeben haben eager Loading für die `Courses` Navigationseigenschaft und für die `Department` Eigenschaft für jeden Kurs. Platzieren Sie Sie der `Courses` Sammlung im Ansichtsmodell, und nachdem auf Sie zugreifen, die `Enrollments` Navigationseigenschaft von einer Entität in der Auflistung. Da Sie eager Loading für nicht angegeben haben die `Course.Enrollments` Navigationseigenschaft, die Daten aus dieser Eigenschaft werden auf der Seite als Ergebnis der Lazy Load angezeigt.

Wenn Sie lazy Loading deaktiviert, ohne den Code auf andere Weise, die `Enrollments` Eigenschaft würde unabhängig davon, wie viele Registrierungen, die die Vorgehensweise bekannt, ob null sein. In diesem Fall beim Laden der `Enrollments` -Eigenschaft müssen entweder eager Loading oder expliziten Laden angeben. Sie haben bereits Eager Load Vorgehensweise gesehen. Um ein Beispiel für explizites Laden anzuzeigen, ersetzen die `Index` Methode durch den folgenden Code, der explizit lädt die `Enrollments` Eigenschaft. Der Code geändert werden hervorgehoben.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Nach dem Abrufen des ausgewählten `Course` Entität, der neue Code lädt explizit des Kurses `Enrollments` Navigationseigenschaft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Anschließend er explizit jedes lädt `Enrollment` Entität die verbundene `Student` Entität:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Beachten Sie, die Sie verwenden die `Collection` Methode zum Laden einer Auflistungseigenschaft, aber für eine Eigenschaft, die nur eine einzelne Entität enthält, verwenden Sie die `Reference` Methode. Sie können die Indexseite für "Instructor" jetzt ausführen, und Sie sehen keinen Unterschied in der Anzeige auf der Seite auf, obwohl Sie geändert haben, wie die Daten abgerufen werden.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle drei Möglichkeiten (lazy, eager und explizite) zum Laden von verknüpfter Daten in die Navigationseigenschaften verwendet. Das nächste Tutorial zeigt die Aktualisierung verwandter Daten.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
