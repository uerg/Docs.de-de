---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 3 | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889253"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 3
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filter-, Sortier- und Gruppieren von Daten

Im vorherigen Lernprogramm Sie verwendet die `EntityDataSource` -Steuerelement zum Anzeigen und Bearbeiten von Daten. In diesem Lernprogramm filtern, Sortieren und Gruppieren von Daten. Hierfür durch Festlegen der Eigenschaften der `EntityDataSource` -Steuerelement, die Syntax unterscheidet sich von anderen Steuerelementen für Datenquellen. Wie Sie sehen, Sie können jedoch die `QueryExtender` Steuerelement, um diese Unterschiede zu minimieren.

Ändern Sie die *Students.aspx* Seite für Studenten, gefiltert nach Namen, und suchen Sie auf Name sortieren. Ändern Sie auch die *Courses.aspx* Seite Kurse für die ausgewählte Abteilung und suchen Sie nach Kursen anhand des Namens angezeigt. Zum Schluss fügen Sie Student Statistiken, die die *About.aspx* Seite.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Mithilfe der EntityDataSource "Where"-Eigenschaft zum Filtern von Daten

Öffnen der *Students.aspx* Seite, die Sie im vorherigen Lernprogramm erstellt haben. Wie zurzeit konfiguriert, die `GridView` -Steuerelement auf der Seite zeigt die Namen aus der `People` Entitätenmenge. Sie möchten jedoch nur Studenten, zeigen Sie dazu finden `Person` Entitäten, die Registrierung von nicht-Null-Datumswerte unterstützen.

Wechseln Sie zur **Entwurf** anzeigen und Auswählen der `EntityDataSource` Steuerelement. In der **Eigenschaften** legen die `Where` Eigenschaft `it.EnrollmentDate is not null`.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Die Syntax, die Sie, in verwenden der `Where` Eigenschaft von der `EntityDataSource` Steuerelement Entity SQL ist. Entity SQL ist ähnlich wie Transact-SQL, aber er ist für die Verwendung mit Entitäten statt Datenbankobjekte angepasst. Im Ausdruck `it.EnrollmentDate is not null`, das Wort `it` einen Verweis auf die von der Abfrage zurückgegebenen Entität darstellt. Aus diesem Grund `it.EnrollmentDate` bezieht sich auf die `EnrollmentDate` Eigenschaft von der `Person` Entität, die `EntityDataSource` gibt steuern.

Führen Sie die Seite. Liste der Studenten enthält jetzt nur Studenten. (Es sind keine Zeilen angezeigt, in denen keine Registrierungsdatum vorhanden ist.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Mithilfe der EntityDataSource 'OrderBy'-Eigenschaft Auftragsdaten

Sie sollten auch diese Liste sortiert werden, wenn er erstmals angezeigt wird. Mit der *Students.aspx* Seite noch geöffnet, in **Entwurf** anzeigen, und mit der `EntityDataSource` Steuerelement noch ausgewählt, in der **Eigenschaften** Fenster Satz der  **OrderBy** Eigenschaft `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Führen Sie die Seite. Liste der Studenten ist jetzt in der Reihenfolge nach dem Nachnamen.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Dabei wird ein Steuerelementparameter zum Festlegen der Eigenschaft "Where"

Wie mit anderen Steuerelementen für Datenquellen, Sie Parameterwerte zu übergeben, können die `Where` Eigenschaft. Auf der *Courses.aspx* Seite, dass Sie in Teil 2 des Lernprogramms erstellt haben, können Sie diese Methode verwenden, um Kurse anzuzeigen, die die Abteilung zugeordnet sind, die ein Benutzer aus der Dropdown Liste auswählt.

Open *Courses.aspx* und wechseln Sie zur **Entwurf** anzeigen. Fügen Sie eine zweite `EntityDataSource` die Steuerung an die Seite, und nennen Sie sie `CoursesEntityDataSource`. Eine Verbindung mit der `SchoolEntities` Modell, und wählen `Courses` als die **EntitySetName** Wert.

In der **Eigenschaften** Fenster, klicken Sie auf die Schaltfläche in der **, in denen** Eigenschaftenfeld. (Stellen Sie sicher, dass die `CoursesEntityDataSource` Steuerelement ausgewählt ist immer noch vor der Verwendung der **Eigenschaften** Fenster.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Die **Ausdrucks-Editor** Dialogfeld wird angezeigt. Wählen Sie in diesem Dialogfeld **automatisch generieren Ausdruck basierend auf den bereitgestellten Parametern**, und klicken Sie dann auf **Parameter hinzufügen**. Nennen Sie den Parameter `DepartmentID`Option **Steuerelement** als die **Parameterquelle** Wert ein, und wählen Sie **DepartmentsDropDownList** als die **ControlID**  Wert.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Klicken Sie auf **erweiterte Eigenschaften einblenden**, und klicken Sie in der **Eigenschaften** Fenster von der **Ausdrucks-Editor** ändern Sie im Dialogfeld der `Type` Eigenschaft `Int32`.

[![image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Wenn Sie fertig sind, klicken Sie auf **OK**.

Fügen Sie unterhalb der Dropdownliste eine `GridView` die Steuerung an die Seite, und nennen Sie sie `CoursesGridView`. Eine Verbindung mit der `CoursesEntityDataSource` Datenquellensteuerelement, klicken Sie auf **Schema aktualisieren**, klicken Sie auf **Spalten bearbeiten**, und entfernen Sie die `DepartmentID` Spalte. Die `GridView` Steuerelementmarkup ähnelt dem folgenden Beispiel.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Wenn der Benutzer die ausgewählte Abteilung in der Dropdown-Liste ändert, möchten Sie die Liste der zugeordneten Kurse automatisch geändert. Um dazu wählen Sie die Dropdownliste und klicken Sie in der **Eigenschaften** Fenster Satz der `AutoPostBack` Eigenschaft `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Nun, dass Sie mithilfe des Designers abgeschlossen haben, wechseln Sie zur **Quelle** anzuzeigen, und Ersetzen Sie die `ConnectionString` und `DefaultContainer` benennen Sie Eigenschaften von der `CoursesEntityDataSource` steuern, mit der `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Wenn Sie fertig sind, wird das Markup für das Steuerelement wie im folgenden Beispiel aussehen.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Führen Sie die Seite, und verwenden Sie die Dropdownliste, um verschiedene Abteilungen auszuwählen. Nur Kurse, die von der ausgewählten Abteilung angeboten werden werden angezeigt, der `GridView` Steuerelement.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Mithilfe der EntityDataSource "Group By"-Eigenschaft zum Gruppieren von Daten

Angenommen, das Contoso-University möchte einige Statistiken Student-Text auf der Seite "zu" abgelegt. Insbesondere, möchte er eine Aufschlüsselung der Anzahl von Schülern nach dem Datum an, die sie registriert.

Öffnen *About.aspx*, und klicken Sie in **Quelle** anzuzeigen, ersetzen Sie den vorhandenen Inhalt der `BodyContent` steuern mit "Student Text Statistiken" zwischen `h2` Tags:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Nachdem die Überschrift, Hinzufügen einer `EntityDataSource` steuern, und nennen Sie sie `StudentStatisticsEntityDataSource`. Eine Verbindung mit `SchoolEntities`, wählen die `People` Entität festgelegt, und lassen Sie die **wählen** Feld im Assistenten unverändert. Legen Sie die folgenden Eigenschaften der **Eigenschaften** Fenster:

- Um für Studenten nur filtern, legen Sie die `Where` Eigenschaft `it.EnrollmentDate is not null`.
- Um die Ergebnisse nach dem Registrierungsdatum zu gruppieren, legen Sie die `GroupBy` Eigenschaft `it.EnrollmentDate`.
- Um das Registrierungsdatum und die Anzahl der Schüler auszuwählen, legen Sie die `Select` Eigenschaft `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Legen Sie zum Sortieren der Ergebnisse nach dem Registrierungsdatum der `OrderBy` Eigenschaft `it.EnrollmentDate`.

In **Quelle** anzuzeigen, ersetzen Sie die `ConnectionString` und `DefaultContainer` benennen Sie Eigenschaften mit einem `ContextTypeName` Eigenschaft. Die `EntityDataSource` Steuerelementmarkup sieht wie im folgende Beispiel.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Die Syntax der `Select`, `GroupBy`, und `Where` Eigenschaften ähnelt der Transact-SQL, mit Ausnahme von der `it` Schlüsselwort, das die aktuelle Entität angibt.

Fügen Sie das folgende Markup zum Erstellen einer `GridView` Steuerelement zum Anzeigen der Daten.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Führen Sie die Seite zum Anzeigen einer Liste, die die Anzahl der Schüler Registrierungsdatum anzeigen.

[![image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Verwenden des QueryExtender-Steuerelements für die Filterung und Sortierung

Die `QueryExtender` Steuerelement bietet eine Möglichkeit zum Filtern und Sortieren in Markup angeben. Die Syntax ist unabhängig von der Verwendung von Datenbank-Managementsystem (DBMS). Es ist auch in der Regel unabhängig von der Entity Framework, mit der Ausnahme, dass die verwendete Syntax für Navigationseigenschaften für das Entity Framework eindeutig ist.

In diesem Teil des Lernprogramms verwenden Sie eine `QueryExtender` Steuerelement zu filtern und OrderBy-Daten, und eine Order by-Felder werden einer Navigationseigenschaft.

(Wenn Sie es vorziehen, verwenden Code anstelle von Markup, um die Abfragen zu erweitern, die durch die automatisch generiert die `EntityDataSource` -Steuerelement, Sie können dies vornehmen, indem Behandlung der `QueryCreated` Ereignis. Dies ist wie die `QueryExtender` Steuerelement erweitert `EntityDataSource` Abfragen auch steuern.)

Öffnen der *Courses.aspx* Seite, und fügen Sie unter dem Markup, das Sie zuvor hinzugefügt haben, das folgende Markup, um eine Überschrift, ein Textfeld zum Eingeben von Suchzeichenfolgen, eine Suchschaltfläche zu erstellen und ein `EntityDataSource` Steuerelement, das an die gebundenist`Courses` Entitätenmenge.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Beachten Sie, dass die `EntityDataSource` des Steuerelements `Include` -Eigenschaftensatz auf `Department`. In der Datenbank die `Course` Tabelle enthält nicht den Abteilungsnamen, enthält der Ausgabeparameter ein `DepartmentID` foreign Key-Spalte. Wenn Sie die Datenbank direkt zum Abrufen des Namens der Abteilung zusammen mit Kursdaten Abfragen wurden, müssten Sie verknüpfen die `Course` und `Department` Tabellen. Durch Festlegen der `Include` Eigenschaft `Department`, Sie angeben, dass das Entity Framework kommunikationsaufrufe den zugehörigen vorgegangen `Department` Entität aus, wenn er erhält eine `Course` Entität. Die `Department` Entität werden gespeichert, der `Department` Navigationseigenschaft der `Course` Entität. (Standardmäßig der `SchoolEntities` -Klasse, die von dem Modell-Designer generiert wurde verknüpfte Daten abruft, wenn ist es erforderlich, und Sie das Datenquellensteuerelement, für diese Klasse gebunden haben, sodass das Festlegen der `Include` Eigenschaft ist nicht erforderlich. Allerdings festlegen verbessert die Leistung der Seite, da andernfalls das Entity Framework machen würden Aufrufe zum Abrufen von Daten für die Datenbank trennen der `Course` Entitäten und für die verknüpfte `Department` Entitäten.)

Nach der `EntityDataSource` Steuerelement, die Sie gerade erstellt haben, fügen Sie das folgende Markup zum Erstellen einer `QueryExtender` Steuerelement, mit denen gebunden ist `EntityDataSource` Steuerelement.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Die `SearchExpression` Element gibt an, um Kurse auszuwählen, deren Namen mit dem Wert, der in das Textfeld eingegeben, werden sollen. Wie viele Zeichen eingegeben wurden, in das Textfeld, verglichen werden da nur die `SearchType` Eigenschaft gibt an, `StartsWith`.

Die `OrderByExpression` Element gibt an, dass das Resultset Kurs Titels in Abteilungsnamen sortiert werden. Beachten Sie, wie der Name der Abteilung angegeben wird: `Department.Name`. Da die Zuordnung zwischen der `Course` Entität und die `Department` Entität 1: 1, ist die `Department` Navigationseigenschaft enthält eine `Department` Entität. (Bei einer 1: n-Beziehung, würde die Eigenschaft eine Auflistung enthalten.) Um den Namen der Abteilung zu erhalten, geben Sie die `Name` Eigenschaft von der `Department` Entität.

Fügen Sie schließlich eine `GridView` -Steuerelement zum Anzeigen der Liste der Kurse:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Die erste Spalte ist eine Vorlagenfeld, das den Abteilungsnamen anzeigt. Gibt an, der Datenbindungsausdruck `Department.Name`, genau wie Sie gesehen, in haben der `QueryExtender` Steuerelement.

Führen Sie die Seite. Die ursprüngliche Anzeige zeigt eine Liste aller Kurse in Reihenfolge an, nach Abteilung und dann nach den Titel.

[![image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Geben Sie einem "m", und klicken Sie auf **Suche** alle Kurse anzeigen, deren Namen mit "m" (die Suche wird keine Groß-/ Kleinschreibung) beginnen.

[![image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Verwenden den Operator "Like" zum Filtern von Daten

Können Sie einen ähnlichen Effekt erreichen der `QueryExtender` des Steuerelements `StartsWith`, `Contains`, und `EndsWith` Suchtypen mithilfe einer `Like` Operator in der `EntityDataSource` des Steuerelements `Where` Eigenschaft. In diesem Teil des Lernprogramms, sehen Sie, wie Sie die `Like` Operator, um ein Student nach Namen suchen.

Open *Students.aspx* in **Quelle** anzeigen. Nach der `GridView` steuern, fügen Sie das folgende Markup hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Dieses Markup ähnelt der was zuvor mit Ausnahme von gesehen der `Where` Eigenschaftswert. Der zweite Teil der `Where` Ausdruck definiert eine teilzeichenfolgensuche (`LIKE %FirstMidName% or LIKE %LastName%`), durchsucht die vor- und Nachnamen Namen für alle von Ihnen gewünschten in das Textfeld eingegeben wird.

Führen Sie die Seite. Anfänglich daraufhin aller Studenten, da der Standardwert für die `StudentName` Parameter ist "%".

[![image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Geben Sie den Buchstaben "g" in das Textfeld ein, und klicken Sie auf **Suche**. Sie sehen eine Liste der Studenten, die ein "g" entweder in den ersten oder letzten Namen aufweisen.

[![image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Sie haben nun angezeigt, aktualisiert, gefiltert, sortiert und gruppiert Daten aus den einzelnen Tabellen. In den nächsten Lernprogrammen beginnen Sie mit verbundenen Daten (Master / Detail-Szenarien) arbeiten.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-4.md)
