---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms - Teil 4 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 5f8b1c15fbfd2d65b603013db3902b42faa40665
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836787"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms - Teil 4
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Arbeiten mit zugehörigen Daten

Im vorherigen Tutorial verwendet Sie die `EntityDataSource` Steuerelement zu filtern, Sortieren und Gruppieren von Daten. In diesem Tutorial werden Sie anzeigen und aktualisieren verwandter Daten.

Erstellen Sie die dozentenseite, die eine Liste der Dozenten zeigt. Wenn Sie ein Dozent ausgewählt haben, sehen Sie eine Liste der Kurse, die durch dieses Dozenten unterrichtet. Wenn Sie einen Kurs auswählen, sehen Sie die Details für den Kurs sowie eine Liste der in diesem Kurs registrierten Studenten. Sie können den Dozenten Einstellungsdatum und bürozuweisung bearbeiten. Die bürozuweisung ist eine separate Entität-Gruppe, die Sie über eine Navigationseigenschaft zugreifen.

Sie können Masterdaten Detaildaten in Markup oder Code verknüpfen. In diesem Teil des Tutorials verwenden Sie beide Methoden.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Anzeigen und aktualisieren verknüpfte Entitäten in einem GridView-Steuerelement

Erstellen einer neuen Webseite mit dem Namen *Instructors.aspx* , verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup zu der `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Dieses Markup erstellt eine `EntityDataSource` Steuerelement, dass Dozenten auswählt und ermöglicht. Die `div` -Element konfiguriert die Markup zum Rendern auf der linken Seite, damit Sie eine Spalte auf der rechten Seite später hinzufügen können.

Zwischen der `EntityDataSource` Markup und dem schließenden `</div>` markieren, fügen Sie das folgende Markup, das erstellt eine `GridView` Steuerelement und ein `Label` -Steuerelement, das Sie für Fehlermeldungen verwenden möchten:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Dies `GridView` Steuerelement ermöglicht die Zeilenauswahl, die ausgewählte Zeile mit einer hellgrauen Hintergrundfarbe hervorgehoben, und gibt an, die Handler (die Sie später erstellen) für die `SelectedIndexChanged` und `Updating` Ereignisse. Es gibt auch an `PersonID` für die `DataKeyNames` -Eigenschaft so, dass der Schlüsselwert der ausgewählten Zeile an einem anderen Steuerelement übergeben werden kann, die Sie später hinzufügen.

Die letzte Spalte enthält die bürozuweisung des Dozenten, die in eine Navigationseigenschaft gespeichert wird die `Person` Entität weil sie von einer zugeordneten Entität stammt. Beachten Sie, dass die `EditItemTemplate` Element gibt `Eval` anstelle von `Bind`, da die `GridView` Steuerelement kann nicht direkt an Navigationseigenschaften gebunden, um sie zu aktualisieren. Sie müssen die bürozuweisung im Code aktualisieren. Zu diesem Zweck benötigen Sie einen Verweis auf die `TextBox` , und Sie erhalten und zu speichern, die in der `TextBox` des Steuerelements `Init` Ereignis.

Nach der `GridView` Steuerelement ist ein `Label` -Steuerelement, das für Fehlermeldungen verwendet wird. Des Steuerelements `Visible` Eigenschaft `false`, und der Ansichtszustand deaktiviert ist, so, dass die Bezeichnung angezeigt wird, nur wenn Code als Reaktion auf einen Fehler sichtbar vereinfacht.

Öffnen der *Instructors.aspx.cs* Datei, und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Fügen Sie einem privaten Klassenfeld direkt nach der Namensdeklaration der partiellen Klasse für einen Verweis auf das Textfeld der Office-Zuweisung.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Fügen Sie einen Stub für die `SelectedIndexChanged` -Ereignishandler, dass Sie Code für die spätere Verwendung hinzufügen. Auch hinzufügen, einen Handler für die bürozuweisung `TextBox` des Steuerelements `Init` Ereignis, damit Sie einen Verweis auf Speichern, können die `TextBox` Steuerelement. Verwenden Sie diesen Verweis, um der vom Benutzer eingegebenen zum Aktualisieren der Entität die Navigationseigenschaft zugeordnet zu nutzen.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Verwenden Sie die `GridView` des Steuerelements `Updating` zu aktualisierendes Ereignis die `Location` Eigenschaft des zugeordneten `OfficeAssignment` Entität. Fügen Sie den folgenden Ereignishandler für die `Updating` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Dieser Code ausgeführt wird, wenn der Benutzer klickt **Update** in einem `GridView` Zeile. Der Code verwendet LINQ to Entities, zum Abrufen der `OfficeAssignment` Entität, die mit dem aktuellen verknüpft ist `Person` Entität mithilfe der `PersonID` der ausgewählten Zeile in das Ereignisargument.

Der Code führt dann eine der folgenden Aktionen je nach dem Wert in der `InstructorOfficeTextBox` Steuerelement:

- Wenn das Textfeld einen Wert und gibt es keine `OfficeAssignment` Entität zu aktualisieren, erstellt es einen.
- Wenn das Textfeld einen Wert und gibt es eine `OfficeAssignment` Entität aktualisiert die `Location` -Eigenschaftswert.
- Wenn das Textfeld leer ist, und eine `OfficeAssignment` Entität vorhanden ist, wird die Entität.

Anschließend die Änderungen in der Datenbank gespeichert. Wenn eine Ausnahme auftritt, wird eine Fehlermeldung angezeigt.

Führen Sie die Seite.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Klicken Sie auf **bearbeiten** und alle Felder zu ändern, in die Textfelder.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Ändern Sie diese Werte, einschließlich **Bürozuweisung**. Klicken Sie auf **Update** sehen Sie die Änderungen in der Liste.

## <a name="displaying-related-entities-in-a-separate-control"></a>Anzeigen von verknüpften Entitäten in einem separaten Steuerelement

Jeden Dozenten kann einen oder mehrere Kurse, bringen, damit Sie hinzufügen, eine `EntityDataSource` Steuerelement und ein `GridView` Steuerelement, um die Liste der Kurse zugeordnet, welches "Instructor" in "Instructor"-Elementen ausgewählt ist `GridView` Steuerelement. Um eine Überschrift zu erstellen und die `EntityDataSource` für Kurse Entitäten zu steuern, fügen Sie das folgende Markup zwischen die Fehlermeldung `Label` -Steuerelement und dem schließenden `</div>` Tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Die `Where` Parameter enthält den Wert des der `PersonID` des Dozenten, deren Zeile, in ausgewählt ist, der `InstructorsGridView` Steuerelement. Die `Where` Eigenschaft enthält einen untergeordneten SELECT-Befehl, der alle zugeordneten ruft `Person` Entitäten aus einer `Course` Entität `People` Navigationseigenschaft und wählt die `Course` Entität nur dann, wenn eines der zugeordneten `Person`Entitäten enthält den ausgewählten `PersonID` Wert.

Zum Erstellen der `GridView` -Steuerelement. Fügen Sie das folgende Markup direkt nach der `CoursesEntityDataSource` Control (vor dem schließenden `</div>` Tag):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Da keine Kurse angezeigt werden, wenn keine "Instructor" ausgewählt ist, eine `EmptyDataTemplate` Element enthalten ist.

Führen Sie die Seite.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Wählen Sie einen Dozenten, der einen oder mehrere Kurse zugewiesen wurde, und den Kurs oder Kurse in der Liste angezeigt. (Hinweis: Obwohl das Schema der Datenbank mehrere Kurse zulässt, in den Testdaten bereitgestellt, mit der Datenbank keine "Instructor" tatsächlich hat mehr als einen Kurs. Sie können mit der Datenbank Kurse hinzufügen, selbst mit der **Server-Explorer** Fenster oder der *CoursesAdd.aspx* Seite, die Sie in einem späteren Tutorial hinzufügen,.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Die `CoursesGridView` Steuerelement zeigt nur einige Felder des Kurses. Um alle Details für einen Kurs anzuzeigen, verwenden Sie eine `DetailsView` Steuerelement für den Kurs, die der Benutzer auswählt. In *Instructors.aspx*, fügen Sie das folgende Markup nach dem schließenden `</div>` Tag (Stellen Sie sicher, dass Sie dieses Markup platzieren **nach** das schließende Div-tag, nicht vor):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Dieses Markup erstellt eine `EntityDataSource` Steuerelement, gebunden ist, die `Courses` Entitätenmenge. Die `Where` -Eigenschaft wählt ein Kurs mit den `CourseID` Wert der ausgewählten Zeile in den Kursen `GridView` Steuerelement. Das Markup gibt einen Handler für die `Selected` Ereignis, das Sie später verwenden werden für die Anzeige von Noten von Studierenden, die einer anderen Ebene in der Hierarchie ist.

In *Instructors.aspx.cs*, erstellen Sie die folgenden Stub für die `CourseDetailsEntityDataSource_Selected` Methode. (Sie müssen diese Stub-ausfüllen, später in diesem Tutorial, jetzt auch noch, damit die Seite kompiliert und ausgeführt werden kann.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Führen Sie die Seite.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Anfänglich stehen keine Kursdetails, da keine Kurs ausgewählt ist. Wählen Sie einen Dozenten, wer einen Kurs zugewiesen hat, und wählen Sie dann einen Kurs aus, um die Details anzuzeigen.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Verwenden das EntityDataSource ausgewählte Ereignis, um verknüpfte Daten anzeigen""

Abschließend möchten alle von der registrierten Studenten und deren Noten für den ausgewählten Kurs anzuzeigen. Zu diesem Zweck verwenden Sie die `Selected` Ereignis die `EntityDataSource` Steuerelement gebunden werden, um den Kurs `DetailsView`.

In *Instructors.aspx*, fügen Sie das folgende Markup nach dem `DetailsView` Steuerelement:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Dieses Markup erstellt eine `ListView` -Steuerelement, das eine Liste der Schüler/Studenten und deren Noten für den ausgewählten Kurs angezeigt. Da Databind-Steuerelements im Code wird, ist keine Datenquelle angegeben. Die `EmptyDataTemplate` -Element bietet eine Meldung angezeigt, wenn keine Kurs ausgewählt ist, befinden sich in diesem Fall keine Schüler angezeigt. Die `LayoutTemplate` Element erstellt eine HTML-Tabelle zum Anzeigen der Liste aus, und die `ItemTemplate` gibt die anzuzeigenden Spalten an. ID der Studierenden und die Note des Studenten stammen aus der `StudentGrade` Entität und den Namen "Student" stammt aus der `Person` Entität, die das Entity Framework in verfügbar macht die `Person` Navigationseigenschaft der `StudentGrade` Entität.

In *Instructors.aspx.cs*, ersetzen Sie die Stub-Out `CourseDetailsEntityDataSource_Selected` Methode durch den folgenden Code:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Das Ereignisargument für dieses Ereignis enthält die ausgewählten Daten in Form einer Auflistung, die hat NULL Elemente, wenn nichts ausgewählt ist oder eine Falls eine `Course` Entität ausgewählt ist. Wenn eine `Course` Entität ausgewählt ist, wird der Code verwendet die `First` Methode, um der Auflistung in ein einzelnes Objekt zu konvertieren. Anschließend ruft es `StudentGrade` Entitäten aus der Navigationseigenschaft für eine Sammlung konvertiert, und bindet die `GradesListView` Steuerelement der Auflistung.

Dies ist ausreichend, Besoldungsgruppen angezeigt, aber Sie möchten, um sicherzustellen, dass die Nachricht in der Vorlage für leere Daten zum ersten Mal angezeigt wird, auf die Seite angezeigt wird, und jedes Mal, wenn ein Kurs nicht ausgewählt ist. Zu diesem Zweck erstellen Sie die folgende Methode, die Sie, an zwei Stellen aufrufen müssen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Rufen Sie diese neue Methode aus der `Page_Load` Methode, um die leere Daten zum ersten Mal angezeigt, die Seite wird angezeigt. Und nennen Sie sie aus der `InstructorsGridView_SelectedIndexChanged` Methode, da das Ereignis ausgelöst wird, wenn ein Dozent ausgewählt wird, was bedeutet, dass neue Kurse werden in den Kursen geladen `GridView` -Steuerelement und keine noch ausgewählt ist. Hier sind die beiden Aufrufe:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Führen Sie die Seite.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Wählen Sie einen Dozenten, der einen Kurs zugewiesen wurde, und wählen Sie dann den Kurs.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Sie haben nun einige Möglichkeiten zum Arbeiten mit verknüpften Daten erfahren. In der folgenden Tutorial erfahren Sie, das Hinzufügen von Beziehungen zwischen vorhandenen Entitäten, Beziehungen zu entfernen, und wie Sie eine neue Entität hinzufügen, die eine Beziehung zu einer vorhandenen Entität verfügt.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-5.md)
