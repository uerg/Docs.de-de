---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms - Teil 3 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 162f93c0249d8fa11d67ea10c68bc2ca8bae7259
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825940"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms - Teil 3
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtern, Sortieren und Gruppieren von Daten

Im vorherigen Tutorial verwendet Sie die `EntityDataSource` -Steuerelement zum Anzeigen und Bearbeiten von Daten. In diesem Tutorial werden Sie filtern, Sortieren und Gruppieren von Daten. Hierfür durch Festlegen der Eigenschaften der `EntityDataSource` -Steuerelement, die Syntax unterscheidet sich von anderen Datenquellen-Steuerelemente. Wie Sie sehen werden, jedoch können Sie die `QueryExtender` Steuerelement, um diese Unterschiede zu minimieren.

Ändern Sie die *Students.aspx* Seite für Schüler/Studenten, gefiltert nach Namen, und suchen Sie nach Name sortieren. Ändern Sie auch die *Courses.aspx* Kurse für die ausgewählte Abteilung, und Suchen nach Kursen anhand des Namens anzuzeigenden Seite. Abschließend fügen Sie studentenstatistiken für Schüler und die *About.aspx* Seite.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Mithilfe der EntityDataSource "Where"-Eigenschaft zum Filtern von Daten

Öffnen der *Students.aspx* Seite, die Sie im vorherigen Tutorial erstellt haben. Derzeitige Konfiguration der `GridView` Steuerelement auf der Seite zeigt alle Namen von der `People` Entitätenmenge. Sie möchten jedoch nur Schüler/Studenten, zeigen finden Sie dazu `Person` Entitäten, die Registrierung für nicht-Null-Datumswerte unterstützen.

Wechseln Sie zur **Entwurf** anzeigen und Auswählen der `EntityDataSource` Steuerelement. In der **Eigenschaften** legen die `Where` Eigenschaft `it.EnrollmentDate is not null`.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Die Syntax, in der `Where` Eigenschaft der `EntityDataSource` Steuerelement ist Entity SQL. Entity SQL ist Transact-SQL ähnelt, aber er ist für die Verwendung mit Entitäten anstelle von Datenbankobjekten angepasst. Im Ausdruck `it.EnrollmentDate is not null`, das Wort `it` stellt einen Verweis auf die von der Abfrage zurückgegebenen Entität dar. Aus diesem Grund `it.EnrollmentDate` bezieht sich auf die `EnrollmentDate` Eigenschaft der `Person` Entität, die die `EntityDataSource` gibt steuern.

Führen Sie die Seite. Liste der Studenten enthält jetzt nur Schüler/Studenten. (Es sind keine Zeilen angezeigt, in denen es liegt kein Registrierungsdatum.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Mithilfe der EntityDataSource "OrderBy"-Eigenschaft Sortieren von Daten

Sie sollten auch diese Liste, um in Reihenfolge der Namen sein, wenn es zuerst angezeigt wird. Mit der *Students.aspx* Seite, die in noch geöffnet **Entwurf** anzeigen, und mit der `EntityDataSource` weiterhin ausgewähltem Steuerelement in der **Eigenschaften** legen die  **OrderBy** Eigenschaft `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Führen Sie die Seite. Liste der Studenten ist jetzt in der Reihenfolge nach Nachnamen.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Verwenden einen Steuerelementparameter zum Festlegen der Eigenschaft "Where"

Wie Sie mit anderen Datenquellen-Steuerelemente, Sie Parameterwerte übergeben, können die `Where` Eigenschaft. Auf der *Courses.aspx* Seite, dass Sie in Teil 2 des Tutorials erstellt haben, können Sie diese Methode verwenden, um Kurse anzuzeigen, die die Abteilung zugeordnet sind, die ein Benutzer aus der Dropdown Liste auswählt.

Open *Courses.aspx* und wechseln Sie zur **Entwurf** anzeigen. Fügen Sie eine zweite `EntityDataSource` die Steuerung an die Seite, und nennen Sie sie `CoursesEntityDataSource`. Verbinden Sie es mit der `SchoolEntities` Modell, und wählen `Courses` als die **EntitySetName** Wert.

In der **Eigenschaften** Fenster, klicken Sie auf die Auslassungspunkte in der **, in denen** Eigenschaftenfeld. (Stellen Sie sicher, dass die `CoursesEntityDataSource` weiterhin ausgewähltem Steuerelement vor der Verwendung der **Eigenschaften** Fenster.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Die **Ausdrucks-Editor** Dialogfeld wird angezeigt. Wählen Sie in diesem Dialogfeld **automatisch generieren Ausdruck auf der Grundlage der bereitgestellten Parameter**, und klicken Sie dann auf **Parameter hinzufügen**. Nennen Sie den Parameter `DepartmentID`Option **Steuerelement** als die **Parameterquelle** Wert ein, und wählen Sie **DepartmentsDropDownList** als die **ControlID**  Wert.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Klicken Sie auf **erweiterte Eigenschaften einblenden**, und klicken Sie in der **Eigenschaften** Fenster die **Ausdrucks-Editor** im Dialogfeld die `Type` Eigenschaft `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Wenn Sie fertig sind, klicken Sie auf **OK**.

Fügen Sie unterhalb der Dropdownliste eine `GridView` die Steuerung an die Seite, und nennen Sie sie `CoursesGridView`. Verbinden Sie es mit der `CoursesEntityDataSource` Datenquellen-Steuerelement, klicken Sie auf **Schema aktualisieren**, klicken Sie auf **Spalten bearbeiten**, und entfernen Sie die `DepartmentID` Spalte. Die `GridView` Markup des Steuerelements ähnelt dem folgenden Beispiel.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Wenn der Benutzer die ausgewählte Abteilung in der Dropdown-Liste ändert, möchten Sie die Liste der zugeordneten Kurse automatisch geändert. Auf dazu wählen Sie die Dropdown-Liste, und klicken Sie in der **Eigenschaften** legen die `AutoPostBack` Eigenschaft `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Nun, da Sie mithilfe des Designers abgeschlossen haben, wechseln Sie zur **Quelle** anzuzeigen, und Ersetzen Sie die `ConnectionString` und `DefaultContainer` Benennen von Eigenschaften der `CoursesEntityDataSource` steuern Sie mit der `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Wenn Sie fertig sind, wird das Markup für das Steuerelement wie im folgenden Beispiel aussehen.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Führen Sie die Seite, und verwenden Sie die Dropdownliste, um verschiedene Abteilungen auszuwählen. Nur die Kurse, die von der ausgewählten Abteilung angebotenen werden angezeigt, der `GridView` Steuerelement.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Mithilfe der EntityDataSource "GroupBy"-Eigenschaft zum Gruppieren von Daten

Nehmen wir an, dass Contoso University möchte einige Student-Body-Statistiken auf der Seite "Info". Insbesondere möchte er eine Aufschlüsselung der Anzahl von Schülern nach dem Datum anzeigen, die sie registriert.

Open *About.aspx*, und klicken Sie in **Quelle** anzuzeigen, ersetzen Sie den vorhandenen Inhalt der der `BodyContent` steuern mit "Statistiken der Studentendaten Text" zwischen `h2` Tags:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Nach der Überschrift, Hinzufügen einer `EntityDataSource` steuern, und nennen Sie sie `StudentStatisticsEntityDataSource`. Verbinden Sie es mit `SchoolEntities`, wählen die `People` Entität festgelegt und lassen Sie die **wählen** Feld im Assistenten nicht geändert. Legen Sie die folgenden Eigenschaften der **Eigenschaften** Fenster:

- Legen Sie zum Filtern für Schüler/Studenten, nur die `Where` Eigenschaft `it.EnrollmentDate is not null`.
- Um die Ergebnisse nach dem Registrierungsdatum zu gruppieren, legen Sie die `GroupBy` Eigenschaft `it.EnrollmentDate`.
- Um das Registrierungsdatum und die Anzahl der Schüler/Studenten auswählen, legen Sie die `Select` Eigenschaft `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Um die Ergebnisse nach dem Registrierungsdatum bestellen, legen Sie die `OrderBy` Eigenschaft `it.EnrollmentDate`.

In **Quelle** anzuzeigen, ersetzen Sie die `ConnectionString` und `DefaultContainer` Benennen von Eigenschaften mit einem `ContextTypeName` Eigenschaft. Die `EntityDataSource` Markup des Steuerelements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Die Syntax der `Select`, `GroupBy`, und `Where` Eigenschaften ähnelt der Transact-SQL, mit Ausnahme von der `it` Schlüsselwort, das die aktuelle Entität angibt.

Fügen Sie das folgende Markup zum Erstellen einer `GridView` Steuerelement zum Anzeigen der Daten.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Führen Sie die Seite zum Anzeigen einer Liste, die die Anzahl der Schüler/Studenten nach Anmeldedatum zeigt.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Verwenden das QueryExtender-Steuerelement für die Filterung und Sortierung

Die `QueryExtender` -Steuerelement bietet eine Möglichkeit zum Filtern und Sortieren in Markup angeben. Die Syntax ist unabhängig von der Verwendung von Datenbank-Managementsystem (DBMS). Es ist auch in der Regel unabhängig von den Entity Framework, mit der Ausnahme, dass die verwendete Syntax für Navigationseigenschaften zu Entity Framework eindeutig ist.

In diesem Teil des Tutorials verwenden Sie eine `QueryExtender` Steuerelement zu filtern und Reihenfolge von Daten und eines der Felder Order by werden eine Navigationseigenschaft.

(Wenn Sie lieber Code anstelle von Markup verwenden, die Abfragen zu erweitern, die automatisch vom generiert werden die `EntityDataSource` -Steuerelement, Sie können dies durch Behandeln der `QueryCreated` Ereignis. Dies ist die `QueryExtender` Steuerelement `EntityDataSource` Abfragen auch steuern.)

Öffnen der *Courses.aspx* Seite, und fügen Sie unten das Markup, das Sie zuvor hinzugefügt haben, das folgende Markup, um eine Überschrift, ein Textfeld zum Eingeben von Suchzeichenfolgen, eine Schaltfläche "Suchen" zu erstellen und ein `EntityDataSource` -Steuerelement, das an die gebundenist`Courses` Entitätenmenge.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Beachten Sie, dass die `EntityDataSource` des Steuerelements `Include` -Eigenschaftensatz auf `Department`. In der Datenbank die `Course` Tabelle enthält nicht den Namen der Abteilung; er enthält eine `DepartmentID` Fremdschlüsselspalte. Wenn Sie die Datenbank direkt Abfragen wurden, um den Abteilungsnamen sowie Kursdaten abzurufen, müssten Sie verknüpfen die `Course` und `Department` Tabellen. Durch Festlegen der `Include` Eigenschaft `Department`, Sie angeben, dass das Entity Framework den zugehörigen kommunikationsaufrufe ausführen soll `Department` Entität aus, wenn er erhält eine `Course` Entität. Die `Department` Entität befindet sich dann in der `Department` Navigationseigenschaft der `Course` Entität. (Standardmäßig die `SchoolEntities` -Klasse, die von der Data Modelldesigner generiert wurde abruft verwandte Daten aus, wenn sie benötigt, und Sie das Datenquellen-Steuerelement, auf diese Klasse gebunden haben, sodass das Festlegen der `Include` Eigenschaft ist nicht erforderlich. Allerdings festlegen verbessert die Leistung der Seite, da andernfalls das Entity Framework machen würden separate Aufrufe an die Datenbank zum Abrufen von Daten für die `Course` Entitäten und für die verknüpfte `Department` Entitäten.)

Nach der `EntityDataSource` Kontrolle, die Sie gerade erstellt haben, fügen Sie das folgende Markup zum Erstellen einer `QueryExtender` -Steuerelement, das an, die gebunden ist `EntityDataSource` Steuerelement.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Die `SearchExpression` Element gibt an, die Kurse auswählen, deren Titel mit dem Wert in das Textfeld eingegeben übereinstimmen, werden sollen. Wie viele Zeichen wie in das Textfeld eingegeben werden, verglichen werden da nur die `SearchType` Eigenschaft gibt an, `StartsWith`.

Die `OrderByExpression` Element gibt an, dass das Resultset Kurstitel innerhalb der Abteilungsname sortiert werden. Beachten Sie, wie der Name der Abteilung angegeben wird: `Department.Name`. Da die Zuordnung zwischen der `Course` Entität und die `Department` Entität 1: 1, ist die `Department` Navigationseigenschaft enthält eine `Department` Entität. (Würde dies eine 1: n Beziehung, würde die Eigenschaft eine Auflistung enthält.) Um den Namen der Abteilung zu erhalten, geben Sie die `Name` Eigenschaft der `Department` Entität.

Fügen Sie abschließend eine `GridView` Steuerelement zum Anzeigen der Liste der Kurse:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Die erste Spalte ist eine Vorlagenfeld, das die Abteilungsnamen anzeigt. Gibt an, der Datenbindungsausdruck `Department.Name`genau wie in der `QueryExtender` Steuerelement.

Führen Sie die Seite. Das erste zeigt einer Liste aller Kurse in der Reihenfolge nach Abteilung und dann nach Titel.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Geben Sie ein "m", und klicken Sie auf **Suche** alle Kurse anzeigen, deren Namen mit "m" (die Suche wird keine Groß-/ Kleinschreibung) beginnen.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Verwenden den Operator "Like" zum Filtern von Daten

Erreichen Sie eine ähnliche Wirkung wie das `QueryExtender` des Steuerelements `StartsWith`, `Contains`, und `EndsWith` Typen suchen, indem eine `Like` -Operator in der `EntityDataSource` des Steuerelements `Where` Eigenschaft. In diesem Teil des Tutorials, sehen Sie, wie Sie mit der `Like` Operator für Schüler/Student nach Namen suchen.

Open *Students.aspx* in **Quelle** anzeigen. Nach der `GridView` Steuerelement, fügen Sie das folgende Markup hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Dieses Markup wird ähnlich wie Sie zuvor mit Ausnahme von haben die `Where` -Eigenschaftswert. Der zweite Teil der `Where` Ausdruck definiert eine teilzeichenfolgensuche (`LIKE %FirstMidName% or LIKE %LastName%`), durchsucht die ersten und letzten Namen für den Inhalt in das Textfeld eingegeben wird.

Führen Sie die Seite. Anfänglich Sie sehen alle Studenten, da der Standardwert für die `StudentName` -Parameter ist "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Geben Sie den Buchstaben "g" in das Textfeld ein, und klicken Sie auf **Suche**. Sie sehen eine Liste der Schüler/Studenten, die ein "g" entweder in den ersten oder letzten Namen haben.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Sie haben jetzt angezeigt, aktualisiert, gefiltert, sortiert und gruppiert die Daten aus den einzelnen Tabellen. Im nächsten Tutorial beginnen Sie zum Arbeiten mit verknüpften Daten (Master / Detail-Szenarien).

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-4.md)
