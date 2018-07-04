---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lesen verwandter Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung (6 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dde9d5022823b252e59949144e3021a53c0bdd3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371670"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Lesen verwandter Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung (6 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


In vorherigen Tutorial in Sie verwandte Daten angezeigt. In diesem Tutorial werden verwandte Daten aktualisiert werden. Für die meisten Beziehungen kann dies erfolgen durch die entsprechenden Felder mit Fremdschlüsseln aktualisieren. Für m: n Beziehungen nicht Entity Framework die verknüpften Tabelle direkt verfügbar machen, daher müssen Sie explizit hinzufügen und Entfernen von Entitäten aus die entsprechenden Navigationseigenschaften.

Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Anpassen der Seiten „Erstellen“ und „Bearbeiten“ für Kurse

Wenn eine neue Kursentität erstellt wird, muss diese in Beziehung zu einer vorhandenen Abteilung stehen. Um dies zu vereinfachen, enthält der Gerüstcode Controllermethoden und Ansichten zum „Erstellen“ und „Bearbeiten“, die eine Dropdownliste enthalten, aus denen der Fachbereich ausgewählt werden kann. Im Dropdown-Liste wird die `Course.DepartmentID` Fremdschlüsseleigenschaft und das ist alles in Entity Framework benötigt, um das Laden der `Department` Navigationseigenschaft mit dem entsprechenden `Department` Entität. Verwenden Sie den Gerüstcode, aber nehmen Sie kleine Änderungen vor, um die Fehlerbehandlung hinzuzufügen und die Dropdownliste zu sortieren.

In *CourseController.cs*, löschen Sie die vier `Edit` und `Create` Methoden und durch den folgenden Code ersetzen:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Die `PopulateDepartmentsDropDownList` Methode ruft eine Liste aller Abteilungen, die nach Namen sortiert ab, erstellt eine `SelectList` Auflistung eine Dropdown-Liste, und übergibt diese Auflistung an die Ansicht in eine `ViewBag` Eigenschaft. Die Methode akzeptiert den optionalen `selectedDepartment`-Parameter, über den der Code das Element angeben kann, das ausgewählt wird, wenn die Dropdownliste gerendert wird. Die Ansicht übergibt den Namen `DepartmentID` zu [der `DropDownList` Helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), und klicken Sie dann das Hilfsprogramm weiß, gesucht werden soll, die `ViewBag` Objekt für eine `SelectList` mit dem Namen `DepartmentID`.

Die `HttpGet` `Create` Methodenaufrufe der `PopulateDepartmentsDropDownList` -Methode ohne das ausgewählte Element festlegen, da für einen neuen Kurs die Abteilung noch nicht bekannt ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Die `HttpGet` `Edit` -Methode legt das ausgewählte Element anhand der ID des Fachbereichs, die bereits den bearbeiteten Kurs zugewiesen ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Die `HttpPost` -Methoden für `Create` und `Edit` enthalten außerdem Code, der das ausgewählte Element festlegt, wenn sie die Seite nach einem Fehler erneut:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Dieser Code stellt sicher, dass es sich bei, wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, beliebige Abteilung ausgewählt wurde erhalten bleibt.

In *Views\Course\Create.cshtml*, fügen Sie den hervorgehobenen Code um ein neues Feld für die Kursnummer vor dem Erstellen der **Titel** Feld. Wie in einem früheren Tutorial erläutert wird, werden nicht standardmäßig Primärschlüsselfelder Gerüst primären Schlüssels ist allerdings sinnvoll ist, sollten Sie die Benutzer zur Eingabe der Schlüssel-Wert kann.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, und *Views\Course\Details.cshtml*, einen Feld für die Kursnummer vor dem Hinzufügen der **Titel**  Feld. Da es sich um den primären Schlüssel handelt, wird angezeigt, aber es kann nicht geändert werden.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Führen Sie die **erstellen** Seite (auf der Indexseite des Kurses angezeigt, und klicken Sie auf **neu erstellen**), und geben Sie Daten für einen neuen Kurs:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Klicken Sie auf **Erstellen**. Die Indexseite des Kurses wird mit den neuen Kurs zur Liste hinzugefügt. Der Fachbereichsname in der Indexseitenliste wurde der Navigationseigenschaft entnommen und deutet darauf hin, dass die Beziehung ordnungsgemäß festgelegt wurde.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Führen Sie die **bearbeiten** Seite (auf der Indexseite des Kurses angezeigt, und klicken Sie auf **bearbeiten** an einem Kurs).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ändern Sie die Daten auf der Seite, und klicken Sie auf **Speichern**. Die Indexseite des Kurses wird mit den aktualisierten Kursdaten angezeigt.

## <a name="adding-an-edit-page-for-instructors"></a>Hinzufügen einer Seite "Bearbeiten" für Dozenten

Bei der Bearbeitung eines Dozentendatensatzes sollten Sie auch die Bürozuweisung des Dozenten aktualisieren. Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität, was bedeutet, müssen Sie die folgenden Situationen behandeln:

- Wenn der Benutzer die bürozuweisung löscht und ursprünglich einen Wert vorhanden, müssen Sie diese entfernen und löschen Sie die `OfficeAssignment` Entität.
- Wenn der Benutzer eine gibt aus, und diese ursprünglich leer war, müssen Sie ein neues erstellen `OfficeAssignment` Entität.
- Wenn der Benutzer den Wert einer bürozuweisung ändert, müssen Sie den Wert in einer vorhandenen ändern `OfficeAssignment` Entität.

Open *InstructorController.cs* und sehen Sie sich die `HttpGet` `Edit` Methode:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Der eingerüstete Code hier nicht Ihren vorstellungen. Ist es einrichten, bis Daten für eine Dropdown-Liste, aber Sie müssen Sie ein Textfeld ist. Ersetzen Sie diese Methode, mit dem folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Dieser Code löscht die `ViewBag` Anweisung und fügt Sie eager Loading für die zugeordnete `OfficeAssignment` Entität. Kann nicht ausgeführt werden eager Loading mit der `Find` -Methode, also die `Where` und `Single` Methoden werden stattdessen verwendet, um den Dozenten zu wählen.

Ersetzen Sie die `HttpPost` `Edit` Methode durch den folgenden Code. Updates für Office-Zuweisung behandelt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Der Code führt Folgendes aus:

- Ruft die aktuelle Entität `Instructor` von der Datenbank über Eager Loading für die Navigationseigenschaft `OfficeAssignment` ab. Dies ist identisch mit was Sie in haben der `HttpGet` `Edit` Methode.
- Aktualisiert die abgerufene Entität `Instructor` mit Werten aus der Modellbindung. Die [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) verwendete Überladung ermöglicht es Ihnen, *Whitelist* die Eigenschaften, die Sie einschließen möchten. Dies verhindert, dass zu viele Angaben, wie unter [im zweiten Artikel](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Wenn der Bürostandort leer ist, wird die `Instructor.OfficeAssignment` Eigenschaft auf null, damit die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht werden.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Speichert die Änderungen in der Datenbank.

In *Views\Instructor\Edit.cshtml*, nachdem die `div` Elemente für die **Hire Date** Feld Hinzufügen eines neuen Felds zum Bearbeiten der Office-Speicherort:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Führen Sie die Seite (Wählen Sie die **Dozenten** Registerkarte, und klicken Sie dann auf **bearbeiten** für einen Dozenten). Ändern Sie den **Standort des Büros**, und klicken Sie auf **Speichern**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Hinzufügen von Kurszuweisungen dem Dozenten bearbeiten Seite

Dozenten können eine beliebige Anzahl von Kursen unterrichten. Jetzt soll die Dozentenseite „Bearbeiten“ verbessert werden, indem es ermöglicht wird, Kurszuweisungen über eine Reihe von Kontrollkästchen zu verändern. Dies wird im folgenden Screenshot dargestellt:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Die Beziehung zwischen der `Course` und `Instructor` Entitäten ist m: n-, was bedeutet, Sie müssen keinen direkten Zugriff auf die Jointabelle. Stattdessen werden Sie hinzufügen und Entfernen von Entitäten in und aus der `Instructor.Courses` Navigationseigenschaft.

Die Benutzeroberfläche, über die Sie ändern können, welchen Kursen ein Dozent zugewiesen ist, besteht aus einer Reihe von Kontrollkästchen. Für jeden Kurs in der Datenbank wird ein Kontrollkästchen angezeigt. Die Kontrollkästchen, denen der Dozent zu diesem Zeitpunkt zugewiesen ist, sind aktiviert. Der Benutzer kann Kontrollkästchen aktivieren oder deaktivieren, um Kurszuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer wäre, würde wahrscheinlich eine andere Methode für das Darstellen der Daten in der Ansicht verwenden möchten, aber verwenden Sie die gleiche Methode zum Bearbeiten von Navigationseigenschaften zum Erstellen oder Löschen von Beziehungen.

Verwenden Sie eine Ansichtsmodellklasse, um Daten für die Ansicht bereitzustellen, um eine Liste mit Kontrollkästchen zu erstellen. Erstellen Sie *Schoolviewmodels* in die *ViewModels* Ordner, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In *InstructorController.cs*, ersetzen Sie die `HttpGet` `Edit` Methode durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Über den Code wird für die `Courses`-Navigationseigenschaft Eager Loading hinzugefügt und die neue `PopulateAssignedCourseData`-Methode aufgerufen, um über die Ansichtsmodellklasse `AssignedCourseData` Informationen für das Kontrollkästchenarray zur Verfügung zu stellen.

Der Code in die `PopulateAssignedCourseData` -Methode liest alle `Course` Modellklasse für Entitäten, um eine Liste der Kurse, die mithilfe der Ansicht zu laden. Für jeden Kurs überprüft der Code, ob dieser in der `Courses`-Navigationseigenschaft des Dozenten vorhanden ist. Um effiziente Suche nach der Erstellung überprüfen, ob ein Kurs dem Dozenten zugewiesen ist, die dem Dozenten zugewiesene Kurse abgelegt sind eine [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) Auflistung. Die `Assigned` -Eigenschaftensatz auf `true` Kurse der Dozent zugewiesen ist. Die Ansicht verwendet diese Eigenschaft, um zu bestimmen, welche Kontrollkästchen als aktiviert angezeigt werden sollen. Zum Schluss wird die Liste übergeben, an die Ansicht in einem `ViewBag` Eigenschaft.

Fügen Sie als nächstes den Code hinzu, der ausgeführt wird, wenn der Benutzer auf **Speichern** klickt. Ersetzen Sie die `HttpPost` `Edit` Methode durch den folgenden Code, der eine neue Methode aufruft, die aktualisiert die `Courses` Navigationseigenschaft der `Instructor` Entität. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Da die Sicht eine Auflistung von nicht `Course` Entitäten die, die modellbindung kein automatisches update der `Courses` Navigationseigenschaft. Anstatt die modellbindung die Navigationseigenschaft Kurse zu aktualisieren, haben Sie Gelegenheit, die in der neuen `UpdateInstructorCourses` Methode. Aus diesem Grund müssen Sie die `Courses`-Eigenschaft von der Modellbindung ausschließen. Dies erfordert keine Änderungen an der aufrufende Code [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) da Sie verwenden die *Whitelists* überladen und `Courses` nicht in der Liste enthalten ist.

Wenn keine Kontrollkästchen aktiviert ist, wird der Code in `UpdateInstructorCourses` initialisiert die `Courses` Navigationseigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Der Code führt dann eine Schleife durch alle Kurse in der Datenbank aus und überprüft jeden Kurs im Hinblick auf die Kurse, die zu diesem Zeitpunkt dem Dozenten zugewiesen sind, und denen, die in der Ansicht aktiviert wurden. Die beiden letzten Auflistungen werden in `HashSet`-Objekten gespeichert, um Suchvorgänge effizienter zu gestalten.

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser der Auflistung in der Navigationseigenschaft hinzugefügt.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser aus der Auflistung in der Navigationseigenschaft gelöscht.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

In *Views\Instructor\Edit.cshtml*, Hinzufügen einer **Kurse** Feld mit einem Array von Kontrollkästchen durch Hinzufügen des Folgendes hervorgehobenen code direkt nach der `div` Elemente für die `OfficeAssignment` Feld:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Dieser Code erstellt eine HTML-Tabelle mit drei Spalten. Jede Spalte enthält ein Kontrollkästchen gefolgt von einem Titel, der aus der Kursnummer und dem Kurstitel besteht. Die Kontrollkästchen haben denselben Namen ("SelectedCourses"), der die modellbindung informiert werden, die sie als Gruppe behandelt werden. Die `value` Attribut der einzelnen Kontrollkästchen wird festgelegt, auf den Wert der `CourseID.` , wenn die Seite zurückgesendet wird, übergibt die modellbindung ein Array mit dem Controller an, das umfasst die `CourseID` Werte für die nur die Kontrollkästchen ausgewählt sind.

Wenn Sie die Kontrollkästchen ursprünglich gerendert wurden, haben, die dem Dozenten zugewiesene Kurse sind `checked` Attribute, die diese aktivieren (angezeigt, die sie aktiviert werden).

Nach dem Ändern von kurszuweisungen, sollten Sie in der Lage, die Änderungen zu überprüfen, zum Beenden die Website der `Index` Seite. Aus diesem Grund müssen Sie eine Spalte in der Tabelle auf dieser Seite hinzufügen. In diesem Fall Sie verwenden, müssen die `ViewBag` Objekt, da die Informationen, die Sie anzeigen möchten, bereits in ist die `Courses` Navigationseigenschaft der `Instructor` Entität, die Sie zur Seite wie das Modell übergeben werden.

In *Views\Instructor\Index.cshtml*, Hinzufügen einer **Kurse** Überschrift unmittelbar nach der **Office** Überschrift, wie im folgenden Beispiel gezeigt:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Fügen Sie eine neue Detailzelle unmittelbar nach der Detailzelle der Office-Speicherort hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Führen Sie die **"Instructor" Index** Seite, um jeden Dozenten zugewiesene Kurse finden Sie unter:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klicken Sie auf **bearbeiten** für einen Dozenten auf der Seite "Bearbeiten" finden Sie unter.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Ändern Sie einige kurszuweisungen, und klicken Sie auf **speichern**. Die Änderungen werden auf der Indexseite angezeigt.

 Hinweis: In diesem Ansatz zum Bearbeiten von Dozenten funktioniert gut, wenn eine begrenzte Anzahl von Kursen vorhanden ist. Bei umfangreicheren Auflistungen wären eine andere Benutzeroberfläche und eine andere Aktualisierungsmethode erforderlich.  
 

## <a name="update-the-delete-method"></a>Aktualisieren Sie die Delete-Methode

Ändern Sie den Code in der HttpPost-Delete-Methode, damit der Eintrag der Office-Zuweisung (sofern vorhanden) gelöscht wird, wenn der Dozent gelöscht wird:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Wenn Sie versuchen, einen Dozenten zu löschen, eine Abteilung als Administrator zugewiesen ist, erhalten Sie einen Fehler der referenziellen Integrität. Finden Sie unter [die aktuelle Version dieses Tutorials](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) zusätzlichen Code für die automatisch die "Instructor" von jede Abteilung entfernt wird, in denen der Dozent als Administrator zugewiesen ist.

## <a name="summary"></a>Zusammenfassung

Sie haben nun diese Einführung in das Arbeiten mit zugehörigen Daten abgeschlossen. Bisher in diesen Tutorials haben Sie eine vollständige Palette von CRUD-Vorgänge ausgeführt, aber Sie noch nicht mit Parallelitätsproblemen behandelt. Im nächste Tutorial wird Stellen Sie das Thema der Parallelität, erklären die Optionen zur Fehlerbehandlung und Hinzufügen von Parallelität, die Ausnahmebehandlung der CRUD-Code, den Sie für einen Entitätstyp bereits geschrieben haben.

Links zu anderen Ressourcen Entity Framework finden Sie am Ende der [letzten Tutorial dieser Reihe](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Zurück](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
