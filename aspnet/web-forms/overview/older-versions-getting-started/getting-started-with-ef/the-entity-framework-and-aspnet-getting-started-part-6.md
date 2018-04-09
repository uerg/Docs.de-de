---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 6 | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 6
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementieren der "Tabelle pro Hierarchie"-Vererbung

Im vorherigen Lernprogramm arbeitet Sie mit verbundenen Daten hinzufügen und Löschen von Beziehungen und durch Hinzufügen einer neuen Entität, die eine Beziehung zu einer vorhandenen Entität mussten. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

Vererbung können Sie in einer objektorientierten Programmierung mit verwandten Klassen arbeiten erleichtern. Sie können z. B. erstellen `Instructor` und `Student` abgeleitete Klassen eine `Person` Basisklasse. Sie können die gleichen Arten von Strukturen der Vererbung zwischen Entitäten im Entity Framework erstellen.

In diesem Teil des Lernprogramms wird keine neuen Webseiten erstellen. Stattdessen Sie Entitäten abgeleitet, dem Datenmodell hinzufügen und Ändern von vorhandenen Seiten, um die neuen Entitäten zu verwenden.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabelle pro Hierarchie im Vergleich zu Tabelle pro Typ Vererbung

Eine Datenbank kann Informationen über verbundene Objekte in einer Tabelle oder in mehreren Tabellen speichern. Beispielsweise ist in der `School` Datenbank, die `Person` Tabelle enthält Informationen zu Studenten und Lehrkräfte in einer einzelnen Tabelle. Einige der Spalten gelten nur für Lehrkräfte (`HireDate`), einige nur für Studenten (`EnrollmentDate`), und einige sowohl (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Sie können konfigurieren, dass das Entity Framework zum Erstellen `Instructor` und `Student` Entitäten, die von erben die `Person` Entität. Dieses Muster für eine Entität Vererbungsstruktur aus einer einzelnen Datenbanktabelle generiert heißt *Tabelle pro Hierarchie* Vererbung (TPH).

Für Kurse die `School` Datenbank verwendet ein anderes Muster. Onlinekurse und vor Ort Kurse werden in separaten Tabellen, von denen jeder hat es sich um einen Fremdschlüssel, die auf zeigt gespeichert die `Course` Tabelle. Gemeinsame Informationen für beide Kurstypen befindet sich nur in der `Course` Tabelle.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Sie können die Entity Framework-Datenmodells konfigurieren, damit `OnlineCourse` und `OnsiteCourse` erbt von der `Course` Entität. Dieses Muster generieren Sie eine Entität Vererbungsstruktur aus separaten Tabellen für jeden Typ, wobei jede separate Datentabelle Rückblick auf eine Tabelle, die Daten, die alle Typen gemeinsam speichert heißt *Tabelle pro Typ* Vererbung (TPT).

TPH Muster der methodenvererbung aufgeführt übermitteln im Entity Framework als TPT Muster der methodenvererbung aufgeführt, in der Regel eine bessere Leistung, TPT Muster können komplexe Join-Abfragen ergeben. Diese exemplarische Vorgehensweise veranschaulicht die TPH-Vererbung zu implementieren. Sie müssen zu diesem Zweck die folgenden Schritte ausführen:

- Erstellen Sie `Instructor` und `Student` Entitätstypen, die davon Herleiten `Person`.
- Move-Eigenschaften, die die abgeleiteten Entitäten aus betreffen die `Person` Entität und abgeleiteten Entitäten.
- Einschränkungen für Eigenschaften in der abgeleiteten Typen festgelegt.
- Stellen Sie die `Person` Entität eine abstrakte Entität.
- Map jede abgeleitete Entität, die die `Person` Tabelle mit einer Bedingung, die angibt, wie Sie feststellen, ob eine `Person` Zeile darstellt, die abgeleiteten Typ.

## <a name="adding-instructor-and-student-entities"></a>Hinzufügen von Dozenten und Student-Entitäten

Öffnen der <em>SchoolModel.edmx</em> Datei der rechten Maustaste auf einen freien Bereich im Designer auswählen <strong>hinzufügen</strong>, und wählen Sie dann <strong>Entität</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

In der **Entität hinzufügen** (Dialogfeld), Name der Entität `Instructor` und legen Sie dessen **Basistyp** option `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Klicken Sie auf **OK**. Der Designer erstellt eine `Instructor` Entität, die von abgeleitet ist die `Person` Entität. Die neue Entität verfügt noch keine Eigenschaften.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Wiederholen Sie das Verfahren zum Erstellen einer `Student` Entität, die auch abgeleitet `Person`.

Nur bei Lehrkräfte mitarbeiteridentifikationsnummern, daher Sie diese Eigenschaft von verschieben müssen der `Person` Entität, die die `Instructor` Entität. In der `Person` -Entität mit der rechten Maustaste die `HireDate` -Eigenschaft, und klicken Sie auf **Ausschneiden**. Klicken Sie dann mit der rechten Maustaste **Eigenschaften** in der `Instructor` Entität, und klicken Sie auf **einfügen**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Das Einstellungsdatum des ein `Instructor` Entität darf nicht null sein. Mit der rechten Maustaste die `HireDate` -Eigenschaft, klicken Sie auf **Eigenschaften**, und klicken Sie dann in der **Eigenschaften** ändern `Nullable` auf `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Wiederholen Sie das Verfahren zum Verschieben der `EnrollmentDate` Eigenschaft aus der `Person` Entität, die die `Student` Entität. Stellen Sie sicher, dass Sie auch festlegen, `Nullable` auf `False` für die `EnrollmentDate` Eigenschaft.

Nachdem eine `Person` Entität hat nur die Eigenschaften, die gelten `Instructor` und `Student` Entitäten (abgesehen von Navigationseigenschaften, die Sie nicht verschieben), kann die Entität nur als Basisentität in der Vererbungsstruktur verwendet werden. Aus diesem Grund müssen Sie sicherstellen, dass es nie als unabhängige Entität behandelt wird. Mit der rechten Maustaste die `Person` Entität, wählen **Eigenschaften**, und klicken Sie dann in der **Eigenschaften** Fenster ändern Sie den Wert des der **abstrakte** Eigenschaft, um  **"True"**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Zuordnen von Dozenten und Student-Entitäten zu der Person-Tabelle

Nun Sie das Entity Framework anweisen, wie Unterscheidung zwischen müssen `Instructor` und `Student` Entitäten in der Datenbank.

Mit der rechten Maustaste die `Instructor` Entität, und wählen **Tabelle zuordnen**. In der **Mappingdetails** Fenster, klicken Sie auf **Hinzufügen einer Tabelle oder Sicht** , und wählen Sie **Person**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Klicken Sie auf **Hinzufügen einer Bedingung**, und wählen Sie dann **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Änderung **Operator** auf **ist** und **Wert / Eigenschaft** auf **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Wiederholen Sie das Verfahren für die `Students` Entität, die angeben, dass diese Entität zugeordnet ist die `Person` bei Verwendung der `EnrollmentDate` Spalte ist nicht null. Speichern Sie und schließen Sie das Datenmodell.

Erstellen Sie das Projekt, um die neuen Entitäten als Klassen zu erstellen und im Designer zur Verfügung stellen.

## <a name="using-the-instructor-and-student-entities"></a>Mithilfe des Dozenten und Student-Entitäten

Beim Erstellen von Webseiten, die mit Student "und" Dozenten Daten datengebundenen Sie damit arbeiten, haben die `Person` Entitätenmenge und gefiltert werden, auf die `HireDate` oder `EnrollmentDate` Eigenschaft, um die zurückgegebenen Daten zu Studenten oder Dozenten einschränken. Jedoch jetzt beim Binden jeder Datenquellen-Steuerelements an die `Person` Entität werden soll, können Sie angeben, dass nur `Student` oder `Instructor` Entitätstypen sollte ausgewählt sein. Da das Entity Framework weiß, wie zur Unterscheidung der Studenten und Kursleiter die `Person` -Entitätenmenge, die Sie entfernen die `Where` eigenschaftseinstellungen, die Sie manuell zu diesem Zweck eingegeben.

In der Visual Studio-Designer können Sie angeben, geben Sie die Entität, die eine `EntityDataSource` Steuerelement sollte wählen Sie in der **EntityTypeFilter** im Dropdown-Feld der `Configure Data Source` Assistenten wie im folgenden Beispiel gezeigt.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Und klicken Sie in der **Eigenschaften** Fenster, die Sie entfernen können `Where` Klausel-Werte, die nicht mehr benötigt werden, wie im folgenden Beispiel gezeigt,.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Jedoch, da das Markup für die Änderungen `EntityDataSource` Steuerelemente zur Verwendung der `ContextTypeName` -Attribut kann nicht ausgeführt werden die **Datenquelle konfigurieren** Assistenten auf `EntityDataSource` Steuerelemente, die Sie bereits erstellt haben. Aus diesem Grund müssen Sie die erforderlichen Änderungen vornehmen, durch Markup stattdessen ändern.

Öffnen der *Students.aspx* Seite. In der `StudentsEntityDataSource` steuern, entfernen Sie die `Where` Attribut, und fügen eine `EntityTypeFilter="Student"` Attribut. Das Markup wird nun im folgende Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Festlegen der `EntityTypeFilter` Attribut wird sichergestellt, dass die `EntityDataSource` Steuerelement wird nur auf den angegebenen Entitätstyp auswählen. Wenn Sie beides abrufen wollten `Student` und `Instructor` Entitätstypen, würden Sie nicht dieses Attribut festgelegt. (Sie haben die Möglichkeit, mehrere Entitätstypen mit einem abrufen `EntityDataSource` steuern nur, wenn Sie das Steuerelement für den schreibgeschützten Datenzugriff verwenden. Bei Verwendung einer `EntityDataSource` zum Einfügen, aktualisieren oder Löschen von Entitäten zu steuern, und wenn die Entitätenmenge, die sie, um gebunden ist mehrere Typen enthalten kann, Sie funktionieren nur mit einem Entitätstyp und müssen Sie dieses Attribut festlegen.)

Wiederholen Sie das Verfahren für die `SearchEntityDataSource` zu steuern, mit der Ausnahme entfernt nur den Teil der `Where` -Attribut, das ausgewählt `Student` Entitäten anstelle die Eigenschaft vollständig entfernen. Das Anfangstag des Steuerelements wird nun im folgende Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Führen Sie die Seite, um sicherzustellen, dass es weiterhin funktioniert, es hat sich nichts geändert.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aktualisieren Sie die folgenden Seiten, die Sie in früheren Lernprogramme erstellt haben, damit sie die neue verwenden `Student` und `Instructor` Entitäten anstelle von `Person` Entitäten, führen sie überprüfen, ob sie funktionieren wie vor:

- In *StudentsAdd.aspx*, hinzufügen `EntityTypeFilter="Student"` auf die `StudentsEntityDataSource` Steuerelement. Das Markup wird nun im folgende Beispiel aussehen: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- In *About.aspx*, hinzufügen `EntityTypeFilter="Student"` auf die `StudentStatisticsEntityDataSource` steuern und Entfernen von `Where="it.EnrollmentDate is not null"`. Das Markup wird nun im folgende Beispiel aussehen: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- In *Instructors.aspx* und *InstructorsCourses.aspx*, hinzufügen `EntityTypeFilter="Instructor"` auf die `InstructorsEntityDataSource` steuern und Entfernen von `Where="it.HireDate is not null"`. Das Markup in *Instructors.aspx* jetzt etwa wie folgt: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Das Markup in *InstructorsCourses.aspx* wird nun im folgende Beispiel aussehen:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Als Ergebnis dieser Änderungen haben Sie die Contoso-University Anwendung Verwaltbarkeit auf unterschiedliche Weise verbessert. Auswahl und Überprüfung Logik aus der Benutzeroberflächenebene verschoben haben (*aspx* Markup), und er einen wesentlicher Bestandteil der Datenzugriffsebene versucht. Dadurch wird um den Anwendungscode über Änderungen zu isolieren, die in der Zukunft an das Datenbankschema oder das Datenmodell vorgenommen werden können. Beispielsweise könnten Sie, dass Studenten als Hilfsmittel für Lehrkräfte eingestellt werden können und daher eine Einstellungsdatum abgerufen werden würde. Sie können eine neue Eigenschaft, um Studenten von Lehrkräfte unterschieden, und aktualisieren Sie das Datenmodell hinzufügen. Kein Code in der Webanwendung müssten ändern, es sei denn, in dem Sie soll demonstriert werden, ein Einstellungsdatum für Studenten. Ein weiterer Vorteil des Hinzufügens von `Instructor` und `Student` Entitäten ist, dass der Code ist leichter verständlich, als wenn sie bezeichnet `Person` Objekte, die tatsächlich Studenten wurden oder Lehrkräfte.

Sie haben nun gesehen, eine Möglichkeit, ein Vererbungsmuster im Entity Framework zu implementieren. Im folgenden Lernprogramm erfahren Sie, wie Sie gespeicherte Prozeduren verwenden, um besser steuern, wie das Entity Framework für die Datenbank zugreift.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-7.md)
