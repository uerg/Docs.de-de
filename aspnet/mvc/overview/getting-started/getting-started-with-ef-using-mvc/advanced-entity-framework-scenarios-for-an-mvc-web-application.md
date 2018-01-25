---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Erweiterte Szenarien für Entity Framework 6 für ein MVC 5-Web-Anwendung (12 12) | Microsoft Docs"
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 85276377671b96e65406639c8584d9ebf8d77ff7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Erweiterte Entity Framework 6 Szenarien für ein MVC 5-Web-Anwendung (12 12)
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm implementiert Sie Tabelle pro Hierarchie Vererbung. Dieses Lernprogramm enthält verschiedene Themen, in denen sind nützlich sein, wenn Sie die Grundlagen des Entwickelns von PHP-Webanwendungen, die mithilfe von Entity Framework Code First hinausgehen beachten führt. Schrittweise Anweisungen führt Sie durch den Code und mithilfe von Visual Studio für die folgenden Themen:

- [Ausführen von unformatierten SQL-Abfragen](#rawsql)
- [Ausführen von Abfragen keine Überwachung](#notracking)
- [Untersuchen von SQL an die Datenbank gesendet](#sql)

Das Lernprogramm führt mehrere Themen mit kurze Einführungen gefolgt von Links und Ressourcen für Weitere Informationen:

- [Repository und die Einheit der Arbeit-Muster](#repo)
- [Proxyklassen](#proxies)
- [Automatische änderungserkennung](#changedetection)
- [Automatische Validierung](#validation)
- [EF-Tools für Visual Studio](#tools)
- [Entity Framework-Quellcodes](#source)

Das Lernprogramm enthält auch die folgenden Abschnitte:

- [Zusammenfassung](#summary)
- [Bestätigungen](#acknowledgments)
- [Ein Hinweis zum VB](#vb)
- [Häufige Fehler und -Lösungen oder problemumgehungen für Sie](#errors)

Für die meisten dieser Themen arbeiten Sie mit der Seiten, die Sie bereits erstellt haben. Um die unformatierten SQL verwenden, um massenaktualisierungen auszuführen erstellen Sie eine neue Seite, die die Anzahl der Gutschriften aller Kurse in der Datenbank aktualisiert:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Leistungsfähige unformatierten SQL-Abfragen

Der Entity Framework Code First-API enthält Methoden, die Ihnen ermöglichen, die SQL-Befehle direkt an die Datenbank übergeben. Sie haben folgende Möglichkeiten:

- Verwenden der [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) Methode zum Abfragen, die Entitätstypen zurückgeben. Die zurückgegebenen Objekte muss mit der vom erwarteten Typ der `DbSet` -Objekt, und sie werden automatisch nachverfolgt vom Kontext Datenbank, wenn Sie die Überwachung deaktivieren. (Finden Sie im folgenden Abschnitt zu den [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) Methode.)
- Verwenden der [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) Methode für Abfragen, die Typen zurückgeben, die Entitäten nicht. Die zurückgegebenen Daten wird nicht von den Datenbankkontext nachverfolgt, auch wenn Sie diese Methode zum Abrufen von Entitätstypen verwenden.
- Verwenden der [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) für nichtabfragebefehle.

Einer der Vorteile der Verwendung von Entity Framework ist, dass diese vermieden wird, den Code zu viel Wert auf eine bestimmte Methode zum Speichern von Daten zu binden. Dies geschieht durch Generieren von SQL-Abfragen und Befehle, die auch Sie freigibt, müssen sie selbst schreiben. Aber herausragende Szenarien stehen, wenn müssen Sie bestimmte SQL-Abfragen ausführen, die Sie manuell erstellt haben, und diese Methoden können Sie solche Ausnahmen zu behandeln.

Wie immer "true" ist, wenn Sie SQL-Befehle in einer Webanwendung ausführen, müssen Sie Vorsichtsmaßnahmen treffen, Ihre Website vor SQL Injection-Angriffen schützen werden. Eine Möglichkeit dazu besteht darin parametrisierte Abfragen zu verwenden, um sicherzustellen, dass Zeichenfolgen, die von einer Webseite übermittelten als SQL-Befehle interpretiert werden können. In diesem Lernprogramm verwenden Sie parametrisierte Abfragen beim Integrieren von Benutzereingaben in einer Abfrage.

### <a name="calling-a-query-that-returns-entities"></a>Aufrufen einer Abfrage, die zurückgegeben Entitäten

Die [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) -Klasse stellt eine Methode, die Sie verwenden können, zum Ausführen einer Abfrage, die eine Entität des Typs zurückgibt `TEntity`. Zu sehen, wie dies funktioniert ändern den Code in der `Details` Methode der `Department` Controller.

In *DepartmentController.cs*in der `Details` -Methode, ersetzen die `db.Departments.FindAsync` Methodenaufruf mit einem `db.Departments.SqlQuery` -Methodenaufruf, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Um sicherzustellen, dass der neue Code ordnungsgemäß funktioniert, wählen Sie die **Abteilungen** Registerkarte und dann **Details** für eines der Abteilungen.

![Department-Details](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Aufrufen von einer Abfrage, zurückgibt andere Typen von Objekten

Zuvor haben Sie ein Student statistikraster für die Info-Seite, die die Anzahl der Schüler für jede Registrierungsdatum ergab erstellt. Der Code, die dies in tut *HomeController.cs* verwendet LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Angenommen Sie, Code schreiben, mit der diese Daten direkt in SQL, anstatt mit LINQ abgerufen werden sollen. Hierzu müssen Sie beim Ausführen einer Abfrage, die einen anderen Wert als Entitätsobjekte zurückgibt, also müssen Sie verwenden die [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) Methode.

In *HomeController.cs*, ersetzen Sie die LINQ-Anweisung in die `About` Methode mit einer SQL-Anweisung, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Führen Sie die Seite "Info". Es zeigt die gleichen Daten wie zuvor.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Aufrufen einer Update-Abfrage

Angenommen Sie, Sie möchten, dass Contoso University Administratoren in der Datenbank, z. B. das Ändern der Anzahl der Gutschriften für jeder Kurs massenänderungen ausführen kann. Verfügt die Universität eine große Anzahl von Kurse, wäre es ineffizient, diese als Entitäten abrufen und ändern Sie sie einzeln. In diesem Abschnitt implementieren Sie eine Webseite, die dem Benutzer ermöglicht, geben Sie einen Faktor, um die Anzahl der Gutschriften für alle Kurse ändern, und fügen Sie die Änderung vornehmen, durch das Ausführen einer SQL `UPDATE` Anweisung. Die Webseite wird wie die folgende Abbildung aussehen:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

In *CourseContoller.cs*, hinzufügen `UpdateCourseCredits` Methoden für `HttpGet` und `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Wenn der Controller verarbeitet ein `HttpGet` Anforderung, nothing wird zurückgegeben, der `ViewBag.RowsAffected` Variable und die Ansicht zeigt ein leeres Textfeld und eine Schaltfläche "Absenden" wie in der vorherigen Abbildung dargestellt.

Wenn die **Update** Schaltfläche geklickt wird, die `HttpPost` -Methode aufgerufen wird, und `multiplier` hat den Wert in das Textfeld eingegeben. Der Code führt dann die SQL, die Kurse aktualisiert und gibt die Anzahl der betroffenen Zeilen in der Sicht in der `ViewBag.RowsAffected` Variable. Wenn die Sicht, ruft einen Wert ab Variable, es zeigt die Anzahl der Zeilen, die aktualisiert werden, anstatt das Textfeld und Senden-Schaltfläche, wie in der folgenden Abbildung gezeigt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

In *CourseController.cs*, mit der rechten Maustaste eine der der `UpdateCourseCredits` Methoden, und klicken Sie dann auf **Ansicht hinzufügen**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

In *Views\Course\UpdateCourseCredits.cshtml*, den Code durch den folgenden Code ersetzen:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Führen Sie die `UpdateCourseCredits` Methode dazu die **Kurse** Registerkarte, klicken Sie dann hinzufügen "/ UpdateCourseCredits" am Ende der URL in die Adressleiste des Browsers (z. B.: `http://localhost:50205/Course/UpdateCourseCredits`). Geben Sie eine Zahl in das Textfeld ein:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klicken Sie auf **Aktualisieren**. Daraufhin wird die Anzahl der betroffenen Zeilen:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klicken Sie auf **zurück zur Listenansicht** um die Liste der Kurse mit der geänderte Anzahl von Gutschriften anzuzeigen.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Weitere Informationen zu unformatierten SQL-Abfragen finden Sie unter [unformatierten SQL-Abfragen](https://msdn.microsoft.com/data/jj592907) auf MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>No-Überwachungsabfragen

Wenn ein Datenbankkontext Ruft die Tabellenzeilen ab und erstellt von Entitätsobjekten, die sie darstellen, werden von nachverfolgt standardmäßig er, ob die Entitäten im Arbeitsspeicher synchron, sind was in der Datenbank ist. Die Daten im Arbeitsspeicher als Cache fungiert und werden verwendet, wenn Sie eine Entität aktualisieren. Dieses Zwischenspeichern ist häufig in einer Webanwendung nicht erforderlich, da Kontext Instanzen sind in der Regel kurzlebige (ein neuer Schlüssel wird erstellt und freigegeben für jede Anforderung) sowie den Kontext, liest eine Entität ist in der Regel verworfen werden, bevor diese Entität erneut verwendet wird.

Sie können die Überwachung von Entitätsobjekten im Arbeitsspeicher deaktivieren, mit der [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) Methode. Typische Szenarios, in denen Sie möchten, die, umfassen Folgendes:

- Eine Abfrage ruft solche eine große Datenmenge, die durch das Ausschalten Überwachung deutlich die Leistung steigern können.
- Sie eine Entität anfügen, um es zu aktualisieren möchten, aber Sie zuvor die gleiche Entität für einen anderen Zweck abgerufen. Da die Entität bereits vom Kontext Datenbank nachverfolgt wird, können Sie die Entität nicht anfügen, die Sie ändern möchten. Eine Möglichkeit für diese Situation ist die Verwendung der `AsNoTracking` Option mit der oben dargestellten Abfrage.

Ein Beispiel für die Verwendung der [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) -Methode finden Sie unter [die frühere Version dieses Lernprogramms](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Diese Version des Lernprogramms nicht das Flag "geändert" auf eine Entität Binder erstellte Modell in der Methode bearbeiten festlegen, damit sie nicht benötigt `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Untersuchen von SQL an die Datenbank gesendet

Manchmal ist es hilfreich, in der Lage, um den tatsächlichen SQL-Abfragen anzuzeigen, die an die Datenbank gesendet werden. In einer früheren Lernprogramm haben Sie gesehen, wie dies im Code der Interceptor; Jetzt sehen Sie einige Möglichkeiten zu diesem Zweck ohne Interceptor Code zu schreiben. Um dies zu testen, müssen Sie betrachten Sie eine einfache Abfrage und betrachten, was darauf, geschieht wie Sie solche Eager laden, Filtern und Sortieren von Optionen hinzufügen.

In *Controller/CourseController*, ersetzen Sie die `Index` Methode durch den folgenden Code, um unverzüglichem Laden vorübergehend zu beenden:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Nun legen Sie einen Haltepunkt auf der `return` Anweisung (mit dem Cursor in dieser Zeile F9). Drücken Sie F5, um das Projekt im Debugmodus ausführen, und wählen den Kurs Indexseite. Wenn der Code den Haltepunkt erreicht, untersuchen die `sql` Variable. Daraufhin wird die Abfrage, die mit SQL Server gesendet wird. Es ist eine einfache `Select` Anweisung.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Klicken Sie auf die vergrößern Klasse aus, um die Abfrage in finden Sie unter der **Text-Schnellansicht**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Nun fügen Sie eine Dropdown-Liste auf die Indexseite Kurse, damit Benutzer nach einer bestimmten Abteilung filtern können. Sortieren Sie die Kurse nach Titel, und geben Sie unverzüglichem Laden für die `Department` Navigationseigenschaft.

In *CourseController.cs*, ersetzen Sie die `Index` -Methode durch folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Stellen Sie den Haltepunkt wieder her, auf die `return` Anweisung.

Die Methode erhält, den ausgewählten Wert aus der Dropdown-Liste in die `SelectedDepartment` Parameter. Wenn nichts ausgewählt ist, wird dieser Parameter null sein.

Ein `SelectList` Auflistung mit allen Abteilungen ist für die Dropdown-Liste an die Ansicht übergeben. Die Parameter zu übergeben, um die `SelectList` Konstruktor angeben, den Wert Feldnamen, den Feldnamen Text und das ausgewählte Element.

Für die `Get` Methode der `Course` -Repository der Code gibt einen Filterausdruck, Sortierreihenfolge und Eager loading für die `Department` Navigationseigenschaft. Gibt der Filterausdruck immer `true` Wenn nichts in der Dropdown-Liste ausgewählt ist (d. h. `SelectedDepartment` ist null).

In *Views\Course\Index.cshtml*, unmittelbar vor dem Öffnen `table` kennzeichnen, fügen Sie den folgenden Code zum Erstellen der Dropdown Liste und eine Schaltfläche "Absenden":

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Der Haltepunkt festgelegt, und führen Sie die Seite für die Kurs-Index. Über das erste Mal, dass der Code einen Haltepunkt trifft fortgesetzt werden, damit die Seite im Browser angezeigt wird. Wählen Sie eine Abteilung aus der Dropdown-Liste, und klicken Sie auf **Filter**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Dieses Mal wird der erste Haltepunkt für die Abteilungen-Abfrage für die Dropdown-Liste. Überspringen, und zeigen Sie an der `query` Variablen das nächste Mal im Code erreicht den Breakpoint um sehen die `Course` Abfragen nun sieht wie. Sie sehen in etwa Folgendes:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Sehen Sie, dass jetzt die Abfrage ist ein `JOIN` Abfrage, die lädt `Department` Daten zusammen mit der `Course` Daten, und es enthält eine `WHERE` Klausel.

Entfernen Sie die `var sql = courses.ToString()` Zeile.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repository und die Einheit der Arbeit-Muster

Viele Entwickler schreiben Code, um das Repository und die Einheit der Arbeit Muster als Wrapper um Code implementieren, die mit dem Entity Framework funktioniert. Diese Muster sollen eine Abstraktionsebene zwischen der Datenzugriffsebene und den Geschäftslogikschicht einer Anwendung zu erstellen. Diese Muster implementieren, um die Anwendung von Änderungen im Datenspeicher zu isolieren und Endprodukts automatisierte Komponententests oder eine testgesteuerte Entwicklung (TDD). Schreiben zusätzlichen Code zum Implementieren dieser Muster ist jedoch nicht immer die beste Wahl für Anwendungen mit EF, verschiedene Ursachen:

- Die Klasse der EF-Kontext selbst isoliert Codes von Store-datenspezifische Code.
- Die EF-Context-Klasse kann fungieren, als eine Arbeitseinheit Klasse für Datenbank-updates, die Sie mithilfe von EF ausführen.
- Funktionen von Entity Framework 6 vereinfachen die testgesteuerte Entwicklung zu implementieren, ohne Repository Code zu schreiben.

Weitere Informationen dazu, wie Sie das Repository und die Einheit der Arbeit Muster zu implementieren, finden Sie unter [die Entity Framework 5 Version dieser Reihe von Lernprogrammen](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Informationen zu Methoden zum TDD in Entity Framework 6 implementieren finden Sie unter den folgenden Ressourcen:

- [Wie EF6 Mocking DbSets leichter ermöglicht](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Ein pseudoframework testen](https://msdn.microsoft.com/data/dn314429)
- [Tests mit Ihren eigenen Testdoubles](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Proxyklassen

Wenn Entity Framework erstellt Instanzen der Entität (z. B. beim Ausführen einer Abfrage), berechnet er häufig als Instanzen eines dynamisch generierten abgeleiteten Typs, der als Proxy für die Entität fungiert. Beispielsweise finden Sie unter den folgenden beiden Abbilder im Debugger. In der ersten Abbildung angezeigt, die die `student` Variable ist der erwartete `Student` Geben Sie sofort, nachdem Sie die Entität instanziieren. Nachdem EF zum Lesen einer Student-Entität aus der Datenbank verwendet wurde, sehen Sie in das zweite Bild die Proxy-Klasse.

![Vor dem Proxy-Klasse](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Nach der Proxy-Klasse](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Dieser Proxyklasse überschreibt einige virtuelle Eigenschaften der Entität einzufügende Hooks für Aktionen automatisch ausgeführt, wenn die Eigenschaft zugegriffen wird. Eine Funktion, der für diesen Mechanismus verwendet wird, ist verzögertes Laden.

In den meisten Fällen müssen Sie nicht diese Verwendung von Proxys bewusst sein, aber es gibt jedoch Ausnahmen:

- In einigen Szenarien empfiehlt es sich um zu verhindern, dass das Entity Framework Proxyinstanzen erstellen. Beim Serialisieren von Entitäten sind möchten Sie z. B. in der Regel POCO-Klassen, die Webdienstproxy-Klassen. Eine Möglichkeit zur Vermeidung von Problemen der Serialisierung wird zum Serialisieren von datenübertragungsobjekte (DTOs) anstelle von Entitätsobjekten, entsprechend der [mithilfe des Web-API mit Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) Lernprogramm. Eine andere Möglichkeit besteht darin [deaktivieren Proxyerstellung](https://msdn.microsoft.com/data/jj592886.aspx).
- Beim Instanziieren einer Entität mithilfe der `new` Operator nicht erhalten Sie eine Proxyinstanz. Dies bedeutet, dass Sie keine Funktionen nutzen, z. B. verzögertes Laden und automatische änderungsnachverfolgung. Dies ist in der Regel sind; im Allgemeinen nicht erforderlich, lazy loading, da Sie eine neue Entität erstellen, die nicht in der Datenbank und änderungsnachverfolgung, wenn Sie die Entität als explizite Markierung sind im Allgemeinen nicht erforderlich, `Added`. Jedoch wenn verzögertes Laden ist erforderlich, und das Nachverfolgen von Änderungen benötigen, können Sie erstellen neue Instanzen der Entität mit Proxys, die mithilfe der [erstellen](https://msdn.microsoft.com/library/gg679504.aspx) Methode der `DbSet` Klasse.
- Möglicherweise möchten einen Proxytyp tatsächlichen Entitätstyp entnommen werden. Können Sie die [GetObjectType hat](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) Methode der `ObjectContext` Klasse, um den tatsächlichen Entitätstyp, der eine Instanz eines Datentyps Proxy abzurufen.

Weitere Informationen finden Sie unter [arbeiten mit Proxys](https://msdn.microsoft.com/data/JJ592886.aspx) auf MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Automatische änderungserkennung

Entity Framework bestimmt wie eine Entität geändert wurde (und daher die Updates an die Datenbank gesendet werden müssen), indem Sie die aktuellen Werte von einer Entität mit den ursprünglichen Werten vergleichen. Die ursprünglichen Werte werden gespeichert, wenn die Entität abgefragt oder angefügt wird. Einige der Methoden, die dazu führen, änderungserkennung für automatische dass lauten wie folgt:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Wenn Sie eine große Anzahl von Entitäten überwachen und einer dieser Methoden oft in einer Schleife aufrufen, erhalten Sie möglicherweise erhebliche Leistungssteigerungen durch vorübergehendes Deaktivieren der automatischen Änderung zustandserkennung mithilfe der [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) Eigenschaft. Weitere Informationen finden Sie unter [automatisch erkennen von Änderungen](https://msdn.microsoft.com/data/jj556205) auf MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Automatische Validierung

Beim Aufrufen der `SaveChanges` -Methode, wird standardmäßig das Entity Framework überprüft die Daten in alle Eigenschaften aller geänderten Entitäten vor dem Aktualisieren der Datenbank. Wenn Sie eine große Anzahl von Entitäten aktualisiert haben und Sie haben bereits überprüft die Daten dieser Aufwand ist nicht erforderlich, und Sie die das Speichern womöglich werden die Änderungen durch Deaktivieren der Validierung vorübergehend weniger Zeit benötigt. Sie erreichen, dass die Verwendung der [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) Eigenschaft. Weitere Informationen finden Sie unter [Überprüfung](https://msdn.microsoft.com/data/gg193959) auf MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework-Powertools

[Entity Framework-Powertools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) ist ein Visual Studio-add-in, mit denen die Daten Modelldiagramme erstellt wurde in diesen Lernprogrammen vorgestellten. Die Tools können auch andere Funktion durchführen, z. B. das Generieren von Entitätsklassen auf Grundlage der Tabellen in einer vorhandenen Datenbank, damit Sie die Datenbank mit Code First verwenden können. Nachdem Sie die Tools installieren, können Sie einige zusätzlichen Optionen in Kontextmenüs angezeigt. Z. B. Wenn Sie mit der rechten Maustaste in Ihre Kontextklasse **Projektmappen-Explorer**, erhalten Sie eine Option aus, um ein Diagramm zu generieren. Wenn Sie Code First verwenden Sie das Datenmodell in das Diagramm können nicht geändert werden, aber Sie können Elemente verschieben, um verstehen zu vereinfachen.

![EF im Kontextmenü](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF-Diagramm](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework-Quellcodes

Der Quellcode für Entity Framework 6 finden Sie unter [GitHub](https://github.com/aspnet/EntityFramework6). Melden Sie den Fehler, und Sie können auch eigene Erweiterungen auf den Quellcode von EF beitragen.

Obwohl der Quellcode geöffnet ist, wird die Entity Framework als ein Produkt von Microsoft vollständig unterstützt. Das Microsoft Entity Framework-Team verfolgt die Kontrolle über die Beiträge akzeptiert werden und testet alle Änderungen am Code zum Gewährleisten der Qualität von jeder Version.

<a id="summary"></a>
## <a name="summary"></a>Zusammenfassung

Dies schließt diese Reihe von Lernprogramme zum Verwenden von Entity Framework in einer ASP.NET MVC-Anwendung. Weitere Informationen zum Arbeiten mit Daten, die mit dem Entity Framework finden Sie unter der [EF-Dokumentationsseite auf MSDN](https://msdn.microsoft.com/data/ee712907) und [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

Weitere Informationen dazu, wie Sie Ihre Webanwendung bereitstellen, nachdem Sie es erstellt haben, finden Sie unter [ASP.NET Web-Bereitstellung – Ressourcen empfohlen](../../../../whitepapers/aspnet-web-deployment-content-map.md) in der MSDN Library.

Weitere Informationen zu anderen Themen im Zusammenhang mit MVC, wie beispielsweise Authentifizierung und Autorisierung, finden Sie unter der [ASP.NET MVC - Ressourcen empfohlen](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Bestätigungen

- Tom Dykstra geschrieben hat die ursprüngliche Version dieses Lernprogramms, Co-Autor EF 5-Aktualisierung und wurde das Update 6 EF geschrieben. Peter ist ein senior programming Writer auf dem Microsoft-Webplattform und Tools Inhaltsteam.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) wurde der Großteil der Arbeit aktualisieren das Lernprogramm für 5 EF und MVC 4 und Co-Autor der EF-6-Update. Rick ist ein senior programming Writer zum Microsoft Azure und MVC konzentrieren.
- [Rowan Miller](http://www.romiller.com) und anderen Mitgliedern des Teams Entity Framework unterstützt mit codereviews und-Abgleich dabei behilflich waren viele Probleme bei Migrationen zu debuggen, die aufgetreten ist, während wir das Lernprogramm für EF 5 und 6 EF aktualisiert wurden.

<a id="vb"></a>
## <a name="vb"></a>VB

Bei das Lernprogramm für EF 4.1 ursprünglich erstellt wurde, bereitgestellt wir c# und VB-Versionen des Projekts abgeschlossenen Download an. Aufgrund der Zeit-Einschränkungen und andere Prioritäten haben wir nicht, die für diese Version ausgeführt. Wenn Sie beim Erstellen eines VB-Projekts mit diesen Lernprogrammen und wäre, die andere Personen freigeben, informieren Sie uns in Kauf zu nehmen.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Häufige Fehler und -Lösungen oder problemumgehungen für Sie

### <a name="cannot-createshadow-copy"></a>Kopieren kann nicht erstellt/Schatten werden

Fehlermeldung:

> Kopieren kann nicht erstellt/Schatten "&lt;Filename&gt;" Wenn die Datei bereits vorhanden ist.


Lösung

Warten Sie einige Sekunden, und aktualisieren Sie die Seite.

### <a name="update-database-not-recognized"></a>Update-Database nicht erkannt.

Fehlermeldung (aus der `Update-Database` -Befehl in der PMC):

> Der Begriff 'Update-Database' ist nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad korrekt ist, und versuchen Sie es erneut.


Lösung

Beenden Sie Visual Studio. Öffnen Sie Projekt erneut, und wiederholen Sie den Vorgang.

### <a name="validation-failed"></a>Fehler bei der Überprüfung

Fehlermeldung (aus der `Update-Database` -Befehl in der PMC):

> Fehler bei der Validierung für eine oder mehrere Entitäten. Finden Sie unter 'EntityValidationErrors'-Eigenschaft für weitere Details.


Lösung

Eine Ursache für dieses Problem sind Überprüfungsfehler bei den `Seed` Methode ausgeführt wird. Finden Sie unter [Seeding und Debuggen von Entity Framework (EF) Datenbanken](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Tipps zum Debuggen der `Seed` Methode.

### <a name="http-50019-error"></a>HTTP 500.19 Fehler

Fehlermeldung:

> HTTP-Fehler 500.19 - Interner Serverfehler  
> Die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig ist.


Lösung

Eine Möglichkeit, die Sie diese Fehlermeldung erhalten, können wird vom Vorhandensein mehrerer Kopien der Lösung, jede von ihnen die gleiche Portnummer verwenden. In der Regel können Sie dieses Problem beheben, indem Beenden alle Instanzen von Visual Studio und anschließend das Projekt, auf dem Sie arbeiten, Neustart. Wenn dies nicht funktioniert, versuchen Sie es ändern der Portnummer. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und klicken Sie dann auf Eigenschaften. Wählen Sie die **Web** Registerkarte, und ändern Sie dann die Portnummer in das **Projekt-Url** Textfeld.

### <a name="error-locating-sql-server-instance"></a>Fehler beim Suchen von SQL Server-Instanz

Fehlermeldung:

> Ein netzwerkbezogener oder Instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden oder es konnte nicht auf ihn zugegriffen werden. Stellen Sie sicher, dass der Instanzname richtig und SQL Server so konfiguriert ist, das Remoteverbindungen zulässig sind. (Anbieter: SQL Network Interfaces, Fehler: 26 - Fehler beim Suchen von Server-Instanz angegeben.)


Lösung

Überprüfen Sie die Verbindungszeichenfolge an. Wenn Sie die Datenbank manuell gelöscht haben, ändern Sie den Namen der Datenbank in der Konstruktionszeichenfolge.

>[!div class="step-by-step"]
[Vorherige](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
