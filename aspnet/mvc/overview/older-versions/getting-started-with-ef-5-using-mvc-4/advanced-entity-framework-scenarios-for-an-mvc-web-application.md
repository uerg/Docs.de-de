---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Erweiterte Entity Framework-Szenarien für eine MVC-Webanwendung (10 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: e4e0a754163ad6b44ca02678fe6a0407e71ec3e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398196"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Erweiterte Entity Framework-Szenarios für eine MVC-Webanwendung (10 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Tutorial implementiert Sie das Repository und arbeitseinheitsmuster. In diesem Tutorial werden die folgenden Themen behandelt:

- Ausführen von unformatierte SQL-Abfragen.
- Ausführen von Abfragen ohne nachverfolgung.
- Untersuchung von Abfragen an die Datenbank gesendet.
- Arbeiten mit Webdienstproxy-Klassen.
- Deaktivieren die automatischen Erkennung von Änderungen.
- Deaktivieren der Validierung beim Speichern der Änderungen.
- [Fehler und Problemumgehungen für Arbeitsaufgaben](#errors)

Für die meisten dieser Themen arbeiten Sie mit der Seiten, die Sie bereits erstellt. Verwendung roher SQL, um massenaktualisierungen auszuführen erstellen Sie eine neue Seite, die die Anzahl der Gutschrift in Höhe von alle Kurse in der Datenbank aktualisiert wird:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Und um eine Abfrage ohne nachverfolgung verwenden, fügen Sie neue Validierungslogik auf der Seite "Abteilung bearbeiten":

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Unformatierte SQL-Abfragen ausführen

Die Entity Framework Code First-API enthält Methoden, die Ihnen ermöglichen, SQL-Befehle direkt an die Datenbank übergeben. Sie haben die folgenden Optionen:

- Verwenden Sie die `DbSet.SqlQuery`-Methode für Abfragen, die Entitätstypen zurückgeben. Die zurückgegebenen Objekte muss mit dem vom erwarteten Typ der `DbSet` -Objekt, und sie werden automatisch nachverfolgt durch den Datenbankkontext, wenn Sie die Überwachung deaktivieren. (Finden Sie im Abschnitt zu den `AsNoTracking` Methode.)
- Verwenden der `Database.SqlQuery` -Methode für Abfragen, die Typen zurückgeben, die nicht von Entitäten. Die zurückgegebenen Daten werden nicht vom Datenbankkontext nachverfolgt, auch wenn Sie diese Methode zum Abrufen von Entitätstypen verwenden.
- Verwenden der [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) für nichtabfragebefehle.

Einer der Vorteile von Entity Framework ist die Tatsache, dass vermieden wird, den Code zu eng an eine bestimmte Methode zum Speichern von Daten zu binden. Dies geschieht, indem SQL-Abfragen und -Befehle für Sie generiert werden. Somit müssen Sie sie nicht selbst schreiben. Aber es gibt außergewöhnliche Szenarios müssen Sie bestimmte SQL-Abfragen ausführen, die Sie manuell erstellt haben, und diese Methoden ermöglichen es Ihnen, solche Ausnahmen zu behandeln.

Dies trifft immer zu, wenn Sie SQL-Befehle in einer Webanwendung ausführen. Treffen Sie deshalb Vorsichtsmaßnahmen, um Ihre Website vor Angriffen durch Einschleusung von SQL-Befehlen zu schützen. Verwenden Sie dazu parametrisierte Abfragen, um sicherzustellen, dass Zeichenfolgen, die von einer Webseite übermittelt werden, nicht als SQL-Befehle interpretiert werden können. In diesem Tutorial verwenden Sie parametrisierte Abfragen beim Integrieren von Benutzereingaben in eine Abfrage.

### <a name="calling-a-query-that-returns-entities"></a>Eine Abfrage aufrufen, gibt Entitäten zurück.

Angenommen, Sie möchten die `GenericRepository` Klasse zusätzliche filtern und Sortieren von Flexibilität ohne, dass Sie eine abgeleitete Klasse mit zusätzlichen Methoden erstellen. Zum Hinzufügen einer Methode, die eine SQL-Abfrage akzeptiert, ist eine Möglichkeit, dies zu erreichen wäre. Sie können dann angeben, soll jede Art von Filtern oder sortieren Sie z. B. im Controller eine `Where` -Klausel, die von einem Joins oder eine Unterabfrage abhängig ist. In diesem Abschnitt sehen Sie, wie Sie eine solche Methode zu implementieren.

Erstellen der `GetWithRawSql` Methode, indem Sie den folgenden Code hinzufügen *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

In *CourseController.cs*, rufen Sie die neue Methode über die `Details` Methode, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

In diesem Fall Sie hätten auch das `GetByID` -Methode, aber Sie verwenden die `GetWithRawSql` Methode, um zu überprüfen, ob die `GetWithRawSQL` Methode funktioniert.

Führen Sie die Seite Details, um sicherzustellen, dass die Select-funktioniert Abfrage (Wählen Sie die **Kurs** Registerkarte und anschließend **Details** für einen Kurs).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Aufrufen von einer Abfrage, zurückgibt andere Typen von Objekten

Sie haben zuvor ein Statistikraster für Studenten für die Infoseite erstellt, das die Anzahl der Studenten für jedes Anmeldedatum zeigt. Der Code in hierfür *"HomeController.cs"* verwendet LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Angenommen Sie, Sie möchten den Code zu schreiben, der diese Daten direkt in SQL, anstatt LINQ abruft. Zu diesem Zweck müssen Sie zum Ausführen einer Abfrage, die etwas anderes als Entitätsobjekte zurückgibt, also müssen Sie verwenden die `Database.SqlQuery` Methode.

In *"HomeController.cs"*, ersetzen Sie die LINQ-Anweisung in der `About` Methode durch den folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Führen Sie die Seite "Info". Sie zeigt die gleichen Daten wie zuvor.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Aufrufen einer Aktualisierungsabfrage

Angenommen Sie, Sie möchten, dass Administratoren der Contoso University massenänderungen in der Datenbank, z. B. das Ändern der Anzahl der Credits für jeden Kurs ausführen können. Wenn die Universität über eine große Anzahl an Kursen verfügt, wäre es ineffizient, sie alle als Entitäten abzurufen und separat zu ändern. In diesem Abschnitt implementieren Sie eine Webseite, die dem Benutzer ermöglicht, geben Sie einen Faktor, um die Anzahl der Credits für alle Kurse zu ändern, und erstellen Sie die Änderung durch Ausführen einer SQL `UPDATE` Anweisung. Die Webseite wird wie die folgende Abbildung aussehen:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Im vorherigen Tutorial verwendet Sie die generischen Repositorys zum Lesen und aktualisieren `Course` Entitäten in der `Course` Controller. Für diesen Vorgang Bulk Update müssen Sie eine neue repositorymethode erstellen, die nicht im generischen-Repository. Zu diesem Zweck erstellen Sie eine dedizierte `CourseRepository` abgeleitete Klasse die `GenericRepository` Klasse.

In der *DAL* Ordner erstellen *CourseRepository.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

In *UnitOfWork.cs*, ändern Sie die `Course` repositorytyp aus `GenericRepository<Course>` auf `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

In *CourseContoller.cs*, Hinzufügen einer `UpdateCourseCredits` Methode:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Diese Methode wird für beide wird `HttpGet` und `HttpPost`. Wenn die `HttpGet` `UpdateCourseCredits` Methode ausgeführt wird, die `multiplier` Variable ist null, und die Ansicht wird ein leeres Textfeld und eine Schaltfläche "Senden" angezeigt, wie in der vorherigen Abbildung dargestellt.

Wenn die **Update** geklickt wird und die `HttpPost` Methode ausgeführt wird, `multiplier` wird den Wert in das Textfeld eingegeben haben. Der Code ruft dann das Repository `UpdateCourseCredits` -Methode, die gibt die Anzahl der betroffenen Zeilen an, und dieser Wert befindet sich in der `ViewBag` Objekt. Wenn die Ansicht empfängt die Anzahl der betroffenen Zeilen in der `ViewBag` -Objekts dieser Zahl statt im Textfeld angezeigt und submit-Schaltfläche in der folgenden Abbildung dargestellt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Erstellen Sie eine Ansicht in der *Views\Course* Ordner für die Seite "Update-Kurses":

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

In *Views\Course\UpdateCourseCredits.cshtml*, ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Führen Sie die `UpdateCourseCredits`-Methode aus, indem Sie die Registerkarte **Courses** (Kurse) auswählen, und dann „/UpdateCourseCredits“ am Ende der URL in die Adressleiste des Browsers einfügen (z.B.: `http://localhost:50205/Course/UpdateCourseCredits`). Geben Sie eine Zahl in das Textfeld ein:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klicken Sie auf **Aktualisieren**. Die Anzahl der betroffenen Zeilen wird angezeigt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klicken Sie auf **Zurück zur Liste**, um die Kursliste mit der geänderte Anzahl von Credits anzuzeigen.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Weitere Informationen zu unformatierten SQL-Abfragen finden Sie unter [unformatierte SQL-Abfragen](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) im Entity Framework-Team-Blog.

## <a name="no-tracking-queries"></a>Abfragen ohne Nachverfolgung

Wenn ein Datenbankkontext Datenbankzeilen abgerufen, und erstellt Entitätsobjekte, die diese darstellen, verfolgt des standardmäßig er, ob die Entitäten im Arbeitsspeicher mit den in der Datenbank synchronisiert werden. Die Daten im Arbeitsspeicher fungieren als Cache und werden verwendet, wenn Sie eine Entität aktualisieren. Diese Zwischenspeicherung ist in Webanwendungen oft überflüssig, da Kontextinstanzen in der Regel kurzlebig sind (eine neue wird bei jeder Anforderung erstellt und gelöscht), und der Kontext, der eine Entität liest, wird in der Regel gelöscht, bevor diese Entität erneut verwendet wird.

Sie können angeben, ob der Kontext Entitätsobjekte für eine Abfrage mit nachverfolgt. die `AsNoTracking` Methode. Typische Szenarios, in denen Sie das möglicherweise tun wollen, umfassen Folgendes:

- Die Abfrage ruft solche eine große Menge von Daten, die durch das Ausschalten Überwachung deutlich die Leistung steigern können.
- Sie möchten eine Entität anfügen, um diese zu aktualisieren, aber Sie zuvor die gleiche Entität für einen anderen Zweck abgerufen. Da diese Entität bereits vom Datenbankkontext nachverfolgt wird, können Sie die zu ändernde Entität nicht anfügen. Eine Möglichkeit, dies zu verhindern ist die Verwendung der `AsNoTracking` Option mit die vorherige Abfrage.

In diesem Abschnitt implementieren Sie die Geschäftslogik, die die zweite dieser Szenarien veranschaulicht. Insbesondere müssen Sie eine Geschäftsregel erzwingen, die besagt, dass ein Dozent der Administrator von mehr als eine Abteilung sein kann.

In *DepartmentController.cs*, fügen eine neue Methode, die Sie aufrufen können, aus der `Edit` und `Create` Methoden, um sicherzustellen, dass keine zwei Abteilungen die gleichen Administratoren haben:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Fügen Sie Code in die `try` -Block den `HttpPost` `Edit` Methode, die diese neue Methode aufgerufen, wenn keine Validierungsfehler vorhanden sind. Die `try` Block sieht nun wie im folgenden Beispiel:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Führen Sie die Seite "Abteilung bearbeiten", und versuchen Sie, eine Abteilung Administrator in ein "Instructor" zu ändern, die der Administrator einer anderen Abteilung bereits ist. Sie erhalten die erwartete Fehlermeldung angezeigt:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Führen Sie jetzt die Abteilung bearbeiten Seite erneut, und diese Zeit geändert der **Budget** Betrag. Beim Klicken auf **speichern**, eine Fehlerseite angezeigt:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Die Fehlermeldung der Ausnahme lautet "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" dies aufgrund der folgenden Abfolge von Ereignissen aufgetreten ist:

- Die `Edit` Methodenaufrufe der `ValidateOneAdministratorAssignmentPerInstructor` -Methode, die alle Abteilungen abruft, die Kim Abercrombie als Administrator. Dies bewirkt den englischen Fachbereich gelesen werden. Da dies die Abteilung bearbeiteten ist, wird kein Fehler gemeldet. Durch dieser Lesevorgang wird jedoch die englische Abteilung-Entität, die aus der Datenbank gelesen wurde jetzt vom Datenbankkontext nachverfolgt.
- Die `Edit` Methode versucht, legen Sie die `Modified` Flag für die englische Entität "Department", die von der MVC-modellbindung erstellt wurde, aber dies fehlschlägt, weil der Kontext verfolgt bereits eine Entität für die englische Abteilung.

Eine Lösung, um dieses Problem ist, um zu verhindern, dass den Kontext abgerufene, indem die überprüfungsabfrage in-Memory-abteilungsentitäten nachverfolgen. Es gibt keinen Nachteil bei der dies, da Sie diese Entität wird nicht aktualisiert oder Lesen sie erneut auf eine Weise, die aus der Zwischenspeicherung im Arbeitsspeicher profitieren würden.

In *DepartmentController.cs*in die `ValidateOneAdministratorAssignmentPerInstructor` -Methode, geben Sie keine nachverfolgung, wie im folgenden dargestellt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Wiederholen Sie versucht haben, bearbeiten Sie die **Budget** Betrag einer Abteilung. Dieses Mal der Vorgang erfolgreich ist und der Standort gibt, wie erwartet auf der Indexseite "Abteilungen", mit dem Wert für die überarbeitete Budget.

## <a name="examining-queries-sent-to-the-database"></a>Untersuchung von Abfragen, die an die Datenbank gesendet

Manchmal ist es hilfreich, die tatsächlichen SQL-Abfragen anzuzeigen, die an die Datenbank gesendet werden. Zu diesem Zweck können Sie eine Abfragevariable im Debugger überprüfen oder die Abfrage des `ToString` Methode. Dies können Sie ausprobieren, Sie eine einfache Abfrage betrachten und sehen Sie sich, was geschieht, wenn Sie diese mittels Eager Load laden, Filtern und Sortieren von Optionen hinzufügen.

In *Controller/CourseController*, ersetzen Sie die `Index` Methode durch den folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Jetzt legen einen Haltepunkt im *GenericRepository.cs* auf die `return query.ToList();` und `return orderBy(query).ToList();` Anweisungen von der `Get` Methode. Führen Sie das Projekt im Debugmodus aus, und wählen Sie die Indexseite des Kurses. Wenn der Code den Haltepunkt erreicht, untersuchen die `query` Variable. Daraufhin wird die Abfrage, die mit SQL Server gesendet wird. Es ist eine einfache `Select` Anweisung:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Abfragen können zu lang, um in die debugging-Fenster in Visual Studio angezeigt werden. Um die gesamte Abfrage anzuzeigen, können Sie den Wert den Variablen kopieren und fügen Sie ihn in einem Text-Editor:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Nun fügen Sie eine Dropdown-Liste der Indexseite des Kurses, sodass Benutzer für eine bestimmte Abteilung filtern können. Sie müssen die Kurse nach Titel sortieren, und geben Sie eager Loading für die `Department` Navigationseigenschaft. In *CourseController.cs*, ersetzen Sie die `Index` Methode durch den folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Die Methode empfängt den ausgewählten Wert aus der Dropdown-Liste in der `SelectedDepartment` Parameter. Wenn nichts ausgewählt ist, wird dieser Parameter null sein.

Ein `SelectList` Auflistung mit allen Abteilungen für die Dropdown-Liste an die Ansicht übergeben wird. Die Parameter zu übergeben, um die `SelectList` Konstruktor angegeben wird, den Feldnamen der Wert, der Feldname für Text und das ausgewählte Element.

Für die `Get` Methode der `Course` Repository, die der Code gibt an, einen Filterausdruck, Sortierreihenfolge und Eager loading für die `Department` Navigationseigenschaft. Gibt zurück, der Filterausdruck immer `true` Wenn nichts in der Dropdown-Liste ausgewählt ist (d. h. `SelectedDepartment` null ist).

In *Views\Course\Index.cshtml*, unmittelbar vor dem öffnenden `table` markieren, fügen Sie den folgenden Code zum Erstellen der Dropdown Liste und eine Schaltfläche "Senden" hinzu:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Mit den Breakpoints im Festlegen der `GenericRepository` -Klasse, führen Sie die Indexseite des Kurses. Durchlaufen Sie die ersten beiden Zeiten, dass der Code einen Haltepunkt trifft, damit die Seite im Browser angezeigt wird. Wählen Sie eine Abteilung aus der Dropdown-Liste, und klicken Sie auf **Filter**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Dieses Mal wird der erste Haltepunkt für die Abteilungen-Abfrage für das Dropdown-Liste sein. Zu überspringen, und zeigen die `query` Variablen das nächste Mal im Code erreicht den Breakpoint um zu sehen, was die `Course` Abfrage sieht jetzt wie. Sie sehen etwa wie folgt:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Sehen Sie, dass die Abfrage ist eine `JOIN` Abfrage, die geladen `Department` Daten zusammen mit den `Course` Daten, und es enthält eine `WHERE` Klausel.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Arbeiten mit Webdienstproxy-Klassen

Wenn Entity Framework erstellt Instanzen der Entität (z. B., wenn Sie eine Abfrage ausführen), wird häufig als Instanzen von einem dynamisch generierten abgeleiteten Typ, der als Proxy für die Entität fungiert. Dieser Proxy überschreibt einige virtuelle Eigenschaften der Entität zum Einfügen von Hooks für Aktionen automatisch ausgeführt, wenn die Eigenschaft zugegriffen wird. Beispielsweise wird dieser Mechanismus verwendet, lazy Loading von Beziehungen unterstützt.

In den meisten Fällen müssen Sie nicht diese Verwendung von Proxys kennen, aber es gibt jedoch Ausnahmen:

- In einigen Fällen empfiehlt es sich um zu verhindern, dass das Entity Framework-Proxyinstanzen erstellen. Serialisieren von nicht-Proxy-Instanzen kann beispielsweise effizienter als das Serialisieren von Proxyinstanzen sein.
- Beim Instanziieren einer Entität Klasse mithilfe der `new` -Operator, erhalten Sie keine Proxyinstanz. Dies bedeutet, dass Sie keine Funktionen erhalten, wie z. B. verzögertes Laden und die automatische änderungsnachverfolgung. Dies ist in der Regel gut. im Allgemeinen nicht erforderlich verzögerten Laden, da Sie eine neue Entität erstellen, die nicht in der Datenbank, und die änderungsnachverfolgung, wenn Sie explizit die Entität als gekennzeichnet sind in der Regel nicht erforderlich, `Added`. Jedoch wenn lazy Loading ist erforderlich, und die änderungsnachverfolgung benötigen, können Sie erstellen neue Instanzen der Entität mit Proxys mit den `Create` Methode der `DbSet` Klasse.
- Sie möchten einen tatsächlichen Entitätstyp von einem Proxytyp zu erhalten. Können Sie die `GetObjectType` Methode der `ObjectContext` Klasse zum Abrufen des tatsächlichen Entitätstyp, der eine Instanz der Proxy-Typ.

Weitere Informationen finden Sie unter [arbeiten mit Proxys in](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) im Entity Framework-Team-Blog.

## <a name="disabling-automatic-detection-of-changes"></a>Deaktivieren die automatischen Erkennung von Änderungen

Entity Framework bestimmt wie eine Entität geändert wurde (und welche Updates an die Datenbank gesendet werden müssen), indem die aktuellen Werte einer Entität mit den ursprünglichen Werten verglichen werden. Die ursprünglichen Werte werden gespeichert, wenn die Entität abgefragt oder angefügt wurde. Einige der Methoden, die automatisch eine Änderungserkennung durchführen, sind die folgenden:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Wenn Sie eine große Anzahl von Entitäten überwachen und Sie eine dieser Methoden oft in einer Schleife aufrufen, erhalten Sie möglicherweise erhebliche Leistungssteigerungen durch vorübergehendes Deaktivieren der automatischen änderungserkennung mithilfe der [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) Eigenschaft. Weitere Informationen finden Sie unter [automatisch erkennen von Änderungen](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Deaktivieren der Validierung beim Speichern der Änderungen

Beim Aufrufen der `SaveChanges` -Methode, wird standardmäßig das Entity Framework überprüft die Daten in der alle Eigenschaften aller geänderten Entitäten vor dem Aktualisieren der Datenbank. Wenn Sie eine große Anzahl von Entitäten aktualisiert haben und Sie haben bereits überprüft die Daten dieser Arbeit ist nicht erforderlich und Sie, die das Speichern vornehmen können werden die Änderungen weniger Zeit durch vorübergehendes Deaktivieren der Validierung. Sie können hierzu die [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) Eigenschaft. Weitere Informationen finden Sie unter [Überprüfung](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Zusammenfassung

Dies schließt diese tutorialreihe zur Verwendung von Entity Framework in einer ASP.NET MVC-Anwendung. Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

Weitere Informationen dazu, wie Sie Ihre Webanwendung bereitstellen, nachdem Sie sie erstellt haben, finden Sie unter [ASP.NET die ASP.NET-Bereitstellung](https://msdn.microsoft.com/library/bb386521.aspx) in der MSDN Library.

Weitere Informationen zu anderen Themen im Zusammenhang mit MVC, wie z. B. Authentifizierung und Autorisierung, finden Sie unter den [empfohlene Ressourcen für MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Danksagungen

- Die ursprüngliche Version dieses Tutorials habe sowie Tom Dykstra und ist ein leitender Redakteur für Programmierung auf der Microsoft-Webplattform und Tools-Team-Inhalte.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter- [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) in diesem Tutorial Co-Autor und hat den Großteil der Arbeit, die für EF 5 und MVC 4 aktualisieren. Rick ist leitender Redakteur bei Microsoft mit Schwerpunkt auf Azure und MVC.
- [Rowan Miller](http://www.romiller.com) und anderen Mitgliedern des Entity Framework-Teams mit codereviews telefonischen und Half, viele Probleme mit Migrationen zu debuggen, die aufgetreten waren, während wir das Tutorial für EF 5 aktualisiert wurden.

## <a name="vb"></a>VB

Wenn Sie das Tutorial ursprünglich erstellt wurde, bereitgestellt wurde c# und VB-Versionen des Projekts Abgeschlossene Downloads. Mit diesem Update werden wir ein C#-herunterladbaren Projekt für die einzelnen Kapitel, um ihn leichter in der Reihe, aber aufgrund der Zeit-Einschränkungen und andere Dinge, die nicht für VB erfolgt an einer beliebigen Stelle Einstieg bereit Wenn Sie beim Erstellen eines VB-Projekts mithilfe dieser Lernprogramme und wäre, die für andere Benutzer freigeben, geben Sie uns möchte.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Fehler und Problemumgehungen

### <a name="cannot-createshadow-copy"></a>Kopieren kann nicht erstellt/Shadow werden

Fehlermeldung:

*Kann nicht erstellt/Shadow kopieren "DotNetOpenAuth.OpenId", wenn die Datei bereits vorhanden ist.*

Projektmappe:

Warten Sie einige Sekunden, und aktualisieren Sie die Seite.

### <a name="update-database-not-recognized"></a>Update-Database nicht erkannt

Fehlermeldung:

*Der Begriff "Update-Database" wird nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad richtig ist, und versuchen Sie es erneut.* (Aus der *`Update-Database`* Befehl in der PMC.)

Projektmappe:

Beenden Sie Visual Studio. Projekt erneut öffnen, und versuchen Sie es noch mal.

### <a name="validation-failed"></a>Fehler bei der Validierung

Fehlermeldung:

*Fehler bei der Überprüfung für eine oder mehrere Entitäten. Finden Sie unter 'EntityValidationErrors'-Eigenschaft für die weitere Details.* (Aus der *`Update-Database`* Befehl in der PMC.)

Projektmappe:

Eine Ursache für dieses Problem sind Überprüfungsfehler bei den `Seed` Methode ausgeführt wird. Finden Sie unter [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Tipps zum Debuggen der `Seed` Methode.

### <a name="http-50019-error"></a>HTTP 500.19 Fehler

Fehlermeldung:

*HTTP-Fehler 500.19 - Interner Serverfehler, die die angeforderte Seite kann nicht zugegriffen werden, weil die zugehörigen Konfigurationsdaten für die Seite ungültig ist.*

Projektmappe:

Eine Möglichkeit, die Sie diese Fehlermeldung ist über das Vorhandensein mehrerer Kopien der Lösung, jeweils die gleiche Portnummer verwenden. In der Regel können Sie dieses Problem beheben, indem Sie alle Instanzen von Visual Studio beenden und anschließend erneute Starten des Projekts mit Ihrer Arbeit an. Wenn dies nicht funktioniert, versuchen Sie, die Portnummer zu ändern. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und klicken Sie dann auf Eigenschaften. Wählen Sie die **Web** Registerkarte aus, und ändern Sie dann die Portnummer in das **Projekt-Url** Textfeld.

### <a name="error-locating-sql-server-instance"></a>Fehler beim Bestimmen der SQL Server-Instanz

Fehlermeldung:

*Ein netzwerkbezogener oder Instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden oder es konnte nicht auf ihn zugegriffen werden. Stellen Sie sicher, dass der Instanzname richtig ist und SQL Server für Remoteverbindungen konfiguriert ist. (Anbieter: SQL Network Interfaces, Fehler: 26 – Fehler beim Suchen von Server-Instanz angegeben.)*

Projektmappe:

Überprüfen Sie die Verbindungszeichenfolge. Wenn Sie die Datenbank manuell gelöscht haben, ändern Sie den Namen der Datenbank in der Konstruktionszeichenfolge.

> [!div class="step-by-step"]
> [Zurück](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Weiter](building-the-ef5-mvc4-chapter-downloads.md)
