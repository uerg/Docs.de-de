---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Erweiterte Szenarien für Entity Framework für ein MVC-Webanwendung (10 von 10) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 277503b65d9b75a9d3cc05538d5327f9367f45e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Erweiterte Entity Framework-Szenarien für ein MVC-Webanwendung (10 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm implementiert Sie das Repository und die Einheit der Arbeitsstruktur. In diesem Lernprogramm werden die folgenden Themen behandelt:

- Ausführen von unformatierten SQL-Abfragen.
- Ausführen von ohne-nachverfolgungsabfragen.
- Untersuchung von Abfragen an die Datenbank gesendet.
- Arbeiten mit Webdienstproxy-Klassen.
- Deaktivieren die automatischen Erkennung von Änderungen.
- Deaktivieren der Validierung beim Speichern der Änderungen.
- [Fehler und Scanning](#errors)

Für die meisten dieser Themen arbeiten Sie mit der Seiten, die Sie bereits erstellt haben. Um die unformatierten SQL verwenden, um massenaktualisierungen auszuführen erstellen Sie eine neue Seite, die die Anzahl der Gutschriften aller Kurse in der Datenbank aktualisiert:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Und um eine Abfrage keine Überwachung verwenden, fügen Sie neue Validierungslogik auf der Seite "Abteilung bearbeiten":

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Leistungsfähige unformatierten SQL-Abfragen

Der Entity Framework Code First-API enthält Methoden, die Ihnen ermöglichen, die SQL-Befehle direkt an die Datenbank übergeben. Sie haben folgende Möglichkeiten:

- Verwenden Sie die `DbSet.SqlQuery`-Methode für Abfragen, die Entitätstypen zurückgeben. Die zurückgegebenen Objekte muss mit der vom erwarteten Typ der `DbSet` -Objekt, und sie werden automatisch nachverfolgt vom Kontext Datenbank, wenn Sie die Überwachung deaktivieren. (Finden Sie im folgenden Abschnitt zu den `AsNoTracking` Methode.)
- Verwenden der `Database.SqlQuery` Methode für Abfragen, die Typen zurückgeben, die Entitäten nicht. Die zurückgegebenen Daten werden nicht vom Datenbankkontext nachverfolgt, auch wenn Sie diese Methode zum Abrufen von Entitätstypen verwenden.
- Verwenden der [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) für nichtabfragebefehle.

Einer der Vorteile von Entity Framework ist die Tatsache, dass vermieden wird, den Code zu eng an eine bestimmte Methode zum Speichern von Daten zu binden. Dies geschieht, indem SQL-Abfragen und -Befehle für Sie generiert werden. Somit müssen Sie sie nicht selbst schreiben. Aber herausragende Szenarien stehen, wenn müssen Sie bestimmte SQL-Abfragen ausführen, die Sie manuell erstellt haben, und diese Methoden können Sie solche Ausnahmen zu behandeln.

Dies trifft immer zu, wenn Sie SQL-Befehle in einer Webanwendung ausführen. Treffen Sie deshalb Vorsichtsmaßnahmen, um Ihre Website vor Angriffen durch Einschleusung von SQL-Befehlen zu schützen. Verwenden Sie dazu parametrisierte Abfragen, um sicherzustellen, dass Zeichenfolgen, die von einer Webseite übermittelt werden, nicht als SQL-Befehle interpretiert werden können. In diesem Tutorial verwenden Sie parametrisierte Abfragen beim Integrieren von Benutzereingaben in eine Abfrage.

### <a name="calling-a-query-that-returns-entities"></a>Aufrufen einer Abfrage, die zurückgegeben Entitäten

Angenommen, Sie möchten die `GenericRepository` Klasse, um zusätzliche filtern und Sortieren von Flexibilität ohne das Erstellen einer abgeleiteten Klasse durch zusätzliche Methoden bereitzustellen. Eine Möglichkeit, dies zu erreichen, wäre eine benutzerdefinierte Methode hinzuzufügen, die eine SQL-Abfrage akzeptiert. Sie können dann angeben, alle Arten von Filtern oder sortieren Sie möchten, in dem Controller an, wie z. B. eine `Where` -Klausel, die von einem Joins oder eine Unterabfrage abhängig ist. In diesem Abschnitt sehen Sie, wie eine solche Methode implementiert wird.

Erstellen der `GetWithRawSql` Methode, indem Sie den folgenden Code hinzufügen *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

In *CourseController.cs*, rufen Sie die neue Methode aus der `Details` Methode, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

In diesem Fall Sie hätte die `GetByID` -Methode, aber Sie verwenden die `GetWithRawSql` Methode, um zu überprüfen, ob die `GetWithRawSQL` Methode funktioniert.

Führen Sie die Seite Details, um sicherzustellen, dass die Select-funktioniert Abfrage (Wählen Sie die **Kurs** Registerkarte und dann **Details** für einen Kurs).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Aufrufen von einer Abfrage, zurückgibt andere Typen von Objekten

Sie haben zuvor ein Statistikraster für Studenten für die Infoseite erstellt, das die Anzahl der Studenten für jedes Anmeldedatum zeigt. Der Code, die dies in tut *HomeController.cs* verwendet LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Angenommen Sie, Code schreiben, mit der diese Daten direkt in SQL, anstatt mit LINQ abgerufen werden sollen. Hierzu müssen Sie beim Ausführen einer Abfrage, die einen anderen Wert als Entitätsobjekte zurückgibt, also müssen Sie verwenden die `Database.SqlQuery` Methode.

In *HomeController.cs*, ersetzen Sie die LINQ-Anweisung in der `About` -Methode durch folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Führen Sie die Seite "Info". Sie zeigt die gleichen Daten wie zuvor.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Aufrufen einer Update-Abfrage

Angenommen Sie, Sie möchten, dass Contoso University Administratoren in der Datenbank, z. B. das Ändern der Anzahl der Gutschriften für jeder Kurs massenänderungen ausführen kann. Wenn die Universität über eine große Anzahl an Kursen verfügt, wäre es ineffizient, sie alle als Entitäten abzurufen und separat zu ändern. In diesem Abschnitt implementieren Sie eine Webseite, die dem Benutzer ermöglicht, geben Sie einen Faktor, um die Anzahl der Gutschriften für alle Kurse ändern, und fügen Sie die Änderung vornehmen, durch das Ausführen einer SQL `UPDATE` Anweisung. Die Webseite wird wie die folgende Abbildung aussehen:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Im vorherigen Lernprogramm verwendet Sie das allgemeine Repository zum Lesen und aktualisieren `Course` Entitäten in der `Course` Controller. Für diesen Vorgang Bulk Update müssen Sie eine neue Repository-Methode erstellen, die nicht im generischen-Repository ist. Zu diesem Zweck erstellen Sie eine dedizierte `CourseRepository` von abgeleitete Klasse die `GenericRepository` Klasse.

In der *DAL* Ordner erstellen *CourseRepository.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

In *UnitOfWork.cs*, ändern Sie die `Course` Repository-Typ aus `GenericRepository<Course>` an `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

In *CourseContoller.cs*, Hinzufügen einer `UpdateCourseCredits` Methode:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Diese Methode wird für beide verwendet werden `HttpGet` und `HttpPost`. Wenn die `HttpGet` `UpdateCourseCredits` Methode ausgeführt wird, die `multiplier` Variable ist null, und die Ansicht wird ein leeres Textfeld und einer Schaltfläche "Absenden" angezeigt, wie in der vorherigen Abbildung dargestellt.

Wenn die **Update** geklickt wird und die `HttpPost` Methode ausgeführt wird, `multiplier` wird den Wert in das Textfeld eingegeben haben. Der Code ruft dann das Repository `UpdateCourseCredits` -Methode, die gibt die Anzahl der betroffenen Zeilen, und dieser Wert wird gespeichert, der `ViewBag` Objekt. Wenn die Sicht empfängt die Anzahl der betroffenen Zeilen in der `ViewBag` -Objekts dieser Zahl statt im Textfeld angezeigt und Schaltfläche "" zu senden, wie in der folgenden Abbildung gezeigt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Erstellen einer Ansicht in der *Views\Course* Ordner für die Kurs-Gutschriften Update-Seite:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

In *Views\Course\UpdateCourseCredits.cshtml*, den Code durch den folgenden Code ersetzen:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Führen Sie die `UpdateCourseCredits`-Methode aus, indem Sie die Registerkarte **Courses** (Kurse) auswählen, und dann „/UpdateCourseCredits“ am Ende der URL in die Adressleiste des Browsers einfügen (z.B.: `http://localhost:50205/Course/UpdateCourseCredits`). Geben Sie eine Zahl in das Textfeld ein:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klicken Sie auf **Aktualisieren**. Die Anzahl der betroffenen Zeilen wird angezeigt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klicken Sie auf **Zurück zur Liste**, um die Kursliste mit der geänderte Anzahl von Credits anzuzeigen.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Weitere Informationen zu unformatierten SQL-Abfragen finden Sie unter [unformatierten SQL-Abfragen](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) im Entity Framework-Team-Blog.

## <a name="no-tracking-queries"></a>No-Überwachungsabfragen

Wenn ein Datenbankkontext ruft Datenbankzeilen ab und erstellt von Entitätsobjekten, die sie darstellen, werden von nachverfolgt standardmäßig er, ob die Entitäten im Arbeitsspeicher synchron, sind was in der Datenbank ist. Die Daten im Arbeitsspeicher fungieren als Cache und werden verwendet, wenn Sie eine Entität aktualisieren. Diese Zwischenspeicherung ist in Webanwendungen oft überflüssig, da Kontextinstanzen in der Regel kurzlebig sind (eine neue wird bei jeder Anforderung erstellt und gelöscht), und der Kontext, der eine Entität liest, wird in der Regel gelöscht, bevor diese Entität erneut verwendet wird.

Sie können angeben, ob der Kontext Entitätsobjekten für eine Abfrage mit verfolgt die `AsNoTracking` Methode. Typische Szenarios, in denen Sie das möglicherweise tun wollen, umfassen Folgendes:

- Die Abfrage ruft solche eine große Datenmenge, die durch das Ausschalten Überwachung deutlich die Leistung steigern können.
- Sie eine Entität anfügen, um es zu aktualisieren möchten, aber Sie zuvor die gleiche Entität für einen anderen Zweck abgerufen. Da diese Entität bereits vom Datenbankkontext nachverfolgt wird, können Sie die zu ändernde Entität nicht anfügen. Eine Möglichkeit, dies zu verhindern ist die Verwendung der `AsNoTracking` Option mit der oben dargestellten Abfrage.

In diesem Abschnitt implementieren Sie Geschäftslogik, die die zweite dieser Szenarien veranschaulicht. Insbesondere müssen Sie eine Geschäftsregel erzwingen, die besagt, dass ein Kursleiter der Administrator von mehr als eine Abteilung sein kann.

In *DepartmentController.cs*, fügen Sie eine neue Methode, die Sie aufrufen können, aus der `Edit` und `Create` Methoden, um sicherzustellen, dass keine zwei Abteilungen derselben Administrator haben:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Fügen Sie Code in der `try` -Block die `HttpPost` `Edit` Methode, die dieser neuen Methode aufgerufen, wenn keine gültigkeitsprobleme bestehen. Die `try` Block sieht nun wie im folgenden Beispiel:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Führen Sie die Seite Abteilung zu bearbeiten, und versuchen Sie, eine Abteilung Administrator in einen Kursleiter zu ändern, der bereits der Administrator von einer anderen Abteilung ist. Sie erhalten die erwartete Fehlermeldung angezeigt:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Führen Sie jetzt die Seite Abteilung bearbeiten und dieser Zeit geändert die **Budget** Betrag. Beim Klicken auf **speichern**, eine Fehlerseite angezeigt:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Die Ausnahmefehlermeldung lautet "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Dies sind die folgenden Abfolge von Ereignissen:

- Die `Edit` Methodenaufrufe der `ValidateOneAdministratorAssignmentPerInstructor` -Methode, die alle Abteilungen abruft, die Kim Abercrombie als Administrator verfügen. Die bewirkt, dass die englische Abteilung gelesen werden. Da dies die Abteilung, die bearbeitet wird, wird kein Fehler gemeldet. Als Ergebnis von diesem Lesevorgang wird jedoch die englische Abteilung-Entität, die aus der Datenbank gelesen wurde nun durch den Datenbankkontext nachverfolgt.
- Die `Edit` Methode versucht, legen Sie die `Modified` -Flag auf Englisch Abteilung Entität erstellt wird, indem Sie die MVC-modellbindung, aber dies fehlschlägt, weil der Kontext eine Entität für die englischen Abteilung bereits nachverfolgt wird.

Eine Lösung für dieses Problem besteht darin, verhindert, dass den Kontext nachverfolgen Abteilung in-Memory-Entitäten, die von der überprüfungsabfrage abgerufen. Es gibt keine Nachteil dadurch, da Sie diese Entität wird nicht aktualisiert, oder Lesen sie erneut auf eine Weise, die von ihm im Arbeitsspeicher zwischengespeichert profitieren würden.

In *DepartmentController.cs*in der `ValidateOneAdministratorAssignmentPerInstructor` -Methode, geben Sie keine Überwachung aus, wie im folgenden gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Wiederholen Sie versucht haben, bearbeiten Sie die **Budget** Betrag einer Abteilung. Dieses Mal der Vorgang erfolgreich ist und der Standort gibt, wie erwartet auf den Abteilungen Indexseite, zeigt die überarbeitete Budget-Wert.

## <a name="examining-queries-sent-to-the-database"></a>Untersuchung von Abfragen an die Datenbank gesendet

Manchmal ist es hilfreich, die tatsächlichen SQL-Abfragen anzuzeigen, die an die Datenbank gesendet werden. Zu diesem Zweck können Sie eine Abfragevariable im Debugger überprüfen oder rufen Sie der Abfrage `ToString` Methode. Um dies zu testen, müssen Sie betrachten Sie eine einfache Abfrage und betrachten, was darauf, geschieht wie Sie solche Eager laden, Filtern und Sortieren von Optionen hinzufügen.

In *Controller/CourseController*, ersetzen Sie die `Index` -Methode durch folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Nun legen Sie einen Haltepunkt *GenericRepository.cs* auf die `return query.ToList();` und die `return orderBy(query).ToList();` Anweisungen von der `Get` Methode. Führen Sie das Projekt im Debugmodus befindet, und wählen Sie den Kurs Indexseite. Wenn der Code den Haltepunkt erreicht, untersuchen die `query` Variable. Daraufhin wird die Abfrage, die mit SQL Server gesendet wird. Es ist eine einfache `Select` Anweisung:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Abfragen können zu viel Zeit in die Debugfenster in Visual Studio angezeigt werden. Um die gesamte Abfrage angezeigt wird, können Sie kopieren Sie den Wert den Variablen, und fügen Sie ihn in einem Text-Editor:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Nun fügen Sie eine Dropdown-Liste auf den Kurs Indexseite, damit Benutzer nach einer bestimmten Abteilung filtern können. Sortieren Sie die Kurse nach Titel, und geben Sie unverzüglichem Laden für die `Department` Navigationseigenschaft. In *CourseController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Die Methode erhält, den ausgewählten Wert aus der Dropdown-Liste in die `SelectedDepartment` Parameter. Wenn nichts ausgewählt ist, wird dieser Parameter null sein.

Ein `SelectList` Auflistung mit allen Abteilungen ist für die Dropdown-Liste an die Ansicht übergeben. Die Parameter zu übergeben, um die `SelectList` Konstruktor angeben, den Wert Feldnamen, den Feldnamen Text und das ausgewählte Element.

Für die `Get` Methode der `Course` -Repository der Code gibt einen Filterausdruck, Sortierreihenfolge und Eager loading für die `Department` Navigationseigenschaft. Gibt der Filterausdruck immer `true` Wenn nichts in der Dropdown-Liste ausgewählt ist (d. h. `SelectedDepartment` ist null).

In *Views\Course\Index.cshtml*, unmittelbar vor dem Öffnen `table` kennzeichnen, fügen Sie den folgenden Code zum Erstellen der Dropdown Liste und eine Schaltfläche "Absenden":

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Mit weiterhin Haltepunkte in der `GenericRepository` -Klasse, führen Sie die Seite für die Kurs-Index. Folgen Sie den ersten zwei Mal, dass der Code einen Haltepunkt trifft, damit die Seite im Browser angezeigt wird. Wählen Sie eine Abteilung aus der Dropdown-Liste, und klicken Sie auf **Filter**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Dieses Mal wird der erste Haltepunkt für die Abteilungen-Abfrage für die Dropdown-Liste. Überspringen, und zeigen Sie an der `query` Variablen das nächste Mal im Code erreicht den Breakpoint um sehen die `Course` Abfragen nun sieht wie. Sie sehen in etwa Folgendes:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Sehen Sie, dass jetzt die Abfrage ist ein `JOIN` Abfrage, die lädt `Department` Daten zusammen mit der `Course` Daten, und es enthält eine `WHERE` Klausel.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Arbeiten mit Webdienstproxy-Klassen

Wenn Entity Framework erstellt Instanzen der Entität (z. B. beim Ausführen einer Abfrage), berechnet er häufig als Instanzen eines dynamisch generierten abgeleiteten Typs, der als Proxy für die Entität fungiert. Dieser Proxy überschreibt einige virtuelle Eigenschaften der Entität einzufügende Hooks für Aktionen automatisch ausgeführt, wenn die Eigenschaft zugegriffen wird. Beispielsweise wird dieser Mechanismus zur Unterstützung von lazy Loading für Beziehungen verwendet.

In den meisten Fällen müssen Sie nicht diese Verwendung von Proxys bewusst sein, aber es gibt jedoch Ausnahmen:

- In einigen Szenarien empfiehlt es sich um zu verhindern, dass das Entity Framework Proxyinstanzen erstellen. Serialisieren von nicht-Proxyinstanzen kann beispielsweise effizienter als die Serialisierung Proxyinstanzen sein.
- Beim Instanziieren einer Entität mithilfe der `new` Operator nicht erhalten Sie eine Proxyinstanz. Dies bedeutet, dass Sie keine Funktionen nutzen, z. B. verzögertes Laden und automatische änderungsnachverfolgung. Dies ist in der Regel sind; im Allgemeinen nicht erforderlich, lazy loading, da Sie eine neue Entität erstellen, die nicht in der Datenbank und änderungsnachverfolgung, wenn Sie die Entität als explizite Markierung sind im Allgemeinen nicht erforderlich, `Added`. Jedoch wenn verzögertes Laden ist erforderlich, und das Nachverfolgen von Änderungen benötigen, können Sie erstellen neue Instanzen der Entität mit Proxys, die mithilfe der `Create` Methode der `DbSet` Klasse.
- Möglicherweise möchten einen Proxytyp tatsächlichen Entitätstyp entnommen werden. Können Sie die `GetObjectType` Methode der `ObjectContext` Klasse, um den tatsächlichen Entitätstyp, der eine Instanz eines Datentyps Proxy abzurufen.

Weitere Informationen finden Sie unter [arbeiten mit Proxys](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) im Entity Framework-Team-Blog.

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

Wenn Sie eine große Anzahl von Entitäten überwachen und einer dieser Methoden oft in einer Schleife aufrufen, erhalten Sie möglicherweise erhebliche Leistungssteigerungen durch vorübergehendes Deaktivieren der automatischen Änderung zustandserkennung mithilfe der [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) Eigenschaft. Weitere Informationen finden Sie unter [automatisch erkennen von Änderungen](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Deaktivieren der Validierung beim Speichern der Änderungen

Beim Aufrufen der `SaveChanges` -Methode, wird standardmäßig das Entity Framework überprüft die Daten in alle Eigenschaften aller geänderten Entitäten vor dem Aktualisieren der Datenbank. Wenn Sie eine große Anzahl von Entitäten aktualisiert haben und Sie haben bereits überprüft die Daten dieser Aufwand ist nicht erforderlich, und Sie die das Speichern womöglich werden die Änderungen durch Deaktivieren der Validierung vorübergehend weniger Zeit benötigt. Sie erreichen, dass die Verwendung der [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) Eigenschaft. Weitere Informationen finden Sie unter [Überprüfung](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Zusammenfassung

Dies schließt diese Reihe von Lernprogramme zum Verwenden von Entity Framework in einer ASP.NET MVC-Anwendung. Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

Weitere Informationen dazu, wie Sie Ihre Webanwendung bereitstellen, nachdem Sie es erstellt haben, finden Sie unter [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx) in der MSDN Library.

Weitere Informationen zu anderen Themen im Zusammenhang mit MVC, wie beispielsweise Authentifizierung und Autorisierung, finden Sie unter der [MVC empfohlen Ressourcen](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Danksagungen

- Tom Dykstra geschrieben hat die ursprüngliche Version des Lernprogramms und einen senior programming Writer auf dem Microsoft-Webplattform und Tools Inhaltsteam ist.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) in diesem Lernprogramm Co-Autor und wurde der Großteil der Arbeit aktualisieren sie 5 EF und MVC 4. Rick ist ein senior programming Writer zum Microsoft Azure und MVC konzentrieren.
- [Rowan Miller](http://www.romiller.com) und anderen Mitgliedern des Teams Entity Framework unterstützt mit codereviews und-Abgleich dabei behilflich waren viele Probleme bei Migrationen zu debuggen, die aufgetreten ist, während wir das Lernprogramm für EF 5 aktualisiert wurden.

## <a name="vb"></a>VB

Bei das Lernprogramm ursprünglich erstellt wurde, bereitgestellt wir c# und VB-Versionen des Projekts abgeschlossenen Download an. Mit diesem Update bieten wir Ihnen ein C#-herunterladbaren Projekt für die einzelnen Kapitel, damit sie leichter in der Reihe, aber aufgrund der Zeit-Einschränkungen und andere Prioritäten wir, die für VB dar. nicht an einer beliebigen Stelle Einstieg Wenn Sie beim Erstellen eines VB-Projekts mit diesen Lernprogrammen und wäre, die andere Personen freigeben, informieren Sie uns in Kauf zu nehmen.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Fehler und Problemumgehungen

### <a name="cannot-createshadow-copy"></a>Kopieren kann nicht erstellt/Schatten werden

Fehlermeldung:

*Kann nicht erstellt/Schatten kopieren "DotNetOpenAuth.OpenId", wenn diese Datei bereits vorhanden ist.*

Projektmappe:

Warten Sie einige Sekunden, und aktualisieren Sie die Seite.

### <a name="update-database-not-recognized"></a>Update-Database nicht erkannt.

Fehlermeldung:

*Der Begriff 'Update-Database' ist nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad korrekt ist, und versuchen Sie es erneut.* (Aus der *`Update-Database`* -Befehl in der PMC.)

Projektmappe:

Beenden Sie Visual Studio. Öffnen Sie Projekt erneut, und wiederholen Sie den Vorgang.

### <a name="validation-failed"></a>Fehler bei der Überprüfung

Fehlermeldung:

*Fehler bei der Validierung für eine oder mehrere Entitäten. Finden Sie unter 'EntityValidationErrors'-Eigenschaft für weitere Details.* (Aus der *`Update-Database`* -Befehl in der PMC.)

Projektmappe:

Eine Ursache für dieses Problem sind Überprüfungsfehler bei den `Seed` Methode ausgeführt wird. Finden Sie unter [Seeding und Debuggen von Entity Framework (EF) Datenbanken](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Tipps zum Debuggen der `Seed` Methode.

### <a name="http-50019-error"></a>HTTP 500.19 Fehler

Fehlermeldung:

*HTTP-Fehler 500.19 - Interner Serverfehler die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.*

Projektmappe:

Eine Möglichkeit, die Sie diese Fehlermeldung erhalten, können wird vom Vorhandensein mehrerer Kopien der Lösung, jede von ihnen die gleiche Portnummer verwenden. In der Regel können Sie dieses Problem lösen, indem Sie alle Instanzen von Visual Studio beenden und Neustarten des Projekts auf dem Sie arbeiten. Wenn dies nicht funktioniert, versuchen Sie es ändern der Portnummer. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und klicken Sie dann auf Eigenschaften. Wählen Sie die **Web** Registerkarte, und ändern Sie dann die Portnummer in das **Projekt-Url** Textfeld.

### <a name="error-locating-sql-server-instance"></a>Fehler beim Bestimmen der SQL Server-Instanz

Fehlermeldung:

*Ein netzwerkbezogener oder Instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden oder es konnte nicht auf ihn zugegriffen werden. Stellen Sie sicher, dass der Instanzname richtig ist und dass SQL Server konfiguriert ist, um Remoteverbindungen zuzulassen. (Anbieter: SQL Network Interfaces, Fehler: 26 - Fehler beim Suchen von Server-Instanz angegeben.)*

Projektmappe:

Überprüfen Sie die Verbindungszeichenfolge. Wenn Sie die Datenbank manuell gelöscht haben, ändern Sie den Namen der Datenbank in der Konstruktionszeichenfolge.

> [!div class="step-by-step"]
> [Zurück](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Weiter](building-the-ef5-mvc4-chapter-downloads.md)
