---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 5 | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7f351718123e881e832f4ac95af506ed601d3337
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 5
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Arbeiten mit verwandten Daten, Fortsetzung

Im vorherigen Lernprogramm begonnen hat, verwenden Sie die `EntityDataSource` -Steuerelement für die Arbeit mit verbundenen Daten. Sie mehrere Ebenen der Hierarchie angezeigt, und Bearbeiten von Daten in den Navigationseigenschaften. In diesem Lernprogramm werden Sie weiterhin die Arbeit mit verbundenen Daten hinzufügen und Löschen von Beziehungen und indem Sie eine neue Entität, die eine zu einer vorhandenen Entität Beziehung hinzufügen.

Erstellen Sie eine Seite Kurse hinzu, die Abteilungen zugewiesen sind. Die Abteilungen bereits vorhanden, und wenn Sie einen neuen Kurs erstellen, gleichzeitig müssen erstellen Sie eine Beziehung zwischen diesem und einem vorhandenen Abteilung.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Erstellen Sie auch eine Seite, die arbeitet mit einer m: n-Beziehung durch einen Kursleiter einem Kurs (Hinzufügen einer Beziehung zwischen zwei Entitäten, die Sie auswählen) zuweisen oder entfernen einen Kursleiter aus einem Kurs (entfernen eine Beziehung zwischen zwei Entitäten, die Sie Wählen Sie). Hinzufügen einer Beziehung zwischen einer Kursleiter und ein Kurs führt in der Datenbank in eine neue Zeile hinzugefügt wird die `CourseInstructor` Zuordnungstabelle, die das Entfernen von eine Beziehung umfasst das Löschen einer Zeile aus der `CourseInstructor` Zuordnungstabelle. Allerdings erfolgt dies in Entity Framework durch Festlegen von Navigationseigenschaften, ohne auf die `CourseInstructor` explizit Tabelle.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Eine Entität mit einer Beziehung hinzufügen mit einer vorhandenen Entität

Erstellen Sie eine neue Webseite mit dem Namen *CoursesAdd.aspx* , verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup zum Rendern der `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Dieses Markup erstellt einen `EntityDataSource` Steuerelement, das wählt Kurse, mit der einfügen und angibt, einen Handler für das `Inserting` Ereignis. Verwenden Sie den Ereignishandler zum Aktualisieren der `Department` Navigationseigenschaft, die beim Erstellen eines neuen `Course` Entität erstellt wird.

Das Markup erstellt außerdem eine `DetailsView` Steuerelements angeben, für das Hinzufügen neuer `Course` Entitäten. Das Markup verwendet gebundene Felder für `Course` Entitätseigenschaften. Sie müssen die eingeben der `CourseID` bewertet werden, da dies ein vom System generierter ID-Feld nicht ist. Stattdessen erfolgt eine Kurs-Zahl, die manuell angegeben werden muss, wenn der Kurs erstellt wird.

Verwenden Sie eine Vorlagenfeld für die `Department` Navigationseigenschaft mit Navigationseigenschaften verwendet werden können `BoundField` Steuerelemente. Das Vorlagenfeld enthält eine Dropdownliste zum Auswählen der Abteilung. Die Dropdown Liste an gebunden ist die `Departments` Entitätenmenge mit `Eval` statt `Bind`, erneut aus, da Sie direkt Navigationseigenschaften Bindung nicht möglich, um diese zu aktualisieren. Geben Sie einen Handler für das `DropDownList` des Steuerelements `Init` Ereignis, damit Sie einen Verweis auf das Steuerelement für die Verwendung vom Code speichern können, die aktualisiert die `DepartmentID` Fremdschlüssel.

In *CoursesAdd.aspx.cs* fügen Sie direkt hinter der Deklaration der partiellen Klasse ein Klassenfeld enthält einen Verweis auf die `DepartmentsDropDownList` Steuerelement:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Fügen Sie einen Handler für das `DepartmentsDropDownList` des Steuerelements `Init` Ereignis, damit Sie einen Verweis auf das Steuerelement speichern können. Auf diese Weise können Sie die rufen Sie den Wert, der der Benutzer eingegeben hat, und verwenden es zum Aktualisieren der `DepartmentID` Wert, der die `Course` Entität.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Fügen Sie einen Handler für das `DetailsView` des Steuerelements `Inserting` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Wenn der Benutzer klickt `Insert`die `Inserting` Ereignis wird ausgelöst, bevor der neue Datensatz eingefügt wird. Ruft der Code im Ereignishandler die `DepartmentID` aus der `DropDownList` steuern und verwendet, um den Wert festlegen, die für verwendet werden, die `DepartmentID` Eigenschaft der `Course` Entität.

Das Entity Framework übernimmt das Hinzufügen von diesem Kurs zu den `Courses` der zugeordneten Navigationseigenschaft `Department` Entität. Es fügt auch die Abteilung, für die `Department` Navigationseigenschaft der `Course` Entität.

Führen Sie die Seite.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Geben Sie eine ID, einen Titel, eine Anzahl von Gutschriften, wählen Sie eine Abteilung ein, und klicken Sie auf **einfügen**.

Führen Sie die *Courses.aspx* Seite, und wählen Sie derselben Abteilung, um den neuen Kurs anzuzeigen.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Arbeiten mit m: N-Beziehungen

Die Beziehung zwischen der `Courses` Entitätenmenge und `People` Entitätenmenge ist eine m: n-Beziehung. Ein `Course` Entität verfügt über eine Navigationseigenschaft, die mit dem Namen `People` , kann 0 (null), ein oder mehrere der im Zusammenhang enthalten `Person` Entitäten (Lehrkräfte zugewiesen, zu diesem Kurs zu schulen darstellt). Und ein `Person` Entität verfügt über eine Navigationseigenschaft, die mit dem Namen `Courses` , kann 0 (null), ein oder mehrere der im Zusammenhang enthalten `Course` Entitäten (Kurse darstellt, Dozenten zugewiesen werden folgende Themen behandelt). Einen Kursleiter möglicherweise mehrere Kurse werden folgende Themen behandelt, und ein Kurs kann durch mehrere Lehrkräfte behandelt werden. In diesem Abschnitt der exemplarischen Vorgehensweise müssen Sie hinzufügen und Entfernen von Beziehungen zwischen `Person` und `Course` Entitäten, indem Sie die Navigationseigenschaften von verknüpften Entitäten aktualisieren.

Erstellen Sie eine neue Webseite mit dem Namen *InstructorsCourses.aspx* , verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup zum Rendern der `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Dieses Markup erstellt einen `EntityDataSource` Steuerelement, das den Namen abruft und `PersonID` von `Person` für Kursleiter für Entitäten. Ein `DropDrownList` gebunden ist die `EntityDataSource` Steuerelement. Die `DropDownList` Steuerelement gibt einen Handler für das `DataBound` Ereignis. Dieser Handler auf Databind die beiden Dropdownlisten verwenden, die Kurse anzeigen.

Das Markup erstellt außerdem die folgende Gruppe von Steuerelementen zum Zuweisen von einen Kurs zu den ausgewählten Dozenten verwendet werden soll:

- Ein `DropDownList` Steuerelement für die Auswahl eines Kurs zuweisen. Dieses Steuerelement wird mit dem Kurse aufgefüllt, die derzeit nicht für den ausgewählten Kursleiter zugewiesen sind.
- Ein `Button` Steuerelement, um die Zuweisung zu initiieren.
- Ein `Label` Steuerelement eine Fehlermeldung angezeigt, wenn die Zuordnung ein Fehler auftritt.

Das Markup erstellt auch eine Gruppe von Steuerelementen zum Entfernen eines Kurs aus den ausgewählten Dozenten verwendet werden soll.

In *InstructorsCourses.aspx.cs*, fügen Sie eine using Anweisung:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Fügen Sie eine Methode für das Auffüllen von zwei Dropdownlisten, die Kurse anzeigen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Dieser Code Ruft alle Kurse aus der `Courses` Entität festgelegt, und ruft die Kurse aus der `Courses` Navigationseigenschaft der `Person` Entität für den ausgewählten Kursleiter. Klicken Sie dann bestimmt, welche Kurse, Dozenten zugewiesen sind, und die Dropdownlisten entsprechend aufgefüllt.

Fügen Sie einen Handler für das `Assign` Schaltfläche `Click` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Dieser Code Ruft die `Person` Entität für den ausgewählten Kursleiter Ruft die `Course` Entität für den ausgewählten Kurs und fügt die ausgewählten Kurs zu den `Courses` Navigationseigenschaft der Kursleiter `Person` Entität. Klicken Sie dann die Änderungen in der Datenbank gespeichert, und füllt die Dropdown-Listen, damit die Ergebnisse sofort angezeigt werden können.

Fügen Sie einen Handler für das `Remove` Schaltfläche `Click` Ereignis:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Dieser Code Ruft die `Person` Entität für den ausgewählten Kursleiter Ruft die `Course` Entität für den ausgewählten Kurs und entfernt den ausgewählten Kurs aus der `Person` Entität `Courses` Navigationseigenschaft. Klicken Sie dann die Änderungen in der Datenbank gespeichert, und füllt die Dropdown-Listen, damit die Ergebnisse sofort angezeigt werden können.

Fügen Sie Code zum der `Page_Load` -Methode, die sicherstellt, dass die Fehlermeldungen sind nicht sichtbar, wenn kein Fehler zu melden, und fügen Ereignishandler für die `DataBound` und `SelectedIndexChanged` Ereignisse der Lehrkräfte Dropdown-Liste zum Auffüllen der Courses-Dropdownlisten:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Führen Sie die Seite.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Wählen Sie einen Kursleiter. Die <strong>weisen einen Kurs</strong> Dropdown-Liste zeigt die Kurse, die der Dozenten zu vermitteln, nicht, und die <strong>entfernen einen Kurs</strong> Dropdown-Liste zeigt die Kurse, die der Dozenten bereits zugewiesen ist. In der <strong>weisen einen Kurs</strong> Abschnitt, wählen Sie einen Kurs aus, und klicken Sie dann auf <strong>zuweisen</strong>. Verschiebt der Kurs der <strong>entfernen einen Kurs</strong> Dropdown-Liste. Wählen Sie einen Kurs in der <strong>entfernen einen Kurs</strong> Abschnitt, und klicken Sie auf <strong>entfernen</strong><em>.</em> Verschiebt der Kurs der <strong>weisen einen Kurs</strong> Dropdown-Liste.

Sie haben jetzt einige weitere Verfahren zum Arbeiten mit verknüpften Daten angezeigt. Im folgenden Lernprogramm erfahren Sie, wie Vererbung in das Datenmodell zu verwenden, um die Verwaltbarkeit Ihrer Anwendung zu verbessern.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-6.md)
