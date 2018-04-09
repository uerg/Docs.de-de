---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 4 | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 4
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Arbeiten mit verknüpften Daten

Im vorherigen Lernprogramm Sie verwendet die `EntityDataSource` Steuerelement zu filtern, Sortieren und Gruppieren von Daten. In diesem Lernprogramm fügen Sie anzeigen und aktualisieren verknüpfte Daten.

Erstellen Sie die Seite "Lehrkräfte", die eine Liste der Lehrkräfte angezeigt wird. Wenn Sie einen Kursleiter auswählen, sehen Sie eine Liste der Kurse, Dozent unterrichtet. Wenn Sie einen Kurs auswählen, sehen Sie Details für den Kurs und eine Liste der Schüler in diesem Kurs registriert. Sie können den Dozenten Einstellungsdatum und bürozuweisung bearbeiten. Die Office-Zuweisung ist eine separate Entitätenmenge, die Sie über eine Navigationseigenschaft zugreifen.

Sie können Masterdaten Detaildaten im Markup oder im Code verknüpfen. In diesem Teil des Lernprogramms verwenden Sie beide Methoden.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Anzeigen und aktualisieren verknüpfte Entitäten in einem GridView-Steuerelement

Erstellen Sie eine neue Webseite mit dem Namen *Instructors.aspx* , verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup zum Rendern der `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Dieses Markup erstellt einen `EntityDataSource` Steuerelement, wählt Dozenten und Updates aktiviert. Die `div` Element konfiguriert Markup auf der linken Seite gerendert werden soll, sodass Sie eine Spalte später auf der rechten Seite hinzufügen können.

Zwischen der `EntityDataSource` Markup und das schließende `</div>` zu kennzeichnen, fügen Sie das folgende Markup, das erstellt eine `GridView` Steuerelement und ein `Label` Steuerelement, mit dem Sie Fehlermeldungen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Dies `GridView` Steuerelement ermöglicht die Zeilenauswahl, die ausgewählte Zeile mit einem hellgraue Hintergrundfarbe hervorgehoben und gibt Handler (die Sie später erstellen) für die `SelectedIndexChanged` und `Updating` Ereignisse. Gibt auch an `PersonID` für die `DataKeyNames` -Eigenschaft, sodass der Schlüsselwert der ausgewählten Zeile auf ein anderes Steuerelement übergeben werden kann, die Sie später hinzufügen.

Die letzte Spalte enthält den Dozenten bürozuweisung, die in eine Navigationseigenschaft gespeichert ist die `Person` Entität, da es einen zugehörigen Entität stammen. Beachten Sie, dass die `EditItemTemplate` -Element gibt `Eval` anstelle von `Bind`, da die `GridView` Steuerelement kann nicht direkt an Navigationseigenschaften gebunden, um diese zu aktualisieren. Aktualisieren Sie die Office-Zuweisung im Code. Zu diesem Zweck benötigen Sie einen Verweis auf die `TextBox` -Steuerelement, und Sie müssen abrufen und zum Speichern, die in der `TextBox` des Steuerelements `Init` Ereignis.

Nach der `GridView` -Steuerelement ist ein `Label` Steuerelement, das für Fehlermeldungen verwendet wird. Des Steuerelements `Visible` Eigenschaft ist `false`, und der Ansichtszustand deaktiviert ist, so, dass die Bezeichnung angezeigt wird, nur wenn Code macht sie sichtbar als Antwort auf einen Fehler.

Öffnen der *Instructors.aspx.cs* Datei, und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Fügen Sie eine private Klasse-Feld unmittelbar auf den der partiellen Klasse-Namensdeklaration, um einen Verweis auf das Textfeld der Office-Zuweisung enthalten.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Fügen Sie einen Stub für die `SelectedIndexChanged` -Ereignishandler, dass Sie Code für die spätere hinzufügen. Auch hinzufügen, einen Handler für die Office-Zuweisung `TextBox` des Steuerelements `Init` Ereignis, damit Sie einen Verweis auf Speichern, können die `TextBox` Steuerelement. Sie müssen diesen Verweis verwenden, um dem Wert vom Benutzer eingegebenen zum Aktualisieren der zugeordneten Navigationseigenschaft Entität abzurufen.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Verwenden Sie die `GridView` des Steuerelements `Updating` das zu aktualisierende Ereignis der `Location` Eigenschaft der zugeordneten `OfficeAssignment` Entität. Fügen Sie den folgenden Ereignishandler für das `Updating` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Dieser Code ausgeführt wird, wenn der Benutzer klickt **Update** in einem `GridView` Zeile. Der Code verwendet LINQ to Entities zum Abrufen der `OfficeAssignment` Entität, die mit dem aktuellen verknüpft ist `Person` Entität mithilfe der `PersonID` der ausgewählten Zeile aus der das Ereignisargument.

Der Code führt dann eine der folgenden Aktionen je nach dem Wert in der `InstructorOfficeTextBox` Steuerelement:

- Wenn das Textfeld einen Wert ist, und es ist keine `OfficeAssignment` Entität zu aktualisieren, erstellt es einen.
- Wenn das Textfeld einen Wert ist, und es ist ein `OfficeAssignment` -Entität aktualisiert die `Location` Eigenschaftswert.
- Wenn das Textfeld leer ist, und eine `OfficeAssignment` Entität vorhanden ist, wird die Entität gelöscht.

Danach müssen die Änderungen in der Datenbank gespeichert. Wenn eine Ausnahme auftritt, wird eine Fehlermeldung angezeigt.

Führen Sie die Seite.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Klicken Sie auf **bearbeiten** und ändern Sie alle Felder in Textfeldern.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Ändern Sie diese Werte, einschließlich **Bürozuweisung**. Klicken Sie auf **Update** und sehen Sie die Änderungen in der Liste wiedergegeben.

## <a name="displaying-related-entities-in-a-separate-control"></a>Anzeigen von verknüpften Entitäten in einem separaten Steuerelement

Jede Dozenten kann werden folgende Themen behandelt einen oder mehrere Kurse, damit Sie hinzufügen, eine `EntityDataSource` Steuerelement und ein `GridView` Steuerelement zum Auflisten der Kurse zugeordnet, unabhängig davon, welche Dozenten in die Kursleiter ausgewählt ist `GridView` Steuerelement. Zum Erstellen einer Überschrift und der `EntityDataSource` für Kurse Entitäten zu steuern, fügen Sie das folgende Markup zwischen der Fehlermeldung `Label` -Steuerelement und das schließende `</div>` Tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Die `Where` Parameter enthält den Wert des der `PersonID` der Kursleiter, deren Zeile, in ausgewählt ist, der `InstructorsGridView` Steuerelement. Die `Where` Eigenschaft enthält einen untergeordneten SELECT-Befehl, der alle zugeordneten ruft `Person` Entitäten aus einer `Course` Entität `People` Navigationseigenschaft und wählt die `Course` Entität nur, wenn eine der zugehörigen `Person`Entitäten enthält die ausgewählten `PersonID` Wert.

Zum Erstellen der `GridView` Steuerelement. Fügen Sie das folgende Markup unmittelbar nach der `CoursesEntityDataSource` Steuerelement (vor der schließenden `</div>` Tag):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Da keine Kurse angezeigt werden, wenn keine Dozenten ausgewählt ist, ein `EmptyDataTemplate` Element enthalten ist.

Führen Sie die Seite.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Wählen einen Kursleiter, der einen oder mehrere Kurse zugewiesen hat, und dem Kurs oder Kurse in der Liste angezeigt. (Hinweis: das Datenbankschema lässt mehrere Kurse in den Testdaten, die im Lieferumfang der Datenbank keine Dozenten tatsächlich hat mehr als einen Kurs. Sie können mit der Datenbank Kurse hinzufügen, selbst mit dem **Server-Explorer** Fenster oder die *CoursesAdd.aspx* Seite, die Sie in einem späteren Lernprogramm hinzufügen.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Die `CoursesGridView` Steuerelement zeigt nur wenige Kurs Felder. Um alle Details für einen Kurs anzuzeigen, verwenden Sie eine `DetailsView` Steuerelement für den Kurs, die der Benutzer auswählt. In *Instructors.aspx*, fügen Sie das folgende Markup nach dem schließenden `</div>` Tag (Stellen Sie sicher, dass Sie dieses Markup platzieren **nach** schließende Div-tag nicht vor):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Dieses Markup erstellt ein `EntityDataSource` Steuerelement gebunden ist, das die `Courses` Entitätenmenge. Die `Where` Eigenschaft wählt einen Kurs mithilfe der `CourseID` Wert der ausgewählten Zeile in der Courses `GridView` Steuerelement. Das Markup gibt einen Handler für das `Selected` -Ereignis, das Sie später für die Anzeige von Student Qualitäten verwendet werden, die eine andere Ebene in der Hierarchie ist.

In *Instructors.aspx.cs*, erstellen Sie die folgenden Stub für die `CourseDetailsEntityDataSource_Selected` Methode. (Sie müssen diese Stub füllen Sie später in diesem Lernprogramm; vorerst Sie ihn benötigen, damit die Seite kompiliert und ausgeführt werden kann.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Führen Sie die Seite.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Anfänglich stehen keine Kursdetails, da kein Kurs ausgewählt wird. Wählen Sie einen Kursleiter, der einen Kurs zugewiesen hat, und wählen Sie dann einen Kurs, um die Details anzuzeigen.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Mithilfe der EntityDataSource aktiviert Ereignis, um verknüpfte Daten anzeigen""

Schließlich möchten Sie alle registrierten Studenten und deren Bewertungen für den ausgewählten Kurs anzeigen. Zu diesem Zweck verwenden Sie die `Selected` -Ereignis für die `EntityDataSource` Steuerelement gebunden werden, um den Kurs `DetailsView`.

In *Instructors.aspx*, fügen Sie das folgende Markup nach dem `DetailsView` Steuerelement:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Dieses Markup erstellt einen `ListView` Steuerelement, das eine Liste der Schüler und deren Bewertungen für den ausgewählten Kurs anzeigt. Keine Datenquelle wird angegeben, da Sie Databind das Steuerelement im Code müssen. Die `EmptyDataTemplate` Element enthält eine Meldung angezeigt, wenn kein Kurs ausgewählt ist – in diesem Fall befinden sich keine Schüler angezeigt. Die `LayoutTemplate` Element erstellt eine HTML-Tabelle, um die Liste anzuzeigen und die `ItemTemplate` gibt die anzuzeigenden Spalten an. Studenten-ID und die Student Grade stammen aus den `StudentGrade` Entität und den Namen Student stammt aus der `Person` Entität, die das Entity Framework in verfügbar macht die `Person` Navigationseigenschaft der `StudentGrade` Entität.

In *Instructors.aspx.cs*, ersetzen Sie die Stub-Out `CourseDetailsEntityDataSource_Selected` -Methode durch folgenden Code:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Das Ereignisargument für dieses Ereignis enthält die ausgewählten Daten in Form einer Auflistung, die haben wird, um ein Element oder 0 (null) Elemente, wenn keine Auswahl getroffen wurde Wenn eine `Course` ausgewählt ist. Wenn eine `Course` Entität ausgewählt ist, wird der Code verwendet die `First` Methode, um der Auflistung in ein einzelnes Objekt konvertiert. Anschließend ruft es `StudentGrade` Entitäten aus der Navigationseigenschaft konvertiert diese in einer Auflistung und bindet die `GradesListView` Steuerelement in der Auflistung.

Dies ist ausreichend Besoldungsgruppen angezeigt, aber Sie möchten sicherstellen, dass die Nachricht in der Vorlage für leere Daten erstmalig angezeigt wird, auf die Seite angezeigt wird, und bei jedem Kurs nicht ausgewählt ist. Zu diesem Zweck erstellen Sie die folgende Methode, die an zwei Stellen heraus aufgerufen werden müssen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Rufen Sie diese neue Methode aus der `Page_Load` Methode, um die leere Daten zum ersten Mal angezeigt, die Seite wird angezeigt. Ein, und rufen sie über die `InstructorsGridView_SelectedIndexChanged` Methode, da das Ereignis ausgelöst wird, wenn ein Kursleiter ausgewählt ist, was bedeutet, dass neue Kurse werden in der Courses geladen `GridView` Steuerelement und keine noch ausgewählt ist. Im folgenden sind die beiden Aufrufe aus:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Führen Sie die Seite.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Wählen Sie einen Kursleiter, der einen Kurs zugewiesen wurde, und wählen Sie dann den Kurs.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Sie haben jetzt einige Verfahren zum Arbeiten mit verknüpften Daten angezeigt. Im folgenden Lernprogramm erfahren Sie zum Hinzufügen von Beziehungen zwischen Entitäten, Beziehungen zu entfernen, und wie Sie eine neue Entität hinzufügen, die eine zu einer vorhandenen Entität Beziehung.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-5.md)
