---
title: "ASP.NET Core MVC mit EF-Kern - Parallelität - 8 von 10"
author: tdykstra
description: "Dieses Lernprogramm zeigt, wie Konflikte zu behandeln, wenn mehrere Benutzer gleichzeitig derselben Entität aktualisieren."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 69ffafc7f92cda75c001fe1098275766063113fb
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>Parallelitätskonflikte - EF-Core mit ASP.NET Core MVC-Lernprogramm (8 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

In früheren Lernprogrammen haben Sie gelernt, Daten zu aktualisieren. Dieses Lernprogramm zeigt, wie Konflikte zu behandeln, wenn mehrere Benutzer gleichzeitig derselben Entität aktualisieren.

Erstellen Sie Webseiten, die die Entität Department arbeiten und Behandeln von Parallelitätsfehlern. Die folgenden Abbildungen zeigen die Seiten bearbeiten und löschen, z. B. einige Nachrichten, die angezeigt werden, wenn einem Parallelitätskonflikt.

![Abteilung bearbeiten (Seite)](concurrency/_static/edit-error.png)

![Seite "Abteilung löschen"](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Parallelitätskonflikte

Parallelitätskonflikt tritt auf, wenn ein Benutzer eine Entität Daten angezeigt, um ihn zu bearbeiten und ein anderer Benutzer aktualisiert dann die gleiche Entität Daten, bevor die erste Änderung des Benutzers in die Datenbank geschrieben wird. Wenn Sie die Erkennung von Konflikte dieser Art nicht aktivieren, muss jede Person zur Aktualisierung der Datenbank zuletzt des anderen Benutzers werden Änderungen überschrieben. In vielen Anwendungen dieses Risiko ist akzeptabel: Wenn nur wenige Benutzer oder einige Updates vorhanden sind oder nicht wirklich wichtig, wenn einige Änderungen überschrieben werden, kann die Kosten für die Programmierung für die Parallelität die Vorteile überwiegen. In diesem Fall müssen Sie die Anwendung zur Handhabung von Parallelitätskonflikten zu konfigurieren.

### <a name="pessimistic-concurrency-locking"></a>Die eingeschränkte Parallelität (Sperren)

Wenn Ihre Anwendung muss in parallelitätsszenarios versehentliche Datenverluste zu verhindern, ist eine Möglichkeit dazu Datenbanksperren verwenden. Dies ist die eingeschränkte Parallelität bezeichnet. Bevor Sie eine Zeile aus einer Datenbank lesen, Sie anfordern, z. B. eine Sperre für nur-Lese oder Aktualisierungszugriff. Wenn Sie eine Zeile für Aktualisierungszugriff sperren, dürfen keine anderen Benutzer der Zeile, die entweder für Sperren nur-Lese oder Aktualisierungszugriff, da erhalten sie eine Kopie der Daten, die nicht gerade geändert wird. Wenn Sie eine Zeile für schreibgeschützten Zugriff gesperrt werden, können andere auch für schreibgeschützten Zugriff jedoch nicht für Update gesperrt.

Verwalten von Sperren hat Nachteile. Es kann zum Programm komplex sein. Erfordert erhebliche Datenbankressourcen für Verwaltung, und es kann zu Leistungsproblemen führen, als die Anzahl der Benutzer einer Anwendung erhöht. Aus diesen Gründen unterstützen nicht alle Datenbank-Managementsystemen eingeschränkte Parallelität. Entity Framework Core bietet keine integrierte Unterstützung dafür, und dieses Lernprogramms nicht zeigen, wie sie implementiert.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative für die eingeschränkte Parallelität ist die vollständige Parallelität. Vollständige Parallelität bedeutet ermöglicht Parallelitätskonflikte durchgeführt werden soll, und klicken Sie dann entsprechend reagieren, wenn dies der Fall. Z. B. Andrea besucht die Seite Abteilung bearbeiten und dabei ändert sich der Betrag für die englischen Abteilung $350,000.00 auf $0,00.

![Ändern Sie das Budget auf 0](concurrency/_static/change-budget.png)

Bevor Andrea klickt **speichern**, John derselben Seite besuchen, und ändert sich der Start Date-Felds von 9/1/2007, 9/1/2013.

![Ändern von Startdatum in 2013](concurrency/_static/change-date.png)

Andrea klickt **speichern** erste und sieht ihr ändern, wenn der Browser an die Indexseite zurückgibt.

![Budget in NULL geändert](concurrency/_static/budget-zero.png)

Dann John klickt **speichern** auf eine Bearbeitungsseite, die ein Budget von $350,000.00 weiterhin angezeigt. Was daraufhin geschieht richtet sich nach der Behandlung von Parallelitätskonflikten.

Einige der Optionen umfassen Folgendes:

* Sie können Nachverfolgen von ein Benutzer geändert hat, dessen Eigenschaft und nur die entsprechenden Spalten in der Datenbank zu aktualisieren.

     In diesem Beispielszenario würde keine Daten verloren, weil Sie unterschiedliche Eigenschaften von zwei Benutzer aktualisiert wurden. Das nächste Mal, das eine Person die englische Abteilung durchsucht, werden Janes und Peters Änderungen--von 9/1/2013 ein Startdatum und ein Budget Null Dollar angezeigt. Diese Methode zur Aktualisierung reduzieren die Anzahl der Konflikte, die zu Datenverlusten führen können, aber es kann nicht Datenverluste vermeiden, wenn die gleiche Eigenschaft einer Entität konkurrierende geändert werden. Ob das Entity Framework auf diese Weise funktioniert, hängt davon ab, wie Update Code implementieren. Es ist häufig nicht praktikabel ist, in einer Web-Anwendung, da erforderlich, große Mengen des Zustands zu verwalten, um nachzuverfolgen, um alle ursprünglichen Eigenschaftswerte für eine Entität sowie die neuen Werte. Verwalten von großen Datenmengen Zustand kann Leistung der Anwendung beeinträchtigen, da es Serverressourcen erfordert oder auf der Webseite selbst (z. B. in ausgeblendeten Feldern) enthalten sein muss oder in einem Cookie.

* Sie können Peters Änderung Janes Änderung überschreiben lassen.

     Das nächste Mal eine Person durchsucht die englische Abteilung, sehen sie, 9/1/2013 und die wiederhergestellten $350,000.00-Wert. Hierbei spricht einen *Client gewinnt* oder *Last in Wins* Szenario. (Alle Werte aus dem Client haben Vorrang vor was im Datenspeicher ist.) Wie in der Einführung zu diesem Abschnitt erwähnt, wenn Sie die Codierung für Parallelitätsbehandlung nicht tun, erfolgt dies automatisch.

* Sie können verhindern, dass Peters Änderung in der Datenbank aktualisiert wird.

     In der Regel würden Sie zeigt eine Fehlermeldung an, zeigen ihm den aktuellen Zustand der Daten und ermöglichen ihm seine Änderungen erneut angewendet, wenn er noch machen möchte. Hierbei spricht einen *Store Wins* Szenario. (Der Datenspeicher Werte haben Vorrang vor den Werten, die vom Client gesendet.) In diesem Lernprogramm implementieren Sie die Wins-Store-Szenario. Diese Methode wird sichergestellt, dass keine Änderungen, ohne dass ein Benutzer wird gewarnt überschrieben werden, was geschieht.

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Konflikten bei der Parallelität

Lösen von Konflikten durch behandeln `DbConcurrencyException` Ausnahmen, die das Entity Framework löst. Damit Sie wissen, wann diese Ausnahmen ausgelöst werden soll, muss das Entity Framework zum Erkennen von Konflikten können. Aus diesem Grund müssen Sie die Datenbank und des Datenmodells entsprechend konfigurieren. Einige Optionen zum Aktivieren der konflikterkennung umfassen Folgendes:

* Enthalten Sie in der Datenbanktabelle eine Überwachung-Spalte, die verwendet werden kann, um zu bestimmen, wenn eine Zeile geändert wurde. Anschließend konfigurieren Sie das Entity Framework Einbeziehung dieser Spalte in der Where-Klausel der SQL-Update oder Delete-Befehle.

     Der Datentyp der Spalte Überwachung ist in der Regel `rowversion`. Die `rowversion` Wert ist eine sequenzielle Zahl, die jedes Mal erhöht ist die Zeile aktualisiert wird. In einem Update- oder Delete-Befehl der Where-Klausel enthält den ursprünglichen Wert des Nachverfolgungsspalte (die ursprüngliche Zeilenversion). Wenn Sie die aktualisierte Zeile bereits durch den Wert in einen anderen Benutzer geändert wurde die `rowversion` Spalte unterscheidet sich von den ursprünglichen Wert werden, damit die Update- oder Delete-Anweisung die Zeile beim Aktualisieren aufgrund der Where findet Klausel. Wenn der Befehl die Entity Framework-feststellt, dass durch die Update- oder Delete keine Zeilen aktualisiert wurden (d. h., wenn die Anzahl der betroffenen Zeilen auf 0 (null) ist) interpretiert, die als Parallelitätskonflikt.

* Konfigurieren Sie das Entity Framework Einbeziehung von den ursprünglichen Werten der jede Spalte in der Tabelle in der Where-Klausel der Update und Delete-Befehle.

     Wie die erste Option, wenn alle Elemente in der Zeile geändert wurde, seit die Zeile zuerst gelesen wurde in der Where Klausel keine Zeile zu aktualisieren, der das Entity Framework als Parallelitätskonflikt interpretiert zurückgegeben. Für Datenbanktabellen, die viele Spalten aufweisen, dieser Ansatz kann dazu führen, sehr große Where-Klausel und kann festlegen, dass Sie große Mengen an Zustand beibehalten. Wie bereits erwähnt, kann das Verwalten von großen Datenmengen Zustand Leistung der Anwendung beeinträchtigen. Aus diesem Grund diese Vorgehensweise wird im Allgemeinen nicht empfohlen, und dies nicht der Fall in diesem Lernprogramm verwendete Methode.

     Wenn Sie diesen Ansatz zur Parallelität implementieren möchten, müssen Sie alle nicht primären Schlüssel-Eigenschaften in der Entität kennzeichnen Sie durch Hinzufügen von Parallelität für nachverfolgen möchten die `ConcurrencyCheck` -Attribut auf diese. Diese Änderung ermöglicht das Entity Framework auf alle Spalten in der SQL-Where-Klausel der Update und Delete-Anweisungen enthalten.

Im weiteren Verlauf dieses Lernprogramms fügen Sie eine `rowversion` tracking-Eigenschaft auf die Entität Department, erstellen Sie einen Controller und Ansichten, und testen, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Hinzufügen einer Überwachung-Eigenschaft auf die Entität Department

In *Models/Department.cs*, Hinzufügen einer Überwachung-Eigenschaft, die mit dem Namen RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

Die `Timestamp` Attribut gibt an, dass diese Spalte eingeschlossen werden in der Where-Klausel der Update und Delete-Befehle an die Datenbank gesendet. Das Attribut heißt `Timestamp` da frühere Versionen von SQL Server eine SQL verwendet `timestamp` -Datentyp, bevor SQL `rowversion` ersetzt. Der Typ .NET für `rowversion` ist ein Bytearray.

Wenn Sie die fluent-API verwenden möchten, können Sie mithilfe der `IsConcurrencyToken` -Methode (in *Data/SchoolContext.cs*) die Tracking-Eigenschaft angeben, wie im folgenden Beispiel gezeigt:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Durch Hinzufügen einer Eigenschaft wird das Datenbankmodell geändert, daher Sie einen anderen Migration ausführen müssen.

Speichern Sie die Änderungen zu und erstellen Sie das Projekt, und geben Sie dann die folgenden Befehle im Befehlsfenster:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Erstellen Sie einen Abteilungen Controller und Ansichten

Erstellen Sie einen Abteilungen Controller und Ansichten aus, wie zuvor für Studenten, Bildungseinrichtungen Kurse und Lehrkräfte.

![Gerüst Abteilung](concurrency/_static/add-departments-controller.png)

In der *DepartmentsController.cs* Datei, ändern Sie alle vier Vorkommen von "FirstMidName" in "FullName", sodass die Abteilung Administrator Dropdown-Listen der vollständige Name der Kursleiter anstatt nur der letzte Name enthält.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Aktualisieren Sie die Indexansicht Abteilungen

Das Modul Gerüstbau RowVersion-Spalte in die Indexansicht erstellt, aber dieses Feld darf nicht angezeigt werden.

Ersetzen Sie den Code in *Views/Departments/Index.cshtml* durch den folgenden Code.

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Dies ändert den Spaltenüberschrift, um "Abteilung", löscht der RowVersion-Spalte und zeigt zuerst anstelle des vollständigen Namens für den Administrator.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Aktualisierungsmethoden Sie "Bearbeiten" im Controller Abteilungen

In beiden die HttpGet `Edit` Methode und die `Details` -Methode hinzufügen `AsNoTracking`. In der HttpGet `Edit` Methode unverzüglichem Laden für den Administrator hinzuzufügen.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Ersetzen Sie den vorhandenen Code für die HttpPost `Edit` -Methode durch folgenden Code:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Der Code wird zuerst beim Lesen der Abteilung aktualisiert werden. Wenn die `SingleOrDefaultAsync` Methode gibt null zurück, die Abteilung wurde von einem anderen Benutzer gelöscht. In diesem Fall verwendet der Code die übermittelte Formularwerte, um eine Abteilung-Entität erstellen, sodass die bearbeiten (Seite) mit einer Fehlermeldung angezeigt werden kann. Als Alternative wäre nicht stehen Ihnen die Entität Department neu zu erstellen, wenn Sie nur eine Fehlermeldung angezeigt, ohne erneute Anzeigen der Abteilung Felder.

Die Sicht speichert die ursprüngliche `RowVersion` Wert in ein verborgenes Feld, und diese Methode empfängt, diesen Wert in der `rowVersion` Parameter. Vor dem Aufruf `SaveChanges`, müssen Sie, die ursprüngliche put `RowVersion` Eigenschaftswert in der `OriginalValues` Auflistung für die Entität.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Und wenn Entity Framework einen UPDATE für SQL-Befehl erstellt, dieser Befehl eine WHERE-Klausel enthalten, die sucht nach einer Zeile, die die ursprüngliche `RowVersion` Wert. Wenn keine Zeilen, durch den Befehl UPDATE betroffen sind (keine Zeilen vorhanden sind, die ursprüngliche `RowVersion` Wert), das Entity Framework löst eine `DbUpdateConcurrencyException` Ausnahme.

Der Code im Catch-Block für diese Ausnahme ruft die betroffene Abteilung-Entität mit den aktualisierten Werten aus der `Entries` -Eigenschaft für das Ausnahmeobjekt.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

Die `Entries` Auflistung hat nur einen `EntityEntry` Objekt.  Dieses Objekt können Sie um die neuen Werte, die vom Benutzer eingegeben werden und die aktuellen Datenbankwerte zu erhalten.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Der Code Fügt eine benutzerdefinierte Fehlermeldung für jede Spalte, die verschiedene Datenbankwerten hat Seite aus, was der Benutzer die Eingabe in der Bearbeitung (nur ein Feld wird hier dargestellt aus Gründen der Übersichtlichkeit).

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Schließlich der Code legt die `RowVersion` Wert, der die `departmentToUpdate` auf den neuen Wert aus der Datenbank abgerufen. Diese neue `RowVersion` Wert wird in das ausgeblendete Feld gespeichert werden, wenn die Seite wird erneut bearbeiten und das nächste die benutzeraufrufen Mal **speichern**, nur Parallelitätsfehlern, die auftreten, da es sich bei der erneuten Anzeige Bearbeitungsseite abgefangen wird.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

Die `ModelState.Remove` Anweisung ist erforderlich, da `ModelState` hat die alte `RowVersion` Wert. In der Ansicht der `ModelState` Wert für ein Feld Vorrang vor den Modell-Eigenschaftswerte hat, wenn beide vorhanden sind.

## <a name="update-the-department-edit-view"></a>Aktualisieren Sie die Ansicht Abteilung bearbeiten

In *Views/Departments/Edit.cshtml*, nehmen Sie die folgenden Änderungen:

* Fügen Sie ein ausgeblendetes Feld zum Speichern der `RowVersion` Eigenschaftswert, der unmittelbar hinter das ausgeblendete Feld für die `DepartmentID` Eigenschaft.

* Fügen Sie eine Option "Administrator aktivieren" auf die Dropdown-Liste hinzu.

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Testen von Parallelitätskonflikten in bearbeiten (Seite)

Führen Sie die app, und navigieren Sie auf der Abteilungen Index. Mit der rechten Maustaste die **bearbeiten** Link für die englischen Abteilung, und wählen **in neuer Registerkarte öffnen**, klicken Sie dann auf die **bearbeiten** Link für die englischen Abteilung. Die zwei browserregisterkarten wird jetzt die gleiche Informationen angezeigt.

Ändern eines Felds in der ersten Registerkarte ' Browser ', und klicken Sie auf **speichern**.

![Abteilung bearbeiten Seite 1 nach der Änderung](concurrency/_static/edit-after-change-1.png)

Der Browser zeigt die Indexseite mit den geänderten Wert an.

Ändern Sie ein Feld in der zweiten Registerkarte.

![Abteilung bearbeiten Seite 2 nach der Änderung](concurrency/_static/edit-after-change-2.png)

Klicken Sie auf **Speichern**. Sie sehen eine Fehlermeldung angezeigt:

![Abteilung bearbeiten Seite Fehlermeldung](concurrency/_static/edit-error.png)

Klicken Sie auf **speichern** erneut aus. Der Wert in der zweiten Registerkarte eingegebene wird gespeichert. Die gespeicherten Werte wird angezeigt, wenn die Indexseite angezeigt wird.

## <a name="update-the-delete-page"></a>Aktualisieren Sie die Seite "löschen"

Für die Seite "löschen" erkennt das Entity Framework Parallelitätskonflikte zurückzuführen, dass jemand die ansonsten die Abteilung auf ähnliche Weise bearbeiten. Wenn die HttpGet `Delete` Methode zeigt, die Bestätigung an, die Ansicht enthält die ursprüngliche `RowVersion` Wert in ein ausgeblendetes Feld. Dieser Wert dann zur Verfügung HttpPost wird `Delete` Methode, die aufgerufen wird, wenn der Benutzer den Löschvorgang bestätigt. Wenn Entity Framework die SQL-Befehl DELETE erstellt, enthält Sie eine WHERE-Klausel mit der ursprünglichen `RowVersion` Wert. Wenn die Ergebnisse von Befehlen in 0 Zeilen betroffen (d. h. die Zeile geändert wurde, nach die Bestätigungsseite löschen angezeigt wurde), wird eine Parallelitätsausnahme ausgelöst, und die HttpGet `Delete` -Methode aufgerufen wird und ein Fehler Flag festgelegt auf "true", um erneut anzeigen der Bestätigungsseite mit einer Fehlermeldung. Es ist auch möglich, dass 0 Zeilen betroffen sind, da die Zeile von einem anderen Benutzer gelöscht wurde, damit in diesem Fall wird keine Fehlermeldung angezeigt wird.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Aktualisieren Sie die Delete-Methoden in den Abteilungen-controller

In *DepartmentsController.cs*, ersetzen Sie die HttpGet `Delete` -Methode durch folgenden Code:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Die Methode akzeptiert einen optionalen Parameter, der angibt, ob die Seite nach einem Parallelitätsfehler wird erneut angezeigt wird. Wenn dieses Flag "true ist", und die Abteilung nicht mehr vorhanden ist, wurde er von einem anderen Benutzer gelöscht. In diesem Fall leitet der Code auf der Seite "Index".  Wenn dieses Flag "true ist", und die Abteilung ist vorhanden, wurde er von einem anderen Benutzer geändert. In diesem Fall sendet der Code eine Fehlermeldung an die Ansicht mit `ViewData`.  

Ersetzen Sie den Code in die HttpPost `Delete` Methode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

In den scaffolded Code, den Sie soeben ersetzt, akzeptiert diese Methode nur eine Datensatz-ID:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Sie haben dieser Parameter auf eine Abteilung Entitätsinstanz erstellt, indem die Modellbinder geändert. So erhält EF Zugriff auf den Wert der RowVersion-Eigenschaft zusätzlich zu dem Datensatzschlüssel.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Sie haben auch den Methodennamen Aktion von geändert `DeleteConfirmed` auf `Delete`. Der scaffolded Code verwendet den Namen `DeleteConfirmed` so erteilen Sie der Methode HttpPost eine eindeutige Signatur. (Die CLR erfordert überladene Methoden zum anderen Methodenparameter angegeben haben.) Nun, dass die Signaturen eindeutig sind, können die MVC-Konvention aufgeben und denselben Namen für die HttpPost und HttpGet Delete-Methoden verwenden.

Wenn die Abteilung bereits gelöscht wird, die `AnyAsync` Methode "false" zurückgegeben, und die Anwendung nur wechselt wieder an die Index-Methode.

Wenn ein Parallelitätsfehler abgefangen wird, wird der Code erneut an die Bestätigungsseite löschen und bietet ein Flag, das darauf eine Fehlermeldung Parallelität angezeigt werden soll.

### <a name="update-the-delete-view"></a>Aktualisieren der Ansicht löschen

In *Views/Departments/Delete.cshtml*, ersetzen Sie den scaffolded Code durch den folgenden Code, der ein Fehler Nachrichtenfeld und ausgeblendete Felder für die Eigenschaften "DepartmentID" und RowVersion hinzufügt. Die Änderungen werden hervorgehoben.

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Dadurch werden die folgenden Änderungen:

* Fügt eine Fehlermeldung zwischen den `h2` und `h3` Überschriften.

* Ersetzt FirstMidName mit FullName in den **Administrator** Feld.

* Entfernt das RowVersion-Feld.

* Fügt ein ausgeblendetes Feld für die `RowVersion` Eigenschaft.

Führen Sie die app, und navigieren Sie auf der Abteilungen Index. Mit der rechten Maustaste die **löschen** Link für die englischen Abteilung, und wählen **in neuer Registerkarte öffnen**, klicken Sie in der ersten Registerkarte der **bearbeiten** Link für die englischen Abteilung.

Im ersten Fenster ändern Sie einen der Werte, und klicken Sie auf **speichern**:

![Abteilung bearbeiten (Seite) nach der Änderung vor dem Löschen](concurrency/_static/edit-after-change-for-delete.png)

Klicken Sie in der zweiten Registerkarte auf **löschen**. Die Parallelität Fehlermeldung angezeigt, und die Department-Werte werden aktualisiert, was derzeit in der Datenbank ist.

![Abteilung Delete Bestätigungsseite mit Parallelitätsfehler](concurrency/_static/delete-error.png)

Wenn Sie auf **löschen** erneut, sind Sie auf die Indexseite, der anzeigt, dass die Abteilung gelöscht wurde umgeleitet.

## <a name="update-details-and-create-views"></a>Updatedetails und Erstellen von Sichten

Optional können Sie Ansichten erstellen und Bereinigen des migrationsgerüst Code in den Details.

Ersetzen Sie den Code in *Views/Departments/Details.cshtml* zum Löschen der RowVersion-Spalte und zeigt den vollständigen Namen des Administrators.

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Ersetzen Sie den Code in *Views/Departments/Create.cshtml* der Dropdown Liste einer Select-Option hinzu.

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Zusammenfassung

Dies schließt die Einführung in die Behandlung von Parallelitätskonflikten. Weitere Informationen zum Behandeln von Parallelität in EF Core finden Sie unter [Parallelitätskonflikte](https://docs.microsoft.com/ef/core/saving/concurrency). Der nächste Lernprogrammen wird gezeigt, wie Tabelle pro Hierarchie Vererbung für den Dozenten und Student Entitäten implementiert wird.

>[!div class="step-by-step"]
[Zurück](update-related-data.md)
[Weiter](inheritance.md)  
