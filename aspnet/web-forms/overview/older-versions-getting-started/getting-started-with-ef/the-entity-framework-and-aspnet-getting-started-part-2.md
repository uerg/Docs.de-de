---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms - Teil 2 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 476f3e45608bf79a6d2665424eba09cbfccd78fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371102"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms - Teil 2
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Das EntityDataSource-Steuerelement

Im vorherigen Tutorial haben Sie eine Website, eine Datenbank und einem Datenmodell erstellt. In diesem Tutorial arbeiten Sie mit der `EntityDataSource` -Steuerelement, das ASP.NET bietet, um die Arbeit mit einem Entity Framework-Datenmodell zu vereinfachen. Erstellen Sie eine `GridView` Steuerelement zum Anzeigen und Bearbeiten von Studentendaten für Schüler und, eine `DetailsView` Steuerelement für das Hinzufügen von neuer Schüler/Students, und ein `DropDownList` Steuerelement zum Auswählen einer Abteilung (die Sie später verwenden werden für die Anzeige von zugeordneten Kurse).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Beachten Sie, dass in dieser Anwendung Sie wird nicht fügen eingabeüberprüfung zu Seiten, die die Datenbank zu aktualisieren, einige der Fehlerbehandlung werden nicht in der so stabil, wie in einer produktionsanwendung benötigt werden. Das hält das Lernprogramm konzentriert sich auf Entity Framework und verhindert, dass er zu lang. Informationen dazu, wie Sie diese Features zu Ihrer Anwendung hinzufügen, finden Sie unter [Validieren der Benutzereingabe in ASP.NET-Webseiten](https://msdn.microsoft.com/library/7kh55542.aspx) und [Fehlerbehandlung in ASP.NET-Seiten und-Anwendungen](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Hinzufügen und konfigurieren das EntityDataSource-Steuerelement

Beginnen Sie mit der Konfiguration einer `EntityDataSource` Steuerelement lesen `Person` Entitäten aus der `People` Entitätenmenge.

Stellen Sie sicher, dass Sie Visual Studio geöffnet haben, und, dass Sie mit dem Projekt arbeiten, ist Sie in Teil 1 erstellt. Wenn Sie seit Sie das Datenmodell erstellt oder seit der letzten Änderung, die Sie an ihr vorgenommenen das Projekt erstellt haben, erstellen Sie jetzt das Projekt. Änderungen am Datenmodell stehen nicht zur Verfügung in dem Designer, bis das Projekt erstellt wird.

Erstellen einer neuen Webseite mit den **Webformular mit Gestaltungsvorlage** Vorlage, und nennen Sie sie *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Geben Sie *Site.Master* als Masterseite. Alle Seiten, die Sie für diese Tutorials erstellen werden diese Masterseite verwenden.

[![image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

In **Quelle** anzeigen, Hinzufügen einer `h2` Spaltenüberschrift, um die `Content` Steuerelement mit dem Namen `Content2`, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Von der **Daten** Registerkarte die **Toolbox**, ziehen Sie ein `EntityDataSource` auf der Seite zu steuern, legen Sie es unter der Überschrift und ändern Sie die ID, die `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Wechseln Sie zur **Entwurf** anzuzeigen, klicken Sie auf das Datenquellensteuerelement Smarttag, und klicken Sie dann auf **Konfigurieren von Datenquellen** zum Starten der **Konfigurieren von Datenquellen** Assistenten.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

In der **ObjectContext konfigurieren** Assistentenschritt, select **SchoolEntities** als Wert für **Verbindung mit dem Namen**, und wählen Sie **SchoolEntities**als die **DefaultContainerName** Wert. Klicken Sie dann auf **Weiter**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Hinweis: Wenn Sie das folgende Dialogfeld an diesem Punkt erhalten, müssen Sie zum Erstellen des Projekts, bevor Sie fortfahren.

[![image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

In der **Datenauswahl konfigurieren** Schritt select **Personen** als Wert für **EntitySetName**. Unter **wählen**, stellen Sie sicher, dass die **wählen Sie ein** alle Kontrollkästchen aktiviert ist. Wählen Sie dann die Optionen zum Aktivieren von aktualisieren und löschen. Wenn Sie fertig sind, klicken Sie auf **Fertig stellen**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurieren von Datenbankregeln für zu löschen

Erstellen Sie eine Seite, die Benutzer löschen von Schülern und Studenten kann die `Person` -Tabelle, die drei Beziehungen mit anderen Tabellen verfügt (`Course`, `StudentGrade`, und `OfficeAssignment`). In der Standardeinstellung die Datenbank ist es nicht das Löschen einer Zeile im `Person` Wenn verknüpfte Zeilen in einem der anderen Tabellen vorhanden sind. Sie können die zugehörigen Zeilen manuell zuerst löschen, oder Sie können die Datenbank so konfigurieren sie automatisch gelöscht, wenn Sie löschen eine `Person` Zeile. Für die Studentendatensätze für Schüler und in diesem Tutorial konfigurieren Sie die Datenbank, um die verwandten Daten automatisch gelöscht. Da der Schüler/Studenten Zeilen verknüpft haben, können nur in der `StudentGrade` Tabelle müssen Sie nur eine der drei Beziehungen konfigurieren.

Bei Verwendung der *School.mdf* Datei, die Sie aus dem Projekt heruntergeladen haben, die in diesem Tutorial geht, können Sie diesen Abschnitt überspringen, da diese Änderungen an der Konfiguration bereits erfolgt sind. Wenn Sie die Datenbank durch Ausführen eines Skripts erstellt haben, können konfigurieren Sie die Datenbank durch Ausführen der folgenden Prozeduren.

In **Server-Explorer**, öffnen Sie das Datenbankdiagramm ein, die Sie in Teil 1 erstellt haben. Mit der rechten Maustaste in der Beziehung zwischen `Person` und `StudentGrade` (die Zeile, die zwischen Tabellen), und wählen Sie dann **Eigenschaften**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

In der **Eigenschaften** Fenster erweitern **INSERT- und UPDATE-Spezifikation** und legen Sie die **DeleteRule** Eigenschaft **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Speichern Sie und schließen Sie das Diagramm. Wenn Sie aufgefordert werden, ob Sie die Datenbank aktualisieren möchten, klicken Sie auf **Ja**.

Um sicherzustellen, dass das Modell Entitäten, die im Arbeitsspeicher, die abgestimmt werden verfolgt, die Datenbank Wozu dient sind, müssen Sie die entsprechende Regeln im Datenmodell festlegen. Open *SchoolModel.edmx*, mit der rechten Maustaste der Zuordnungslinie zwischen `Person` und `StudentGrade`, und wählen Sie dann **Eigenschaften**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

In der **Eigenschaften** legen **End1 OnDelete** zu **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Speichern und schließen Sie die *SchoolModel.edmx* Datei, und klicken Sie dann das Projekt neu erstellen.

Wenn die Datenbank geändert wird, haben Sie in der Regel mehrere Optionen für das Modell zu synchronisieren:

- Für bestimmte Arten von Änderungen (z. B. hinzufügen oder Aktualisieren von Tabellen, Sichten oder gespeicherte Prozeduren), mit der rechten Maustaste im Designer, und wählen Sie **Modell aus der Datenbank aktualisieren** damit der Designer stellen die Änderungen automatisch.
- Generieren Sie das Datenmodell.
- Stellen Sie die manuelle Updates dieser Art.

In diesem Fall Sie konnte das Modell generiert haben, oder aktualisiert die Tabellen, die von der Änderung betroffen sind, aber dann müssten Sie die Feldnamen Änderung erneut vorgenommen werden (von `FirstName` zu `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Verwenden ein GridView-Steuerelement zum Lesen und Aktualisieren von Entitäten

In diesem Abschnitt verwenden Sie eine `GridView` Steuerelement zum Anzeigen, aktualisieren oder Löschen der Schüler/Studenten.

Öffnen oder wechseln Sie zur *Students.aspx* und wechseln Sie zur **Entwurf** anzeigen. Von der **Daten** Registerkarte die **Toolbox**, ziehen Sie eine `GridView` Steuerelement rechts neben der `EntityDataSource` steuern, nennen Sie es `StudentsGridView`, klicken Sie auf das Smarttag, und wählen Sie dann  **StudentsEntityDataSource** als Datenquelle.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Klicken Sie auf **Schema aktualisieren** (klicken Sie auf **Ja** Wenn Sie aufgefordert werden, um zu bestätigen), klicken Sie dann auf **Paging aktivieren**, **Sortieren aktivieren**, **Bearbeitung aktivieren**, und **löschen aktivieren**.

Klicken Sie auf **Spalten bearbeiten**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

In der **ausgewählte Felder** löschen **PersonID**, **"LastName"**, und **HireDate**. Sie nicht in der Regel eine Datensatzschlüssel für Benutzer anzeigen, Einstellungsdatum ist nicht relevant für Schüler/Studenten und fügen Sie beide Teile des Namens in einem Feld, benötigen Sie nur eines der Namensfelder.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Wählen Sie die **"firstmidname"** ein, und klicken Sie dann auf **dieses Feld in ein TemplateField konvertieren**.

Gleiches gilt für **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Klicken Sie auf **OK** und wechseln Sie zur **Quelle** anzeigen. Die verbleibenden Änderungen werden einfacher, direkt im Markup. Die `GridView` Markup sieht wie im folgenden Beispiel zu steuern.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Die erste Spalte nach Befehlsfeld ein Vorlagenfeld, die derzeit ist zeigt den Vornamen an. Ändern Sie das Markup für dieses Vorlagenfeld wie im folgenden Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

Im Anzeigemodus zwei `Label` Steuerelemente zeigen Sie die erste und letzte Bezeichnung. Im Bearbeitungsmodus befindet werden zwei Textfelder bereitgestellt, damit Sie den ersten und letzten Namen ändern können. Wie bei der `Label` Steuerelemente im Anzeigemodus, den Sie verwenden `Bind` und `Eval` Ausdrücke, wie Sie mit ASP.NET Datenquellen-Steuerelemente, die direkt mit Datenbanken verbinden würden. Der einzige besteht Unterschied darin, dass Sie die Eigenschaften der Entität anstatt Datenbankspalten angegeben sind.

Die letzte Spalte ist eine Vorlagenfeld, das die Anmeldedatum zeigt. Ändern Sie das Markup für dieses Feld, wie im folgenden Beispiel aussehen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

In beiden anzuzeigen, und in den Bearbeitungsmodus die Formatzeichenfolge "{0, d}" führt dazu, dass das Datum im Format "kurzes Datum" angezeigt werden. (Ihren Computer möglicherweise konfiguriert werden, um dieses Format anders als die Bilder in diesem Tutorial gezeigt anzuzeigen.)

Beachten Sie, dass in jedem dieser Felder für die Vorlage, die der Designer verwendet eine `Bind` Ausdruck, durch die standardmäßige, aber Sie geändert haben, um eine `Eval` Ausdruck in der `ItemTemplate` Elemente. Die `Bind` Ausdruck werden die Daten in verfügbar `GridView` Steuerelementeigenschaften, falls Sie auf die Daten im Code zugreifen müssen. Auf dieser Seite, die Sie auf diese Daten im Code, damit Sie verwenden können, müssen keine `Eval`, dies ist effizienter. Weitere Informationen finden Sie unter [Abrufen Ihrer Daten aus den Datensteuerelementen](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Überarbeiten EntityDataSource-Steuerelement-Markup zur Verbesserung der Leistung

Im Markup für die `EntityDataSource` steuern, entfernen Sie die `ConnectionString` und `DefaultContainerName` Attribute aus, und Ersetzen Sie sie durch eine `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Dies ist eine Änderung, Sie stellen jedes Mal, wenn Sie erstellen, eine `EntityDataSource` zu steuern, es sei denn, Sie müssen eine Verbindung zu verwenden, die sich von dem unterscheidet, die in der Objektkontextklasse hartcodiert ist. Mithilfe der `ContextTypeName` Attribut bietet die folgenden Vorteile:

- Bessere Leistung. Wenn die `EntityDataSource` Steuerelement initialisiert, das Modell mithilfe der `ConnectionString` und `DefaultContainerName` Attribute, es werden zusätzliche Aufgaben zum Laden von Metadaten für jede Anforderung ausgeführt. Dies ist nicht erforderlich, wenn Sie angeben, die `ContextTypeName` Attribut.
- Lazy Loading ist standardmäßig aktiviert, in der generierten Objektkontextklassen (z. B. `SchoolEntities` in diesem Tutorial) im Entity Framework 4.0. Dies bedeutet, dass Navigationseigenschaften mit verbundenen Daten automatisch geladen werden bei Bedarf. Lazy Loading wird später in diesem Lernprogramm ausführlicher erläutert.
- Alle Anpassungen, die Sie auf die Objektkontextklasse angewendet haben (in diesem Fall die `SchoolEntities` Klasse) stehen für Steuerelemente, mit denen die `EntityDataSource` Steuerelement. Anpassen der Objektkontextklasse ist ein Thema für fortgeschrittene, die in diesem Tutorial nicht behandelt wird. Weitere Informationen finden Sie unter [Erweitern von Entity Framework generierten Typen](https://msdn.microsoft.com/library/dd456844.aspx).

Das Markup wird nun im folgende Beispiel aussehen (die Reihenfolge der Eigenschaften möglicherweise unterschiedliche):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Die `EnableFlattening` -Attribut verweist auf eine Funktion, die in früheren Versionen von Entity Framework benötigt wurde, da die foreign Key-Spalten nicht als Entitätseigenschaften verfügbar gemacht wurden. Die aktuelle Version ist es möglich, *fremdschlüsselzuordnungen*, was bedeutet, dass Fremdschlüsseleigenschaften werden verfügbar gemacht werden, für alle außer m: n-Zuordnungen. Wenn Ihre Entitäten Fremdschlüsseleigenschaften und keine [komplexe Typen](https://msdn.microsoft.com/library/bb738472.aspx), lassen Sie dieses Attribut auf `False`. Das Attribut nicht aus dem Markup, entfernt werden, da der Standardwert ist `True`. Weitere Informationen finden Sie unter [vereinfachen Objekte (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Führen Sie die Seite, und Sie sehen eine Liste der Studierenden und Mitarbeiter (Sie werden für nur Schüler und Studenten im nächsten Tutorial Filtern). Der Vorname und Nachname werden zusammen angezeigt.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Um die Anzeige zu sortieren, klicken Sie auf einen Spaltennamen an.

Klicken Sie auf **bearbeiten** in einer Zeile. Textfelder werden angezeigt, in dem Sie den ersten und letzten Namen ändern können.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Die **löschen** Schaltfläche auch funktioniert. Klicken Sie auf "löschen", um eine Zeile mit einer Registrierungsdatum und die Zeile nicht mehr angezeigt wird. (Zeilen ohne Registrierungsdatum darstellen, Dozenten und erhalten Sie möglicherweise einen Fehler der referenziellen Integrität. Im nächsten Tutorial müssen Sie diese Liste, um nur Schüler und Studenten einzubeziehen filtern.)

## <a name="displaying-data-from-a-navigation-property"></a>Anzeigen von Daten von einer Navigationseigenschaft

Jetzt wird angenommen, Sie möchten wissen, wie viele Kurse jeder Schüler/Student bei registriert. Das Entity Framework enthält diese Informationen in den `StudentGrades` Navigationseigenschaft der `Person` Entität. Da ein Schüler/Student einen Kurs registriert werden, ohne dass eine Grade-Eigenschaft zugewiesen von Entwurf der Datenbank nicht möglich ist, für dieses Tutorial können Sie davon ausgehen, dass durch eine Zeile der `StudentGrade` Tabellenzeile, die mit einem Kurs zugewiesen ist entspricht, das in diesem Kurs registriert. (Die `Courses` Navigationseigenschaft ist nur für Dozenten.)

Bei Verwendung der `ContextTypeName` Attribut der `EntityDataSource` -Steuerelement, das Entity Framework automatisch Ruft Informationen für eine Navigationseigenschaft beim Zugriff auf diese Eigenschaft. Dies wird als bezeichnet *Lazy Load*. Dies kann jedoch ineffizient sein, da es einen separaten Aufruf von der Datenbank führt, die jedes Mal zusätzliche Informationen benötigt werden. Wenn Sie Daten aus der Navigationseigenschaft für jede zurückgegebene Entität benötigen die `EntityDataSource` -Steuerelement, ist es effizienter sein, um die verknüpften Daten zusammen mit der Entität selbst in einem einzigen Aufruf in der Datenbank abzurufen. Dies wird als bezeichnet *Eager Load*, und geben Sie eager Loading für eine Navigationseigenschaft, durch Festlegen der `Include` Eigenschaft der `EntityDataSource` Steuerelement.

In *Students.aspx*, die Anzahl der Kurse für alle Schüler/Studenten angezeigt werden sollen, also eager Loading ist die beste Wahl. Wenn Sie alle Schüler/Studenten anzeigen, aber mit der Anzahl der Kurse wurden kann nur für einige davon (die Schreiben von Code neben dem Markup erfordern würden), Lazy Load eine bessere Wahl sein.

Öffnen, oder wechseln Sie zur *Students.aspx*, wechseln Sie zur **Entwurf** Ansicht `StudentsEntityDataSource`, und klicken Sie in der **Eigenschaften** legen die **Include**Eigenschaft **StudentGrades**. (Wenn Sie mehrere Navigationseigenschaften abrufen möchten, geben Sie Namen durch Kommas getrennt, z. B. **StudentGrades, Kursen**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Wechseln Sie zur **Quelle** anzeigen. In der `StudentsGridView` -Steuerelement, nach dem letzten `asp:TemplateField` -Element, das folgende neue Vorlagenfeld hinzufügen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

In der `Eval` Ausdruck können Sie die Navigationseigenschaft verweisen `StudentGrades`. Da diese Eigenschaft eine Auflistung enthält, hat eine `Count` -Eigenschaft, die Sie verwenden können, um die Anzahl der Kurse anzuzeigen, in dem der Student, der registriert wird. In einem späteren Tutorial sehen Sie, wie Sie zum Anzeigen von Daten von Navigationseigenschaften, die einzelne Entitäten anstelle von Sammlungen enthalten. (Beachten Sie, dass Sie können nicht `BoundField` Elemente zum Anzeigen von Daten von Navigationseigenschaften.)

Führen Sie die Seite, und Sie jetzt sehen, wie viele Kurse für Schüler und Studenten registriert ist.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Verwenden von einem DetailsView-Steuerelement zum Einfügen von Entitäten

Der nächste Schritt ist die Erstellung eine Seite, die eine `DetailsView` Steuerelement, mit denen Sie die neue Studenten hinzufügen. Schließen Sie den Browser, und klicken Sie dann erstellen Sie eine neue Webseite mit den *Site.Master* Masterseite. Nennen Sie die Seite *StudentsAdd.aspx*, und wechseln Sie zur **Quelle** anzeigen.

Fügen Sie das folgende Markup zum Ersetzen Sie des vorhandenen Markups für die `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Dieses Markup erstellt eine `EntityDataSource` Steuerelement, das ähnlich dem im erstellten *Students.aspx*, es sei denn sie einfügen kann. Wie bei der `GridView` zu steuern, die gebundene Felder der `DetailsView` Steuerelement codiert werden, genau wie bei der für ein Steuerelement, das direkt mit einer Datenbank, verbunden mit dem Unterschied, dass sie die Eigenschaften der Entität verweisen. In diesem Fall die `DetailsView` Steuerelement wird verwendet, nur für das Einfügen von Zeilen, aus, damit Sie den Standardmodus, um eingerichtet haben `Insert`.

Führen Sie die Seite, und fügen Sie einen neuen Studenten hinzu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

"Nothing" erfolgt, nachdem Sie einen neuen Studenten eingefügt, aber wenn Sie jetzt ausführen *Students.aspx*, sehen Sie die Informationen des neuen für Schüler und Studenten.

## <a name="displaying-data-in-a-drop-down-list"></a>Anzeigen von Daten in einem Dropdown-Liste

In den folgenden Schritten müssen Sie die Databind eine `DropDownList` Steuerelement eine Entitätenmenge, die mit einem `EntityDataSource` Steuerelement. In diesem Teil des Tutorials nicht Sie viel mit dieser Liste. Im weiteren Verlauf werden Sie die Liste jedoch verwenden, können Benutzer die Auswahl einer Abteilung zum Anzeigen der Kurse, die die Abteilung zugeordnet.

Erstellen einer neuen Webseite mit dem Namen *Courses.aspx*. In **Quelle** anzuzeigen, fügen Sie eine Spaltenüberschrift, um die `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

In **Entwurf** anzeigen, Hinzufügen einer `EntityDataSource` Steuerelement auf der Seite wie zuvor, außer dass diesmal fußballfanartikel `DepartmentsEntityDataSource`. Wählen Sie **Abteilungen** als die **EntitySetName** Wert ein, und wählen Sie nur die **"DepartmentID"** und **Namen** Eigenschaften.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Aus der **Standard** Registerkarte die **Toolbox**, ziehen Sie eine `DropDownList` die Steuerung an die Seite, nennen Sie es `DepartmentsDropDownList`, klicken Sie auf das Smarttag, und wählen **Datenquelle auswählen** auf Starten Sie den **DataSource-Konfigurations-Assistenten**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

In der **Auswählen einer Datenquelle** Schritt select **DepartmentsEntityDataSource** als Datenquelle, klicken Sie auf **Schema aktualisieren**, und wählen Sie dann **Namen** wie das Datenfeld angezeigt und **"DepartmentID"** als Datenfeld Wert. Klicken Sie auf **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Die Methode man das Steuerelement mit dem Entity Framework entspricht derjenigen mit anderen ASP.NET-Daten Datenquellen-Steuerelemente, außer dass Sie Entitäten und Eigenschaften der Entität angeben.

Wechseln Sie zur **Quelle** anzeigen und hinzufügen "Wählen Sie einen Fachbereich:" direkt vor der `DropDownList` Steuerelement.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Zur Erinnerung: Ändern Sie das Markup für die `EntityDataSource` Steuerelement an diesem Punkt durch Ersetzen der `ConnectionString` und `DefaultContainerName` Attribute mit einem `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Es ist oft sinnvoll, zu warten, bis Sie die vom datengebundenen Steuerelement erstellt haben, die mit den Datenquellen-Steuerelement verknüpft ist, bevor Sie ändern die `EntityDataSource` Markup zu steuern, da nach dem vornehmen der Änderung der Designer keine Ihnen wird ein **aktualisieren Schema** Option im datengebundenen Steuerelement.

Führen Sie die Seite, und Sie können eine Abteilung auswählen, aus der Dropdown-Liste.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Dies schließt die Einführung in die Verwendung der `EntityDataSource` Steuerelement. Arbeiten mit diesem Steuerelement unterscheidet sich in der Regel nicht von der Arbeit mit anderen ASP.NET-Daten-Datenquellen-Steuerelemente, mit dem Unterschied, dass Sie Entitäten und Eigenschaften anstelle von Tabellen und Spalten verweisen. Die einzige Ausnahme ist, wenn Sie Navigationseigenschaften zugreifen möchten. Im nächsten Tutorial sehen Sie, dass Sie die Syntax mit verwenden `EntityDataSource` Steuerelement möglicherweise auch unterscheiden sich von anderen Datenquellen-Steuerelemente, wenn Sie filtern, gruppieren und Sortieren von Daten.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-3.md)
