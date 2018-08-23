---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 5 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 3209ab3bca58e0dde90cf279732d177418b034e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836789"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 5
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Arbeiten mit zugehörigen Daten, Fortsetzung

Im vorherigen Tutorial begonnen hat, verwenden Sie die `EntityDataSource` Steuerelement zum Arbeiten mit verknüpften Daten. Sie mehrere Ebenen der Hierarchie angezeigt, und Daten in den Navigationseigenschaften bearbeitet. In diesem Tutorial können Sie weiterhin mit verbundenen Daten arbeiten, indem hinzufügen und Löschen von Beziehungen und Hinzufügen einer neuen Entität, die eine Beziehung zu einer vorhandenen Entität verfügt.

Erstellen Sie eine Seite, die Kurse hinzufügt, die Abteilungen zugewiesen sind. Die Abteilungen bereits vorhanden sind, und wenn Sie einen neuen Kurs erstellen, gleichzeitig werden erstellen Sie eine Beziehung zwischen diesem und einer vorhandenen Abteilung.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Sie erstellen auch eine Seite, die funktioniert mit einer m: n Beziehung durch ein Dozent einem Kurs (Hinzufügen einer Beziehung zwischen zwei Entitäten, die Sie auswählen) zuweisen oder entfernen einen Dozenten aus einem (durch das Entfernen einer Beziehung zwischen zwei Entitäten, die Sie Wählen Sie). Hinzufügen einer Beziehung zwischen einem Dozenten sowie einen Kurs führt in der Datenbank in eine neue Zeile hinzugefügt wird die `CourseInstructor` Zuordnungstabelle: entfernen eine Beziehung umfasst das Löschen einer Zeile aus der `CourseInstructor` Zuordnungstabelle. Aber Sie hierzu im Entity Framework festlegen Navigationseigenschaften, ohne auf die `CourseInstructor` Tabelle explizit.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Beim Hinzufügen einer Entität mit einer Beziehung zu einer vorhandenen Entität

Erstellen einer neuen Webseite mit dem Namen *CoursesAdd.aspx* , verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup zu der `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Dieses Markup erstellt eine `EntityDataSource` Steuerelement, das Kurse, wählt, ermöglicht das Einfügen von angibt und festlegt, einen Handler für die `Inserting` Ereignis. Verwenden Sie zum Aktualisieren den Handler die `Department` Navigationseigenschaft, die beim Erstellen eines neuen `Course` Entität wird erstellt.

Das Markup erstellt außerdem eine `DetailsView` Steuerelements angeben, für das Hinzufügen neuer `Course` Entitäten. Das Markup verwendet gebundene Felder für `Course` Entitätseigenschaften. Sie haben, geben die `CourseID` bewertet werden, da dies kein vom System generierte ID-Feld ist. Stattdessen ist es eine Kursnummer, die manuell angegeben werden muss, wenn der Kurs erstellt wird.

Verwenden Sie eine Vorlagenfeld für die `Department` Navigationseigenschaft da Navigationseigenschaften verwendet werden können, mit `BoundField` Steuerelemente. Das Vorlagenfeld enthält eine Dropdownliste, um die Abteilung auswählen. Die Dropdown Liste an gebunden ist die `Departments` Entitätenmenge mit `Eval` statt `Bind`, erneut aus, da Sie direkt Navigationseigenschaften Binden nicht möglich, um sie zu aktualisieren. Geben Sie einen Handler für die `DropDownList` des Steuerelements `Init` Ereignis, damit Sie einen Verweis auf das Steuerelement für die Verwendung vom Code speichern können, die aktualisiert, die `DepartmentID` Fremdschlüssel.

In *CoursesAdd.aspx.cs* direkt nach der Deklaration der partiellen Klasse Klassenfeld hinzufügen, eine um einen Verweis auf die `DepartmentsDropDownList` Steuerelement:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Hinzufügen eines ereignishandlers für das `DepartmentsDropDownList` des Steuerelements `Init` Ereignis, damit Sie einen Verweis auf das Steuerelement speichern können. Dadurch können Sie rufen Sie den Wert, der der Benutzer eingegeben wurde, und verwenden sie zum Aktualisieren der `DepartmentID` Wert, der die `Course` Entität.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Hinzufügen eines ereignishandlers für das `DetailsView` des Steuerelements `Inserting` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Wenn der Benutzer klickt `Insert`, `Inserting` Ereignis wird ausgelöst, bevor der neue Datensatz eingefügt wird. Ruft der Code im Ereignishandler der `DepartmentID` aus der `DropDownList` steuern und wird verwendet, um den Wert festzulegen, die für verwendet werden, die `DepartmentID` Eigenschaft der `Course` Entität.

Das Entity Framework kümmert sich diesen Kurs zum Hinzufügen der `Courses` der zugeordneten Navigationseigenschaft `Department` Entität. Es fügt auch die Abteilung der `Department` Navigationseigenschaft der `Course` Entität.

Führen Sie die Seite.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Geben Sie eine ID, einen Titel, eine Anzahl Guthaben, wählen Sie eine Abteilung ein, und klicken Sie auf **einfügen**.

Führen Sie die *Courses.aspx* Seite, und wählen Sie auf den neuen Kurs finden Sie unter derselben Abteilung.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Arbeiten mit m: N Beziehungen

Die Beziehung zwischen der `Courses` Entitätenmenge und `People` Entitätenmenge ist eine m: n Beziehung. Ein `Course` Entität verfügt über eine Navigationseigenschaft, die mit dem Namen `People` darf, die 0 (null), ein oder mehrere der im Zusammenhang `Person` Entitäten (für Dozenten, bringen Sie dieser Kurs zugewiesen). Und ein `Person` Entität verfügt über eine Navigationseigenschaft, die mit dem Namen `Courses` darf, die 0 (null), ein oder mehrere der im Zusammenhang `Course` Entitäten (Kurse darstellt, "Instructor" bringen Sie zugewiesen ist). Bringen Sie einen Dozenten möglicherweise mehrere Kurse und ein Kurs kann von mehreren Dozenten unterrichtet werden. In diesem Abschnitt der exemplarischen Vorgehensweise werden Sie hinzufügen und entfernen Sie Beziehungen zwischen `Person` und `Course` Entitäten durch Aktualisieren der Eigenschaften von verknüpften Entitäten.

Erstellen einer neuen Webseite mit dem Namen *InstructorsCourses.aspx* , verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup zu der `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Dieses Markup erstellt eine `EntityDataSource` Steuerelement, das den Namen abruft und `PersonID` von `Person` Entitäten für Dozenten. Ein `DropDrownList` -Steuerelement gebunden ist, die `EntityDataSource` Steuerelement. Die `DropDownList` Steuerelement gibt einen Handler für die `DataBound` Ereignis. Verwenden Sie diese Handler Databind die beiden Dropdownlisten, die Kurse anzeigen.

Das Markup erstellt außerdem die folgende Gruppe von Steuerelementen zur Verwendung für einen Kurs dem Dozenten zugewiesen:

- Ein `DropDownList` Steuerelement zum Auswählen eines Kurses zuweisen. Dieses Steuerelement wird mit Kursen aufgefüllt, die derzeit nicht zu dem Dozenten zugewiesen sind.
- Ein `Button` Steuerelement, um die Zuweisung zu initiieren.
- Ein `Label` Steuerelement, das eine Fehlermeldung angezeigt, wenn die Zuweisung ein Fehler auftritt.

Schließlich erstellt das Markup auch eine Gruppe von Steuerelementen für einen Kurs aus den ausgewählten Dozenten zu entfernen.

In *InstructorsCourses.aspx.cs*, fügen Sie eine using Anweisung:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Fügen Sie eine Methode zum Auffüllen der zwei Dropdownlisten, die Kurse anzeigen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Dieser Code Ruft alle Kurse aus der `Courses` Entitätenmengen und ruft die Kurse aus der `Courses` Navigationseigenschaft der `Person` Entität für den ausgewählten Dozenten. Klicken Sie dann bestimmt, welchen Kursen dieses Dozenten zugewiesen sind, und es wird die Dropdown-Listen entsprechend aufgefüllt.

Hinzufügen eines ereignishandlers für das `Assign` Schaltfläche `Click` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Dieser Code Ruft die `Person` Entität für den ausgewählten Dozenten, ruft der `Course` Entität für den ausgewählten Kurs, und fügt den ausgewählten Kurs aus, um die `Courses` -Navigationseigenschaft des Dozenten `Person` Entität. Klicken Sie dann die Änderungen in der Datenbank gespeichert, und füllt die Dropdown-Listen, damit die Ergebnisse sofort angezeigt werden.

Hinzufügen eines ereignishandlers für das `Remove` Schaltfläche `Click` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Dieser Code Ruft die `Person` Entität für den ausgewählten Dozenten, ruft der `Course` Entität für den ausgewählten Kurs, und entfernt den ausgewählten Kurs aus der `Person` Entität `Courses` Navigationseigenschaft. Klicken Sie dann die Änderungen in der Datenbank gespeichert, und füllt die Dropdown-Listen, damit die Ergebnisse sofort angezeigt werden.

Fügen Sie Code in die `Page_Load` -Methode, die sicherstellt, dass die Fehlermeldungen sind nicht sichtbar, wenn kein Fehler zu melden, Hinzufügen von Ereignishandlern für die `DataBound` und `SelectedIndexChanged` Ereignisse der Dozenten Dropdown-Liste, um die Dropdownlisten Kurse zu füllen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Führen Sie die Seite.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Wählen Sie einen Dozenten. Die <strong>weisen Sie einen Kurs</strong> Dropdown-Liste zeigt die Kurse, die der Dozenten zu unterrichten, nicht, und die <strong>entfernen Sie einen Kurs</strong> Dropdown-Liste zeigt die Kurse, die bereits der Dozenten zugewiesen ist. In der <strong>weisen Sie einen Kurs</strong> Abschnitt, wählen Sie einen Kurs aus, und klicken Sie dann auf <strong>weisen</strong>. Der Kurs verschiebt, in der <strong>entfernen Sie einen Kurs</strong> Dropdown-Liste. Wählen Sie einen Kurs in der <strong>entfernen Sie einen Kurs</strong> aus, und klicken Sie auf <strong>entfernen</strong><em>.</em> Der Kurs verschiebt, in der <strong>weisen Sie einen Kurs</strong> Dropdown-Liste.

Sie haben nun einige weitere Möglichkeiten zum Arbeiten mit verknüpften Daten erfahren. Im folgenden Tutorial erfahren Sie, wie Sie die Vererbung im Datenmodell zu verwenden, um die Verwaltbarkeit Ihrer Anwendung zu verbessern.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-6.md)
