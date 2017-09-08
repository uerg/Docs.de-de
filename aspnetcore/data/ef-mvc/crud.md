---
title: ASP.NET Core MVC mit EF-Kern - CRUD - 2-10
author: tdykstra
description: 
keywords: "ASP.NET Core, Entity Framework Core, CRUD, erstellen, lesen, aktualisieren und löschen"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 855f060a6404dedff310b288ada9738689069ceb
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Erstellen Sie, lesen Sie, aktualisieren Sie und löschen Sie-EF-Core mit ASP.NET Core MVC-Lernprogramm (2 von 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

Im vorherigen Lernprogramm erstellt Sie eine MVC-Anwendung, die gespeichert und Daten mit dem Entity Framework und SQL Server LocalDB angezeigt. In diesem Lernprogramm müssen Sie überprüfen und anpassen, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Code, der die MVC-Gerüstbau automatisch für Sie in den Controller und Ansichten erstellt.

> [!NOTE] 
> Es ist allgemein üblich, des Repositorymusters zu implementieren, um eine Abstraktionsebene zwischen Ihrem Domänencontroller und die Datenzugriffsebene zu erstellen. Um diese Lernprogramme einfach und konzentriert sich auf durcharbeiten zum Verwenden von Entity Framework selbst zu halten, verwenden sie nicht die Repositorys. Informationen des Repositorys mit EF finden Sie unter [im letzten Lernprogramm dieser Reihe](advanced.md).

In diesem Lernprogramm arbeiten Sie mit den folgenden Webseiten:

![Seite "Details" Student](crud/_static/student-details.png)

![Seite "Student erstellen"](crud/_static/student-create.png)

![Student bearbeiten (Seite)](crud/_static/student-edit.png)

![Seite "Student löschen"](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Anpassen der Seite "Details"

Der scaffolded Code für die Studenten Indexseite außer acht gelassen der `Enrollments` -Eigenschaft, da diese Eigenschaft eine Auflistung enthält. In der **Details** Seite zeigen Sie den Inhalt der Auflistung in einer HTML-Tabelle.

In *Controllers/StudentsController.cs*, die Aktionsmethode Details anzeigen, verwendet der `SingleOrDefaultAsync` Methode zum Abrufen einer einzelnes `Student` Entität. Fügen Sie Code, der aufruft `Include`. `ThenInclude`, und `AsNoTracking` Methoden, wie im folgenden hervorgehobenen Code gezeigt.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

Die `Include` und `ThenInclude` Methoden dazu führen, dass den Kontext beim Laden der `Student.Enrollments` Navigationseigenschaft, und innerhalb jeder Registrierung der `Enrollment.Course` Navigationseigenschaft.  Erfahren Sie mehr über diese Methoden in der [lesen verknüpfte Daten](read-related-data.md) Lernprogramm.

Die `AsNoTracking` Methode verbessert die Leistung in Szenarien, in dem die zurückgegebenen Entitäten nicht in den aktuellen Kontext Lebensdauer aktualisiert werden werden. Erfahren Sie mehr über `AsNoTracking` am Ende dieses Lernprogramms.

### <a name="route-data"></a>Weiterleitung von Daten

Der Schlüssel-Wert, der an die `Details` Methode stammen aus *Weiterleiten von Daten*. Routendaten sind Daten, die der Modellbinder in einem Segment der URL gefunden. Die Standardroute gibt z. B. Controller, Aktion und Id Segmente:

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

In der folgenden URL wird die Standardroute Dozenten wie den Controller, der Index als Aktion, und mit der Id 1 zugeordnet; Hierbei handelt es sich um Datenwerte für die Route.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

Der letzte Teil der URL ("? CourseID 2021 =") ist ein Abfrage-Zeichenfolgenwert. Übergibt der Modellbinder auch den ID-Wert, der `Details` Methode `id` Parameter an, wenn Sie es als ein Wert der Abfragezeichenfolge übergeben:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Hyperlink-URLs werden in die Indexseite von Tag Helper-Anweisungen in der Razor-Ansicht erstellt. Im folgenden Razor-Code der `id` Parameter entspricht der Standardroute daher `id` die Routendaten hinzugefügt wird.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Dadurch wird der folgenden HTML-Code generiert beim `item.ID` 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

Im folgenden Razor-Code `studentID` einen Parameter in die Standardroute stimmt nicht überein, damit sie als eine Abfragezeichenfolge hinzugefügt wird.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Dadurch wird der folgenden HTML-Code generiert beim `item.ID` 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Weitere Informationen zu den Tag-Hilfsprogrammen, finden Sie unter [Tag Hilfsprogramme in ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Die Detailansicht Registrierung hinzugefügt

Open *Views/Students/Details.cshtml*. Jedes Feld wird angezeigt, mit `DisplayNameFor` und `DisplayFor` -Hilfen, wie im folgenden Beispiel gezeigt:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Nachdem das letzte Feld und unmittelbar vor der schließenden `</dl>` kennzeichnen, fügen Sie folgenden Code aus, um eine Liste der Bereitstellungen anzuzeigen:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Wenn Code Einzug falsch ist, nachdem Sie den Code einfügen, drücken Sie STRG + K + D, um sie zu korrigieren.

Dieser Code durchläuft die Entitäten in der `Enrollments` Navigationseigenschaft. Für jede Anmeldung zeigt es den Kurstitel und die Dienstqualität dargestellt. Der Titel des Kurses wird abgerufen, aus der Kurs-Entität, die in gespeichert ist die `Course` Navigationseigenschaft der Registrierung Entität.

Die Anwendung auszuführen, wählen Sie die **Studenten** Registerkarte, und klicken Sie auf die **Details** Link, um ein Student. Sie können die Liste der Kurse und den Qualitäten für den ausgewählten Schüler anzuzeigen:

![Seite "Details" Student](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Aktualisieren Sie die Seite "erstellen"

In *StudentsController.cs*, ändern Sie die HttpPost `Create` Methode durch eine Try-Catch-Block hinzufügen und Entfernen von-ID aus der `Bind` Attribut.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Dieser Code fügt die Student-Entität erstellt, indem der ASP.NET MVC-Modellbinder für die Entität Studenten festgelegt, und klicken Sie dann die Änderungen in der Datenbank gespeichert. (Modellbinder bezieht sich auf die ASP.NET MVC-Funktionalität, die Sie zum Arbeiten mit Daten, die von einem Formular übermittelt erleichtert, ein Modellbinder übermittelte Formularwerte in CLR-Typen konvertiert, und übergibt sie an die Aktionsmethode im Parameters. In diesem Fall instanziiert der Modellbinder eine Student-Entität für Sie Eigenschaftswerte aus der Auflistung Formular verwenden.)

Sie entfernt `ID` aus der `Bind` Attribut, da-ID des Primärschlüsselwerts ist die SQL Server automatisch festgelegt wird, wenn die Zeile eingefügt wird. Eingaben des Benutzers ist nicht mit den ID-Wert festgelegt.

Anders als die `Bind` Try-Catch-Block befindet, die einzige Änderung, die Sie an den scaffolded Code vorgenommen haben. Wenn eine Ausnahme, die abgeleitet `DbUpdateException` wird erfasst, während die Änderungen gespeichert werden, wird eine allgemeine Fehlermeldung angezeigt. `DbUpdateException`Ausnahmen werden manchmal etwas anderes außerhalb der Anwendung statt auf einen Programmierfehler verursacht, damit der Benutzer informiert wird, versuchen Sie es erneut. Obwohl in diesem Beispiel nicht implementiert wird, würde eine Qualität produktionsanwendung protokolliert die Ausnahme. Weitere Informationen finden Sie unter der **Protokoll Einblicke** im Abschnitt [Überwachung und Telemetrie (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

Die `ValidateAntiForgeryToken` Attribut hilft dabei, siteübergreifende Anforderung Websiteübergreifender Anforderungsfälschungsangriffe zu verhindern. Das Token wird automatisch eingefügt, in der Sicht durch die [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) und enthalten ist, wenn das Formular vom Benutzer gesendet wird. Das Token wird überprüft, indem die `ValidateAntiForgeryToken` Attribut. Weitere Informationen zu CSRF, finden Sie unter [Anti-Anforderungsfälschung](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Sicherheitshinweis zum overposting

Die `Bind` -Attribut, das auf den scaffolded Code enthält die `Create` Methode ist eine Möglichkeit zum Schutz vor Overposting in Szenarien zu erstellen. Nehmen wir beispielsweise an, die Student-Entität enthält eine `Secret` -Eigenschaft, die Sie nicht, dass dieser Webseite festlegen möchten.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

Auch wenn Sie besitzen eine `Secret` Feld auf der Webseite, die ein Hacker konnte ein Tool wie Fiddler verwenden, oder Schreiben von JavaScript, zum Bereitstellen einer `Secret` Wert bilden. Ohne die `Bind` Attribut beschränken die Felder, die der Modellbinder verwendet, bei der Erstellung einer Student-Instanz den Modellbinder würde, die abholen `Secret` Wert bilden, und verwenden, um die Entitätsinstanz Student erstellen. Und dann nach Belieben Hackers für angegebene Wert die `Secret` Formularfeld wird in der Datenbank aktualisiert werden. Die folgende Abbildung zeigt das Hinzufügen von Fiddler-Tool die `Secret` Feld (mit dem Wert "OverPost"), um die übermittelte Formularwerte.

![Hinzufügen eines geheimen Felds Fiddler](crud/_static/fiddler.png)

Der Wert "OverPost" würde dann erfolgreich hinzugefügt der `Secret` Eigenschaft des eingefügten Zeile, obwohl Sie nie beabsichtigt waren, dass auf der Webseite auf diese Eigenschaft festgelegt werden.

Sie können Overposting in bearbeiten Szenarien verhindern, indem Sie zuerst die Entität aus der Datenbank liest und dem anschließenden Aufrufen `TryUpdateModel`, und übergeben Sie eine explizite zulässigen Eigenschaften-Liste. Dies ist die Methode, die in diesen Lernprogrammen verwendet.

Eine alternative Möglichkeit, Overposting zu vermeiden, die von vielen Entwicklern bevorzugt wird, ist Ansichtsmodelle anstelle von Entitätsklassen mit wurden die modellbindung verwenden. Schließen Sie nur die Eigenschaften, die Sie in das Ansichtsmodell aktualisieren möchten. Sobald die MVC-modellbindung abgeschlossen ist, kopieren Sie die Modelleigenschaften Ansicht auf die Entitätsinstanz, optional mit einem Tool wie AutoMapper. Verwendung `_context.Entry` auf die Entitätsinstanz, legen Sie seinen Status auf `Unchanged`, und legen Sie dann `Property("PropertyName").IsModified` auf "true" für jede Eigenschaft der Entität, die in das Ansichtsmodell enthalten ist. Diese Methode funktioniert in beiden bearbeiten und Erstellen von Szenarien.

### <a name="test-the-create-page"></a>Testen Sie die Seite "erstellen"

Der Code in *Views/Students/Create.cshtml* verwendet `label`, `input`, und `span` (für validierungsmeldungen) Hilfen für jedes Feld zu kennzeichnen.

Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf **neu erstellen**.

Geben Sie Namen und ein Datum. Versuchen Sie, ein ungültiges Datum eingeben, wenn Sie Ihren Browser, die Optionen bereit. (Von einigen Browsern erzwingen eine Datumsauswahl verwenden.) Klicken Sie dann auf **erstellen** um die Fehlermeldung anzuzeigen.

![Datum-Validierungsfehler](crud/_static/date-error.png)

Dies ist eine serverseitige Validierung, die Sie in der Standardeinstellung erhalten. in einem späteren Lernprogramm sehen Sie, wie Attribute hinzugefügt, die Code für die clientseitige Validierung ebenfalls generiert. Die folgende hervorgehobene Code zeigt die Modell-Überprüfung in der `Create` Methode.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Ändern Sie das Datum in einen gültigen Wert ein, und klicken Sie auf **erstellen** , finden in der neuen Studenten in angezeigt werden die **Index** Seite.

## <a name="update-the-edit-page"></a>Aktualisieren Sie die Seite "Bearbeiten"

In *StudentController.cs*, die HttpGet `Edit` -Methode (der die `HttpPost` Attribut) verwendet die `SingleOrDefaultAsync` Methode, um die ausgewählte Student-Entität abzurufen, wie Sie gesehen, in haben der `Details` Methode. Sie müssen diese Methode ändern.

### <a name="recommended-httppost-edit-code-read-and-update"></a>HttpPost Bearbeiten von Code empfohlen: Lesen und aktualisieren

Ersetzen Sie die Aktionsmethode HttpPost bearbeiten, durch den folgenden Code.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Eine bewährte Sicherheitsmethode um zu verhindern, dass Overposting Implementierung dieser Änderungen. Die Scaffolder generiert eine `Bind` Attribut, und die Entität erstellt, indem die Modellbinder, die Entitätenmenge mit hinzugefügt eine `Modified` Flag. Code für viele Szenarien da nicht empfohlen wird, die `Bind` Attribut löscht vorhandenen Daten in Feldern, die nicht aufgeführt, der `Include` Parameter.

Der neue Code liest, die vorhandene Entität und ruft `TryUpdateModel` Felder in die abgerufene Entität aktualisieren [basierend auf Benutzereingaben in das gesendete Formulardaten](xref:mvc/models/model-binding#how-model-binding-works). Die automatische änderungsnachverfolgung der Entity Framework legt die `Modified` Flag für die Felder, die durch die Formulareingabe geändert werden. Wenn die `SaveChanges` -Methode aufgerufen wird, Entity Framework erstellt SQL-Anweisungen, um die Datenbankzeile zu aktualisieren. Parallelitätskonflikte werden ignoriert, und nur die Tabellenspalten, die vom Benutzer aktualisiert wurden, werden in der Datenbank aktualisiert. (Einem späteren Lernprogramm zeigt, wie Parallelitätskonflikte behandelt.)

Als bewährte Methode, um zu verhindern, overposting die Felder, die von aktualisierbar sein sollen die **bearbeiten** Seite sind in der Zulassungsliste enthalten in der `TryUpdateModel` Parameter. (Die leere Zeichenfolge, die vor der Liste der Felder in der Parameterliste ist für ein Präfix für die Verwendung mit den Namen der Form Felder.) Derzeit sind keine zusätzlichen Felder, die Sie schützen möchten, jedoch mit den Feldern, die den Modellbinder binden soll sichergestellt werden, wenn Sie Felder in der Zukunft in das Datenmodell hinzufügen, diese sich automatisch geschützt sind, bis Sie explizit hier hinzufügen.

Als Ergebnis dieser Änderungen, die Methodensignatur des HttpPost `Edit` Methode ist identisch mit der HttpGet `Edit` Methode; daher haben Sie die Methode umbenannt `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternativer HttpPost bearbeiten-Code: Erstellen und Anfügen

Der empfohlene HttpPost bearbeiten Code stellt sicher, dass nur die geänderte Spalten aktualisiert, und behält die Daten in den Eigenschaften, die Sie nicht möchten, die aus Gründen der modellbindung. Die Read-First-Ansatz erfordert jedoch eine zusätzliche Datenbank lesen, und kann dazu führen, mehr komplexen Code für die Behandlung von Parallelitätskonflikten. Eine Alternative besteht Anfügen einer Entität, die von den Modellbinder für den Kontext EF erstellt und als geändert markiert. (Aktualisieren Sie das Projekt nicht mit dem folgenden Code ist es nur um eine optionale Vorgehensweise veranschaulichen angezeigt.) 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Sie können diesen Ansatz verwenden, wenn das Webseiten-Benutzeroberfläche alle Felder in der Entität enthält und können ggf. aktualisieren.

Der scaffolded Code die Methode zum Erstellen und Anfügen verwendet jedoch nur fängt `DbUpdateConcurrencyException` Ausnahmen und gibt 404-Fehlercodes.  Gezeigten Beispiel fängt alle Datenbank-Update-Ausnahme ab und zeigt eine Fehlermeldung an.

### <a name="entity-states"></a>Status der Entität

Der Datenbankkontext der nachverfolgt, ob Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind, und diese Informationen bestimmt, was geschieht, wenn Sie rufen die `SaveChanges` Methode. Wenn Sie z. B. eine neue Entität zum Übergeben der `Add` Methode, die Zustand der Entität, um festgelegt ist `Added`. Klicken Sie dann beim Aufrufen der `SaveChanges` -Methode, der Datenbankkontext stellt einen SQL INSERT-Befehl.

Eine Entität kann in einem der folgenden Zustände werden:

* `Added`. Die Entität ist noch nicht in der Datenbank vorhanden. Die `SaveChanges` -Methode gibt eine INSERT-Anweisung.

* `Unchanged`. Keine Aktionen erforderlich, mit diese Entität durch erfolgen die `SaveChanges` Methode. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.

* `Modified`. Einige oder alle Eigenschaftswerte für die Entität wurden geändert. Die `SaveChanges` -Methode gibt eine UPDATE-Anweisung.

* `Deleted`. Die Entität wurde zum Löschen markiert wurde. Die `SaveChanges` -Methode gibt eine DELETE-Anweisung.

* `Detached`. Die Entität ist nicht vom Kontext Datenbank nachverfolgt.

Zustandsänderungen werden in einer Desktopanwendung in der Regel automatisch festgelegt. Lesen eine Entität, und nehmen Sie Änderungen an einige Eigenschaftswerte. Dies bewirkt, dass automatisch geändert werden, um zugehörige Entitätsstatus `Modified`. Klicken Sie dann beim Aufrufen `SaveChanges`, Entity Framework generiert eine SQL UPDATE-Anweisung, die nur die tatsächlichen Eigenschaften aktualisiert, die Sie geändert haben.

In einer Web-app die `DbContext` , die anfänglich liest eine Entität und zeigt seine Daten bearbeitet werden freigegeben, nachdem eine Seite gerendert wird. Wenn die HttpPost `Edit` Aktionsmethode aufgerufen wird, erfolgt eine neue webanforderung aus, und Sie haben eine neue Instanz der dem `DbContext`. Wenn Sie die Entität in diesem neuen Kontext erneut lesen, simulieren Sie desktop-Verarbeitung.

Aber wenn Sie möchten der zusätzlichen Lesevorgang, müssen Sie das Entitätsobjekt erstellt, indem die modellbindung verwenden.  Die einfachste Möglichkeit hierzu ist für den Entitätsstatus "geändert" festzulegen, wie in den zuvor aufgeführten alternative HttpPost bearbeiten Code verwendet werden. Klicken Sie dann beim Aufrufen `SaveChanges`, Entity Framework alle Spalten der Datenbankzeile aktualisiert, da der Kontext kann nicht wissen, welche Eigenschaften Sie geändert haben.

Wenn Sie die Read-First-Ansatz vermeiden möchten, aber außerdem SQL UPDATE-Anweisung, um nur die Felder zu aktualisieren, die der Benutzer tatsächlich geändert werden soll, ist der Code komplexer. Müssen Sie die ursprünglichen Werte in irgendeiner Form speichern (z. B. ausgeblendete Felder mit), damit sie verfügbar wenn sind HttpPost `Edit` -Methode aufgerufen wird. Anschließend können Sie eine Student-Entität mit den ursprünglichen Werten Aufruf erstellen die `Attach` Methode mit diesem Originalversion der Entität, Werten der Entität mit den neuen Werten aktualisiert, und rufen dann `SaveChanges`.

### <a name="test-the-edit-page"></a>Testen Sie die Seite "Bearbeiten"

Führen Sie die Anwendung, und wählen Sie die **Studenten** tab, dann klicken Sie auf eine **bearbeiten** Link.

![Studenten bearbeiten (Seite)](crud/_static/student-edit.png)

Ändern Sie einige der Daten und klicken Sie auf **speichern**. Die **Index** Seite geöffnet, und die geänderten Daten sehen.

## <a name="update-the-delete-page"></a>Aktualisieren Sie die Seite "löschen"

In *StudentController.cs*, der Vorlagencode für die HttpGet `Delete` -Methode verwendet die `SingleOrDefaultAsync` Methode, um die ausgewählte Student-Entität abzurufen, wie Sie in den Details und bearbeiten Methoden gesehen haben. Allerdings zum Implementieren eine benutzerdefinierte Fehlermeldung angezeigt, wenn der Aufruf von `SaveChanges` ein Fehler auftritt, fügen Sie einige Funktionen dieser Methode und seine entsprechende Ansicht.

Wie Sie gesehen, für das Update haben und Vorgänge zu erstellen, erfordern Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Antwort auf eine GET-Anforderung aufgerufen wird, zeigt eine Sicht, die dem Benutzer hat die Möglichkeit, zu genehmigen, oder brechen Sie den Löschvorgang. Wenn der Benutzer genehmigt wird, wird eine POST-Anforderung erstellt. Wenn dies geschieht, HttpPost `Delete` Methode wird aufgerufen, und diese Methode führt dann tatsächlich den Löschvorgang.

Fügen Sie einen Try / Catch-Block, HttpPost `Delete` Methode zum Behandeln von Fehlern, die auftreten können, wenn die Datenbank aktualisiert wird. Wenn ein Fehler auftritt, ruft die Löschmethode HttpPost HttpGet Delete-Methode, und übergeben sie einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die HttpGet Delete-Methode zeigt die Seite "Bestätigung" zusammen mit der Fehlermeldung, dass der Benutzer eine Möglichkeit zum Abbrechen oder Wiederholen Sie den Vorgang dann erneut an.

Ersetzen Sie die HttpGet `Delete` -Aktionsmethode durch folgenden Code wird die Fehlerberichterstattung verwaltet.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Dieser Code akzeptiert einen optionalen Parameter, der angibt, ob die Methode, nach einem Fehler aufgerufen wurde, um Änderungen zu speichern. Dieser Parameter ist "false", wenn die HttpGet `Delete` Methode wird aufgerufen, ohne Sie zu einem vorherigen Fehler. Bei Aufruf durch die HttpPost `Delete` Methode als Reaktion auf eine Datenbank-Aktualisierungsfehler, der Parameter ist "true", und eine Fehlermeldung an die Ansicht übergeben wird.

### <a name="the-read-first-approach-to-httppost-delete"></a>Die Read-First-Ansatz zur HttpPost löschen

Ersetzen Sie die HttpPost `Delete` Aktionsmethode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code dem führt des eigentlichen Löschvorgangs und fängt alle Datenbank-Update-Fehler ab.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Dieser Code Ruft die ausgewählte Entität ruft dann die `Remove` Methode, um die Entität Status festgelegt wird, um `Deleted`. Wenn `SaveChanges` aufgerufen wird, wird eine SQL-DELETE-Befehl generiert wird.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Der Ansatz erstellen und anfügen, HttpPost löschen

Ist das Verbessern der Leistung einer Anwendung hoher Priorität, vermeiden Sie unnötige SQL-Abfrage durch Instanziieren einer Student-Entität, die mit nur der primäre Schlüssel, Wert festlegen und dann der Entitätszustand auf `Deleted`. Das ist alles, die das Entity Framework benötigt wird, um die Entität zu löschen. (Fügen Sie diesen Code in Ihrem Projekt nicht; er wird hier nur um eine Alternative zu veranschaulichen.)

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Wenn die Entität zugehörige Daten hat, die ebenfalls gelöscht werden soll, stellen Sie sicher, dass diese kaskadierte Löschung in der Datenbank konfiguriert ist. Bei diesem Ansatz zum Löschen der Entität EF bemerken möglicherweise nicht, dass verknüpfte Entitäten zu löschenden vorhanden sind.

### <a name="update-the-delete-view"></a>Aktualisieren der Ansicht löschen

In *Views/Student/Delete.cshtml*, eine Fehlermeldung zwischen h2 Überschrift und der h3-Überschrift, hinzufügen, wie im folgenden Beispiel gezeigt:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Führen Sie die Seite durch Auswahl der **Studenten** Registerkarte und dann auf eine **löschen** Link:

![Seite "Bestätigung" löschen](crud/_static/student-delete.png)

Klicken Sie auf **löschen**. Die Indexseite wird ohne die gelöschten Studenten angezeigt. (Ein Beispiel für den Fehlerbehandlungscode in Aktion im Lernprogramm Parallelität angezeigt.)

## <a name="closing-database-connections"></a>Schließen von Verbindungen mit der Datenbank

Um die Ressourcen freizugeben, die eine Verbindung mit Datenbank enthält, muss die Kontextinstanz so bald wie möglich freigegeben werden, wenn Sie damit fertig sind. Die integrierte ASP.NET Core [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) übernimmt diese Aufgabe für Sie.

In *Startup.cs*, rufen Sie die [AddDbContext Erweiterungsmethode](https://github.com/aspnet/EntityFramework/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) für die Bereitstellung der `DbContext` -Klasse in ASP.NET DI-Container. Methode der Lebensdauer von Diensten fest, um `Scoped` standardmäßig. `Scoped`bedeutet, dass die Lebensdauer eines Objekts Kontext mit der Lebensdauer der Web-Anforderung Zeitangabe und die `Dispose` Methode wird automatisch am Ende der webanforderung aufgerufen werden.

## <a name="handling-transactions"></a>Behandeln von Transaktionen

Standardmäßig wird das Entity Framework implizit Transaktionen implementiert. In Szenarien, in dem Sie Änderungen auf mehrere Zeilen oder Tabellen, und rufen dann `SaveChanges`, Entity Framework wird automatisch sichergestellt, dass alle Änderungen erfolgreich abgeschlossen oder alle fehlschlagen. Wenn einige Änderungen zuerst fertig sind, und klicken Sie dann ein Fehler tritt auf, werden diese Änderungen automatisch zurückgesetzt. Für Szenarien, in denen mehr, – beispielsweise gesteuert müssen wenn Vorgängen, die außerhalb von Entity Framework in einer Transaktion--enthalten sein sollen, finden Sie unter [Transaktionen](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>No-Überwachungsabfragen

Wenn ein Datenbankkontext Ruft die Tabellenzeilen ab und erstellt von Entitätsobjekten, die sie darstellen, werden von nachverfolgt standardmäßig er, ob die Entitäten im Arbeitsspeicher synchron, sind was in der Datenbank ist. Die Daten im Arbeitsspeicher als Cache fungiert und werden verwendet, wenn Sie eine Entität aktualisieren. Dieses Zwischenspeichern ist häufig in einer Webanwendung nicht erforderlich, da Kontext Instanzen sind in der Regel kurzlebige (ein neuer Schlüssel wird erstellt und freigegeben für jede Anforderung) sowie den Kontext, liest eine Entität ist in der Regel verworfen werden, bevor diese Entität erneut verwendet wird.

Sie können die Überwachung von Entitätsobjekten im Arbeitsspeicher deaktivieren, durch Aufrufen der `AsNoTracking` Methode. Typische Szenarios, in denen Sie möchten, die, umfassen Folgendes:

* Während der Lebensdauer der Kontext müssen Sie keine Entitäten aktualisieren, und ist nicht erforderlich, um EF [Navigationseigenschaften automatisch zu laden, Entitäten, die abgerufen, indem eine eigene Abfrage](read-related-data.md). Häufig werden diese Bedingungen in einem Controller HttpGet Aktionsmethoden erfüllt.

* Ausführen einer Abfrage, die eine große Datenmenge abruft, und wird nur ein kleiner Teil der zurückgegebenen Daten aktualisiert werden. Es ist möglicherweise effizienter, Deaktivieren der Überwachung für große Abfrage, und führen Sie eine Abfrage später für einige Entitäten, die aktualisiert werden müssen.

* Sie eine Entität anfügen, um es zu aktualisieren möchten, aber zuvor abgerufenen dieselbe Entität für unterschiedliche Zwecke erfüllen. Da die Entität bereits vom Kontext Datenbank nachverfolgt wird, können Sie die Entität nicht anfügen, die Sie ändern möchten. Eine Methode für diese Situation ist das Aufrufen `AsNoTracking` auf der oben dargestellten Abfrage.

Weitere Informationen finden Sie unter [Vs nachverfolgen. Keine Überwachung](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Zusammenfassung

Sie verfügen jetzt über einen vollständigen Satz von Seiten, die einfache CRUD-Operationen für Student Entitäten ausführen. In den nächsten Lernprogrammen erweitern Sie die Funktionalität der **Index** Seite durch Hinzufügen von sortieren, Filtern und Paging.

>[!div class="step-by-step"]
[Zurück](intro.md)
[Weiter](sort-filter-page.md)  
