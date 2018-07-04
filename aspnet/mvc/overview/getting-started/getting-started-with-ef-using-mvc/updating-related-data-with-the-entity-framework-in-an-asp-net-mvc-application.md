---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lesen verwandter Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 05b2f92155a4c3cac7ec8edd36b8ac6724b21888
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370930"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Lesen verwandter Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


In vorherigen Tutorial in Sie verwandte Daten angezeigt. In diesem Tutorial werden verwandte Daten aktualisiert werden. Für die meisten Beziehungen kann dies erfolgen durch Aktualisieren von Eigenschaften oder Felder mit Fremdschlüsseln. Für m: n Beziehungen nicht Entity Framework die verknüpften Tabelle direkt verfügbar machen, damit Sie hinzufügen und Entfernen von Entitäten aus die entsprechenden Navigationseigenschaften.

In den folgenden Abbildungen werden die Seiten dargestellt, mit denen Sie arbeiten werden.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Bearbeiten von "Instructor" mit Kursen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Anpassen der Seiten „Erstellen“ und „Bearbeiten“ für Kurse

Wenn eine neue Kursentität erstellt wird, muss diese in Beziehung zu einer vorhandenen Abteilung stehen. Um dies zu vereinfachen, enthält der Gerüstcode Controllermethoden und Ansichten zum „Erstellen“ und „Bearbeiten“, die eine Dropdownliste enthalten, aus denen der Fachbereich ausgewählt werden kann. Im Dropdown-Liste wird die `Course.DepartmentID` Fremdschlüsseleigenschaft und das ist alles in Entity Framework benötigt, um das Laden der `Department` Navigationseigenschaft mit dem entsprechenden `Department` Entität. Verwenden Sie den Gerüstcode, aber nehmen Sie kleine Änderungen vor, um die Fehlerbehandlung hinzuzufügen und die Dropdownliste zu sortieren.

In *CourseController.cs*, löschen Sie die vier `Create` und `Edit` Methoden und durch den folgenden Code ersetzen:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Fügen Sie die folgenden `using` -Anweisung am Anfang der Datei:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Die `PopulateDepartmentsDropDownList` Methode ruft eine Liste aller Abteilungen, die nach Namen sortiert ab, erstellt eine `SelectList` Auflistung eine Dropdown-Liste, und übergibt diese Auflistung an die Ansicht in eine `ViewBag` Eigenschaft. Die Methode akzeptiert den optionalen `selectedDepartment`-Parameter, über den der Code das Element angeben kann, das ausgewählt wird, wenn die Dropdownliste gerendert wird. Die Ansicht übergibt den Namen `DepartmentID` auf die [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) Helper und das Hilfsprogramm gesucht werden soll, dann weiß die `ViewBag` Objekt für eine `SelectList` mit dem Namen `DepartmentID`.

Die `HttpGet` `Create` Methodenaufrufe der `PopulateDepartmentsDropDownList` -Methode ohne das ausgewählte Element festlegen, da für einen neuen Kurs die Abteilung noch nicht bekannt ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Die `HttpGet` `Edit` -Methode legt das ausgewählte Element anhand der ID des Fachbereichs, die bereits den bearbeiteten Kurs zugewiesen ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Die `HttpPost` -Methoden für `Create` und `Edit` enthalten außerdem Code, der das ausgewählte Element festlegt, wenn sie die Seite nach einem Fehler erneut:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Dieser Code stellt sicher, dass es sich bei, wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, beliebige Abteilung ausgewählt wurde erhalten bleibt.

Die kursansichten sind bereits mit den Dropdownlisten für das Feld "Department" erstellt haben, möchten jedoch nicht die Beschriftung "DepartmentID" für dieses Feld, damit stellen Sie die folgenden hervorgehobenen ändern Sie in der *Views\Course\Create.cshtml* Datei Ändern Sie die Beschriftung an.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Stellen Sie die gleiche Änderung in *Views\Course\Edit.cshtml*.

Der gerüstbauer erstellen nicht normalerweise einen Primärschlüssel, da der Schlüsselwert wird von der Datenbank generiert und kann nicht geändert werden und ist nicht keinen sinnvollen Wert, der Benutzern angezeigt werden. Für Course-Entitäten umfasst der gerüstbauer ein Textfeld für die `CourseID` Feld, da er, die versteht die `DatabaseGeneratedOption.None` Attribut bedeutet, dass der Benutzer sollte sein Geben Sie den Primärschlüsselwert. Versteht aber nicht, da die Zahl sinnvoll ist es in den anderen Ansichten angezeigt werden, daher Sie manuell hinzugefügt müssen werden sollen.

In *Views\Course\Edit.cshtml*, einen Feld für die Kursnummer vor dem Hinzufügen der **Titel** Feld. Da es sich um den primären Schlüssel handelt, wird angezeigt, aber es kann nicht geändert werden.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Es ist bereits ein ausgeblendetes Feld (`Html.HiddenFor` Helper) für die Kursnummer in der Bearbeitungsansicht. Hinzufügen einer *Html.LabelFor* Hilfsprogramm nicht vermeiden, müssen dem ausgeblendeten Feld, da es nicht dazu, dass die Kursnummer in die bereitgestellten Daten eingeschlossen werden, wenn der Benutzer klickt **speichern** auf der Seite "Bearbeiten".

In *Views\Course\Delete.cshtml* und *Views\Course\Details.cshtml*, ändern Sie die Beschriftung ein Department Name von "Name", "Department", und fügen Sie einen Feld für die Kursnummer vor der **Titel**  Feld.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Führen Sie die **erstellen** Seite (auf der Indexseite des Kurses angezeigt, und klicken Sie auf **neu erstellen**), und geben Sie Daten für einen neuen Kurs:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Klicken Sie auf **Erstellen**. Die Indexseite des Kurses wird mit den neuen Kurs zur Liste hinzugefügt. Der Fachbereichsname in der Indexseitenliste wurde der Navigationseigenschaft entnommen und deutet darauf hin, dass die Beziehung ordnungsgemäß festgelegt wurde.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Führen Sie die **bearbeiten** Seite (auf der Indexseite des Kurses angezeigt, und klicken Sie auf **bearbeiten** an einem Kurs).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ändern Sie die Daten auf der Seite, und klicken Sie auf **Speichern**. Die Indexseite des Kurses wird mit den aktualisierten Kursdaten angezeigt.

## <a name="adding-an-edit-page-for-instructors"></a>Hinzufügen einer Seite "Bearbeiten" für Dozenten

Bei der Bearbeitung eines Dozentendatensatzes sollten Sie auch die Bürozuweisung des Dozenten aktualisieren. Die `Instructor` Entität verfügt über eine 1: 0 (null)-oder-1-Beziehung mit der `OfficeAssignment` Entität, was bedeutet, müssen Sie die folgenden Situationen behandeln:

- Wenn der Benutzer die bürozuweisung löscht und ursprünglich einen Wert vorhanden, müssen Sie diese entfernen und löschen Sie die `OfficeAssignment` Entität.
- Wenn der Benutzer eine gibt aus, und diese ursprünglich leer war, müssen Sie ein neues erstellen `OfficeAssignment` Entität.
- Wenn der Benutzer den Wert einer bürozuweisung ändert, müssen Sie den Wert in einer vorhandenen ändern `OfficeAssignment` Entität.

Open *InstructorController.cs* und sehen Sie sich die `HttpGet` `Edit` Methode:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Der eingerüstete Code hier nicht Ihren vorstellungen. Ist es einrichten, bis Daten für eine Dropdown-Liste, aber Sie müssen Sie ein Textfeld ist. Ersetzen Sie diese Methode, mit dem folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Dieser Code löscht die `ViewBag` Anweisung und fügt Sie eager Loading für die zugeordnete `OfficeAssignment` Entität. Kann nicht ausgeführt werden eager Loading mit der `Find` -Methode, also die `Where` und `Single` Methoden werden stattdessen verwendet, um den Dozenten zu wählen.

Ersetzen Sie die `HttpPost` `Edit` Methode durch den folgenden Code. Updates für Office-Zuweisung behandelt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Der Verweis auf `RetryLimitExceededException` erfordert eine `using` -Anweisung hinzufügen, mit der rechten Maustaste `RetryLimitExceededException`, und klicken Sie dann auf **beheben** - **mit System.Data.Entity.Infrastructure**.

![Lösen Sie Ausnahmen bei Wiederholungsversuchen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Der Code führt Folgendes aus:

- Ändert den Methodennamen, `EditPost` weil die Signatur jetzt identisch ist die `HttpGet` Methode (die `ActionName` Attribut gibt an, dass die URL/Edit / weiterhin verwendet wird).
- Ruft die aktuelle Entität `Instructor` von der Datenbank über Eager Loading für die Navigationseigenschaft `OfficeAssignment` ab. Dies ist identisch mit was Sie in haben der `HttpGet` `Edit` Methode.
- Aktualisiert die abgerufene Entität `Instructor` mit Werten aus der Modellbindung. Die [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) verwendete Überladung ermöglicht es Ihnen, *Whitelist* die Eigenschaften, die Sie einschließen möchten. Dies verhindert, dass zu viele Angaben, wie unter [im zweiten Artikel](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Wenn der Bürostandort leer ist, wird die `Instructor.OfficeAssignment` Eigenschaft auf null, damit die zugehörige Zeile in der `OfficeAssignment` Tabelle gelöscht werden.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Speichert die Änderungen in der Datenbank.

In *Views\Instructor\Edit.cshtml*, nachdem die `div` Elemente für die **Hire Date** Feld Hinzufügen eines neuen Felds zum Bearbeiten der Office-Speicherort:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Führen Sie die Seite (Wählen Sie die **Dozenten** Registerkarte, und klicken Sie dann auf **bearbeiten** für einen Dozenten). Ändern Sie den **Standort des Büros**, und klicken Sie auf **Speichern**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Hinzufügen von Kurszuweisungen dem Dozenten bearbeiten Seite

Dozenten können eine beliebige Anzahl von Kursen unterrichten. Jetzt soll die Dozentenseite „Bearbeiten“ verbessert werden, indem es ermöglicht wird, Kurszuweisungen über eine Reihe von Kontrollkästchen zu verändern. Dies wird im folgenden Screenshot dargestellt:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Die Beziehung zwischen der `Course` und `Instructor` Entitäten ist m: n-, was bedeutet, Sie müssen keinen direkten Zugriff auf die Fremdschlüsseleigenschaften die sind in der verknüpften Tabelle. Stattdessen hinzufügen und Entfernen von Entitäten in und aus der `Instructor.Courses` Navigationseigenschaft.

Die Benutzeroberfläche, über die Sie ändern können, welchen Kursen ein Dozent zugewiesen ist, besteht aus einer Reihe von Kontrollkästchen. Für jeden Kurs in der Datenbank wird ein Kontrollkästchen angezeigt. Die Kontrollkästchen, denen der Dozent zu diesem Zeitpunkt zugewiesen ist, sind aktiviert. Der Benutzer kann Kontrollkästchen aktivieren oder deaktivieren, um Kurszuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer wäre, würde wahrscheinlich eine andere Methode für das Darstellen der Daten in der Ansicht verwenden möchten, aber verwenden Sie die gleiche Methode zum Bearbeiten von Navigationseigenschaften zum Erstellen oder Löschen von Beziehungen.

Verwenden Sie eine Ansichtsmodellklasse, um Daten für die Ansicht bereitzustellen, um eine Liste mit Kontrollkästchen zu erstellen. Erstellen Sie *Schoolviewmodels* in die *ViewModels* Ordner, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

In *InstructorController.cs*, ersetzen Sie die `HttpGet` `Edit` Methode durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Über den Code wird für die `Courses`-Navigationseigenschaft Eager Loading hinzugefügt und die neue `PopulateAssignedCourseData`-Methode aufgerufen, um über die Ansichtsmodellklasse `AssignedCourseData` Informationen für das Kontrollkästchenarray zur Verfügung zu stellen.

Der Code in die `PopulateAssignedCourseData` -Methode liest alle `Course` Modellklasse für Entitäten, um eine Liste der Kurse, die mithilfe der Ansicht zu laden. Für jeden Kurs überprüft der Code, ob dieser in der `Courses`-Navigationseigenschaft des Dozenten vorhanden ist. Um effiziente Suche nach der Erstellung überprüfen, ob ein Kurs dem Dozenten zugewiesen ist, die dem Dozenten zugewiesene Kurse abgelegt sind eine [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) Auflistung. Die `Assigned` -Eigenschaftensatz auf `true` Kurse der Dozent zugewiesen ist. Die Ansicht verwendet diese Eigenschaft, um zu bestimmen, welche Kontrollkästchen als aktiviert angezeigt werden sollen. Zum Schluss wird die Liste übergeben, an die Ansicht in einem `ViewBag` Eigenschaft.

Fügen Sie als nächstes den Code hinzu, der ausgeführt wird, wenn der Benutzer auf **Speichern** klickt. Ersetzen Sie die `EditPost` Methode durch den folgenden Code, der eine neue Methode aufruft, die aktualisiert die `Courses` Navigationseigenschaft der `Instructor` Entität. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Die Methodensignatur unterscheidet sich jetzt von der `HttpGet` `Edit` Methode, sodass der Name der Methode von ändert `EditPost` an `Edit`.

Da die Sicht eine Auflistung von nicht `Course` Entitäten die, die modellbindung kein automatisches update der `Courses` Navigationseigenschaft. Verwenden Sie statt des Modellbinders zum Aktualisieren der `Courses` Navigationseigenschaft Sie hierfür das in der neuen `UpdateInstructorCourses` Methode. Aus diesem Grund müssen Sie die `Courses`-Eigenschaft von der Modellbindung ausschließen. Dies erfordert keine Änderungen an der aufrufende Code [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) da Sie verwenden die *Whitelists* überladen und `Courses` nicht in der Liste enthalten ist.

Wenn keine Kontrollkästchen aktiviert ist, wird der Code in `UpdateInstructorCourses` initialisiert die `Courses` Navigationseigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Der Code führt dann eine Schleife durch alle Kurse in der Datenbank aus und überprüft jeden Kurs im Hinblick auf die Kurse, die zu diesem Zeitpunkt dem Dozenten zugewiesen sind, und denen, die in der Ansicht aktiviert wurden. Die beiden letzten Auflistungen werden in `HashSet`-Objekten gespeichert, um Suchvorgänge effizienter zu gestalten.

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser der Auflistung in der Navigationseigenschaft hinzugefügt.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser aus der Auflistung in der Navigationseigenschaft gelöscht.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

In *Views\Instructor\Edit.cshtml*, Hinzufügen einer **Kurse** Feld mit einem Array von Kontrollkästchen durch Hinzufügen des folgenden code direkt nach der `div` Elemente für die `OfficeAssignment` Feld und Bevor Sie die `div` -Element für die **speichern** Schaltfläche:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Nach dem Einfügen der Code, wenn Zeilenumbrüche und Einzug nicht aussehen wie hier, alles, damit sie aussieht, was Sie sehen hier manuell korrigieren. Der Einzug muss nicht perfekt sein, die Zeilen `@</tr><tr>`, `@:<td>`, `@:</td>` und `@</tr>` müssen jedoch, wie dargestellt, jeweils in einer einzelnen Zeile stehen. Ansonsten wird ein Laufzeitfehler ausgelöst.

Dieser Code erstellt eine HTML-Tabelle mit drei Spalten. Jede Spalte enthält ein Kontrollkästchen gefolgt von einem Titel, der aus der Kursnummer und dem Kurstitel besteht. Die Kontrollkästchen haben denselben Namen ("SelectedCourses"), der die modellbindung informiert werden, die sie als Gruppe behandelt werden. Die `value` Attribut der einzelnen Kontrollkästchen wird festgelegt, auf den Wert der `CourseID.` , wenn die Seite zurückgesendet wird, übergibt die modellbindung ein Array mit dem Controller an, das umfasst die `CourseID` Werte für die nur die Kontrollkästchen ausgewählt sind.

Wenn Sie die Kontrollkästchen ursprünglich gerendert wurden, haben, die dem Dozenten zugewiesene Kurse sind `checked` Attribute, die diese aktivieren (angezeigt, die sie aktiviert werden).

Nach dem Ändern von kurszuweisungen, sollten Sie in der Lage, die Änderungen zu überprüfen, zum Beenden die Website der `Index` Seite. Aus diesem Grund müssen Sie eine Spalte in der Tabelle auf dieser Seite hinzufügen. In diesem Fall Sie verwenden, müssen die `ViewBag` Objekt, da die Informationen, die Sie anzeigen möchten, bereits in ist die `Courses` Navigationseigenschaft der `Instructor` Entität, die Sie zur Seite wie das Modell übergeben werden.

In *Views\Instructor\Index.cshtml*, Hinzufügen einer **Kurse** Überschrift unmittelbar nach der **Office** Überschrift, wie im folgenden Beispiel gezeigt:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Fügen Sie eine neue Detailzelle unmittelbar nach der Detailzelle der Office-Speicherort hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Führen Sie die **"Instructor" Index** Seite, um jeden Dozenten zugewiesene Kurse finden Sie unter:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie auf **bearbeiten** für einen Dozenten auf der Seite "Bearbeiten" finden Sie unter.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Ändern Sie einige kurszuweisungen, und klicken Sie auf **speichern**. Die Änderungen werden auf der Indexseite angezeigt.

 Hinweis: Der Ansatz zum Bearbeiten von Dozenten hier funktioniert gut, wenn eine begrenzte Anzahl von Kursen vorhanden ist. Bei umfangreicheren Auflistungen wären eine andere Benutzeroberfläche und eine andere Aktualisierungsmethode erforderlich.  
 

## <a name="update-the-deleteconfirmed-method"></a>Aktualisieren Sie die Methode "deleteconfirmed"

In *InstructorController.cs*, löschen Sie die `DeleteConfirmed` Methode, und fügen Sie den folgenden code an.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Dieser Code macht die folgende Änderung:

- Wenn der Dozent als Administrator für jede Abteilung zugewiesen ist, wird die dozentenzuweisung aus dieser Abteilung entfernt. Ohne diesen Code erhalten Sie einen Fehler der referenziellen Integrität, würden Sie versuchen, einen Dozenten zu löschen, die als Administrator für eine Abteilung zugewiesen wurde.

Dieser Code behandelt nicht das Szenario ein "Instructor" als Administrator für mehrere Abteilungen zugewiesen. Im letzten Tutorial fügen Sie Code, der verhindert, dass dieses Szenario durchgeführt.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Hinzufügen von einem Bürostandort und von Kursen zu der Seite „Erstellen“

In *InstructorController.cs*, löschen Sie die `HttpGet` und `HttpPost` `Create` Methoden, und fügen Sie dann den folgenden Code an ihrer Stelle:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Dieser Code ist ähnlich, was Sie für den Edit-Methoden gesehen, mit dem Unterschied, dass anfänglich keine Kurse ausgewählt sind. Die `HttpGet` `Create` Methodenaufrufe der `PopulateAssignedCourseData` Methode nicht verwendet werden, weil möglicherweise Kurse ausgewählt sind, aber in sortieren, zu der eine leere Auflistung für die `foreach` Schleife in der Ansicht (andernfalls den Anzeigecode würden eine null-Verweis-Ausnahme auslösen ).

Die HttpPost erstellen-Methode fügt jeden der ausgewählten Kurse mit der Navigationseigenschaft Kurse, bevor Sie den Code ein, der auf Überprüfungsfehler überprüft und der Datenbank die neue "Instructor" hinzugefügt. Kurse hinzugefügt werden, selbst wenn es Modellfehler gibt, damit bei der Modellfehler (beispielsweise ein Benutzer ein ungültiges Datum verschlüsselt) vorliegen, wenn die Seite mit einer Fehlermeldung erneut angezeigt wird, werden automatisch alle Course-Auswahl, die vorgenommen wurden wiederhergestellt.

Beachten Sie, dass Sie die `Courses`-Navigationseigenschaft als leere Auflistung initialisieren müssen, wenn Sie dieser Kurse hinzufügen möchten:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Wenn Sie dies nicht im Controllercode durchführen möchten, können Sie dies auch im Dozentenmodell tun, indem Sie den Eigenschaftengetter ändern, um falls nötig automatisch die Auflistung zu erstellen. Dies wird im folgenden Code dargestellt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Wenn Sie die `Courses`-Eigenschaft auf diese Weise ändern, können Sie den expliziten Code zum Initialisieren der Eigenschaft aus dem Controller entfernen.

In *Views\Instructor\Create.cshtml*, fügen Sie eine Office-Speicherort-Textfeld hinzu und Kurs Kontrollkästchen aus, nachdem die Hire date-Feld und vor der **senden** Schaltfläche.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Nachdem Sie den Code einfügen, beheben Sie Zeilenumbrüche und Einzug wie zuvor für die Seite "Bearbeiten".

Führen Sie die Seite "erstellen" und fügen Sie einen Dozenten.

![Erstellen "Instructor" mit Kursen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Verarbeiten von Transaktionen

Siehe die [grundlegende CRUD-Funktionen Tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), standardgemäß implementiert Entity Framework implizit Transaktionen. Weitere Szenarien, in dem Sie genauer – z. B. kontrollieren müssen wenn die Vorgänge, die außerhalb von Entity Framework in einer Transaktion ausgeführt werden sollen, finden Sie unter [arbeiten mit Transaktionen](https://msdn.microsoft.com/data/dn456843) auf MSDN.

## <a name="summary"></a>Zusammenfassung

Sie haben nun diese Einführung in das Arbeiten mit zugehörigen Daten abgeschlossen. Bisher haben in diesen Tutorials Sie mit Code gearbeitet, die synchrone e/a ausführt. Können Sie die Anwendung mit dem Webserver Ressourcen effizienter nutzen, durch die Implementierung von asynchronen Codes vornehmen, und das ist was Sie im nächsten Tutorial erledigen.

Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können. Sie können auch neue Themen anfordern [anzeigen Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Ressourcen des Entity Framework finden Sie im [ASP.NET-Datenzugriff – empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
