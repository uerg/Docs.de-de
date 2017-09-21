---
title: ASP.NET Core MVC mit EF-Kern - Datenmodell - 5, 10
author: tdykstra
description: "In diesem Lernprogramm fügen Sie weitere Entitäten und Beziehungen und das Datenmodell anpassen, indem Sie Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben."
keywords: ASP.NET Core, Entity Framework Core-datenanmerkungen
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: dde50f766dc9842089cbb0561b8bd6e2d8e7c34f
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>Erstellen ein Modell mit komplexen Daten - EF-Core mit ASP.NET Core MVC-Lernprogramm (5 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

In den vorherigen Lernprogrammen, die Sie mit der ein einfaches Datenmodell, das aus drei Entitäten zusammengesetzt wurde gearbeitet haben. In diesem Lernprogramm fügen Sie weitere Entitäten und Beziehungen, und Sie müssen das Datenmodell anpassen, indem Sie Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben.

Wenn Sie fertig sind, werden die Entitätsklassen abgeschlossenen Datenmodell bilden, die in der folgenden Abbildung gezeigt wird:

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Anpassen des Datenmodells mithilfe von Attributen

In diesem Abschnitt sehen Sie zum Anpassen des Datenmodells mithilfe von Attributen, die die Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben. Klicken Sie dann Attribute in mehreren der folgenden Abschnitte erstellen Sie nun das vollständige Modell "School" Daten durch Hinzufügen von den Klassen Sie bereits erstellt haben, und erstellen neue Klassen für die verbleibenden Entitätstypen im Modell.

### <a name="the-datatype-attribute"></a>Das DataType-Attribut

Für Student Registrierung Datumsangaben Anzeigen aller Web Pages derzeit der Zeit zusammen mit dem Datum, obwohl alle, können Sie für dieses Feld ist das Datum. Mit Datenattributen für die Anmerkung, stellen Sie eine codeänderung, die das Anzeigeformat in jeder Ansicht behoben werden, die Daten anzeigt. Sehen Sie ein Beispiel dazu, dass Sie ein Attribut zum Hinzufügen der `EnrollmentDate` Eigenschaft in der `Student` Klasse.

In *Models/Student.cs*, Hinzufügen einer `using` -Anweisung für die `System.ComponentModel.DataAnnotations` Namespace und fügen `DataType` und `DisplayFormat` -Attribute verwenden, um die `EnrollmentDate` -Eigenschaft verwenden, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

Das `DataType`-Attribut wird verwendet, um einen Datentyp anzugeben, der spezifischer als der datenbankinterne Typ ist. In diesem Fall möchten wir nur zum Nachverfolgen der Datum nicht das Datum und die Uhrzeit. Die `DataType` Enumeration stellt für viele Datentypen, wie Datum, Uhrzeit, Währung, PhoneNumber, EmailAddress und vieles mehr. Das `DataType`-Attribut kann der Anwendung auch ermöglichen, typspezifische Features bereitzustellen. Beispielsweise kann ein `mailto:`-Link für `DataType.EmailAddress` erstellt werden, und eine Datumsauswahl kann in Browsern mit Unterstützung für HTML5 für `DataType.Date` bereitgestellt werden. Die `DataType` Attribut ausgibt HTML 5 `data-` (ausgesprochen als Daten Dash) Attribute, die HTML 5-Browser zu bringen. Die `DataType` Attribute bieten keine Validierung.

`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird. Standardmäßig wird das Feld "Daten" entsprechend der Standardformate basierend auf dem Server CultureInfo angezeigt.

Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

Die Einstellung `ApplyFormatInEditMode` gibt an, dass die Formatierung auch angewendet werden soll, wenn der Wert in einem Textfeld zur Bearbeitung angezeigt wird. (Sie möchten nicht, dass für einige Felder – z. B. für Währungsangaben, nicht das Währungssymbol in das Textfeld für die Bearbeitung empfehlenswert.)

Können Sie die `DisplayFormat` Attribut von selbst, aber es ist im Allgemeinen empfiehlt sich, verwenden Sie die `DataType` auch Attribut. Die `DataType` Attribut übermittelt, die die Semantik der Daten im Gegensatz zu wie auf dem Bildschirm gerendert werden soll, und bietet die folgenden Vorteile, die Sie nicht mit erhalten `DisplayFormat`:

* Der Browser HTML5-Funktionen aktivieren kann (z. B. zum Anzeigen eines Kalendersteuerelements, dem Gebietsschema entsprechende Währungssymbol, Links per e-Mail einige clientseitige Eingabe Validierung usw..).

* Standardmäßig rendert der Browser Daten mit dem ordnungsgemäßen auf Ihrem Gebietsschema basierenden Format.

Weitere Informationen finden Sie unter der [ \<input >-Tag Helper Dokumentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Führen Sie die app aus, wechseln Sie zu der Studenten Indexseite, und beachten Sie, dass die Zeiten für die Datumsangaben für die Registrierung nicht mehr angezeigt werden. "True" für jede Ansicht wird übereinstimmen, die der Student-Objektmodell verwendet.

![Studenten Indexseite mit Datumsangaben ohne Zeiten](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Das StringLength-Attribut

Sie können auch Regeln für die Überprüfung von Daten und Verwenden von Attributen Überprüfungsfehlermeldungen angeben. Die `StringLength` Attribut definiert die maximale Länge in der Datenbank und bietet clientseitige und serverseitige Validierung für ASP.NET MVC. Sie können auch die minimale Zeichenfolgenlänge in diesem Attribut angeben, aber der minimale Wert hat keine Auswirkungen auf das Datenbankschema.

Angenommen, Sie möchten sicherstellen, dass Benutzer nicht mehr als 50 Zeichen für einen Namen eingeben. Um diese Einschränkung hinzuzufügen, fügen Sie `StringLength` -Attribute verwenden, um die `LastName` und `FirstMidName` Eigenschaften, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Die `StringLength` Attribut wird nicht verhindern, dass einen Benutzer Leerzeichen für einen Namen eingeben. Sie können die `RegularExpression` Attribut Einschränkungen auf die Eingabe anwenden. Beispielsweise erfordert, dass der folgende Code das erste Zeichen Großbuchstaben bestehen muss und die übrigen Zeichen ein Buchstabe sein:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

Die `MaxLength` Attribut bietet ähnliche Funktionen der `StringLength` -Attribut aber nicht den clientseitigen Validierung.

Das Datenbankmodell hat jetzt so geändert, die eine Änderung im Datenbankschema erfordert. Verwenden Sie Migrationen, um das Schema aktualisieren, ohne Verlust von Daten, die Sie möglicherweise mit der Datenbank hinzugefügt haben, mithilfe der Benutzeroberfläche der Anwendung.

Speichern Sie die Änderungen zu, und erstellen Sie das Projekt. Klicken Sie dann öffnen Sie das Befehlsfenster zu, in den Projektordner, und geben Sie die folgenden Befehle:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

Die `migrations add` Befehl gibt eine Warnung aus, dass Daten verloren gehen können, da die Änderung die maximale Länge für zwei Spalten kürzer ist.  Migrationen erstellt eine Datei namens * \<Zeitstempel > _MaxLengthOnNames.cs*. Diese Datei enthält Code, in der `Up` -Methode, die die Datenbank entsprechend der aktuellen Datenmodell aktualisiert wird. Die `database update` -Befehl ausgeführt haben, diesen Code.

Der Zeitstempel, der den Dateinamen Migrationen vorangestellt wird vom Entity Framework verwendet, um die Migrationen zu bestellen. Sie können mehrere livemigrationen erstellen, vor dem Ausführen des Update-Database-Befehls, und klicken Sie dann alle die Migrationen in der Reihenfolge, in der sie erstellt wurden, angewendet werden.

Die app auszuführen, wählen Sie die **Studenten** auf **neu erstellen**, und geben Sie entweder mehr als 50 Zeichen ein. Beim Klicken auf **erstellen**, clientseitige Validierung zeigt eine Fehlermeldung angezeigt.

![Studenten Indexseite mit Zeichenfolge LÄNGENFEHLER](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Das Spaltenattribut

Attribute können auch steuern, wie die Klassen und Eigenschaften in der Datenbank zugeordnet werden. Angenommen, Sie den Namen verwendet haben `FirstMidName` für den Vornamen-Feld verwendet wird, weil das Feld kann auch ein zweiter Vorname enthalten. Die Datenbankspalte benannt werden, möchten jedoch `FirstName`, da der Benutzer, die Ad-hoc-Abfragen für die Datenbank geschrieben werden auf diesen Namen daran gewöhnt sind. Um diese Zuordnung vornehmen, können Sie die `Column` Attribut.

Die `Column` Attribut gibt an, wenn die Datenbank erstellt wurde, ist die Spalte von der `Student` Tabelle, zugeordnet der `FirstMidName` Eigenschaft den Namen `FirstName`. Das heißt, wenn Ihr Code bezieht sich auf `Student.FirstMidName`, die Daten aus stammen oder aktualisiert werden, der `FirstName` Spalte die `Student` Tabelle. Wenn Sie keinen Spaltennamen angeben, werden ihm den gleichen Namen wie den Namen der Eigenschaft zugewiesen.

In der *Student.cs* hinzufügen. eine `using` -Anweisung für `System.ComponentModel.DataAnnotations.Schema` und Hinzufügen der Spalte Name-Attribut, das `FirstMidName` Eigenschaft, die in den folgenden hervorgehobenen Code dargestellt:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Das Hinzufügen der `Column` Attribut ändert die Sicherung des Modells die `SchoolContext`, sodass die Datenbank keine Übereinstimmung wird nicht.

Speichern Sie die Änderungen zu, und erstellen Sie das Projekt. Klicken Sie dann öffnen Sie das Befehlsfenster zu, in den Projektordner, und geben Sie die folgenden Befehle zum Erstellen von einem anderen Migration:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

In **Objekt-Explorer von SQL Server**, öffnen Sie die Student-Tabellen-Designer durch Doppelklicken auf die **Student** Tabelle.

![Students-Tabelle in SSOX nach der Migration](complex-data-model/_static/ssox-after-migration.png)

Bevor Sie die ersten beiden Migrationen angewendet, wurden die Namensspalten von Typ nvarchar(MAX). Sie sind jetzt nvarchar(50) und der Spaltennamen FirstMidName FirstName geändert hat.

> [!Note]
> Wenn Sie versuchen, die kompiliert werden, bevor Sie alle der Entitätsklassen in den folgenden Abschnitten erstellt haben, erhalten Sie möglicherweise Compilerfehler angezeigt.

## <a name="final-changes-to-the-student-entity"></a>Letzte Änderungen an der Student-Entität

![Student-Entität](complex-data-model/_static/student-entity.png)

In *Models/Student.cs*, die von Ihnen hinzugefügte Code zuvor durch den folgenden Code ersetzen. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Das erforderliche Attribut

Die `Required` Attribut macht die erforderlichen Felder des Name-Eigenschaften. Die `Required` Attribut ist nicht erforderlich, für NULL-Typen wie Werttypen ("DateTime", "Int", double, float usw..). Typen, die nicht null sein können, werden automatisch als Pflichtfelder behandelt.

Könnten Sie entfernen die `Required` Attribut, und Ersetzen Sie ihn durch einen Parameter Mindestlänge für die `StringLength` Attribut:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Das Attribut anzeigen

Die `Display` Attribut gibt an, dass die Beschriftung für die Textfelder "Vorname", "Last Name", "Full Name" und "Registrierungsdatum" anstelle des Namens der Eigenschaft in jeder Instanz werden soll (der kein Leerzeichen, die die Wörter geteilt besitzt).

### <a name="the-fullname-calculated-property"></a>Die berechnete FullName-Eigenschaft

`FullName`ist eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch Verketten von zwei weitere Eigenschaften erstellt wird. Daher verfügt über einen Get-Accessor und keine `FullName` Spalte in der Datenbank generiert werden.

## <a name="create-the-instructor-entity"></a>Die Instructor-Entität erstellen

![Instructor-Entität](complex-data-model/_static/instructor-entity.png)

Erstellen Sie *Models/Instructor.cs*, ersetzen den Code durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Beachten Sie, dass mehrere Eigenschaften in den Entitäten Student "und" Dozenten identisch sind. In der [Implementieren von Vererbung](inheritance.md) weiter unten in diesem Lernprogramm müssen Sie diesen Code aus, um die Redundanz auszuschließen Umgestalten.

Sie können mehrere Attribute in einer Zeile einfügen, deshalb können Sie auch schreiben konnte die `HireDate` Attribute wie folgt:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Die Navigationseigenschaften CourseAssignments und OfficeAssignment

Die `CourseAssignments` und `OfficeAssignment` Eigenschaften sind Navigationseigenschaften.

Ein Kursleiter kann eine beliebige Anzahl von Kurse werden folgende Themen behandelt, sodass `CourseAssignments` als eine Auflistung definiert ist.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Wenn eine Navigationseigenschaft über mehrere Entitäten enthalten kann, muss der Typ in der Einträge hinzugefügt, gelöscht und aktualisiert werden können, eine Liste.  Sie können angeben, `ICollection<T>` oder ein Typ sein wie z. B. `List<T>` oder `HashSet<T>`. Bei Angabe von `ICollection<T>`, EF erstellt eine `HashSet<T>` Auflistung standardmäßig.

Der Grund, warum dies sind `CourseAssignment` Entitäten wird unten im Abschnitt über viele-zu-viele-Beziehungen erläutert.

Contoso University Geschäftsregeln der Status, der ein Kursleiter nur höchstens eine Niederlassung haben, kann daher die `OfficeAssignment` Eigenschaft enthält eine einzelne OfficeAssignment-Entität (die möglicherweise null, wenn keine Office zugewiesen ist).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Erstellen Sie die Entität OfficeAssignment

![OfficeAssignment-Entität](complex-data-model/_static/officeassignment-entity.png)

Erstellen Sie *Models/OfficeAssignment.cs* durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Das Key-Attribut

Es ist eine auf 0 (null) oder eine 1-Beziehung zwischen den Dozenten und den OfficeAssignment Entitäten. Eine bürozuweisung existiert nur in Bezug auf den Dozenten, dem sie zugewiesen ist, und primäre Schlüssel ist daher auch die Fremdschlüssel für die Instructor-Entität. Aber das Entity Framework nicht InstructorID automatisch als primären Schlüssel für diese Entität erkannt werden, da der Name der ID oder ClassnameID-Benennungskonvention folgen, nicht. Aus diesem Grund die `Key` Attribut wird verwendet, um sie als Schlüssel zu identifizieren:

```csharp
[Key]
public int InstructorID { get; set; }
```

Sie können auch die `Key` Attribut, wenn die Entität verfügt über einen eigenen Primärschlüssel, aber die Eigenschaft als ClassnameID oder ID etwas benennen möchten

Standardmäßig behandelt EF des Schlüssels als nicht-datenbankgeneriert, da die Spalte für eine identifizierende Beziehung ist.

### <a name="the-instructor-navigation-property"></a>Die Navigationseigenschaft für Dozenten

Die Instructor-Entität verfügt über einen `OfficeAssignment` Navigationseigenschaft (da eine bürozuweisung möglicherweise nicht über ein Kursleiter verfügen), und die Entität "OfficeAssignment" hat eine NULL- `Instructor` Navigationseigenschaft (da eine bürozuweisung kann nicht ohne einen Kursleiter--vorhanden `InstructorID` ist NULL-Werte zulässt). Wenn eine Instructor-Entität eine verknüpfte OfficeAssignment Entität verfügen, wird jede Entität einen Verweis auf die andere in die Navigationseigenschaft verfügen.

Sie setzen konnte eine `[Required]` Attribut für die Instructor-Navigationseigenschaft, um anzugeben, dass eine verwandte Dozenten muss, aber Sie nicht verfügen, da dies der `InstructorID` Fremdschlüssel (also auch die Taste, um diese Tabelle) ist NULL-Werte zulässt.

## <a name="modify-the-course-entity"></a>Ändern Sie die Kurs-Entität

![Kurs-Entität](complex-data-model/_static/course-entity.png)

In *Models/Course.cs*, die von Ihnen hinzugefügte Code zuvor durch den folgenden Code ersetzen. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Die Kurs-Entität verfügt über eine Fremdschlüsseleigenschaft `DepartmentID` hat das auf die verknüpfte Entität Department und zeigt eine `Department` Navigationseigenschaft.

Das Entity Framework erfordern nicht, dass Sie eine foreign Key-Eigenschaft mit dem Datenmodell hinzufügen, wenn eine Navigationseigenschaft für einer verknüpften Entität sind.  EF Fremdschlüssel in der Datenbank erstellt wird, werden im Bedarfsfall automatisch und erstellt [Shadowing von Eigenschaften](https://docs.microsoft.com/ef/core/modeling/shadow-properties) für sie. Jedoch müssen die Fremdschlüssel im Datenmodell kann Updates vornehmen, einfacher und effizienter. Z. B. Wenn Sie eine Entität Kurs bearbeiten abzurufen, die Entität Department ist null, wenn Sie es nicht geladen werden, also bei der Aktualisierung von der Entität Kurs Sie müssten zuerst die Entität Department abzurufen. Wenn die Fremdschlüsseleigenschaft `DepartmentID` enthalten ist in das Datenmodell, müssen Sie die Entität Department zu abzurufen, bevor Sie aktualisieren.

### <a name="the-databasegenerated-attribute"></a>Das DatabaseGenerated-Attribut

Die `DatabaseGenerated` -Attribut mit dem `None` Parameter auf die `CourseID` Eigenschaft gibt an, dass Primärschlüsselwerte sind vom Benutzer bereitgestellte anstatt von der Datenbank generiert.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Entity Framework geht standardmäßig davon aus, dass Primärschlüssel von der Datenbank generiert werden. Das ist in den meisten Fällen erwünscht. Allerdings für die Kurs-Entitäten, Sie verwenden eine benutzerdefinierte Kurs Zahl wie z. B. eine Reihe von 1000 für eine Abteilung, eine Reihe 2000 für eine andere Abteilung und usw..

Die `DatabaseGenerated` Attribut kann auch verwendet werden, um Default-Werte zu generieren, wie im Fall von Datenbankspalten verwendet, um das Datum aufzuzeichnen eine Zeile erstellt oder aktualisiert wurde.  Weitere Informationen finden Sie unter [Eigenschaften generiert](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key- und Navigation-Eigenschaften

Die Fremdschlüsseleigenschaften und Navigationseigenschaften der Entität Kurs reflektieren die folgenden Beziehungen:

Ein Kurs wird an eine Abteilung zugewiesen, daher besteht eine `DepartmentID` Fremdschlüssel und einem `Department` Navigationseigenschaft für die oben genannten Gründe.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Ein Kurs kann eine beliebige Anzahl von Lernende, haben also die `Enrollments` Navigationseigenschaft ist eine Sammlung:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Ein Kurs kann von mehreren Lehrkräfte vermittelten werden daher die `CourseAssignments` Navigationseigenschaft ist eine Auflistung (den Typ `CourseAssignment` wird erläutert [später](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Erstellen Sie die Entität Department

![Entität Department](complex-data-model/_static/department-entity.png)


Erstellen Sie *Models/Department.cs* durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Das Spaltenattribut

Zuvor Sie verwendet die `Column` Attribut, um die Zuordnung für die Namen von Spalten zu ändern. Im Code für die Entität Department der `Column` Attribut wird verwendet, um das Ändern der SQL datentypzuordnung so, dass die Spalte mit den SQL Server-Money-Datentyp in der Datenbank definiert werden:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Die Zuordnung ist im Allgemeinen nicht erforderlich, da das Entity Framework wählt die entsprechenden SQL Server-Datentyp, der basierend auf dem CLR-Typ, den Sie für die Eigenschaft zu definieren. Die CLR `decimal` wird zugeordnet zu SQL Server `decimal` Typ. In diesem Fall wissen, dass die Spalte wird Währungsbeträge belegen, und der Money-Datentyp eignet sich besser für die aber.

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key- und Navigation-Eigenschaften

Die folgenden Beziehungen entsprechen den foreign key- und Navigation-Eigenschaften:

Eine Abteilung kann oder besitzen möglicherweise nicht von einem Administrator und ein Administrator ist immer einen Kursleiter. Aus diesem Grund die `InstructorID` Eigenschaft ist als der Fremdschlüssel für die Instructor-Entität enthalten, und ein Fragezeichen wird hinzugefügt, nachdem die `int` Geben Sie die Bezeichnung, markieren Sie die Eigenschaft als NULL-Werte zulässt. Die Navigationseigenschaft heißt `Administrator` jedoch eine Instructor-Entität enthält:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Eine Abteilung möglicherweise viele Kurse, daher eine Navigationseigenschaft Kurse besteht:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Gemäß der Konvention ermöglicht das Entity Framework kaskadierendes Delete für die NULL-Fremdschlüssel und für viele-zu-viele-Beziehungen. Dies kann zu zirkuläre Cascade Delete Regeln führen, der eine Ausnahme verursacht wird, wenn Sie versuchen, eine Migration hinzuzufügen. Z. B. Wenn Sie die Eigenschaft Department.InstructorID als auf NULL festlegbar definiert haben, würden EF eine Cascade Löschregel konfigurieren den Dozenten löschen, beim Löschen der Abteilung, also nicht passieren sollen. Bei Bedarf Ihre Geschäftsregeln der `InstructorID` Eigenschaft nicht NULL-Werte zulässt, müssten Sie die folgende fluent-API-Anweisung verwenden, um kaskadierte Löschung auf der Beziehung zu deaktivieren:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Ändern Sie die Registrierung-Entität

![Registrierung-Entität](complex-data-model/_static/enrollment-entity.png)

In *Models/Enrollment.cs*, die von Ihnen hinzugefügte Code zuvor durch den folgenden Code ersetzen:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key- und Navigation-Eigenschaften

Die foreign Key-Eigenschaften und Navigationseigenschaften reflektieren die folgenden Beziehungen:

Ein Anmeldungsdatensatz ist für eine einzelnes Kurs, es ist also eine `CourseID` foreign Key-Eigenschaft und eine `Course` Navigationseigenschaft:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Ein Anmeldungsdatensatz ist für einen einzelnen Studenten, es ist also eine `StudentID` foreign Key-Eigenschaft und eine `Student` Navigationseigenschaft:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>n:n-Beziehungen

Besteht eine m: n-Beziehung zwischen den Entitäten Student "und" Kurs und die Registrierung Entität fungiert als Jointabelle m: n- *mit Nutzlast* in der Datenbank. "Mit der Nutzlast" bedeutet, dass die Enrollment-Tabelle zusätzliche Daten als Fremdschlüssel für den verknüpften Tabellen (in diesem Fall eine primary key- und eine Eigenschaft Grade) enthält.

Die folgende Abbildung zeigt, wie diese Beziehungen in einem Entitätsdiagramm aussieht. (Dieses Diagramm generiert wurde, verwenden die Entity Framework-Powertools für EF 6.x, erstellen das Diagramm ist nicht Teil des Lernprogramms wird gerade verwendet wird hier als veranschaulicht.)

![Student-Kurs m: n-Beziehung](complex-data-model/_static/student-course.png)

Jede Beziehungslinie hat eine 1 an einem Ende und ein Sternchen (*) am anderen Eintrag, der angibt, einer 1: n-Beziehung.

Wenn die Enrollment-Tabelle Grade Informationen nicht enthalten waren, müssten sie nur die zwei Fremdschlüssel CourseID und StudentID enthalten. In diesem Fall wäre es eine m: n-Jointabelle ohne Nutzlast (oder eine reine Jointabelle) in der Datenbank. Die Instructor und Kurs Entitäten sein, diese Art von m: n-Beziehung, und der nächste Schritt ist die Erstellung eine Entitätsklasse zur als eine Jointabelle ohne Nutzlast fungieren.

(EF 6.x unterstützt, die implizite Verknüpfung von Tabellen für m: n-Beziehungen, aber EF Core nicht. Weitere Informationen finden Sie unter der [Diskussion in der EF-Core-GitHub-Repository](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>Die CourseAssignment-Entität

![CourseAssignment-Entität](complex-data-model/_static/courseassignment-entity.png)

Erstellen Sie *Models/CourseAssignment.cs* durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Entitätsnamen verknüpfen

Eine Jointabelle in der Datenbank für die Instructor-Kurse-viele-zu-viele-Beziehung erforderlich ist, und er muss über eine Entitätenmenge dargestellt werden. Es ist üblich, benennen Sie eine Joinentität `EntityName1EntityName2`, die in diesem Fall wäre `CourseInstructor`. Allerdings wird empfohlen, dass Sie einen Namen auswählen, der die Beziehung beschreibt. Datenmodelle fangen einfache und hinausgeht, mit ohne-Nutzlast Joins häufig Nutzlasten später abrufen. Wenn Sie mit einem aussagekräftigen Entitätsname beginnen, müssen Sie den Namen später ändern. Im Idealfall würden die Joinentität einen eigene natürlichen (möglicherweise einzelne Wort)-Namen in der Business-Domäne haben. Beispielsweise konnte Büchern und Kunden über Bewertungen verknüpft werden. Für diese Beziehung `CourseAssignment` ist besser geeignet als `CourseInstructor`.

### <a name="composite-key"></a>Zusammengesetzter Schlüssel

Da der Fremdschlüssel keine NULL-Werte zulässt und zusammen eindeutig sind jede Zeile der Tabelle zu identifizieren, besteht keine Notwendigkeit für eine separate Primärschlüssel. Die *InstructorID* und *CourseID* Eigenschaften als einen zusammengesetzten Primärschlüssel fungieren soll. Die einzige Möglichkeit zum Identifizieren von zusammengesetzten Primärschlüssel zur EF ist die Verwendung der *fluent-API* (kann nicht mithilfe von Attributen ausgeführt werden). Gewusst wie: Konfigurieren von zusammengesetzten Primärschlüssel im nächsten Abschnitt angezeigt.

Des zusammengesetzten Schlüssels wird sichergestellt, dass während Sie über mehrere Zeilen für einen Kurs und mehrere Zeilen für einen Kursleiter verfügen können, Sie mehrere Zeilen für die gleiche Kursleiter und Kurs nicht zulässig. Die `Enrollment` Joinentität definiert einen eigenen Primärschlüssel aus, damit Duplikate dieser Art möglich sind. Um zu verhindern, dass solche Duplikate, konnte Sie einen eindeutigen Index für die foreign Key-Felder hinzufügen oder konfigurieren `Enrollment` mit einem zusammengesetzten Primärschlüssel ähnelt `CourseAssignment`. Weitere Informationen finden Sie unter [Indizes](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aktualisieren Sie den Datenbankkontext

Fügen Sie den folgenden hervorgehobenen Code in die *Data/SchoolContext.cs* Datei:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Dieser Code fügt das neuen Entitäten und konfiguriert die CourseAssignment Entität zusammengesetzten Primärschlüssel.

## <a name="fluent-api-alternative-to-attributes"></a>Fluent-API-Alternative zu Attributen

Der Code in der `OnModelCreating` Methode der `DbContext` -Klasse verwendet die *fluent-API* EF Verhalten konfigurieren. Die API wird "fluent" bezeichnet, da es häufig von einer Reihe von Methodenaufrufen, die zusammen in einer einzigen Anweisung, wie in diesem Beispiel aus Vermittlungsstellen verwendet die [EF Basisdokumentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

In diesem Lernprogramm verwenden Sie die fluent-API nur für datenbankzuordnung, die Sie mit Attributen können nicht. Allerdings auch können die fluent-API Sie die meisten Formatierung, Überprüfung und Zuordnungsregeln an, denen Sie durchführen können, mithilfe von Attributen angeben. Einige Attribute wie z. B. `MinimumLength` kann nicht mit der fluent-API angewendet werden. Wie bereits erwähnt, `MinimumLength` ändert nicht das Schema gilt nur für eine Validierungsregel Client und Server-Seite.

Einige Entwickler bevorzugen die fluent-API ausschließlich, damit sie ihre Entitätsklassen "bereinigen" bleibt Sie können Attribute und fluent-API kombinieren, wenn werden sollen, und es gibt einige Anpassungen, die nur mithilfe der fluent-API ausgeführt werden können, aber im Allgemeinen wird empfohlen, wählen eine dieser beiden Ansätze und konsistent, so weit wie möglich verwenden. Wenn Sie beides verwenden, beachten Sie, dass immer ein Konflikt vorliegt, Fluent-API für Attribute überschreibt.

Weitere Informationen zu Attributen im Vergleich zu fluent-API finden Sie unter [Methoden der Konfiguration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Anzeigen von Entitätsbeziehungen-Diagramm

Die folgende Abbildung zeigt das Diagramm, das den Entity Framework-Power-Tools für das abgeschlossene Modell "School" zu erstellen.

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

Neben den Zeilen 1: n-Beziehung (1 bis \*), sehen Sie hier zu 0 (null) oder eins eine Beziehungslinie (1 zu 0.. 1) zwischen den Entitäten Dozenten und OfficeAssignment und 0 (null)-oder-1-n-Beziehung (0.. 1 auf *) zwischen den Instructor "und" Department-Entitäten.

## <a name="seed-the-database-with-test-data"></a>Ausgangswert für die Datenbank mit Testdaten

Ersetzen Sie den Code in der *Data/DbInitializer.cs* Datei durch den folgenden Code um Seed-Daten für die neuen Entitäten zu gewährleisten Sie erstellt haben.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Im ersten Lernprogramm haben Sie gesehen, wird die meisten dieser Code einfach neue Entitätsobjekte erstellt und lädt Beispieldaten in die Eigenschaften nach Bedarf zu Testzwecken. Beachten Sie, wie die m: n-Beziehungen behandelt werden: der Code erstellt Beziehungen durch das Erstellen von Entitäten in der `Enrollments` und `CourseAssignment` Entitätenmengen verknüpfen.

## <a name="add-a-migration"></a>Fügen Sie eine migration

Speichern Sie die Änderungen zu, und erstellen Sie das Projekt. Klicken Sie dann das Befehlsfenster zu öffnen, in den Projektordner, und geben Sie die `migrations add` Befehl (nicht ausführen den Update-Database-Befehl noch):

```console
dotnet ef migrations add ComplexDataModel
```

Sie erhalten eine Warnung bezüglich eines möglichen Datenverlusts.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Falls Sie versucht haben, führen Sie die `database update` an diesem Punkt Befehl (nicht gerade noch), erhalten Sie die folgende Fehlermeldung:

> Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK_dbo. Course_dbo. Department_DepartmentID". Der Konflikt trat in der Datenbank "ContosoUniversity", Tabelle "Dbo. Abteilung", Spalte"DepartmentID".

In einigen Fällen, wenn Sie Migrationen mit vorhandenen Daten ausführen, müssen Sie zum Einfügen von Stubdaten in die Datenbank foreign Key-Einschränkungen nicht erfüllen. Der generierte Code in der `Up` -Methode die Course-Tabelle NULL-Fremdschlüssel "DepartmentID" hinzugefügt. Wenn Zeilen vorhanden bereits in die Course-Tabelle beim Ausführen des Codes sind die `AddColumn` Vorgang schlägt fehl, da SQL Server nicht weiß, welcher Wert in die Spalte einfügen, die darf nicht null sein. In diesem Lernprogramm führen Sie die Migration auf eine neue Datenbank, jedoch in einer produktionsanwendung müssten Sie stellen die Migration vorhandene Daten verarbeiten, daher die folgenden Anweisungen ein Beispiel hierzu anzeigen.

Stellen diese Migration arbeiten mit vorhandenen Daten, die Sie so ändern Sie den Code so erteilen Sie der neuen Spalte einen Standardwert verfügen, und erstellen Sie eine Abteilung ein Stub mit dem Namen "Temp" als Standard-Abteilung zu agieren. Folglich vorhandenen Kurs Zeilen werden alle werden im Zusammenhang mit der Abteilung "Temp" nach der `Up` Methode ausgeführt wird.

* Öffnen der *{timestamp}_ComplexDataModel.cs* Datei. 

* Kommentieren Sie die Codezeile, die die Course-Tabelle die Spalte "DepartmentID" hinzufügt.

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Fügen Sie nach dem Code, der die Department-Tabelle erstellt die folgenden hervorgehobenen Code hinzu:

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

In einer produktionsanwendung würde Sie schreiben Code oder Skripts für die Abteilung Zeilen hinzuzufügen, und die neuen Zeilen der Abteilung Kurs Zeilen beziehen. Die Abteilung "Temp" oder den Standardwert für die Spalte Course.DepartmentID würden Sie dann nicht mehr benötigen.

Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.

## <a name="change-the-connection-string-and-update-the-database"></a>Ändern Sie die Verbindungszeichenfolge und aktualisieren die Datenbank

Sie haben jetzt die neuen Code, der `DbInitializer` -Klasse, die mit einer leeren Datenbank Ausgangswert Daten für die neue Entitäten hinzugefügt. Um EF erstellen Sie eine neue leere Datenbank zu machen, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge in *appsettings.json* ContosoUniversity3 oder einen anderen Namen, die Sie verwenden, die Sie nicht auf dem Computer verwendet haben.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Speichern Sie die Änderung zu *appsettings.json*.

> [!NOTE]
> Als Alternative zum Ändern des Datenbanknamens können Sie die Datenbank löschen. Verwendung **Objekt-Explorer von SQL Server** (SSOX) oder die `database drop` CLI-Befehl:
> ```console
> dotnet ef database drop
> ```

Nachdem Sie den Datenbanknamen geändert oder die Datenbank gelöscht haben, führen Sie die `database update` im Befehlsfenster die Migrationen auszuführende Befehl.

```console
dotnet ef database update
```

Ausführen die app, die dazu führen, dass die `DbInitializer.Initialize` Methode zum Ausführen und die neue Datenbank aufgefüllt.

Öffnen Sie die Datenbank im SSOX, wie zuvor, und erweitern Sie die **Tabellen** Knoten, um anzuzeigen, dass alle Tabellen erstellt wurden. (Wenn Sie aus der früheren Zeitpunkt öffnen SSOX weiterhin bestehen, klicken Sie auf die **aktualisieren** Schaltfläche.)

![Tabellen in SSOX](complex-data-model/_static/ssox-tables.png)

Führen Sie die app, um den Initialisierer Code auslösen, der die Datenbank startet.

Mit der rechten Maustaste die **CourseAssignment** Tabelle, und wählen Sie **Ansichtsdaten** um sicherzustellen, dass es Daten enthält.

![CourseAssignment Daten in SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt eine komplexere Datenmodell und die entsprechende Datenbank. Im folgenden Lernprogramm erfahren Sie mehr über Zugriff auf zugehörige Daten.

>[!div class="step-by-step"]
[Zurück](migrations.md)
[Weiter](read-related-data.md)  
