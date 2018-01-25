---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Aktualisieren von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft Docs"
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 205d5ddcd0c3240c87ec5705a6676215eb67942d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Aktualisieren von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm angezeigten Sie verwandte Daten; In diesem Lernprogramm werden Sie verknüpfte Daten aktualisieren. Für die meisten Beziehungen kann dadurch erfolgen durch foreign Key-Felder oder Navigationseigenschaften aktualisieren. Für viele-zu-viele-Beziehungen nicht Entity Framework der Jointabelle direkt verfügbar machen, damit Sie hinzufügen und Entfernen von Entitäten, die verschiedene andere und aus der entsprechenden Navigationseigenschaften.

Die folgenden Abbildungen zeigen einige Seiten, denen Sie verwenden müssen.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Mit Kurse Instructor-bearbeiten](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Das Erstellen und von Bearbeitungsseiten für Kurse anpassen

Wenn eine neue Kurs-Entität erstellt wird, muss sie eine Beziehung zu einer vorhandenen Abteilung verfügen. Um dies zu ermöglichen, enthält der scaffolded Code Controllermethoden und erstellen und Bearbeiten von Ansichten, die eine Dropdown-Liste für die Auswahl der Abteilung enthalten. Im Dropdown-Liste wird die `Course.DepartmentID` Fremdschlüsseleigenschaft, und das ist alles Entity Framework zu benötigt, um das Laden der `Department` Navigationseigenschaft mit dem entsprechenden `Department` Entität. Sie müssen den scaffolded Code, aber ändern, damit Sie etwas hinzufügen Fehlerbehandlung und sortieren Sie die Dropdown-Liste.

In *CourseController.cs*, löschen Sie die vier `Create` und `Edit` Methoden und Ersetzen Sie sie durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Fügen Sie die folgenden `using` Anweisung am Anfang der Datei:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Die `PopulateDepartmentsDropDownList` Methode ruft eine Liste aller Abteilungen, sortiert nach dem Namen ab, erstellt eine `SelectList` Auflistung eine Dropdown-Liste, und übergibt Sie zur Ansicht in der Auflistung eine `ViewBag` Eigenschaft. Akzeptiert die Methode den optionalen `selectedDepartment` Parameter, der den Aufrufcode das Element an, die beim Rendern der Dropdown Liste ausgewählt werden kann. Die Ansicht wird der Name übergeben `DepartmentID` auf die [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) -Hilfsobjekt und das Hilfsprogramm zum Suchen in weiß der `ViewBag` -Objekt für eine `SelectList` mit dem Namen `DepartmentID`.

Die `HttpGet` `Create` Methodenaufrufe der `PopulateDepartmentsDropDownList` Methode ohne das ausgewählte Element festlegen, da für einen neuen Kurs die Abteilung noch nicht eingerichtet wurde:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Die `HttpGet` `Edit` -Methode legt das ausgewählte Element, das anhand der ID der Abteilung, die den zu bearbeitenden Kurs bereits zugewiesen ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Die `HttpPost` für beide Methoden `Create` und `Edit` auch Code enthalten, der das ausgewählte Element festgelegt werden soll, wenn sie die Seite nach einem Fehler erneut:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Mit diesem Code wird sichergestellt, dass wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, die Abteilung ausgewählt wurde ausgewählten bleibt.

Die Kurs-Sichten sind bereits mit den Dropdownlisten für das Abteilungsfeld Gerüstbau, möchten jedoch nicht die Beschriftung "DepartmentID" für dieses Feld, deshalb stellen die folgenden hervorgehobenen ändern, um die *Views\Course\Create.cshtml* Datei Ändern Sie die Beschriftung ein.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Stellen Sie die gleiche Änderung in *Views\Course\Edit.cshtml*.

Normalerweise erstellen nicht die Scaffolder einen Primärschlüssel, da der Schlüsselwert wird von der Datenbank generiert und kann nicht geändert werden und ist nicht keinen sinnvollen Wert an der Benutzern angezeigt werden. Für Entitäten Kurs umfasst die Scaffolder ein Textfeld für die `CourseID` -Feld verwendet wird, weil er, die versteht die `DatabaseGeneratedOption.None` Attribut bedeutet, dass die Benutzer sollten in der Lage sein Geben Sie den Wert des Primärschlüssels. Aber es ist nicht verstehen, da die Anzahl sinnvoll ist, die Sie in den anderen Ansichten angezeigt wird, daher Sie sie manuell hinzufügen müssen möchten.

In *Views\Course\Edit.cshtml*, ein Zahlenfeld "Kurs" vor dem Hinzufügen der **Titel** Feld. Da es sich um den primären Schlüssel handelt, wird angezeigt, jedoch nicht geändert werden kann.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Es ist bereits ein ausgeblendetes Feld (`Html.HiddenFor` Helper) für die Kursnummer in der Bearbeitungsansicht. Hinzufügen einer *Html.LabelFor* Hilfsprogramm nicht entfällt der Bedarf für das ausgeblendete Feld, da sie nicht dazu, dass die Anzahl der Kurs in die bereitgestellten Daten aufgenommen werden, wenn der Benutzer klickt **speichern** auf der Seite "Bearbeiten".

In *Views\Course\Delete.cshtml* und *Views\Course\Details.cshtml*, ändern Sie die Beschriftung ein Department Name von "Name", "Abteilung", und ein Kurs Zahlenfeld vor dem Hinzufügen der **Titel**  Feld.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Führen Sie die **erstellen** Seite (die Kurs Indexseite angezeigt, und klicken Sie auf **neu erstellen**), und geben Sie Daten für einen neuen Kurs:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Klicken Sie auf **Erstellen**. Die Kurs Indexseite wird mit den neuen Kurs zur Liste hinzugefügt angezeigt. Der Abteilungsname in der Liste der Index-Seite stammt die Navigationseigenschaft, die anzeigt, dass die Beziehung ordnungsgemäß eingerichtet wurde.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Führen Sie die **bearbeiten** Seite (die Kurs Indexseite angezeigt, und klicken Sie auf **bearbeiten** für eine Vorgehensweise).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ändern von Daten auf der Seite, und klicken Sie auf **speichern**. Die Kurs Indexseite wird mit den aktualisierten Kursdaten angezeigt.

## <a name="adding-an-edit-page-for-instructors"></a>Hinzufügen eine Bearbeitungsseite für Dozenten

Beim Bearbeiten eines Instructor-Datensatzes möchten Sie den Dozenten bürozuweisung aktualisieren können. Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität, d. h., Sie müssen die folgenden Situationen behandeln:

- Wenn der Benutzer die Office-Zuweisung löscht, und er ursprünglich einen Wert hatte, müssen Sie diese entfernen und Löschen der `OfficeAssignment` Entität.
- Wenn der Benutzer eine Office-Zuweisungswert gibt, und es ursprünglich leer war, müssen Sie ein neues erstellen `OfficeAssignment` Entität.
- Wenn der Benutzer den Wert einer Zuweisung von Office ändert, muss, ändern Sie den Wert in einer vorhandenen `OfficeAssignment` Entität.

Open *InstructorController.cs* und sehen Sie sich die `HttpGet` `Edit` Methode:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Hier die scaffolded Code nicht erwünscht. Es ist einrichten Daten für eine Dropdown-Liste angezeigt, aber Sie müssen Sie ein Textfeld ist. Ersetzen Sie diese Methode, durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Dieser Code löscht die `ViewBag` Anweisung und fügt unverzüglichem Laden für den zugeordneten `OfficeAssignment` Entität. Mit unverzüglichem Laden nicht möglich den `Find` -Methode, sodass der `Where` und `Single` Methoden werden stattdessen verwendet, um den Dozenten auswählen.

Ersetzen Sie die `HttpPost` `Edit` -Methode durch folgenden Code. Office-Zuweisung Updates behandelt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Der Verweis auf `RetryLimitExceededException` erfordert eine `using` -Anweisung zum Hinzufügen der rechten Maustaste auf `RetryLimitExceededException`, und klicken Sie dann auf **beheben** - **mit System.Data.Entity.Infrastructure**.

![Beheben von Ausnahmen bei Wiederholungsversuchen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Der Code führt Folgendes aus:

- Ändert den Namen der Methode, um `EditPost` , da die Signatur jetzt identisch ist die `HttpGet` -Methode (die `ActionName` Attribut gibt an, dass die URL/Edit / weiterhin verwendet wird).
- Ruft die aktuellen `Instructor` Entität aus der Datenbank mit unverzüglichem Laden für die `OfficeAssignment` Navigationseigenschaft. Dies ist identisch mit was Sie getan, in haben der `HttpGet` `Edit` Methode.
- Aktualisiert die abgerufenen `Instructor` Entität mit Werten aus den Modellbinder. Die [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) verwendete Überladung ermöglicht es Ihnen, *Positivliste* die Eigenschaften, die Sie einschließen möchten. Dies verhindert die übermäßige Buchung, wie in beschrieben [das zweite Lernprogramm](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Wenn der Niederlassung leer ist, legt die `Instructor.OfficeAssignment` -Eigenschaft auf null, damit die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht werden.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Speichert die Änderungen in der Datenbank an.

In *Views\Instructor\Edit.cshtml*nach der `div` Elemente für die **Hire Date** Feld Hinzufügen eines neuen Felds für die Bearbeitung der Bürostandort am:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Führen Sie die Seite (Wählen Sie die **Lehrkräfte** Registerkarte, und klicken Sie dann auf **bearbeiten** auf einen Kursleiter). Ändern der **Filiale** , und klicken Sie auf **speichern**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Hinzufügen von Kurs Zuweisungen zu den Dozenten bearbeiten (Seite)

Lehrkräfte können eine beliebige Anzahl von Kurse werden folgende Themen behandelt. Jetzt müssen Sie die Seite Dozenten bearbeiten verbessern, durch Hinzufügen der Funktion zum Ändern der Kurs-Zuordnung, die eine Gruppe von Kontrollkästchen, wie im folgenden Screenshot gezeigt:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Die Beziehung zwischen der `Course` und `Instructor` Entitäten ist m: n-, d. h. Sie keinen direkten Zugriff auf die Fremdschlüsseleigenschaften in der verknüpften Tabelle sind. Stattdessen hinzufügen und Entfernen von Entitäten in und aus der `Instructor.Courses` Navigationseigenschaft.

Die Benutzeroberfläche, die Sie ändern, welche Kurse ermöglicht ein Kursleiter ist ist zugewiesen eine Gruppe von Kontrollkästchen. Ein Kontrollkästchen für jede Vorgehensweise in der Datenbank angezeigt wird, und diejenigen Argumente, die der Dozenten derzeit zugewiesen ist, werden ausgewählt. Der Benutzer kann aktivieren oder deaktivieren Kontrollkästchen, um den Kurs Zuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer sind, Sie möchten wahrscheinlich verwenden Sie eine andere Methode zur Darstellung der Daten in der Ansicht von, aber verwenden Sie die gleiche Methode Navigationseigenschaften zu erstellen oder Löschen von Beziehungen zu bearbeiten.

Um Daten zur Ansicht für die Liste von Kontrollkästchen bereitzustellen, verwenden Sie eine Modellklasse anzeigen. Erstellen Sie *AssignedCourseData.cs* in der *ViewModels* Ordner und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

In *InstructorController.cs*, ersetzen Sie die `HttpGet` `Edit` -Methode durch folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Der Code fügt unverzüglichem Laden für die `Courses` Navigationseigenschaft und ruft die neue `PopulateAssignedCourseData` -Methode zum Bereitstellen von Informationen für das Kontrollkästchen Array mithilfe der `AssignedCourseData` Modellklasse anzeigen.

Der Code in der `PopulateAssignedCourseData` Methode liest, bis alle `Course` Entitäten, um eine Liste der Kurse, die in der Ansicht laden model-Klasse. Für jeden Kurs im Code wird überprüft, ob der Kurs in des Dozenten befindet `Courses` Navigationseigenschaft. Um effiziente Suche nach erstellen, bei der Überprüfung, ob ein Kurs zu den Dozenten zugewiesen wird, den Dozenten zugewiesene Kurse abgelegt sind eine [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) Auflistung. Die `Assigned` -Eigenschaftensatz auf `true` Kurse Kursleiter zugewiesen ist. Die Ansicht wird diese Eigenschaft verwenden, um zu bestimmen, welche Felder als angezeigt werden, müssen aktiviert. Schließlich wird die Liste übergeben, auf die Ansicht in einem `ViewBag` Eigenschaft.

Als Nächstes fügen Sie den Code, der ausgeführt wird, wenn der Benutzer klickt **speichern**. Ersetzen Sie die `EditPost` Methode durch den folgenden Code, der eine neue Methode aufruft, die aktualisiert die `Courses` Navigationseigenschaft der `Instructor` Entität. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Die Methodensignatur unterscheidet sich jetzt von der `HttpGet` `Edit` Methode, sodass der Name der Methode von ändert `EditPost` an `Edit`.

Da die Sicht eine Auflistung von besitzt `Course` Entitäten, die der Modellbinder kann nicht automatisch aktualisiert. die `Courses` Navigationseigenschaft. Verwenden Sie statt des Modellbinders zum Aktualisieren der `Courses` Navigationseigenschaft vornehmen, die in der neuen `UpdateInstructorCourses` Methode. Daher müssen Sie zum Ausschließen der `Courses` Eigenschaft von der modellbindung. Dies erfordert jede Änderung an der aufrufende Code [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , da Sie verwenden die *Whitelisting* überladen und `Courses` befindet sich nicht in der Include-Liste.

Wenn keine Überprüfung aktiviert wurden, den Code in `UpdateInstructorCourses` initialisiert die `Courses` Navigationseigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Der Code durchläuft alle Kurse in der Datenbank und jeder Kurs gegen diejenigen zugewiesener den Dozenten im Vergleich zu den Vorgängen, die in der Ansicht ausgewählt wurden überprüft. Um effiziente Suchvorgänge zu erleichtern, werden die letzten beiden Auflistungen in gespeichert `HashSet` Objekte.

Wenn das Kontrollkästchen für einen Kurs ausgewählt wurde, aber der Kurs befindet sich nicht in der `Instructor.Courses` Navigationseigenschaft, die Auflistung in der Navigationseigenschaft des Betriebs hinzugefügt wird.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Wenn das Kontrollkästchen für einen Kurs wurde nicht aktiviert, aber die Vorgehensweise in ist der `Instructor.Courses` Navigationseigenschaft, des Betriebs aus der Navigationseigenschaft entfernt wird.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

In *Views\Instructor\Edit.cshtml*, Hinzufügen einer **Kurse** Feld mit einem Array von Kontrollkästchen durch das Hinzufügen der folgenden code unmittelbar nach der `div` Elemente für die `OfficeAssignment` Feld und Bevor Sie die `div` -Element für die **speichern** Schaltfläche:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Nach dem Einfügen der Code, wenn die Zeilenumbrüche und Einzüge nicht aussehen wie hier, alles, damit es sieht wie folgt hier Sie sehen manuell korrigieren. Der Einzug keinen perfekt, aber die `@</tr><tr>`, `@:<td>`, `@:</td>`, und `@</tr>` Zeilen müssen jeweils in einer einzelnen Zeile dargestellten oder erhalten Sie einen Laufzeitfehler.

Dieser Code erstellt eine HTML-Tabelle, die drei Spalten aufweist. In jeder Spalte wird ein Kontrollkästchen, gefolgt von einem Titel, der den Titel und Kursnummer besteht. Die Kontrollkästchen, die alle haben den gleichen Namen ("SelectedCourses"), der den Modellbinder informiert, die sie als eine Gruppe behandelt werden soll. Die `value` Attribut der einzelnen Kontrollkästchen wird festgelegt, auf den Wert der `CourseID.` , wenn die Seite zurückgesendet wird, übergibt der Modellbinder ein Array mit dem Controller an, aus denen besteht die `CourseID` Werte für nur die Kontrollkästchen ausgewählt sind.

Wenn die Kontrollkästchen gerendert werden, denen, die für Kurse zu den Dozenten zugewiesen sind haben `checked` Attribute, die ausgewählt werden (angezeigt, die sie eingecheckt werden).

Nach dem Ändern der Kurs Zuweisungen, sollten Sie in der Lage, die Änderungen zu überprüfen, Rückkehr die Website der `Index` Seite. Aus diesem Grund müssen Sie eine Spalte der Tabelle auf dieser Seite hinzufügen. In diesem Fall Sie verwenden, müssen die `ViewBag` Objekt, da die Informationen, die Sie anzeigen möchten, bereits in wird die `Courses` Navigationseigenschaft der `Instructor` Entität, die Sie zur Seite wie das Modell übergeben möchten.

In *Views\Instructor\Index.cshtml*, Hinzufügen einer **Kurse** Überschrift unmittelbar nach der **Office** Überschrift, wie im folgenden Beispiel gezeigt:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Fügen Sie dann eine neue Detailzelle unmittelbar nach der Office-Speicherort-Detailzelle hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Führen Sie die **Dozenten Index** Bild, um die Kurse, jede Kursleiter zugewiesen anzuzeigen:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie auf **bearbeiten** auf einen Kursleiter zum Anzeigen der Seite "Bearbeiten".

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Einige Kurs Zuweisungen ändern, und klicken Sie auf **speichern**. Die Änderungen, die Sie vornehmen, werden auf der Seite "Index" angezeigt.

 Hinweis: Ansatz wird hier zum Bearbeiten von Daten für Dozenten Kurs funktioniert gut, wenn eine begrenzte Anzahl von Kurse. Bei Auflistungen, die viel größer sind, wäre eine andere Benutzeroberfläche und eine andere Methode für die Aktualisierung erforderlich.  
 

## <a name="update-the-deleteconfirmed-method"></a>Aktualisieren Sie die DeleteConfirmed-Methode

In *InstructorController.cs*, löschen Sie die `DeleteConfirmed` -Methode und einfügen, die im folgenden an seiner Stelle Codebeispiel.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Dieser Code macht die folgende Änderung:

- Die Instructor als Administrator der Abteilung zugewiesen wird, entfernt die Instructor-Zuordnung aus dieser Abteilung. Ohne diesen Code erhalten Sie einen Fehler der referenziellen Integrität, würden Sie versuchen, einen Kursleiter zu löschen, die als Administrator für eine Abteilung zugewiesen wurde.

Dieser Code nicht das Szenario einer Kursleiter als Administrator für mehrere Abteilungen zugewiesen zu behandeln. In den letzten Lernprogramm fügen Sie Code, der verhindert, dieses Szenario zu vermeiden dass.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Hinzufügen von Office-Speicherort und Kurse auf der Seite "erstellen"

In *InstructorController.cs*, löschen Sie die `HttpGet` und `HttpPost` `Create` Methoden, und fügen Sie dann den folgenden Code an ihrer Stelle:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Dieser Code ist ähnlich, was Sie für die Methoden bearbeiten haben gesehen, mit dem Unterschied, dass zunächst keine Kurse ausgewählt werden. Die `HttpGet` `Create` Methodenaufrufe der `PopulateAssignedCourseData` Methode nicht, da aber in ausgewählte Kurse möglicherweise sortieren, geben Sie eine leere Auflistung für die `foreach` Schleife in der Ansicht (andernfalls den Code anzeigen Auslösung ausgeben würde ein null-Verweis ).

Die HttpPost erstellen Methode fügt jeder ausgewählten Kurs mit der Navigationseigenschaft Kurse vor den Vorlagencode, der Überprüfung auf Fehler überprüft und der Datenbank den neuen Kursleiter hinzugefügt. Kurse hinzugefügt werden, selbst wenn Modellfehler vorliegen, damit beim Modellfehler (beispielsweise ein Benutzer ein ungültiges Datum sortiert) vorliegen, wenn die Seite mit einer Fehlermeldung erneut angezeigt wird, werden keine Kurs-Auswahl, die vorgenommen wurden automatisch wiederhergestellt.

Beachten Sie, dass damit Kurse zum Hinzufügen der `Courses` Navigationseigenschaft Sie die Eigenschaft als eine leere Auflistung zu initialisieren müssen:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Als Alternative zur controllercode Dadurch konnte Sie dazu im Modell Dozenten ändern den Eigenschaftengetter, um automatisch den Sammlungssatz zu erstellen, wenn er nicht vorhanden ist, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Wenn Sie ändern die `Courses` Eigenschaft auf diese Weise können Sie explizite Eigenschaft Initialisierungscode im Controller entfernen.

In *Views\Instructor\Create.cshtml*, fügen Sie ein Office-Speicherort-Textfeld hinzu und Kontrollkästchen Kurs, nachdem die Hire date-Feld und vor der **Absenden** Schaltfläche.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Nachdem Sie den Code einfügen, beheben Sie Zeilenumbrüche und Einzüge, wie zuvor für die Seite "Bearbeiten".

Führen Sie die Seite "erstellen" und fügen Sie einen Kursleiter.

![Instructor mit Kurse erstellen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Behandeln von Transaktionen

Wie in beschrieben die [grundlegende CRUD-Funktionalität Lernprogramm](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), standardmäßig das Entity Framework implizit Transaktionen implementiert. Für Szenarien, in denen mehr, – beispielsweise gesteuert müssen wenn Vorgängen, die außerhalb von Entity Framework in einer Transaktion--enthalten sein sollen, finden Sie unter [arbeiten mit Transaktionen](https://msdn.microsoft.com/data/dn456843) auf MSDN.

## <a name="summary"></a>Zusammenfassung

Sie haben diese Einführung in die Arbeit mit verknüpften Daten jetzt abgeschlossen. Bisher haben in diesen Lernprogrammen Sie mit Code gearbeitet, die synchrone e/a ausführt. Können Sie die Anwendung, die Web-Server-Ressourcen effizienter nutzen, durch die Implementierung von asynchronen Code vornehmen, und Sie in den nächsten Lernprogrammen erledigen müssen.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie im [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Weiter](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
