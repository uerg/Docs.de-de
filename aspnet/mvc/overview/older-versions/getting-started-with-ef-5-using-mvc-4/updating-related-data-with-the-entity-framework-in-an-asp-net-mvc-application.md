---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualisieren von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (6 von 10) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualisieren von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (6 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm angezeigten Sie verwandte Daten; In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren. Für die meisten Beziehungen können dazu werden die entsprechenden foreign Key-Felder aktualisieren. Für viele-zu-viele-Beziehungen nicht das Entity Framework der Jointabelle direkt, verfügbar machen, müssen Sie explizit hinzufügen und Entfernen von Entitäten, die verschiedene andere und aus der entsprechenden Navigationseigenschaften.

Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Anpassen der Seiten „Erstellen“ und „Bearbeiten“ für Kurse

Wenn eine neue Kursentität erstellt wird, muss diese in Beziehung zu einer vorhandenen Abteilung stehen. Um dies zu vereinfachen, enthält der Gerüstcode Controllermethoden und Ansichten zum „Erstellen“ und „Bearbeiten“, die eine Dropdownliste enthalten, aus denen der Fachbereich ausgewählt werden kann. Im Dropdown-Liste wird die `Course.DepartmentID` Fremdschlüsseleigenschaft, und das ist alles Entity Framework zu benötigt, um das Laden der `Department` Navigationseigenschaft mit dem entsprechenden `Department` Entität. Verwenden Sie den Gerüstcode, aber nehmen Sie kleine Änderungen vor, um die Fehlerbehandlung hinzuzufügen und die Dropdownliste zu sortieren.

In *CourseController.cs*, löschen Sie die vier `Edit` und `Create` Methoden und Ersetzen Sie sie durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Die `PopulateDepartmentsDropDownList` Methode ruft eine Liste aller Abteilungen, sortiert nach dem Namen ab, erstellt eine `SelectList` Auflistung eine Dropdown-Liste, und übergibt Sie zur Ansicht in der Auflistung eine `ViewBag` Eigenschaft. Die Methode akzeptiert den optionalen `selectedDepartment`-Parameter, über den der Code das Element angeben kann, das ausgewählt wird, wenn die Dropdownliste gerendert wird. Die Ansicht wird der Name übergeben `DepartmentID` auf [der `DropDownList` Helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), und das Hilfsprogramm dann weiß, dass er das Suchen in der `ViewBag` -Objekt für eine `SelectList` mit dem Namen `DepartmentID`.

Die `HttpGet` `Create` Methodenaufrufe der `PopulateDepartmentsDropDownList` Methode ohne das ausgewählte Element festlegen, da für einen neuen Kurs die Abteilung noch nicht eingerichtet wurde:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Die `HttpGet` `Edit` -Methode legt das ausgewählte Element, das anhand der ID der Abteilung, die den zu bearbeitenden Kurs bereits zugewiesen ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Die `HttpPost` für beide Methoden `Create` und `Edit` auch Code enthalten, der das ausgewählte Element festgelegt werden soll, wenn sie die Seite nach einem Fehler erneut:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Mit diesem Code wird sichergestellt, dass wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, die Abteilung ausgewählt wurde ausgewählten bleibt.

In *Views\Course\Create.cshtml*, den hervorgehobenen Code zum Erstellen einer neuen Kurs Zahlenfeld vor dem Hinzufügen der **Titel** Feld. Wie in einer früheren Lernprogramm erläutert wird, werden nicht standardmäßig Primärschlüsselfelder Gerüstbau, aber der Primärschlüssel ist aussagekräftigeren Namen, damit der Benutzer in der Lage, zur Eingabe der Schlüssel-Wert sein soll.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, und *Views\Course\Details.cshtml*, ein Zahlenfeld "Kurs" vor dem Hinzufügen der **Titel**  Feld. Da es sich um den primären Schlüssel handelt, wird angezeigt, jedoch nicht geändert werden kann.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Führen Sie die **erstellen** Seite (die Kurs Indexseite angezeigt, und klicken Sie auf **neu erstellen**), und geben Sie Daten für einen neuen Kurs:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Klicken Sie auf **Erstellen**. Die Kurs Indexseite wird mit den neuen Kurs zur Liste hinzugefügt angezeigt. Der Fachbereichsname in der Indexseitenliste wurde der Navigationseigenschaft entnommen und deutet darauf hin, dass die Beziehung ordnungsgemäß festgelegt wurde.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Führen Sie die **bearbeiten** Seite (die Kurs Indexseite angezeigt, und klicken Sie auf **bearbeiten** für eine Vorgehensweise).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ändern Sie die Daten auf der Seite, und klicken Sie auf **Speichern**. Die Kurs Indexseite wird mit den aktualisierten Kursdaten angezeigt.

## <a name="adding-an-edit-page-for-instructors"></a>Hinzufügen eine Bearbeitungsseite für Dozenten

Bei der Bearbeitung eines Dozentendatensatzes sollten Sie auch die Bürozuweisung des Dozenten aktualisieren. Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität, d. h., Sie müssen die folgenden Situationen behandeln:

- Wenn der Benutzer die Office-Zuweisung löscht, und er ursprünglich einen Wert hatte, müssen Sie diese entfernen und Löschen der `OfficeAssignment` Entität.
- Wenn der Benutzer eine Office-Zuweisungswert gibt, und es ursprünglich leer war, müssen Sie ein neues erstellen `OfficeAssignment` Entität.
- Wenn der Benutzer den Wert einer Zuweisung von Office ändert, muss, ändern Sie den Wert in einer vorhandenen `OfficeAssignment` Entität.

Open *InstructorController.cs* und sehen Sie sich die `HttpGet` `Edit` Methode:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Hier die scaffolded Code nicht erwünscht. Es ist einrichten Daten für eine Dropdown-Liste angezeigt, aber Sie müssen Sie ein Textfeld ist. Ersetzen Sie diese Methode, durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Dieser Code löscht die `ViewBag` Anweisung und fügt unverzüglichem Laden für den zugeordneten `OfficeAssignment` Entität. Mit unverzüglichem Laden nicht möglich den `Find` -Methode, sodass der `Where` und `Single` Methoden werden stattdessen verwendet, um den Dozenten auswählen.

Ersetzen Sie die `HttpPost` `Edit` -Methode durch folgenden Code. Office-Zuweisung Updates behandelt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Der Code führt Folgendes aus:

- Ruft die aktuelle Entität `Instructor` von der Datenbank über Eager Loading für die Navigationseigenschaft `OfficeAssignment` ab. Dies ist identisch mit was Sie getan, in haben der `HttpGet` `Edit` Methode.
- Aktualisiert die abgerufene Entität `Instructor` mit Werten aus der Modellbindung. Die [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) verwendete Überladung ermöglicht es Ihnen, *Positivliste* die Eigenschaften, die Sie einschließen möchten. Dies verhindert die übermäßige Buchung, wie in beschrieben [das zweite Lernprogramm](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Wenn der Niederlassung leer ist, legt die `Instructor.OfficeAssignment` -Eigenschaft auf null, damit die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht werden.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Speichert die Änderungen in der Datenbank.

In *Views\Instructor\Edit.cshtml*nach der `div` Elemente für die **Hire Date** Feld Hinzufügen eines neuen Felds für die Bearbeitung der Bürostandort am:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Führen Sie die Seite (Wählen Sie die **Lehrkräfte** Registerkarte, und klicken Sie dann auf **bearbeiten** auf einen Kursleiter). Ändern Sie den **Standort des Büros**, und klicken Sie auf **Speichern**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Hinzufügen von Kurs Zuweisungen zu den Dozenten bearbeiten (Seite)

Dozenten können eine beliebige Anzahl von Kursen unterrichten. Jetzt soll die Dozentenseite „Bearbeiten“ verbessert werden, indem es ermöglicht wird, Kurszuweisungen über eine Reihe von Kontrollkästchen zu verändern. Dies wird im folgenden Screenshot dargestellt:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Die Beziehung zwischen der `Course` und `Instructor` Entitäten ist m: n-, d. h. Sie keinen direkten Zugriff auf die Jointabelle. Stattdessen wird hinzufügen und Entfernen von Entitäten in und aus der `Instructor.Courses` Navigationseigenschaft.

Die Benutzeroberfläche, über die Sie ändern können, welchen Kursen ein Dozent zugewiesen ist, besteht aus einer Reihe von Kontrollkästchen. Für jeden Kurs in der Datenbank wird ein Kontrollkästchen angezeigt. Die Kontrollkästchen, denen der Dozent zu diesem Zeitpunkt zugewiesen ist, sind aktiviert. Der Benutzer kann Kontrollkästchen aktivieren oder deaktivieren, um Kurszuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer sind, Sie möchten wahrscheinlich verwenden Sie eine andere Methode zur Darstellung der Daten in der Ansicht von, aber verwenden Sie die gleiche Methode Navigationseigenschaften zu erstellen oder Löschen von Beziehungen zu bearbeiten.

Verwenden Sie eine Ansichtsmodellklasse, um Daten für die Ansicht bereitzustellen, um eine Liste mit Kontrollkästchen zu erstellen. Erstellen Sie *AssignedCourseData.cs* in der *ViewModels* Ordner und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In *InstructorController.cs*, ersetzen Sie die `HttpGet` `Edit` -Methode durch folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Über den Code wird für die `Courses`-Navigationseigenschaft Eager Loading hinzugefügt und die neue `PopulateAssignedCourseData`-Methode aufgerufen, um über die Ansichtsmodellklasse `AssignedCourseData` Informationen für das Kontrollkästchenarray zur Verfügung zu stellen.

Der Code in der `PopulateAssignedCourseData` Methode liest, bis alle `Course` Entitäten, um eine Liste der Kurse, die in der Ansicht laden model-Klasse. Für jeden Kurs überprüft der Code, ob dieser in der `Courses`-Navigationseigenschaft des Dozenten vorhanden ist. Um effiziente Suche nach erstellen, bei der Überprüfung, ob ein Kurs zu den Dozenten zugewiesen wird, den Dozenten zugewiesene Kurse abgelegt sind eine [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) Auflistung. Die `Assigned` -Eigenschaftensatz auf `true` Kurse Kursleiter zugewiesen ist. Die Ansicht verwendet diese Eigenschaft, um zu bestimmen, welche Kontrollkästchen als aktiviert angezeigt werden sollen. Schließlich wird die Liste übergeben, auf die Ansicht in einem `ViewBag` Eigenschaft.

Fügen Sie als nächstes den Code hinzu, der ausgeführt wird, wenn der Benutzer auf **Speichern** klickt. Ersetzen Sie die `HttpPost` `Edit` Methode durch den folgenden Code, der eine neue Methode aufruft, die aktualisiert die `Courses` Navigationseigenschaft der `Instructor` Entität. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Da die Sicht eine Auflistung von besitzt `Course` Entitäten, die der Modellbinder kann nicht automatisch aktualisiert. die `Courses` Navigationseigenschaft. Anstatt den Modellbinder Navigationseigenschaft Kurse zu aktualisieren, müssen Sie die ausführen, die in der neuen `UpdateInstructorCourses` Methode. Aus diesem Grund müssen Sie die `Courses`-Eigenschaft von der Modellbindung ausschließen. Dies erfordert jede Änderung an der aufrufende Code [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , da Sie verwenden die *Whitelisting* überladen und `Courses` befindet sich nicht in der Include-Liste.

Wenn keine Überprüfung aktiviert wurden, den Code in `UpdateInstructorCourses` initialisiert die `Courses` Navigationseigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Der Code führt dann eine Schleife durch alle Kurse in der Datenbank aus und überprüft jeden Kurs im Hinblick auf die Kurse, die zu diesem Zeitpunkt dem Dozenten zugewiesen sind, und denen, die in der Ansicht aktiviert wurden. Die beiden letzten Auflistungen werden in `HashSet`-Objekten gespeichert, um Suchvorgänge effizienter zu gestalten.

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser der Auflistung in der Navigationseigenschaft hinzugefügt.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser aus der Auflistung in der Navigationseigenschaft gelöscht.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

In *Views\Instructor\Edit.cshtml*, Hinzufügen einer **Kurse** Feld mit einem Array von Kontrollkästchen, indem Sie den folgenden Code hervorgehoben code unmittelbar nach der `div` Elemente für die `OfficeAssignment` Feld:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Dieser Code erstellt eine HTML-Tabelle mit drei Spalten. Jede Spalte enthält ein Kontrollkästchen gefolgt von einem Titel, der aus der Kursnummer und dem Kurstitel besteht. Die Kontrollkästchen, die alle haben den gleichen Namen ("SelectedCourses"), der den Modellbinder informiert, die sie als eine Gruppe behandelt werden soll. Die `value` Attribut der einzelnen Kontrollkästchen wird festgelegt, auf den Wert der `CourseID.` , wenn die Seite zurückgesendet wird, übergibt der Modellbinder ein Array mit dem Controller an, aus denen besteht die `CourseID` Werte für nur die Kontrollkästchen ausgewählt sind.

Wenn die Kontrollkästchen gerendert werden, denen, die für Kurse zu den Dozenten zugewiesen sind haben `checked` Attribute, die ausgewählt werden (angezeigt, die sie eingecheckt werden).

Nach dem Ändern der Kurs Zuweisungen, sollten Sie in der Lage, die Änderungen zu überprüfen, Rückkehr die Website der `Index` Seite. Aus diesem Grund müssen Sie eine Spalte der Tabelle auf dieser Seite hinzufügen. In diesem Fall Sie verwenden, müssen die `ViewBag` Objekt, da die Informationen, die Sie anzeigen möchten, bereits in wird die `Courses` Navigationseigenschaft der `Instructor` Entität, die Sie zur Seite wie das Modell übergeben möchten.

In *Views\Instructor\Index.cshtml*, Hinzufügen einer **Kurse** Überschrift unmittelbar nach der **Office** Überschrift, wie im folgenden Beispiel gezeigt:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Fügen Sie dann eine neue Detailzelle unmittelbar nach der Office-Speicherort-Detailzelle hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Führen Sie die **Dozenten Index** Bild, um die Kurse, jede Kursleiter zugewiesen anzuzeigen:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klicken Sie auf **bearbeiten** auf einen Kursleiter zum Anzeigen der Seite "Bearbeiten".

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Einige Kurs Zuweisungen ändern, und klicken Sie auf **speichern**. Die Änderungen werden auf der Indexseite angezeigt.

 Hinweis: Ansatz zum Bearbeiten von Daten für Dozenten Kurs funktioniert gut, wenn eine begrenzte Anzahl von Kurse. Bei umfangreicheren Auflistungen wären eine andere Benutzeroberfläche und eine andere Aktualisierungsmethode erforderlich.  
 

## <a name="update-the-delete-method"></a>Aktualisieren Sie die Delete-Methode

Ändern Sie den Code in der HttpPost Delete-Methode, damit der Datensatz der Office-Zuweisung (sofern vorhanden) gelöscht wird, wenn der Kursleiter gelöscht wird:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Wenn Sie versuchen, einen Kursleiter zu löschen, eine Abteilung als Administrator zugewiesen ist, erhalten Sie einen Fehler der referenziellen Integrität. Finden Sie unter [die aktuelle Version dieses Lernprogramms](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) für zusätzlichen Code, der automatisch den Dozenten von Abteilung entfernt werden soll, in dem die Instructor als Administrator zugewiesen ist.

## <a name="summary"></a>Zusammenfassung

Sie haben diese Einführung in die Arbeit mit verknüpften Daten jetzt abgeschlossen. Bisher in diesen Lernprogrammen haben Sie einen vollständigen Bereich von CRUD-Vorgänge ausgeführt, aber Sie noch nicht mit Parallelitätsproblemen behandelt. Des nächsten Lernprogramms wird Stellen Sie das Thema der Parallelität, erklären Sie Optionen zur Fehlerbehandlung und Hinzufügen von Parallelität Ausnahmebehandlung des CRUD-Codes, den Sie bereits für einen Entitätstyp geschrieben haben.

Links zu anderen Ressourcen Entity Framework befinden sich am Ende der [im letzten Lernprogramm dieser Reihe](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Zurück](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
