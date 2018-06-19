---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 2 | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a6c95a92aa77e2bb73aa513a207e0469d1aedbd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890553"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 2
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource-Steuerelements

Im vorherigen Lernprogramm haben Sie eine Website, eine Datenbank und ein Datenmodell erstellt. In diesem Lernprogramm arbeiten Sie mit der `EntityDataSource` -Steuerelement, das ASP.NET zu erhalten, sodass ein Entity Framework-Datenmodells arbeiten erleichtern. Erstellen Sie eine `GridView` Steuerelement zum Anzeigen und Bearbeiten von Daten zu Studenten, eine `DetailsView` Steuerelement zum Hinzufügen von neuen Studenten und ein `DropDownList` Steuerelement für die Auswahl einer Abteilung (die Sie später für die Anzeige von zugeordneten Kurse verwendet werden).

[![image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Beachten Sie, dass in dieser Anwendung Sie wird nicht werden hinzufügen Validierung von Benutzereingaben zu Seiten, die die Datenbank zu aktualisieren, und einige der Fehlerbehandlung weniger robust werden als erforderlich ist, in einer produktionsanwendung. Behält das Lernprogramm konzentriert sich auf das Entity Framework und verhindert, dass er zu lang. Weitere Informationen dazu, wie diese Funktionen zu einer Anwendung hinzufügen, finden Sie unter [Validieren von Benutzereingaben in ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) und [Error Handling in ASP.NET Webseiten und Anwendungen](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Hinzufügen und Konfigurieren des EntityDataSource-Steuerelements

Sie beginnen, indem Sie konfigurieren eine `EntityDataSource` Steuerelement lesen `Person` Entitäten aus der `People` Entitätenmenge.

Stellen Sie sicher, dass Sie Visual Studio geöffnet haben, und, dass Sie mit dem Projekt arbeiten, ist Sie in Teil 1 erstellt. Wenn Sie seit der Erstellung des Datenmodells oder seit der letzten Änderung, das Sie vorgenommen haben, das Projekt erstellt haben, erstellen Sie jetzt das Projekt. Änderungen am Datenmodell sind nicht zur Verfügung gestellt Designer erst das Projekt erstellt wird.

Erstellen Sie eine neue Webseite mit den **Webformular mit Gestaltungsvorlage** Vorlage, und nennen Sie sie *Students.aspx*.

[![image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Geben Sie *Site.Master* als die Gestaltungsvorlage. Alle Seiten, die Sie für diese Lernprogramme erstellen verwendet diese Masterseite.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

In **Quelle** anzeigen, Hinzufügen einer `h2` Überschrift, um die `Content` Steuerelement namens `Content2`, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Aus der **Daten** auf der Registerkarte die **Toolbox**, ziehen Sie ein `EntityDataSource` auf der Seite zu steuern, legen Sie es unterhalb der Überschrift und ändern Sie die ID in `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Wechseln Sie zur **Entwurf** anzeigen, klicken Sie auf das Datenquellensteuerelement Smarttag, und klicken Sie dann auf **Datenquelle konfigurieren** zum Starten der **Datenquelle konfigurieren** Assistenten.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

In der **ObjectContext konfigurieren** Schritt des Assistenten, wählen **den Eintrag SchoolEntities** als Wert für **Verbindung mit dem Namen**, und wählen Sie **den Eintrag SchoolEntities**als die **DefaultContainerName** Wert. Klicken Sie dann auf **Weiter**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Hinweis: Wenn Sie das folgende Dialogfeld an diesem Punkt erhalten, müssen Sie zum Erstellen des Projekts, bevor Sie fortfahren.

[![image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

In der **Datenauswahl konfigurieren** Schritt wählen **Personen** als Wert für **EntitySetName**. Unter **wählen**, stellen Sie sicher, dass die **wählen Sie ein** ll Kontrollkästchen aktiviert ist. Wählen Sie dann die Optionen zum Aktivieren von aktualisieren und löschen. Wenn Sie fertig sind, klicken Sie auf **Fertig stellen**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurieren von Datenbankregeln, um löschen zu ermöglichen

Sie erstellen eine Seite, die Benutzer Studenten löschen kann die `Person` Tabelle, die drei Beziehungen zu anderen Tabellen hat (`Course`, `StudentGrade`, und `OfficeAssignment`). Wird standardmäßig die Datenbank wird verhindert, dass Sie löschen eine Zeile in `Person` verknüpfte Zeilen in einem der anderen Tabellen vorhanden sind. Sie können die zugehörigen Zeilen manuell zuerst löschen, oder Sie können konfigurieren, dass die Datenbank, um diese automatisch gelöscht, wenn Sie löschen eine `Person` Zeile. Für Studentendatensätze in diesem Lernprogramm konfigurieren Sie die Datenbank, um die verknüpften Daten automatisch zu löschen. Da Studenten Zeilen verknüpft haben, können nur in der `StudentGrade` Tabelle, müssen Sie nur eine der drei Beziehungen konfigurieren.

Bei Verwendung der *School.mdf* Datei, die Sie aus dem Projekt heruntergeladen haben, die für dieses Lernprogramm geht, können Sie diesen Abschnitt überspringen, da diese Änderungen an der Konfiguration wurden bereits geschehen. Wenn Sie die Datenbank durch Ausführen eines Skripts erstellt haben, können konfigurieren Sie die Datenbank durch Ausführen der folgenden Verfahren.

In **Server-Explorer**, öffnen Sie das Datenbankdiagramm, die Sie in Teil 1 erstellt haben. Mit der rechten Maustaste in der Beziehung zwischen `Person` und `StudentGrade` (die Linie zwischen Tabellen), und wählen Sie dann **Eigenschaften**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

In der **Eigenschaften** Fenster, erweitern Sie **INSERT- und UPDATE-Spezifikation** und legen Sie die **DeleteRule** Eigenschaft **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Speichern Sie und schließen Sie das Diagramm. Wenn Sie gefragt werden, ob Sie die Datenbank aktualisieren möchten, klicken Sie auf **Ja**.

Um sicherzustellen, dass das Modell Entitäten, die im Arbeitsspeicher verfolgt mit die Datenbank ausgeführten synchron sind, müssen Sie die entsprechende Regeln im Datenmodell festlegen. Open *SchoolModel.edmx*, mit der rechten Maustaste die Assoziationslinie zwischen `Person` und `StudentGrade`, und wählen Sie dann **Eigenschaften**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

In der **Eigenschaften** legen **End1 OnDelete-** auf **Cascade**.

[![image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Speichern und schließen Sie die *SchoolModel.edmx* Datei, und klicken Sie dann das Projekt neu.

Wenn die Datenbank geändert wird, müssen Sie im Allgemeinen mehrere Optionen zum Einrichten des Modells zu synchronisieren:

- Für bestimmte Arten von Änderungen (z. B. hinzufügen oder Aktualisieren von Tabellen, Sichten oder gespeicherte Prozeduren), mit der rechten Maustaste in den Designer, und wählen **Modell aus der Datenbank aktualisieren** Designer stellen automatisch die Änderungen aufweisen.
- Generieren Sie das Datenmodell.
- Stellen Sie die manuelle Aktualisierungen, wie diese.

In diesem Fall konnte haben das Modell generiert oder aktualisiert die Tabellen, die von der beziehungsänderung betroffen sind, müssen dann aber würde die Feldnamen Änderung erneut durchgeführt (aus `FirstName` auf `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Verwenden eine GridView-Steuerelements zum Lesen und Aktualisieren von Entitäten

In diesem Abschnitt wird anhand einer `GridView` Steuerelement zum Anzeigen, aktualisieren oder löschen Studenten.

Öffnen oder wechseln Sie zur *Students.aspx* und wechseln Sie zur **Entwurf** anzeigen. Aus der **Daten** auf der Registerkarte die **Toolbox**, ziehen Sie eine `GridView` Steuerelement rechts neben der `EntityDataSource` steuern, benennen Sie sie `StudentsGridView`, klicken Sie auf das Smarttag, und wählen Sie dann  **StudentsEntityDataSource** als Datenquelle.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Klicken Sie auf **Schema aktualisieren** (klicken Sie auf **Ja** Wenn Sie aufgefordert werden, um zu bestätigen), klicken Sie dann auf **Paging aktivieren**, **Sortieren aktivieren**, **Bearbeitung aktivieren**, und **löschen aktivieren**.

Klicken Sie auf **Spalten bearbeiten**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

In der **ausgewählte Felder** löschen **PersonID**, **LastName**, und **HireDate**. Sie nicht in der Regel eine Datensatzschlüssel für Benutzer anzeigen, Einstellungsdatum ist nicht relevant für Studenten, und bringen Sie beide Teile des Namens in einem Feld, sodass Sie nur eines der Felder für die Dinge.)

[![image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Wählen Sie die **FirstMidName** Feld, und klicken Sie dann auf **dieses Feld in ein TemplateField konvertieren**.

Führen Sie dieselben Aktionen für **EnrollmentDate**.

[![image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Klicken Sie auf **OK** und wechseln Sie zur **Quelle** anzeigen. Die verbleibenden Änderungen wird einfacher, direkt im Markup. Die `GridView` Markup sieht wie im folgenden Beispiel steuern.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Die erste Spalte nach dem Befehlsfeld, der derzeit ein Vorlagenfeld ist zeigt den Vornamen an. Ändern Sie das Markup für dieses Feld "Vorlage" wie im folgenden Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

Im Anzeigemodus zwei `Label` Steuerelemente zeigen Sie die erste und letzte Bezeichnung. In den Bearbeitungsmodus wechseln werden zwei Textfelder bereitgestellt, damit Sie die vor- und Nachnamen ändern können. Wie bei der `Label` Steuerelemente im Anzeigemodus, verwenden Sie `Bind` und `Eval` würden Sie Ausdrücke, wie Sie mit ASP.NET-Datensteuerelementen für Datenquellen, die direkt mit Datenbanken herstellen. Der einzige Unterschied ist, dass Sie die Eigenschaften der Entität anstatt Datenbankspalten angegeben sind.

Die letzte Spalte ist eine Vorlagenfeld, das Registrierungsdatum anzeigt. Ändern Sie das Markup für dieses Feld, wie im folgenden Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

In beiden anzeigen und bearbeiten-Modus die Formatzeichenfolge "{0, d}" bewirkt, dass das Datum im Format "kurzes Datum" angezeigt werden. (Der Computer möglicherweise konfiguriert werden anders als die Bilder in diesem Lernprogramm erläuterten-Format anzuzeigen.)

Beachten Sie, dass in jedem dieser Vorlage Felder können der Designer verwendet eine `Bind` Ausdruck, durch die standardmäßige, aber Sie geändert haben, um eine `Eval` Ausdruck in der `ItemTemplate` Elemente. Die `Bind` Ausdruck werden die Daten in `GridView` Eigenschaften für den Fall, dass Sie benötigen Zugriff auf die Daten im Code zu steuern. In dieser Seite müssen Sie Zugriff auf diese Daten im Code, damit Sie verwenden können `Eval`, dies ist effizienter. Weitere Informationen finden Sie unter [Abrufen Ihrer Daten außerhalb des Data-Steuerelemente](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Überarbeiten EntityDataSource-Steuerelement-Markup zum Verbessern der Leistung

In das Markup für die `EntityDataSource` steuern, entfernen Sie die `ConnectionString` und `DefaultContainerName` -Attribute, und Ersetzen Sie sie durch eine `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Dies ist eine Änderung, Sie nehmen jedes Mal, wenn Sie erstellen, ein `EntityDataSource` zu steuern, es sei denn, Sie müssen eine Verbindung zu verwenden, die sich von der unterscheidet, die in der Objektkontextklasse hartcodiert ist. Mithilfe der `ContextTypeName` Attribut bietet folgende Vorteile:

- Bessere Leistung. Wenn die `EntityDataSource` Steuerelement initialisiert das Modell mithilfe der `ConnectionString` und `DefaultContainerName` Attribute, es werden zusätzliche Aufgaben zum Laden der Metadaten für jede Anforderung ausgeführt. Dies ist nicht erforderlich, wenn Sie angeben, die `ContextTypeName` Attribut.
- Verzögertes Laden ist standardmäßig aktiviert, in dem generierten Kontext Objektklassen (wie z. B. `SchoolEntities` in diesem Lernprogramm) in Entity Framework 4.0. Dies bedeutet, dass Navigationseigenschaften mit verbundenen Daten automatisch geladen werden rechts bei Bedarf. Verzögertes Laden wird weiter unten in diesem Lernprogramm ausführlicher erläutert.
- Alle Anpassungen, die auf die Objektkontextklasse angewendet haben (in diesem Fall die `SchoolEntities` Klasse) stehen, Steuerelemente, mit denen die `EntityDataSource` Steuerelement. Anpassen der Context-Klasse des Objekts ist ein-Thema für fortgeschrittene, die in diesem Lernprogramm Reihe nicht behandelt wird. Weitere Informationen finden Sie unter [Erweitern von Entity Framework generierten Typen](https://msdn.microsoft.com/library/dd456844.aspx).

Das Markup wird jetzt dem folgenden Beispiel entsprechen (die Reihenfolge der Eigenschaften möglicherweise unterschiedliche):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Die `EnableFlattening` Attribut bezieht sich auf eine Funktion, die in früheren Versionen von Entity Framework erforderlich war, da Fremdschlüsselspalten als Eigenschaften der Entität nicht verfügbar gemacht wurden. Die aktuelle Version ermöglicht das verwenden *foreign Key-Zuordnungen*, was bedeutet, dass Fremdschlüsseleigenschaften für alle bis m: n-Zuordnungen verfügbar gemacht werden. Wenn die Entitäten Fremdschlüsseleigenschaften und keine haben [komplexe Typen](https://msdn.microsoft.com/library/bb738472.aspx), lassen Sie dieses Attribut `False`. Das Attribut nicht von der Markuperweiterung entfernt werden, da der Standardwert ist `True`. Weitere Informationen finden Sie unter [vereinfachen von Objekten (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Führen Sie die Seite, und Sie sehen eine Liste der Schüler und Mitarbeiter (Sie müssen für Studenten einfach in den nächsten Lernprogrammen ' filter '). Die vor- und Nachname werden zusammen angezeigt.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Um die Anzeige zu sortieren, klicken Sie auf einen Spaltennamen an.

Klicken Sie auf **bearbeiten** in einer Zeile. Textfelder werden angezeigt, in dem Sie die vor- und Nachnamen ändern können.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Die **löschen** Schaltfläche auch funktioniert. Klicken Sie auf "löschen", für die Zeile, die ein Registrierungsdatum verfügt nicht mehr angezeigt wird. (Zeilen ohne eine Registrierungsdatum Dozenten und erhalten Sie möglicherweise einen Fehler der referenziellen Integrität. In den nächsten Lernprogrammen müssen Sie diese Liste, sodass nur Studenten filtern.)

## <a name="displaying-data-from-a-navigation-property"></a>Anzeigen von Daten von einer Navigationseigenschaft aus

Angenommen, Sie möchten wissen, wie viele Kurse ist jeder Student jetzt bei registriert. Das Entity Framework stellt diese Informationen in den `StudentGrades` Navigationseigenschaft der `Person` Entität. Da eine Student in einen Kurs registriert werden, ohne dass eine Klasse zugewiesen aufgrund des Datenbankentwurfs nicht möglich, für dieses Lernprogramm können Sie davon ausgehen, dass eine Zeile der `StudentGrade` Tabellenzeile, die mit einem Kurs anfallen entspricht dem in diesem Kurs registriert wird. (Die `Courses` Navigationseigenschaft ist nur für Lehrkräfte.)

Bei Verwendung der `ContextTypeName` Attribut von der `EntityDataSource` -Steuerelement, das Entity Framework automatisch Ruft Informationen für eine Navigationseigenschaft beim Zugriff auf diese Eigenschaft. Hierbei spricht *verzögertes Laden*. Dies kann jedoch ineffizient sein, da es einen separaten Aufruf von der Datenbank führt, die jedes Mal zusätzliche Informationen benötigt werden. Wenn Sie Daten aus der Navigationseigenschaft für jede Entität zurückgegebenes benötigen die `EntityDataSource` -Steuerelement, es ist jedoch effizienter, die verknüpften Daten zusammen mit der Entität selbst in einem einzigen Aufruf der Datenbank abgerufen. Hierbei spricht *unverzüglichem Laden*, und geben Sie für eine Navigationseigenschaft unverzüglichem Laden durch Festlegen der `Include` Eigenschaft von der `EntityDataSource` Steuerelement.

In *Students.aspx*, die Anzahl der Kurse für jede Student angezeigt werden sollen, sodass unverzüglichem Laden ist die beste Wahl. Wenn Sie alle Studenten anzeigen, jedoch mit der Anzahl der Kurse wurden möglicherweise nur für einige davon (, müsste das Schreiben von Code neben dem Markup), verzögertes Laden eine bessere Wahl sein.

Öffnen oder wechseln Sie zur *Students.aspx*, wechseln Sie zur **Entwurf** wählen `StudentsEntityDataSource`, und klicken Sie in der **Eigenschaften** Fenster Satz der **Include**Eigenschaft **StudentGrades**. (Wenn Sie mehrere Navigationseigenschaften abrufen möchten, geben Sie deren Namen durch Kommas getrennt – z. B. **StudentGrades, Kursen**.)

[![image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Wechseln Sie zur **Quelle** anzeigen. In der `StudentsGridView` -Steuerelement, nach dem letzten `asp:TemplateField` -Element, das folgende Vorlagenfeld der neuen hinzufügen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

In der `Eval` Ausdruck können Sie die Navigationseigenschaft verweisen `StudentGrades`. Da diese Eigenschaft eine Auflistung enthält, hat eine `Count` -Eigenschaft, die Sie verwenden können, um die Anzahl der Kurse anzeigen, in dem die Studenten registriert ist. In einem späteren Lernprogramm sehen Sie, wie Daten von Navigationseigenschaften dargestellt, die einzelne Entitäten anstelle von Sammlungen enthalten. (Beachten Sie, das nicht `BoundField` Elemente zum Anzeigen von Daten von Navigationseigenschaften.)

Führen Sie die Seite, und Sie nun sehen, wie viele Kurse Student registriert ist.

[![image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Verwenden ein DetailsView-Steuerelement zum Einfügen von Entitäten

Der nächste Schritt besteht, um eine Seite zu erstellen, verfügt ein `DetailsView` Steuerelement, mit denen Sie die neue Studenten hinzufügen. Schließen Sie den Browser, und klicken Sie dann erstellen Sie eine neue Webseite mit den *Site.Master* Masterseite. Nennen Sie die Seite *StudentsAdd.aspx*, und wechseln Sie zur **Quelle** anzeigen.

Fügen Sie das folgende Markup ersetzt das vorhandene Markup für die `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Dieses Markup erstellt einen `EntityDataSource` Steuerelement, das mit dem Umwandlungsoperator im erstellten *Students.aspx*, außer dass es einfügen kann. Wie bei der `GridView` zu steuern, die gebundenen Felder von der `DetailsView` Steuerelement codiert werden, genau so, wie sie für ein Steuerelement, das direkt mit einer Datenbank verbunden werden würde, mit dem Unterschied, dass sie die Eigenschaften der Entität verweisen. In diesem Fall die `DetailsView` Steuerelement dient nur für das Einfügen von Zeilen, damit Sie den Standardmodus, um festgelegt haben `Insert`.

Führen Sie die Seite, und fügen Sie einen neuen Studenten hinzu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Keine Aktion ausgeführt, nachdem Sie einen neuen Studenten einfügen, aber wenn Sie jetzt ausführen *Students.aspx*, sehen Sie die neuen Student-Informationen.

## <a name="displaying-data-in-a-drop-down-list"></a>Anzeigen von Daten in eine Dropdown-Liste

In den folgenden Schritten fügen Sie Databind eine `DropDownList` Steuerelement, um eine Entitätenmenge, die mit einer `EntityDataSource` Steuerelement. In diesem Teil des Lernprogramms nicht Sie viel mit dieser Liste. Im weiteren Verlauf jedoch verwenden die Liste Sie können Benutzer wählen Sie eine Abteilung anzuzeigenden Kurse der Abteilung zugeordnet.

Erstellen Sie eine neue Webseite mit dem Namen *Courses.aspx*. In **Quelle** anzuzeigen, fügen Sie eine Spaltenüberschrift, um die `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

In **Entwurf** anzeigen, Hinzufügen einer `EntityDataSource` Steuerelement auf der Seite "wie zuvor, mit Ausnahme von diesmal nennen Sie sie `DepartmentsEntityDataSource`. Wählen Sie **Abteilungen** als die **EntitySetName** -Wert aus, und wählen Sie nur die **"DepartmentID"** und **Namen** Eigenschaften.

[![image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Aus der **Standard** auf der Registerkarte die **Toolbox**, ziehen Sie eine `DropDownList` auf der Seite zu steuern, benennen Sie sie `DepartmentsDropDownList`, klicken Sie auf das Smarttag, und wählen **Datenquelle auswählen** an Starten Sie die **DataSource-Konfigurations-Assistenten**.

[![image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

In der **wählen Sie eine Datenquelle** Schritt wählen **DepartmentsEntityDataSource** als Datenquelle, klicken Sie auf **Schema aktualisieren**, und wählen Sie dann **Namen** als das Feld "Daten" angezeigt und **"DepartmentID"** als Datenfeld Wert. Klicken Sie auf **OK**.

[![image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Die Methode, die Sie verwenden man das Steuerelement mit dem Entity Framework ist der identisch mit anderen ASP.NET-Daten Datenquellensteuerelemente, außer dass Sie Entitäten und Eigenschaften der Entität angegeben haben.

Wechseln Sie zur **Quelle** anzeigen und hinzufügen "Wählen Sie eine Abteilung:" direkt vor der `DropDownList` Steuerelement.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Zur Erinnerung, ändern Sie das Markup für die `EntityDataSource` Steuerelement zu diesem Zeitpunkt durch Ersetzen der `ConnectionString` und `DefaultContainerName` Attribute mit einer `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Es ist oft sinnvoll, bis gewartet wird, nachdem Sie das datengebundene Steuerelement erstellt haben, das mit den Datenquellen-Steuerelement verknüpft ist, bevor Sie ändern die `EntityDataSource` Markup zu steuern, da nach dem vornehmen der Änderung der Designer nicht Sie mit bereitstellen, wird eine **aktualisieren Schema** -Option in das datengebundene Steuerelement.

Führen Sie die Seite, und Sie können eine Abteilung aus der Dropdown Liste auswählen.

[![image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Dies schließt die Einführung in die Verwendung der `EntityDataSource` Steuerelement. Arbeiten mit diesem Steuerelement wird im Allgemeinen unterscheidet sich von der Arbeit mit anderen ASP.NET-Daten Datenquellen-Steuerelementen, mit dem Unterschied, dass Sie Entitäten und Eigenschaften anstelle von Tabellen und Spalten verweisen. Die einzige Ausnahme ist, wenn Sie Navigationseigenschaften zugreifen möchten. In den nächsten Lernprogrammen sehen Sie, dass Sie die Syntax mit verwenden `EntityDataSource` Steuerelement möglicherweise auch aus anderen Steuerelementen für Datenquellen unterscheiden, wenn Sie filtern, gruppieren und Sortieren von Daten.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-3.md)
