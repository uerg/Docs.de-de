---
title: ASP.NET Core MVC mit EF-Core - erweitert - 10 von 10
author: tdykstra
description: "Dieses Lernprogramm führt verschiedene Themen, die nützlich sein, wenn Sie die Grundlagen des Entwickelns von ASP.NET-Webanwendungen, mit dem Entity Framework Core hinausgehen bewusst sind."
keywords: "ASP.NET Core, Entity Framework Core unformatierten Sql untersuchen, Sql, Repositorymusters, Einheit Arbeit für ein Muster, automatische änderungserkennung, vorhandene Datenbank"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 92a2986a-d005-4ff6-9559-6657fd466bb7
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: 70434d1c814af2a96493027c6a2ad87845cd5cae
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2017
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>Weiterführende Themen - EF-Core mit ASP.NET Core MVC-Lernprogramm (10 von 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

Im vorherigen Lernprogramm implementiert Sie Tabelle pro Hierarchie Vererbung. Dieses Lernprogramm führt verschiedene Themen, die nützlich sein, wenn Sie die Grundlagen des Entwickelns von ASP.NET Core-Webanwendungen, die Verwenden von Entity Framework Core, hinausgehen bewusst sind.

## <a name="raw-sql-queries"></a>RAW-SQL-Abfragen

Einer der Vorteile der Verwendung von Entity Framework ist, dass diese vermieden wird, den Code zu viel Wert auf eine bestimmte Methode zum Speichern von Daten zu binden. Dies geschieht durch Generieren von SQL-Abfragen und Befehle, die auch Sie freigibt, müssen sie selbst schreiben. Aber es gibt außergewöhnliche Szenarien auf, wenn müssen Sie bestimmte SQL-Abfragen ausführen, die Sie manuell erstellt haben. Für diese Szenarien umfasst der Entity Framework Code First-API-Methoden, die Ihnen ermöglichen, die SQL-Befehle direkt an die Datenbank übergeben. Sie haben folgende Möglichkeiten, in der EF Core 1.0:

* Verwenden der `DbSet.FromSql` Methode zum Abfragen, die Entitätstypen zurückgeben. Die zurückgegebenen Objekte muss mit der vom erwarteten Typ der `DbSet` -Objekt, und sie werden automatisch nachverfolgt vom Kontext Datenbank, wenn Sie [Überwachung deaktivieren](crud.md#no-tracking-queries).

* Verwenden der `Database.ExecuteSqlCommand` für nichtabfragebefehle.

Wenn Sie beim Ausführen einer Abfrage, die Typen zurückgibt, die Entitäten nicht möchten, können Sie mit der datenbankverbindung von EF bereitgestellten ADO.NET. Die zurückgegebenen Daten wird nicht von den Datenbankkontext nachverfolgt, auch wenn Sie diese Methode zum Abrufen von Entitätstypen verwenden.

Wie immer "true" ist, wenn Sie SQL-Befehle in einer Webanwendung ausführen, müssen Sie Vorsichtsmaßnahmen treffen, Ihre Website vor SQL Injection-Angriffen schützen werden. Eine Möglichkeit dazu besteht darin parametrisierte Abfragen zu verwenden, um sicherzustellen, dass Zeichenfolgen, die von einer Webseite übermittelten als SQL-Befehle interpretiert werden können. In diesem Lernprogramm verwenden Sie parametrisierte Abfragen beim Integrieren von Benutzereingaben in einer Abfrage.

## <a name="call-a-query-that-returns-entities"></a>Rufen Sie eine Abfrage, die Entitäten zurückgibt.

Die `DbSet<TEntity>` -Klasse stellt eine Methode, die Sie verwenden können, zum Ausführen einer Abfrage, die eine Entität des Typs zurückgibt `TEntity`. Um festzustellen, wie dies funktioniert ändern den Code in der `Details` -Methode des Controllers Abteilung.

In *DepartmentsController.cs*in der `Details` -Methode, ersetzen Sie den Code, der eine Abteilung mit abruft eine `FromSql` -Methodenaufruf, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Um sicherzustellen, dass der neue Code ordnungsgemäß funktioniert, wählen Sie die **Abteilungen** Registerkarte und dann **Details** für eines der Abteilungen.

![Department-Details](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Rufen Sie eine Abfrage, die andere Typen zurückgibt

Zuvor haben Sie ein Student statistikraster für die Info-Seite, die die Anzahl der Schüler für jede Registrierungsdatum ergab erstellt. Sie haben die Daten aus der Entitätssammlung Studenten (`_context.Students`) und LINQ verwendet, um die Ergebnisse in eine Liste mit zu projizieren `EnrollmentDateGroup` Model-Objekte anzeigen. Nehmen Sie an, dass die SQL selbst anstatt mit LINQ geschrieben werden soll. Hierzu müssen Sie beim Ausführen einer SQL-Abfrage zurückgibt, die einen anderen Wert als Entitätsobjekten. In EF Core 1.0 ist eine Möglichkeit dazu ADO.NET-Code schreiben und die datenbankverbindung von EF abrufen.

In *HomeController.cs*, ersetzen Sie die `About` -Methode durch folgenden Code:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Fügen Sie eine using Anweisung:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Führen Sie die app, und wechseln Sie zu der Seite "Info". Es zeigt die gleichen Daten wie zuvor.

![Zu den Seiten](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Rufen Sie eine Update-Abfrage

Nehmen Sie an, dass Contoso University Administratoren globale Änderungen in der Datenbank, z. B. das Ändern der Anzahl der Gutschriften für jeder Kurs ausführen möchten. Verfügt die Universität eine große Anzahl von Kurse, wäre es ineffizient, diese als Entitäten abrufen und ändern Sie sie einzeln. In diesem Abschnitt implementieren Sie eine Webseite, die dem Benutzer ermöglicht, geben Sie einen Faktor, um die Anzahl der Gutschriften für alle Kurse ändern, und Sie treffen die Änderung durch eine SQL UPDATE-Anweisung ausführen. Die Webseite wird wie die folgende Abbildung aussehen:

![Seite zum Aktualisieren des Kurses Gutschriften](advanced/_static/update-credits.png)

In *CoursesContoller.cs*, UpdateCourseCredits Methoden für HttpGet und HttpPost hinzufügen:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Wenn der Controller eine HttpGet-Anforderung verarbeitet, wird nichts in zurückgegeben `ViewData["RowsAffected"]`, und die Ansicht ein leeres Textfeld und einer Schaltfläche "Absenden" zeigt, wie in der vorherigen Abbildung dargestellt.

Wenn die **Update** geklickt wird, die HttpPost-Methode aufgerufen wird und Multiplikator hat den Wert in das Textfeld eingegeben. Der Code führt dann die SQL, die Kurse aktualisiert und gibt die Anzahl der betroffenen Zeilen zurück, an der Sicht `ViewData`. Wenn die Sicht Ruft eine `RowsAffected` Wert, es zeigt die Anzahl der aktualisierten Zeilen.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Ansichten/Kurse* Ordner, und klicken Sie dann auf **hinzufügen > Neues Element**.

In der **neues Element hinzufügen** Dialogfeld klicken Sie auf **ASP.NET** unter **installiert** klicken Sie im linken Bereich auf **MVC View Page**, und nennen Sie die neue Ansicht * UpdateCourseCredits.cshtml*.

In *Views/Courses/UpdateCourseCredits.cshtml*, den Code durch den folgenden Code ersetzen:

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Führen Sie die `UpdateCourseCredits` Methode dazu die **Kurse** Registerkarte, klicken Sie dann hinzufügen "/ UpdateCourseCredits" am Ende der URL in die Adressleiste des Browsers (z. B.: `http://localhost:5813/Courses/UpdateCourseCredits`). Geben Sie eine Zahl in das Textfeld ein:

![Seite zum Aktualisieren des Kurses Gutschriften](advanced/_static/update-credits.png)

Klicken Sie auf **Aktualisieren**. Daraufhin wird die Anzahl der betroffenen Zeilen:

![Update Kurs Gutschriften Seite betroffene Zeilen](advanced/_static/update-credits-rows-affected.png)

Klicken Sie auf **zurück zur Listenansicht** um die Liste der Kurse mit der geänderte Anzahl von Gutschriften anzuzeigen.

Beachten Sie, dass Produktionscode müssen sicherstellen, dass, dass gültige Daten immer updates. Der hier dargestellte vereinfachte Code konnte die Anzahl der Gutschriften genug zu Zahlen, die größer als 5 führt multiplizieren. (Die `Credits` Eigenschaft verfügt über eine `[Range(0, 5)]` Attribut.) Die Update-Abfrage funktioniert, aber die ungültigen Daten könnte zu unerwarteten Ergebnissen führen, in anderen Teilen des Systems, die davon ausgehen, dass die Anzahl der Gutschriften 5 oder weniger ist.

Weitere Informationen zu unformatierten SQL-Abfragen finden Sie unter [unformatierten SQL-Abfragen](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Überprüfen von SQL, die an die Datenbank gesendet

Manchmal ist es hilfreich, in der Lage, um den tatsächlichen SQL-Abfragen anzuzeigen, die an die Datenbank gesendet werden. Integrierte Protokollfunktion für ASP.NET Core wird automatisch von EF Core verwendet, Protokolle zu schreiben, die die SQL-Abfragen und Updates enthalten. In diesem Abschnitt sehen Sie einige Beispiele für die SQL-Protokollierung.

Open *StudentsController.cs* und klicken Sie in der `Details` Methode legen Sie einen Haltepunkt auf der `if (student == null)` Anweisung.

Führen Sie die app im Debugmodus befindet, und wechseln Sie zur Seite "Details" für ein Student.

Wechseln Sie zu der **Ausgabe** Fenster mit der Debug-Ausgabe, und sehen Sie die Abfrage:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Sie sehen hier etwas, was möglicherweise überraschen: der SQL SELECT-Anweisungen bis zu 2 Zeilen (`TOP(2)`) aus der Person-Tabelle. Die `SingleOrDefaultAsync` Methode kann nicht in 1 Zeile auf dem Server aufgelöst. Erläuterung:

* Wenn die Abfrage mehrere Zeilen zurückgeben würde, gibt die Methode null zurück.
* Um zu bestimmen, ob die Abfrage mehrere Zeilen zurückgeben würde, muss EF überprüfen Sie, ob mindestens 2 zurückgegeben.

Beachten Sie, dass Sie nicht den Debugmodus verwenden und an einem Haltepunkt abzurufenden Protokollierungsausgabe beenden müssen die **Ausgabe** Fenster. Es ist eine komfortable Methode, um die Protokollierung an dem Punkt zu beenden, die an die Ausgabe gesucht werden soll. Protokollierung wird fortgesetzt, wenn Sie dies tun, und Sie einen Bildlauf zurück, um die Teile zu suchen, denen Sie interessiert sind.

## <a name="repository-and-unit-of-work-patterns"></a>Repository und die Einheit der Arbeit-Muster

Viele Entwickler schreiben Code, um das Repository und die Einheit der Arbeit Muster als Wrapper um Code implementieren, die mit dem Entity Framework funktioniert. Diese Muster sollen eine Abstraktionsebene zwischen der Datenzugriffsebene und den Geschäftslogikschicht einer Anwendung zu erstellen. Diese Muster implementieren, um die Anwendung von Änderungen im Datenspeicher zu isolieren und Endprodukts automatisierte Komponententests oder eine testgesteuerte Entwicklung (TDD). Schreiben zusätzlichen Code zum Implementieren dieser Muster ist jedoch nicht immer die beste Wahl für Anwendungen mit EF, verschiedene Ursachen:

* Die Klasse der EF-Kontext selbst isoliert Codes von Store-datenspezifische Code.

* Die EF-Context-Klasse kann fungieren, als eine Arbeitseinheit Klasse für Datenbank-updates, die Sie mithilfe von EF ausführen.

* EF enthält Funktionen zum Implementieren der testgesteuerten Entwicklung ohne Repository Code zu schreiben.

Informationen dazu, wie Sie das Repository und die Einheit der Arbeit Muster zu implementieren, finden Sie unter [die Entity Framework 5 Version dieser Reihe von Lernprogrammen](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Core implementiert einen speicherinternen Datenbank-Anbieter, der für Tests verwendet werden kann. Weitere Informationen finden Sie unter [Testen mit InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Automatische änderungserkennung

Entity Framework bestimmt wie eine Entität geändert wurde (und daher die Updates an die Datenbank gesendet werden müssen), indem Sie die aktuellen Werte von einer Entität mit den ursprünglichen Werten vergleichen. Die ursprünglichen Werte werden gespeichert, wenn die Entität abgefragt oder angefügt wird. Einige der Methoden, die dazu führen, änderungserkennung für automatische dass lauten wie folgt:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Wenn Sie eine große Anzahl von Entitäten überwachen und einer dieser Methoden oft in einer Schleife aufrufen, erhalten Sie möglicherweise erhebliche Leistungssteigerungen durch vorübergehendes Deaktivieren der automatischen Änderung Erkennung verwendet die `ChangeTracker.AutoDetectChangesEnabled` Eigenschaft. Zum Beispiel:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core Source Code und Pläne

Der Quellcode für Entity Framework Core finden Sie unter [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore). Neben dem Quellcode, können Sie über Nacht erstellte Builds abrufen, Verfolgung von Problemen, feature Spezifikationen, Entwerfen Sie Besprechungsnotizen, [der Roadmap für künftige Entwicklungen](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap), und vieles mehr. Melden Sie den Fehler, und Sie können auch eigene Erweiterungen auf den Quellcode von EF beitragen.

Obwohl der Quellcode geöffnet ist, wird die Entity Framework Core als ein Produkt von Microsoft vollständig unterstützt. Das Microsoft Entity Framework-Team verfolgt die Kontrolle über die Beiträge akzeptiert werden und testet alle Änderungen am Code zum Gewährleisten der Qualität von jeder Version.

## <a name="reverse-engineer-from-existing-database"></a>Reverse Engineering aus vorhandenen Datenbank

Verwenden Sie zum zurückentwickeln, ein Datenmodell, einschließlich der Entitätsklassen aus einer vorhandenen Datenbank, die [Gerüst Dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) Befehl. Finden Sie unter der [erste-Schritte-Lernprogramm](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Verwenden Sie dynamische LINQ zum Sortieren Auswahl Code vereinfachen

Die [dritte Lernprogramm dieser Reihe](sort-filter-page.md) wird gezeigt, wie LINQ Code schreiben, durch eine feste Programmierung von Spaltennamen in einer `switch` Anweisung. Mit zwei Spalten zur Auswahl ist hervorragend, aber wenn Sie viele Spalten aufweisen konnte der Code ausführliche erhalten. Um dieses Problem zu beheben, können Sie die `EF.Property` Methode, um den Namen der Eigenschaft als Zeichenfolge anzugeben. Um diesen Ansatz auszuprobieren, ersetzen die `Index` Methode in der `StudentsController` durch den folgenden Code.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Nächste Schritte

Dies schließt diese Reihe von Lernprogramme zum Verwenden der Entity Framework Core in einer ASP.NET MVC-Anwendung.

Weitere Informationen zu EF Core finden Sie unter der [Dokumentation zu Entity Framework Core](https://docs.microsoft.com/ef/core). Ein Buch ist ebenfalls verfügbar: [Entity Framework Core in Aktion](https://www.manning.com/books/entity-framework-core-in-action).

Informationen dazu, wie Sie Ihre Webanwendung bereitstellen, nachdem Sie es erstellt haben, finden Sie unter [Veröffentlichung und Bereitstellung](../../publishing/index.md).

Weitere Informationen zu anderen Themen im Zusammenhang mit ASP.NET Core MVC, wie beispielsweise Authentifizierung und Autorisierung, finden Sie unter der [Dokumentation zu ASP.NET Core](https://docs.microsoft.com/aspnet/core/).

## <a name="acknowledgments"></a>Bestätigungen

Tom Dykstra und Rick Anderson (twitter @RickAndMSFT) wurde in diesem Lernprogramm geschrieben. Rowan Miller Diego Vega und anderen Mitgliedern der Entity Framework-Team telefonischen mit codereviews und-Abgleich dabei behilflich waren, Debuggen von Problemen, die aufgetreten ist, während wir für die Lernprogramme Code geschrieben wurden.

## <a name="common-errors"></a>Häufige Fehler  

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll, die von einem anderen Prozess verwendet

Fehlermeldung:

> Kann nicht geöffnet werden "... bin\Debug\netcoreapp1.0\ContosoUniversity.dll" zum Schreiben: "der Prozess kann nicht auf die Datei zugreifen"... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll ", da sie von einem anderen Prozess verwendet wird.

Projektmappe:

Beenden Sie die Website in IIS Express. Wechseln Sie zu der Windows-Taskleiste "Suchen" IIS Express und mit der rechten Maustaste des Symbol, wählen Sie den Standort Contoso University und klicken Sie dann auf **Website beenden**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Migration ohne Code in nach oben oder unten Methoden Gerüstbau

Mögliche Ursache:

EF-CLI-Befehle nicht automatisch zu schließen und speichern die Codedateien. Wenn Sie Änderungen beim Ausführen gespeicherte der `migrations add` Befehl EF, kann die Änderungen nicht finden.

Projektmappe:

Führen Sie die `migrations remove` Befehl, speichern Sie Ihre Änderungen am Code und erneuten Ausführen der `migrations add` Befehl.

### <a name="errors-while-running-database-update"></a>Fehler beim laufenden Datenbankupdate

Es ist möglich, andere Fehler auftreten, wenn schemaänderungen in einer Datenbank zu bestimmen, die vorhandene Daten enthält. Wenn Sie Fehler bei der Migration zu, die Sie nicht beheben können erhalten, können Sie den Datenbanknamen in der Verbindungszeichenfolge ändern oder löschen Sie die Datenbank. Mit einer neuen Datenbank es sind keine Daten zu migrieren, und der Update-Database-Befehl ist weitaus höheren Wahrscheinlichkeit in ohne Fehler abgeschlossen werden.

Der einfachste Ansatz besteht darin, benennen Sie die Datenbank in *appsettings.json*. Das nächste Mal ausführen `database update`, eine neue Datenbank erstellt werden.

Zum Löschen einer Datenbank in SSOX mit der rechten Maustaste in der Datenbank aus, klicken Sie auf **löschen**, und klicken Sie dann in der **Datenbank löschen** aktivieren Sie im Dialogfeld **bestehende Verbindungen schließen** , und klicken Sie auf ** OK**.

Führen Sie zum Löschen einer Datenbank mithilfe der CLI die `database drop` CLI-Befehl:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Fehler beim Suchen von SQL Server-Instanz

Fehlermeldung:

> Ein netzwerkbezogener oder Instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden oder es konnte nicht auf ihn zugegriffen werden. Stellen Sie sicher, dass der Instanzname richtig und SQL Server so konfiguriert ist, das Remoteverbindungen zulässig sind. (Anbieter: SQL Network Interfaces, Fehler: 26 - Fehler beim Suchen von Server-Instanz angegeben.)

Projektmappe:

Überprüfen Sie die Verbindungszeichenfolge an. Wenn Sie manuell die Datenbankdatei gelöscht haben, ändern Sie den Namen der Datenbank in der Konstruktionszeichenfolge mit einer neuen Datenbank zu beginnen.

>[!div class="step-by-step"]
[Zurück](inheritance.md)
