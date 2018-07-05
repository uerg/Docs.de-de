---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 6 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 99861197e00a3f2f6811ef13136fac63b993ef32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372105"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 6
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementieren der "Tabelle pro Hierarchie"-Vererbung

Im vorherigen Tutorial haben Sie mit verwandten Daten, hinzufügen und Löschen von Beziehungen und Hinzufügen einer neuen Entität, die eine Beziehung zu einer vorhandenen Entität. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

In der objektorientierten Programmierung können Sie die Vererbung verwenden, zum Arbeiten mit verknüpften Klassen vereinfachen. Sie können z. B. erstellen `Instructor` und `Student` von abgeleiteten Klassen eine `Person` Basisklasse. Sie können die gleichen Arten von Strukturen der Vererbung zwischen Entitäten im Entity Framework erstellen.

In diesem Teil des Tutorials wird nicht Sie alle neuen Webseiten erstellen. Stattdessen Sie hinzufügen, in das Datenmodell Entitäten abgeleitet und ändern die vorhandene Seiten, um die neuen Entitäten zu verwenden.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabelle pro Hierarchie im Vergleich zu Tabelle-pro-Typ '-Vererbung

Eine Datenbank können Sie Informationen zu verknüpften Objekten in einer Tabelle oder in mehreren Tabellen speichern. Z. B. in der `School` -Datenbank, die `Person` Tabelle enthält Informationen über Schüler/Studenten und Dozenten in einer einzelnen Tabelle. Einige der Spalten gelten nur für Dozenten (`HireDate`), einige nur für Studenten (`EnrollmentDate`), und einige auf beide (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Sie können konfigurieren, dass das Entity Framework zum Erstellen `Instructor` und `Student` Entitäten, die von erben die `Person` Entität. Dieses Muster zum Generieren von eine Vererbungsstruktur für Entitäten aus einer einzelnen Datenbanktabelle heißt *Tabelle pro Hierarchie* (TPH)-Vererbung.

Für Kurse die `School` -Datenbank verwendet ein anderes Muster. Online-Kurse und vor Ort stattfindenden Kurse werden gespeichert, in separaten Tabellen, von denen jeder hat es sich um einen Fremdschlüssel, die auf zeigt die `Course` Tabelle. Gemeinsame Informationen für beide Kurstypen befindet sich nur in der `Course` Tabelle.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Sie können das Entity Framework-Datenmodell konfigurieren, damit `OnlineCourse` und `OnsiteCourse` erbt von der `Course` Entität. Dieses Muster zum Generieren von eine Vererbungsstruktur für Entitäten aus separaten Tabellen für jeden Typ mit den separaten Tabellen Rückblick auf eine Tabelle, die Daten für alle Typen, speichert heißt *Tabelle pro Typ* (TPT)-Vererbung.

TPH-vererbungsmustern in der Regel eine bessere Leistung im Entity Framework als TPT-vererbungsmustern, da TPT-Muster zu komplexen joinabfrage führen können. Diese exemplarische Vorgehensweise veranschaulicht, wie TPH-Vererbung implementiert wird. Sie müssen dazu die folgenden Schritte ausführen:

- Erstellen Sie `Instructor` und `Student` Entitätstypen, die abgeleitet `Person`.
- Move-Eigenschaften, die die abgeleiteten Entitäten aus betreffen, die `Person` Entität, die den abgeleiteten Entitäten.
- Legen Sie Einschränkungen für Eigenschaften in der abgeleiteten Typen.
- Stellen Sie die `Person` Entität eine abstrakte Entität.
- Zuordnung jeder abgeleiteten Entität, die die `Person` Tabelle mit einer Bedingung an, der angibt, wie Sie feststellen, ob eine `Person` Zeile darstellt, die abgeleiteten Typ.

## <a name="adding-instructor-and-student-entities"></a>Hinzufügen von "Instructor" und "Student"-Entitäten

Öffnen der <em>SchoolModel.edmx</em> Datei der rechten Maustaste auf einen freien Bereich im Designer auf <strong>hinzufügen</strong>, und wählen Sie dann <strong>Entität</strong><em>.</em>

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

In der **Entität hinzufügen** klicken Sie im Dialogfeld die Namen der Entität `Instructor` und legen Sie dessen **Basistyp** option `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Klicken Sie auf **OK**. Der Designer erstellt eine `Instructor` Entität, die von abgeleitet ist die `Person` Entität. Die neue Entität verfügt noch keine Eigenschaften.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Wiederholen Sie das Verfahren zum Erstellen einer `Student` Entität, die auch abgeleitet `Person`.

Nur für Dozenten Anstellungsdatum, weshalb Sie verschieben Sie diese Eigenschaft aus der `Person` Entität, die die `Instructor` Entität. In der `Person` Entität mit der rechten Maustaste die `HireDate` -Eigenschaft, und klicken Sie auf **Ausschneiden**. Klicken Sie dann mit der rechten Maustaste **Eigenschaften** in die `Instructor` Entität, und klicken Sie auf **einfügen**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Das Einstellungsdatum des ein `Instructor` Entität darf nicht null sein. Mit der rechten Maustaste die `HireDate` -Eigenschaft, klicken Sie auf **Eigenschaften**, und klicken Sie dann in der **Eigenschaften** Fenster ändern `Nullable` zu `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Wiederholen Sie das Verfahren zum Verschieben der `EnrollmentDate` Eigenschaft aus der `Person` Entität, die die `Student` Entität. Stellen Sie sicher, dass Sie auch festlegen, `Nullable` zu `False` für die `EnrollmentDate` Eigenschaft.

Nachdem eine `Person` Entität hat nur die Eigenschaften, die für `Instructor` und `Student` Entitäten (abgesehen von Navigationseigenschaften, die Sie nicht verschieben), kann die Entität nur als Basisentität in die Vererbungsstruktur verwendet werden. Aus diesem Grund müssen Sie sicherstellen, dass es nie als unabhängige Einheit behandelt wird. Mit der rechten Maustaste die `Person` Entität, auf **Eigenschaften**, und klicken Sie dann im der **Eigenschaften** Fenster ändern Sie den Wert von der **abstrakte** Eigenschaft, um  **"True"**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Zuordnen von Instructor und Student-Entitäten zu der Person-Tabelle

Nun Sie das Entity Framework, wie unterscheiden soll müssen `Instructor` und `Student` Entitäten in der Datenbank.

Mit der rechten Maustaste die `Instructor` Entität, und wählen **Tabellenzuordnung**. In der **Mappingdetails** Fenster, klicken Sie auf **Hinzufügen einer Tabelle oder Sicht** , und wählen Sie **Person**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Klicken Sie auf **Hinzufügen einer Bedingung**, und wählen Sie dann **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Änderung **Operator** zu **ist** und **Wert / Eigenschaft** zu **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Wiederholen Sie das Verfahren für die `Students` Entität angeben, dass diese Entität zugeordnet ist die `Person` Tabelle, wenn die `EnrollmentDate` Spalte ist nicht null. Speichern Sie und schließen Sie das Datenmodell.

Erstellen Sie das Projekt aus, um die neuen Entitäten als Klassen zu erstellen und im Designer zur Verfügung stellen.

## <a name="using-the-instructor-and-student-entities"></a>Mithilfe der Instructor und Student-Entitäten

Bei der Erstellung von Webseiten, die mit "Student" und "Instructor" Daten Sie Datenbindung zu arbeiten, die `Person` Entitätenmenge und gefiltert werden, auf die `HireDate` oder `EnrollmentDate` Eigenschaft, um die zurückgegebenen Daten auf Schüler/Studenten oder Dozenten zu beschränken. Aber jetzt beim Binden jeder Datenquellen-Steuerelement die `Person` Entität festgelegt ist, können Sie angeben, dass nur `Student` oder `Instructor` Entitätstypen ausgewählt werden sollen. Da das Entity Framework an Schüler/Studenten und Dozenten in unterscheiden die `Person` -Entitätenmenge, die Sie entfernen die `Where` eigenschafteneinstellungen, die Sie manuell zu diesem Zweck eingegeben.

In der Visual Studio-Designer können Sie angeben, geben Sie die Entität, die eine `EntityDataSource` Steuerelement sollte wählen Sie in der **EntityTypeFilter** Dropdown-Feld des der `Configure Data Source` Assistenten wie im folgenden Beispiel gezeigt.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Und klicken Sie in der **Eigenschaften** Fenster können Sie entfernen `Where` Klausel-Werte, die nicht mehr benötigt werden, wie im folgenden Beispiel gezeigt,.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Jedoch, da Sie das Markup für geändert haben `EntityDataSource` Steuerelemente zur Verwendung der `ContextTypeName` -Attribut kann nicht ausgeführt werden die **Konfigurieren von Datenquellen** Assistenten auf `EntityDataSource` Steuerelemente, die Sie bereits erstellt haben. Daher müssen Sie die erforderlichen Änderungen vornehmen, durch Markup stattdessen zu ändern.

Öffnen der *Students.aspx* Seite. In der `StudentsEntityDataSource` steuern, entfernen Sie die `Where` Attribut, und fügen eine `EntityTypeFilter="Student"` Attribut. Das Markup wird nun im folgende Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Festlegen der `EntityTypeFilter` Attribut wird sichergestellt, dass die `EntityDataSource` Steuerelement wird nur den angegebene Entitätstyp auswählen. Wenn Sie beides abrufen möchten `Student` und `Instructor` Entitätstypen, würden Sie nicht dieses Attribut festgelegt. (Sie haben die Möglichkeit zum Abrufen von mehreren Entitätstypen mit einer `EntityDataSource` steuern nur, wenn Sie das Steuerelement für die schreibgeschützten Datenzugriff verwenden. Bei Verwendung einer `EntityDataSource` zum Einfügen, aktualisieren oder Löschen von Entitäten zu steuern, wenn die Entitätenmenge, die es, um gebunden ist mehrere Typen enthalten kann, Sie können nur mit einem Entitätstyp arbeiten, und müssen Sie dieses Attribut festgelegt.)

Wiederholen Sie das Verfahren für die `SearchEntityDataSource` zu steuern, mit der Ausnahme entfernen Sie nur den Teil der `Where` -Attribut, das ausgewählt `Student` Entitäten anstelle von entfernen Sie die Eigenschaft vollständig. Das öffnende Tag des Steuerelements wird nun im folgende Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Führen Sie die Seite, um sicherzustellen, dass es weiterhin funktioniert wie gehabt.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aktualisieren Sie die folgenden Seiten, die Sie in den vorherigen Tutorials erstellt haben, damit sie die Verwendung der neuen `Student` und `Instructor` Entitäten anstelle von `Person` Entitäten, führen Sie diese, um sicherzustellen, dass sie wie zuvor funktionieren:

- In *StudentsAdd.aspx*, fügen `EntityTypeFilter="Student"` auf die `StudentsEntityDataSource` Steuerelement. Das Markup wird nun im folgende Beispiel aussehen: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- In *About.aspx*, hinzufügen `EntityTypeFilter="Student"` auf die `StudentStatisticsEntityDataSource` steuern und Entfernen von `Where="it.EnrollmentDate is not null"`. Das Markup wird nun im folgende Beispiel aussehen: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![Image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- In *Instructors.aspx* und *InstructorsCourses.aspx*, hinzufügen `EntityTypeFilter="Instructor"` auf die `InstructorsEntityDataSource` steuern und Entfernen von `Where="it.HireDate is not null"`. Das Markup in *Instructors.aspx* jetzt etwa wie folgt: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Das Markup in *InstructorsCourses.aspx* wird jetzt im folgende Beispiel entsprechen:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Sie haben diesen Änderungen, resultiert wartbarkeit von der Contoso University-Anwendung auf verschiedene Arten verbessert. Auswahl und Überprüfung der Logik aus der UI-Ebene verschoben haben (*aspx* Markup), und es einen wesentlicher Bestandteil der Datenzugriffsschicht. Dadurch wird um Ihr Anwendungscode von Änderungen zu isolieren, die Sie in der Zukunft, um das Datenbankschema oder das Datenmodell vornehmen können. Beispielsweise könnten Sie, dass Schüler/Studenten als Hilfsmittel für Lehrkräfte eingestellt werden können und daher ein Einstellungsdatum erhalten. Sie können dann eine neue Eigenschaft, um Schüler und Studenten von Dozenten unterscheiden, und Aktualisieren des Datenmodells hinzufügen. Kein Code in der Webanwendung müssen ändern, es sei denn, in dem Sie ein Einstellungsdatum für Schüler/Studenten zeigen wollte. Ein weiterer Vorteil des Hinzufügens `Instructor` und `Student` Entitäten besteht darin, dass Ihr Code leichter verständlich ist, als wenn es bezeichnet `Person` Objekte, die tatsächlich Schüler/Studenten waren oder Dozenten.

Sie haben nun eine Möglichkeit zum Implementieren von einem Vererbungsmuster in Entity Framework gesehen werden. Im folgenden Tutorial erfahren Sie, wie Sie gespeicherte Prozeduren verwenden, um besser steuern, wie das Entity Framework die Datenbank zugreift.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-7.md)
