---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Aktualisieren von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (6 von 10) | Microsoft Docs"
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
ms.openlocfilehash: f2d480793d02c8bfa25c05fd11fa2e6ef9e54a60
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualisieren von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (6 von 10)
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm angezeigten Sie verwandte Daten; In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren. Für die meisten Beziehungen können dazu werden die entsprechenden foreign Key-Felder aktualisieren. Für viele-zu-viele-Beziehungen nicht das Entity Framework der Jointabelle direkt, verfügbar machen, müssen Sie explizit hinzufügen und Entfernen von Entitäten, die verschiedene andere und aus der entsprechenden Navigationseigenschaften.

Die folgenden Abbildungen zeigen den Seiten, arbeiten Sie mit.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Das Erstellen und von Bearbeitungsseiten für Kurse anpassen

Wenn eine neue Kurs-Entität erstellt wird, muss sie eine Beziehung zu einer vorhandenen Abteilung verfügen. Um dies zu ermöglichen, enthält der scaffolded Code Controllermethoden und erstellen und Bearbeiten von Ansichten, die eine Dropdown-Liste für die Auswahl der Abteilung enthalten. Im Dropdown-Liste wird die `Course.DepartmentID` Fremdschlüsseleigenschaft, und das ist alles Entity Framework zu benötigt, um das Laden der `Department` Navigationseigenschaft mit dem entsprechenden `Department` Entität. Sie müssen den scaffolded Code, aber ändern, damit Sie etwas hinzufügen Fehlerbehandlung und sortieren Sie die Dropdown-Liste.

In *CourseController.cs*, löschen Sie die vier `Edit` und `Create` Methoden und Ersetzen Sie sie durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Die `PopulateDepartmentsDropDownList` Methode ruft eine Liste aller Abteilungen, sortiert nach dem Namen ab, erstellt eine `SelectList` Auflistung eine Dropdown-Liste, und übergibt Sie zur Ansicht in der Auflistung eine `ViewBag` Eigenschaft. Akzeptiert die Methode den optionalen `selectedDepartment` Parameter, der den Aufrufcode das Element an, die beim Rendern der Dropdown Liste ausgewählt werden kann. Die Ansicht wird der Name übergeben `DepartmentID` auf [der `DropDownList` Helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), und das Hilfsprogramm dann weiß, dass er das Suchen in der `ViewBag` -Objekt für eine `SelectList` mit dem Namen `DepartmentID`.

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

Klicken Sie auf **Erstellen**. Die Kurs Indexseite wird mit den neuen Kurs zur Liste hinzugefügt angezeigt. Der Abteilungsname in der Liste der Index-Seite stammt die Navigationseigenschaft, die anzeigt, dass die Beziehung ordnungsgemäß eingerichtet wurde.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Führen Sie die **bearbeiten** Seite (die Kurs Indexseite angezeigt, und klicken Sie auf **bearbeiten** für eine Vorgehensweise).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ändern von Daten auf der Seite, und klicken Sie auf **speichern**. Die Kurs Indexseite wird mit den aktualisierten Kursdaten angezeigt.

## <a name="adding-an-edit-page-for-instructors"></a>Hinzufügen eine Bearbeitungsseite für Dozenten

Beim Bearbeiten eines Instructor-Datensatzes möchten Sie den Dozenten bürozuweisung aktualisieren können. Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität, d. h., Sie müssen die folgenden Situationen behandeln:

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

- Ruft die aktuellen `Instructor` Entität aus der Datenbank mit unverzüglichem Laden für die `OfficeAssignment` Navigationseigenschaft. Dies ist identisch mit was Sie getan, in haben der `HttpGet` `Edit` Methode.
- Aktualisiert die abgerufenen `Instructor` Entität mit Werten aus den Modellbinder. Die [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.108).aspx) verwendete Überladung ermöglicht es Ihnen, *Positivliste* die Eigenschaften, die Sie einschließen möchten. Dies verhindert die übermäßige Buchung, wie in beschrieben [das zweite Lernprogramm](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Wenn der Niederlassung leer ist, legt die `Instructor.OfficeAssignment` -Eigenschaft auf null, damit die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht werden.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Speichert die Änderungen in der Datenbank an.

In *Views\Instructor\Edit.cshtml*nach der `div` Elemente für die **Hire Date** Feld Hinzufügen eines neuen Felds für die Bearbeitung der Bürostandort am:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Führen Sie die Seite (Wählen Sie die **Lehrkräfte** Registerkarte, und klicken Sie dann auf **bearbeiten** auf einen Kursleiter). Ändern der **Filiale** , und klicken Sie auf **speichern**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Hinzufügen von Kurs Zuweisungen zu den Dozenten bearbeiten (Seite)

Lehrkräfte können eine beliebige Anzahl von Kurse werden folgende Themen behandelt. Jetzt müssen Sie die Seite Dozenten bearbeiten verbessern, durch Hinzufügen der Funktion zum Ändern der Kurs-Zuordnung, die eine Gruppe von Kontrollkästchen, wie im folgenden Screenshot gezeigt:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Die Beziehung zwischen der `Course` und `Instructor` Entitäten ist m: n-, d. h. Sie keinen direkten Zugriff auf die Jointabelle. Stattdessen wird hinzufügen und Entfernen von Entitäten in und aus der `Instructor.Courses` Navigationseigenschaft.

Die Benutzeroberfläche, die Sie ändern, welche Kurse ermöglicht ein Kursleiter ist ist zugewiesen eine Gruppe von Kontrollkästchen. Ein Kontrollkästchen für jede Vorgehensweise in der Datenbank angezeigt wird, und diejenigen Argumente, die der Dozenten derzeit zugewiesen ist, werden ausgewählt. Der Benutzer kann aktivieren oder deaktivieren Kontrollkästchen, um den Kurs Zuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer sind, Sie möchten wahrscheinlich verwenden Sie eine andere Methode zur Darstellung der Daten in der Ansicht von, aber verwenden Sie die gleiche Methode Navigationseigenschaften zu erstellen oder Löschen von Beziehungen zu bearbeiten.

Um Daten zur Ansicht für die Liste von Kontrollkästchen bereitzustellen, verwenden Sie eine Modellklasse anzeigen. Erstellen Sie *AssignedCourseData.cs* in der *ViewModels* Ordner und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In *InstructorController.cs*, ersetzen Sie die `HttpGet` `Edit` -Methode durch folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Der Code fügt unverzüglichem Laden für die `Courses` Navigationseigenschaft und ruft die neue `PopulateAssignedCourseData` -Methode zum Bereitstellen von Informationen für das Kontrollkästchen Array mithilfe der `AssignedCourseData` Modellklasse anzeigen.

Der Code in der `PopulateAssignedCourseData` Methode liest, bis alle `Course` Entitäten, um eine Liste der Kurse, die in der Ansicht laden model-Klasse. Für jeden Kurs im Code wird überprüft, ob der Kurs in des Dozenten befindet `Courses` Navigationseigenschaft. Um effiziente Suche nach erstellen, bei der Überprüfung, ob ein Kurs zu den Dozenten zugewiesen wird, den Dozenten zugewiesene Kurse abgelegt sind eine [HashSet](https://msdn.microsoft.com/en-us/library/bb359438.aspx) Auflistung. Die `Assigned` -Eigenschaftensatz auf `true` Kurse Kursleiter zugewiesen ist. Die Ansicht wird diese Eigenschaft verwenden, um zu bestimmen, welche Felder als angezeigt werden, müssen aktiviert. Schließlich wird die Liste übergeben, auf die Ansicht in einem `ViewBag` Eigenschaft.

Als Nächstes fügen Sie den Code, der ausgeführt wird, wenn der Benutzer klickt **speichern**. Ersetzen Sie die `HttpPost` `Edit` Methode durch den folgenden Code, der eine neue Methode aufruft, die aktualisiert die `Courses` Navigationseigenschaft der `Instructor` Entität. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Da die Sicht eine Auflistung von besitzt `Course` Entitäten, die der Modellbinder kann nicht automatisch aktualisiert. die `Courses` Navigationseigenschaft. Anstatt den Modellbinder Navigationseigenschaft Kurse zu aktualisieren, müssen Sie die ausführen, die in der neuen `UpdateInstructorCourses` Methode. Daher müssen Sie zum Ausschließen der `Courses` Eigenschaft von der modellbindung. Dies erfordert jede Änderung an der aufrufende Code [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.98).aspx) , da Sie verwenden die *Whitelisting* überladen und `Courses` befindet sich nicht in der Include-Liste.

Wenn keine Überprüfung aktiviert wurden, den Code in `UpdateInstructorCourses` initialisiert die `Courses` Navigationseigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Der Code durchläuft alle Kurse in der Datenbank und jeder Kurs gegen diejenigen zugewiesener den Dozenten im Vergleich zu den Vorgängen, die in der Ansicht ausgewählt wurden überprüft. Um effiziente Suchvorgänge zu erleichtern, werden die letzten beiden Auflistungen in gespeichert `HashSet` Objekte.

Wenn das Kontrollkästchen für einen Kurs ausgewählt wurde, aber der Kurs befindet sich nicht in der `Instructor.Courses` Navigationseigenschaft, die Auflistung in der Navigationseigenschaft des Betriebs hinzugefügt wird.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Wenn das Kontrollkästchen für einen Kurs wurde nicht aktiviert, aber die Vorgehensweise in ist der `Instructor.Courses` Navigationseigenschaft, des Betriebs aus der Navigationseigenschaft entfernt wird.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

In *Views\Instructor\Edit.cshtml*, Hinzufügen einer **Kurse** Feld mit einem Array von Kontrollkästchen, indem Sie den folgenden Code hervorgehoben code unmittelbar nach der `div` Elemente für die `OfficeAssignment` Feld:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Dieser Code erstellt eine HTML-Tabelle, die drei Spalten aufweist. In jeder Spalte wird ein Kontrollkästchen, gefolgt von einem Titel, der den Titel und Kursnummer besteht. Die Kontrollkästchen, die alle haben den gleichen Namen ("SelectedCourses"), der den Modellbinder informiert, die sie als eine Gruppe behandelt werden soll. Die `value` Attribut der einzelnen Kontrollkästchen wird festgelegt, auf den Wert der `CourseID.` , wenn die Seite zurückgesendet wird, übergibt der Modellbinder ein Array mit dem Controller an, aus denen besteht die `CourseID` Werte für nur die Kontrollkästchen ausgewählt sind.

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

Einige Kurs Zuweisungen ändern, und klicken Sie auf **speichern**. Die Änderungen, die Sie vornehmen, werden auf der Seite "Index" angezeigt.

 Hinweis: Ansatz zum Bearbeiten von Daten für Dozenten Kurs funktioniert gut, wenn eine begrenzte Anzahl von Kurse. Bei Auflistungen, die viel größer sind, wäre eine andere Benutzeroberfläche und eine andere Methode für die Aktualisierung erforderlich.  
 

## <a name="update-the-delete-method"></a>Aktualisieren Sie die Delete-Methode

Ändern Sie den Code in der HttpPost Delete-Methode, damit der Datensatz der Office-Zuweisung (sofern vorhanden) gelöscht wird, wenn der Kursleiter gelöscht wird:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Wenn Sie versuchen, einen Kursleiter zu löschen, eine Abteilung als Administrator zugewiesen ist, erhalten Sie einen Fehler der referenziellen Integrität. Finden Sie unter [die aktuelle Version dieses Lernprogramms](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) für zusätzlichen Code, der automatisch den Dozenten von Abteilung entfernt werden soll, in dem die Instructor als Administrator zugewiesen ist.

## <a name="summary"></a>Zusammenfassung

Sie haben diese Einführung in die Arbeit mit verknüpften Daten jetzt abgeschlossen. Bisher in diesen Lernprogrammen haben Sie einen vollständigen Bereich von CRUD-Vorgänge ausgeführt, aber Sie noch nicht mit Parallelitätsproblemen behandelt. Des nächsten Lernprogramms wird Stellen Sie das Thema der Parallelität, erklären Sie Optionen zur Fehlerbehandlung und Hinzufügen von Parallelität Ausnahmebehandlung des CRUD-Codes, den Sie bereits für einen Entitätstyp geschrieben haben.

Links zu anderen Ressourcen Entity Framework befinden sich am Ende der [im letzten Lernprogramm dieser Reihe](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

>[!div class="step-by-step"]
[Zurück](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
