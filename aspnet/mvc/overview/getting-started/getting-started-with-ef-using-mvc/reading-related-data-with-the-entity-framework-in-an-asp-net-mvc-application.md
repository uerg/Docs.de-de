---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Lesen von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft Docs"
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7a74d01f306abeeac5ac28c942f03001e0fe00f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Lesen-bezogene Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm abgeschlossen Sie das Datenmodell "School". In diesem Lernprogramm werden Sie lesen und Anzeigen verknüpfter Daten – d. h. die Daten, die das Entity Framework in Navigationseigenschaften lädt.

Die folgenden Abbildungen zeigen den Seiten, arbeiten Sie mit.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Verzögerte Eager und explizite Laden von verknüpften Daten

Es gibt mehrere Möglichkeiten, das Entity Framework in die Navigationseigenschaften einer Entität verknüpfte Daten geladen werden können:

- *Verzögertes Laden*. Wenn die Entität zuerst gelesen wird, ist nicht verbundene Daten abgerufen. Allerdings werden beim ersten Versuch, auf eine Navigationseigenschaft, für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt zu mehreren Abfragen an die Datenbank gesendet – eine für die Entität selbst und eine jedes Mal, die Daten, die für die Entität verknüpft abgerufen werden muss. Die `DbContext` Klasse verzögertes Laden sind standardmäßig aktiviert. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Unverzüglichem Laden*. Wenn die Entität gelesen wird, werden darin verknüpfte Daten abgerufen. Dies führt normalerweise zu einer einzelnen Join-Abfrage, die alle Daten abruft, die erforderlich ist. Geben Sie unverzüglichem Laden mithilfe der `Include` Methode.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explizites Laden*. Dies gleicht dem verzögerten Laden, mit dem Unterschied, dass Sie explizit die verknüpften Daten im Code abrufen; Es ist nicht automatisch ausgeführt, wenn Sie Zugriff auf eine Navigationseigenschaft. Sie laden verknüpfte Daten manuell, indem beim Abrufen der Objekt-Eintrag für die Status-Manager für eine Entität und der Aufruf der [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) Methode für Sammlungen oder die [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) Methode für die Eigenschaften, enthalten ein einzelne Entität. (Im folgenden Beispiel, mussten Sie zum Laden der Navigationseigenschaft Administrator ersetzen `Collection(x => x.Courses)` mit `Reference(x => x.Administrator)`.) In der Regel verwenden Sie explizite Laden nur, wenn Sie lazy loading-aktiviert haben.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Da sie nicht die Eigenschaftswerte sofort abrufen, verzögertes Laden und explizites Laden sind auch beide genannt *verzögertes Laden*.

### <a name="performance-considerations"></a>Überlegungen zur Leistung

Wenn Sie, die Sie für jede Entität abgerufen verknüpfte Daten benötigen wissen, bietet eager loading, häufig die beste Leistung, da eine einzelne Abfrage, die an die Datenbank gesendet, in der Regel effizienter als separate Abfragen für jede Entität abgerufen wird. Angenommen Sie in den obigen Beispielen wird, dass jede Abteilung zehn Verwandte Kurse gelten. Im Beispiel unverzüglichem Laden führt in nur einer einzigen (Join)-Abfrage und einem einzelnen Roundtrip mit der Datenbank. Das verzögerte Laden und explizites Laden Beispiele für ergibt beide elf Abfragen und elf Roundtrips zur Datenbank. Zusätzliche Roundtrips zur Datenbank sind vor allem auf die Leistung beeinträchtigen, wenn Latenz hoch ist.

In einigen Szenarien ist das verzögertes Laden andererseits, effizienter. Unverzüglichem Laden kann es sich um eine sehr komplexe Verknüpfung erzeugt werden kann, führen, der SQL Server effizient verarbeiten kann. Oder wenn Sie den Zugriff auf eine Entität Navigationseigenschaften nur für einen Teil einer Reihe von Entitäten müssen Sie verarbeiten möchten, verzögertes Laden möglicherweise eine bessere Leistung, da unverzüglichem Laden mehr Daten als benötigt abrufen würde. Wenn die Leistung kritisch ist, empfiehlt es sich zum Testen von Leistung beides Möglichkeiten, um die beste Wahl vornehmen zu können.

Verzögertes Laden kann Code maskieren, die Leistungsprobleme verursacht. Beispielsweise ist die Code, der eager oder expliziten Laden nicht angeben, aber eine große Anzahl von Entitäten verarbeitet und verwendet mehrere Navigationseigenschaften in jeder Iteration (aufgrund von viele Roundtrips zur Datenbank) möglicherweise sehr ineffizient. Eine Anwendung, die auch in der Entwicklung mit einer lokalen SQL Servers führt möglicherweise Leistungsprobleme, wenn aufgrund der Latenzzeit und lazy Loading zu Azure SQL-Datenbank verschoben. Profilerstellung für die Datenbankabfragen mit einer realistischen testladevorgang hilft zu bestimmen, ob das verzögertes Laden geeignet ist. Weitere Informationen finden Sie unter [Demystifying Entity Framework-Strategien: Laden von verknüpften Daten](https://msdn.microsoft.com/magazine/hh205756.aspx) und [mit dem Entity Framework zum Verringern der Netzwerklatenz in SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Das verzögertes Laden vor der Serialisierung deaktivieren

Wenn Sie lazy loading-aktivierten während der Serialisierung lassen, können Sie schließlich Abfragen deutlich mehr Daten als beabsichtigt. Serialisierung funktioniert im Allgemeinen durch den Zugriff auf jede Eigenschaft in einer Instanz eines Typs. Eigenschaftenzugriff löst das verzögertes Laden, und die verzögerten geladenen Entitäten serialisiert werden. Der Serialisierungsprozess greift dann auf jede Eigenschaft des verzögerten geladene Entitäten verursacht möglicherweise noch mehr verzögertes Laden und Serialisierung. Um diese runaway Kettenreaktion zu verhindern, aktivieren Sie lazy loading-deaktivieren, bevor Sie eine Entität serialisieren.

Serialisierung kann auch durch die Webdienstproxy-Klassen, die das Entity Framework verwendet, kompliziert, wie beschrieben in der [erweiterte Szenarien der Lernprogramm](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Eine Möglichkeit zur Vermeidung von Problemen der Serialisierung wird zum Serialisieren von datenübertragungsobjekte (DTOs) anstelle von Entitätsobjekten, entsprechend der [mithilfe des Web-API mit Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) Lernprogramm.

Wenn Sie DTOs nicht verwenden, können Sie diese deaktivieren verzögerten Laden und vermeiden Probleme mit Proxy von [deaktivieren Proxyerstellung](https://msdn.microsoft.com/data/jj592886.aspx).

Hier sind einige andere [Möglichkeiten, deaktivieren Sie das verzögertes Laden](https://msdn.microsoft.com/data/jj574232):

- Lassen Sie für bestimmte Navigationseigenschaften, die `virtual` -Schlüsselwort, wenn Sie die Eigenschaft deklarieren.
- Legen Sie für alle Navigationseigenschaften `LazyLoadingEnabled` zu `false`, platzieren Sie den folgenden Code im Konstruktor der Context-Klasse: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Erstellen Sie eine Seite Kurse, zeigt Abteilungsnamen

Die `Course` Entität enthält, eine Navigationseigenschaft, die enthält die `Department` Entität der Abteilung, die der Kurs zugewiesen ist. Um den Namen der zugewiesenen Abteilung in einer Liste von Kurse anzuzeigen, müssen Sie die `Name` Eigenschaft aus der `Department` Entität, die in der `Course.Department` Navigationseigenschaft.

Erstellen Sie einen Controller mit dem Namen `CourseController` (nicht CoursesController) für die `Course` Entitätstyp mit denselben Optionen für die **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** Scaffolder, die Sie zuvor für die `Student` Controller, wie in der folgenden Abbildung gezeigt:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Open *Controllers\CourseController.cs* und sehen Sie sich die `Index` Methode:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der automatische Gerüstbau angegeben unverzüglichem Laden für die `Department` Navigationseigenschaft mithilfe der `Include` Methode.

Open *Views\Course\Index.cshtml* , und Ersetzen Sie den Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Sie haben die folgenden Änderungen an der scaffolded Code vorgenommen:

- Die Überschrift aus geändert **Index** auf **Kurse**.
- Hinzugefügt eine **Anzahl** Spalte, die zeigt die `CourseID` Eigenschaftswert. Primärschlüssel werden nicht standardmäßig Gerüstbau, da normalerweise sie Endbenutzern bedeutungslos sind. Allerdings in diesem Fall der Primärschlüssel sinnvoll ist und Sie sie anzeigen möchten.
- Verschoben der **Abteilung** Spalte auf der rechten Seite und die Spaltenüberschrift geändert. Die Scaffolder ordnungsgemäß anzeigen möchten die `Name` Eigenschaft aus der `Department` Entität jedoch hier in der Seite Kurs den Spaltenüberschrift muss **Abteilung** statt **Namen**.

Beachten Sie, dass für die Department-Spalte der scaffolded Code zeigt die `Name` Eigenschaft von der `Department` Entität, die geladen wird die `Department` Navigationseigenschaft:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Führen Sie die Seite (Wählen Sie die **Kurse** Registerkarte auf der Startseite des Contoso-University) um die Liste mit den Abteilungsnamen anzuzeigen.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Erstellen Sie eine Lehrkräfte-Seite, in der Kurse und Registrierung angezeigt.

In diesem Abschnitt Sie erstellen einen Controller und zeigen Sie für die `Instructor` Entität, um die Seite Lehrkräfte anzuzeigen:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Auf dieser Seite liest und zeigt verknüpfte Daten auf folgende Weise:

- Die Liste der Lehrkräfte zeigt verknüpfte Daten aus der `OfficeAssignment` Entität. Die `Instructor` und `OfficeAssignment` Entitäten sind in einer 1: 0 (null)-oder-1-Beziehung. Verwenden Sie unverzüglichem Laden für die `OfficeAssignment` Entitäten. Wie zuvor erläutert, ist die unverzüglichem Laden in der Regel effizienter, wenn Sie die verknüpften Daten für alle abgerufenen Zeilen von der primären Tabelle benötigen. In diesem Fall möchten Sie Office-Zuweisungen für alle angezeigten Lehrkräfte anzuzeigen.
- Wenn der Benutzer wählt einen Kursleiter, die im Zusammenhang `Course` Entitäten werden angezeigt. Die `Instructor` und `Course` Entitäten sind in einer m: n-Beziehung. Verwenden Sie unverzüglichem Laden für die `Course` Entitäten und die zugehörigen `Department` Entitäten. In diesem Fall kann das verzögertes Laden effizienter sein, da Sie nur für den ausgewählten Dozenten Kurse benötigen. Dieses Beispiel zeigt jedoch mit unverzüglichem Laden für Navigationseigenschaften innerhalb von Entitäten, die selbst in den Navigationseigenschaften werden.
- Wenn der Benutzer einen Kurs auswählt, beziehen Sie Daten aus der `Enrollments` Entitätenmenge wird angezeigt. Die `Course` und `Enrollment` Entitäten sind in einer 1: n-Beziehung. Fügen Sie das explizite Laden für `Enrollment` Entitäten und die zugehörigen `Student` Entitäten. (Explizites Laden nicht erforderlich, da verzögertes Laden ist aktiviert, aber dies veranschaulicht das explizite laden.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Erstellen Sie ein Ansichtsmodell für die Indexansicht Dozenten

Die Seite "Lehrkräfte" zeigt drei verschiedene Tabellen. Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält jede mit den Daten für eine der Tabellen.

In der *ViewModels* Ordner erstellen *InstructorIndexData.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Erstellen Sie die Instructor-Controller und Ansichten

Erstellen Sie eine `InstructorController` (nicht InstructorsController)-Controller mit EF Lese-/schreibaktionen wie in der folgenden Abbildung gezeigt:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Open *Controllers\InstructorController.cs* und Hinzufügen einer `using` -Anweisung für die `ViewModels` Namespace:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Der scaffolded Code in der `Index` Methode gibt unverzüglichem Laden nur für die `OfficeAssignment` Navigationseigenschaft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ersetzen Sie die `Index` Methode durch den folgenden Code aus, um die zusätzliche Last-bezogene Daten, und fügen Sie ihn in das Modell anzeigen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die Methode akzeptiert optionale Routendaten (`id`) und einen Abfragezeichenfolgenparameter (`courseID`), geben Sie die ID-Werte der ausgewählten Kursleiter und ausgewählten Kurs, und übergibt alle erforderlichen Daten an die Ansicht. Die Parameter bereitgestellt werden, indem die **wählen** links auf der Seite.

Der Code wird zuerst Erstellen einer Instanz des Modells anzeigen und darin die Liste der Lehrkräfte einfügen. Der Code gibt unverzüglichem Laden für die `Instructor.OfficeAssignment` und `Instructor.Courses` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Die zweite `Include` Methode lädt Kurse und für jeden Kurs, die geladen wird, ist dies der Fall ist unverzüglichem Laden für die `Course.Department` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Wie bereits erwähnt, unverzüglichem Laden ist nicht erforderlich, jedoch wird durchgeführt, um die Leistung zu verbessern. Da die Sicht muss die `OfficeAssignment` Entität, es ist jedoch effizienter, die in derselben Abfrage abgerufen. `Course`Entitäten sind erforderlich, wenn auf der Webseite ein Kursleiter ausgewählt ist, damit unverzüglichem Laden ist besser als lazy loading, nur dann, wenn die Seite mit einem Kurs ausgewählt als ohne häufiger angezeigt wird.

Instructor-ID wurde ausgewählt, wird der ausgewählte Kursleiter aus der Liste der Kursleiter das Ansichtsmodell abgerufen. Des Ansichtsmodells `Courses` Eigenschaft wird dann geladen, mit der `Course` Entitäten aus dieser Dozenten `Courses` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `Where` -Methode gibt eine Auflistung, aber in diesem Fall die Kriterien, die nur eine einzelne Methode zu übergeben `Instructor` Entität zurückgegeben wird. Die `Single` -Methode konvertiert die Auflistung in einem einzelnen `Instructor` Entität, die Sie Zugriff auf diese Entität gewährt `Courses` Eigenschaft.

Verwenden Sie die [einzelne](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) Methode auf eine Auflistung, wenn Sie wissen, dass die Auflistung wird nur ein Element verfügen. Die `Single` Methode löst eine Ausnahme aus, wenn die übergebene Auflistung leer ist oder wenn mehr als ein Element vorhanden ist. Ist eine Alternative [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), dem einen Standardwert zurückgegeben (`null` in diesem Fall), wenn die Auflistung leer ist. Jedoch in diesem Fall, da immer noch ansonsten eine Ausnahme (aus beim Suchen nach einer `Courses` Eigenschaft auf einen `null` Verweis), und die Ausnahmemeldung würde weniger deutlich die Ursache des Problems angeben. Beim Aufrufen der `Single` -Methode, können Sie auch übergeben der `Where` Bedingung statt der `Where` Methode getrennt:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

anstelle von:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Als Nächstes wird ein Kurs ausgewählt wurde, der ausgewählte Kurs aus der Liste der Kurse in das Ansichtsmodell abgerufen. Klicken Sie dann die des Ansichtsmodells `Enrollments` Eigenschaft geladen wird, mit der `Enrollment` Entitäten aus dieser Kurs `Enrollments` Navigationseigenschaft.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Ändern Sie die Indexansicht Dozenten

In *Views\Instructor\Index.cshtml*, den Code durch den folgenden Code ersetzen. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Sie haben die folgenden Änderungen an den vorhandenen Code vorgenommen:

- Die Modellklasse, um geändert `InstructorIndexData`.
- Der Seitenname von geändert **Index** auf **Lehrkräfte**.
- Hinzugefügt ein **Office** Spalte `item.OfficeAssignment.Location` nur, wenn `item.OfficeAssignment` ist ungleich null. (Da dies eine 1: 0 (null)-oder-1-Beziehung ist, gibt es möglicherweise keiner verknüpften `OfficeAssignment` Entität.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Code hinzugefügt, die dynamisch hinzufügen `class="success"` auf die `tr` Element des ausgewählten Kursleiter. Dadurch wird eine Hintergrundfarbe für die ausgewählte Zeile mit einem [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) Klasse. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Ein neues hinzugefügt `ActionLink` mit der Bezeichnung **wählen** unmittelbar vor der Links von anderen in jeder Zeile, die die ausgewählte Instructor-ID, die zu sendende verursacht die `Index` Methode.

Führen Sie die Anwendung, und wählen Sie die **Lehrkräfte** Registerkarte. Die Seite zeigt die `Location` Eigenschaft verwandter `OfficeAssignment` Entitäten und eine leere Tabelle Zelle bei Nein, die im Zusammenhang `OfficeAssignment` Entität.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

In der *Views\Instructor\Index.cshtml* Datei, nach dem schließenden `table` Element (am Ende der Datei), fügen Sie folgenden Code. Dieser Code zeigt eine Liste der Kurse, die im Zusammenhang mit einen Kursleiter beim ein Kursleiter ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Dieser Code liest die `Courses` Eigenschaft des Modells anzeigen, das eine Liste der Kurse anzeigen. Sie bietet außerdem eine `Select` Link, der die ID der ausgewählten Kurs, sendet der `Index` Aktionsmethode.

Führen Sie die Seite, und wählen Sie einen Kursleiter. Jetzt sehen Sie ein Raster mit den für den ausgewählten Kursleiter zugewiesene Kurse zeigt an, und jeder Kurs Sie finden Sie unter den Namen der zugewiesenen Abteilung.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fügen Sie nachdem der Codeblock, den Sie gerade hinzugefügt haben den folgenden Code ein. Dadurch wird eine Liste der Studenten, die registriert werden in einen Kurs, wenn dieser Kurs ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Dieser Code liest die `Enrollments` Eigenschaft des Modells Ansicht um eine Liste der Schüler anzuzeigen, die in den Kurs registriert sind.

Führen Sie die Seite, und wählen Sie einen Kursleiter. Wählen Sie dann einen Kurs, finden in der Liste der registrierten Studenten und deren Qualitäten.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Explizites Laden hinzufügen

Open *InstructorController.cs* und suchen Sie nach, wie die `Index` Methode ruft die Liste der Registrierung für einen ausgewählten Kurs:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Wenn Sie die Liste der Lehrkräfte abgerufen, angegebene unverzüglichem Laden für die `Courses` Navigationseigenschaft und für die `Department` Eigenschaft jeder natürlich. Sie Sie platzieren der `Courses` Sammlung das Ansichtsmodell und nachdem Sie zugreifen, die `Enrollments` Navigationseigenschaft von einer Entität in der Auflistung. Da Sie nicht unverzüglichem Laden für angegeben haben die `Course.Enrollments` Navigationseigenschaft, die Daten aus dieser Eigenschaft auf der Seite aufgrund einer verzögerten Laden angezeigt werden.

Wenn Sie das verzögertes Laden deaktiviert, ohne den Code auf andere Weise ändern die `Enrollments` Eigenschaft würde unabhängig davon, wie viele Registrierungen in Kurs Wirklichkeit null sein. In diesem Fall wird beim Laden der `Enrollments` -Eigenschaft, müssten Sie unverzüglichem Laden oder explizites Laden angeben. Sie haben bereits gesehen, wie unverzüglichem Laden ausgeführt werden. Um ein Beispiel für explizite Laden angezeigt wird, ersetzen die `Index` Methode durch den folgenden Code, die explizit lädt die `Enrollments` Eigenschaft. Der Code geändert werden hervorgehoben.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Nach dem Abrufen der ausgewählten `Course` Entität, der neue Code lädt explizit des Kurses `Enrollments` Navigationseigenschaft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Anschließend er explizit jedes lädt `Enrollment` Entität die verbundene `Student` Entität:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Beachten Sie, mit denen Sie die `Collection` -Methode zum Laden von Auflistungseigenschaft, aber für eine Eigenschaft, die nur eine Entität enthält, verwenden Sie die `Reference` Methode.

Die Instructor-Indexseite jetzt ausführen und keinen Unterschied in der Anzeige auf der Seite wird angezeigt, obwohl Sie geändert haben, wie die Daten abgerufen werden.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle drei Arten (lazy eager und explizite) zum Laden von verknüpfter Daten in den Navigationseigenschaften verwendet werden. In den nächsten Lernprogrammen erfahren Sie, wie verknüpfte Daten aktualisiert werden.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Weiter](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
