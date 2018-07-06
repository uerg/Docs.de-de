---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementieren das Repository und Arbeitseinheitsmuster in einer ASP.NET MVC-Anwendung (9 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9de8b33e4c533b90b7653544a6814d1ee756cf50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814568"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementieren das Repository und Arbeitseinheitsmuster in einer ASP.NET MVC-Anwendung (9 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Tutorial Sie Vererbung verwendet, um die redundanten Code im Verringern der `Student` und `Instructor` Entitätsklassen. In diesem Tutorial sehen Sie einige Möglichkeiten, um das Repository- und arbeitseinheitsmuster arbeitseinheitenmustern für CRUD-Vorgänge verwenden. Wie im vorherigen Tutorial in diesem ändern wie Sie Ihren Code mit Seiten funktioniert Sie bereits erstellt und nicht als neue Seiten erstellen.

## <a name="the-repository-and-unit-of-work-patterns"></a>Das Repository und Arbeitseinheitsmuster

Die Repository- und arbeitseinheitsmuster Muster sollen eine Abstraktionsebene zwischen der Datenzugriffsebene und den Geschäftslogikebene einer Anwendung erstellen. Die Implementierung dieser Muster unterstützt die Isolation Ihrer Anwendung vor Änderungen im Datenspeicher und kann automatisierte Komponententests oder eine testgesteuerte Entwicklung (Test-Driven Development, TDD) erleichtern.

In diesem Tutorial implementieren Sie eine repositoryklasse für jeden Entitätstyp. Für die `Student` Entitätstyps, die Sie erstellen eine Repositoryschnittstelle und einer Repository-Klasse. Wenn Sie das Repository in Ihre Controller instanziieren, verwenden Sie die Schnittstelle, damit der Controller einen Verweis auf ein Objekt akzeptiert, der die Repositoryschnittstelle implementiert. Wenn der Controller in einen Webserver ausgeführt wird, erhält sie ein Repository, das mit dem Entity Framework arbeitet. Wenn der Controller unter einer Komponententestklasse ausgeführt wird, erhält sie ein Repository, das mit Daten in einer Weise, die Sie problemlos ändern können, für das Testen, wie z. B. eine in-Memory-Sammlung funktioniert.

Später in diesem Tutorial verwenden Sie mehrere Repositorys und eine Einheit der Arbeitsaufgaben-Klasse für den `Course` und `Department` Entitätstypen der `Course` Controller. Die Einheit der Arbeit Klasse koordiniert die Arbeit von mehreren Repositorys durch Erstellen einer einzeldatenbank Kontext-Klasse, die von allen gemeinsam verwendet werden. Falls gewünscht, um automatisierte Komponententests durchführen zu können, würden Sie das Erstellen und verwenden Sie Schnittstellen für diese Klassen, auf die gleiche Weise wie für die `Student` Repository. Sie müssen jedoch zur Vereinfachung des Tutorials erstellen und verwenden Sie diese Klassen ohne Schnittstellen.

Die folgende Abbildung zeigt eine Möglichkeit, die Beziehungen zwischen den Controller und die im Vergleich zu nicht mit dem Repository oder das arbeitseinheitsmuster alle Kontextklassen konzeptualisieren.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Sie wird nicht in dieser tutorialreihe Komponententests erstellen. Eine Einführung in die testgesteuerte Entwicklung mit einer MVC-Anwendung, die das Repository-Muster verwendet werden soll, finden Sie unter [Exemplarische Vorgehensweise: Verwenden von TDD mit ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Weitere Informationen über das Repositorymuster finden Sie unter den folgenden Ressourcen:

- [Das Repositorymuster](https://msdn.microsoft.com/library/ff649690.aspx) auf MSDN.
- [Verwenden Muster für Repository und Arbeitseinheit mit Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) im Entity Framework-Team-Blog.
- [Agile Entity Framework 4-Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) -Serie von Posts auf Julie lermans Blog.
- [Erstellen das Konto auf einen Blick HTML5/jQuery-Anwendung](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) wahlin-Blog.

> [!NOTE]
> Es gibt viele Möglichkeiten, die Repository- und arbeitseinheitsmuster Muster zu implementieren. Sie können die Repository-Klassen mit oder ohne eine Einheit der Arbeitsaufgaben-Klasse verwenden. Sie können ein zentrales Repository für alle Entitätstypen und eine für jeden Typ implementieren. Wenn Sie eine für jeden Typ implementieren, können Sie separate Klassen eine generische Basisklasse und abgeleitete Klassen oder eine abstrakte Basisklasse und abgeleitete Klassen. Können Sie Geschäftslogik in Ihrem Repository enthalten oder auf die Logik für den Datenzugriff zu beschränken. Sie können auch eine Abstraktionsebene in Ihrem Datenbankkontext-Klasse erstellen, mit [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) Schnittstellen es anstelle von ["DbSet"](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) -Typen für Ihre Entitätenmengen. Der Ansatz für die Implementierung einer Abstraktionsschicht, die in diesem Tutorial gezeigt ist eine Option für berücksichtigt werden, nicht auf eine Empfehlung für alle Szenarien und Umgebungen.


## <a name="creating-the-student-repository-class"></a>Erstellen der Repository-Klasse "Student"

In der *DAL* Ordner, erstellen Sie eine Klassendatei mit dem Namen *IStudentRepository.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code deklariert eine Reihe von CRUD-Methoden, einschließlich der zwei Methoden für das Lesen – eine, die alle zurückgibt `Student` Entitäten und eine, die eine einzelne findet `Student` Entität anhand der ID.

In der *DAL* Ordner, erstellen Sie eine Klassendatei mit dem Namen *StudentRepository.cs* Datei. Ersetzen Sie den vorhandenen Code durch den folgenden Code, der implementiert die `IStudentRepository` Schnittstelle:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der Datenbankkontext in eine Klassenvariable definiert ist, und der Konstruktor erwartet das aufrufende Objekt eine Instanz des Kontext übergeben:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Instanziieren Sie einen neuen Kontext in das Repository, aber wenn Sie über mehrere Repositorys in einem Controller verwendet, jede würde letztendlich mit einem separaten Kontext. Später verwenden Sie mehrere Repositorys in der `Course` Controller, und es erscheint wie eine Einheit der Arbeitsaufgaben-Klasse sicherstellen kann, dass alle Repositorys denselben Kontext verwenden.

Implementiert das Repository ["IDisposable"](https://msdn.microsoft.com/library/system.idisposable.aspx) im Persistenzspeicher und entfernt Sie den Datenbankkontext aus, wie Sie gesehen, weiter oben in den Controller haben, und die CRUD-Methoden Aufrufe an den Datenbankkontext auf die gleiche Weise, die Sie zuvor gesehen haben.

## <a name="change-the-student-controller-to-use-the-repository"></a>Ändern des Controllers für Schüler und Studenten, um das Repository zu verwenden.

In *StudentController.cs*, ersetzen Sie den Code derzeit in der Klasse durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Der Controller jetzt deklariert eine Klassenvariable für ein Objekt, implementiert die `IStudentRepository` Schnittstelle anstelle der Context-Klasse:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Der (parameterlose) Standardkonstruktor erstellt eine neue Kontextinstanz und ein optionale Konstruktor ermöglicht den Aufrufer die Übergabe in einem Context-Instanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Bei der Verwendung *Abhängigkeitsinjektion*, oder Dependency Injection, wäre nicht der Standard-Konstruktor, da die DI-Software würde beispielsweise sicherstellen, dass die richtigen Repository-Objekt immer gestellt werden.)

In den CRUD-Methoden heißt das Repository jetzt anstelle des Kontexts:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Und die `Dispose` Methode entfernt jetzt das Repository anstelle des Kontexts:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Führen Sie die Website, und klicken Sie auf die **Schüler/Studenten** Registerkarte.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Die Seite sieht aus, und es funktioniert genauso wie zuvor, bevor Sie den Code zur Verwendung des Repository geändert, und die anderen Student-Seiten auch funktionieren auf dieselbe Weise. Es ist jedoch ein wichtiger Unterschied in der die `Index` -Methode des Controllers ist, Filtern und sortieren. Die ursprüngliche Version dieser Methode enthält den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Die aktualisierte `Index` Methode den folgenden Code enthält:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Nur der hervorgehobene Code verändert wurde.

In der ursprünglichen Version des Codes `students` typisiert ist, als ein `IQueryable` Objekt. Die Abfrage ist nicht an die Datenbank gesendet, bis zur Konvertierung in eine Sammlung mit einer Methode wie z. B. `ToList`, nicht auftreten, bis die Ansicht "Index" das Modell "Student" zugreift. Die `Where` -Methode in den ursprünglichen Code oben wird eine `WHERE` -Klausel in der SQL-Abfrage, die an die Datenbank gesendet wird. Das bedeutet wiederum, dass nur die ausgewählten Entitäten von der Datenbank zurückgegeben werden. Allerdings Folge der Änderung `context.Students` zu `studentRepository.GetStudents()`, `students` Variable aus, nachdem Sie diese Anweisung ist ein `IEnumerable` -Auflistung, die alle Schüler/Studenten in der Datenbank enthält. Das Ergebnis des Anwendens der `Where` Methode ist identisch, aber jetzt die Arbeit im Arbeitsspeicher auf dem Webserver und nicht von der Datenbank erfolgt. Dies kann bei Abfragen, die große Mengen an Daten zurückgeben, ineffizient sein.

> [!TIP]
> 
> **"IQueryable" im Vergleich zu "IEnumerable"**
> 
> Nachdem Sie das Repository implementiert wie hier gezeigt, auch wenn Sie etwas in Geben Sie die **Suche** Feld der Abfrage an SQL Server gesendet werden alle Zeilen von "Student" zurückgegeben, da Ihre Suchkriterien nicht berücksichtigt:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Diese Abfrage gibt alle Studentendaten für Schüler und, da das Repository die Abfrage ausgeführt, ohne zu wissen, zu den Suchkriterien entsprechen. Der Prozess der Sortierung, Suchkriterien angewendet werden und eine Teilmenge der Daten auswählen, für Paging (in diesem Fall zeigt nur 3 Zeilen) im Arbeitsspeicher erfolgt später, wenn die `ToPagedList` Methode wird aufgerufen, auf die `IEnumerable` Auflistung.
> 
> In der vorherigen Version des Codes (bevor Sie das Repository implementiert), die Abfrage ist nicht an die Datenbank gesendet erst nach dem Anwenden der Suchkriterien, wenn `ToPagedList` aufgerufen wird, auf die `IQueryable` Objekt.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Wenn ToPagedList aufgerufen wird, auf eine `IQueryable` Objekt, an SQL Server gesendete Abfrage gibt die Suchzeichenfolge, daher werden nur die Zeilen, die den Suchkriterien entsprechen zurückgegeben und keine Filterung im Arbeitsspeicher ausgeführt werden muss.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Im folgende Tutorial wird erläutert, wie Abfragen mit SQL Server gesendet zu untersuchen.)


Der folgende Abschnitt veranschaulicht, wie Repository-Methoden implementieren, mit denen Sie angeben, dass diese Arbeit von der Datenbank ausgeführt werden soll.

Sie haben nun eine Abstraktionsschicht zwischen dem Controller und der Datenbankkontext Entity Framework erstellt. Wenn Sie automatisierte Komponententests, die mit dieser Anwendung ausführen möchten, können Sie eine alternative repositoryklasse erstellen, in ein Komponententestprojekt, die implementiert `IStudentRepository` *.* Anstelle des Kontexts zum Lesen und Schreiben von Daten, konnte dieser pseudorepository-Klasse Auflistungen im Arbeitsspeicher bearbeiten, um die Controllerfunktionen zu testen.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementieren eines generischen Repositorys und eine Einheit der Arbeitsaufgaben-Klasse

Erstellen einer repositoryklasse für jeden Entitätstyp in vielen redundanten Code verursachen, und es kann teilweise Updates zu. Nehmen wir beispielsweise an, dass Sie an zwei verschiedene Entitätstypen im Rahmen derselben Transaktion zu aktualisieren. Wenn jeweils eine separate Datenbank Context-Instanz verwendet wird, eine möglicherweise erfolgreich, und die andere schlägt möglicherweise fehl. Eine Möglichkeit zum Minimieren der redundanten Codes ist die Verwendung eines generischen Repositorys, und eine Möglichkeit, sicherzustellen, dass alle Repositorys verwenden Sie den Kontext der gleichen Datenbank (und somit alle Updates zu koordinieren) ist eine Einheit der Arbeitsaufgaben-Klasse verwenden.

In diesem Abschnitt des Tutorials erstellen Sie eine `GenericRepository` Klasse und ein `UnitOfWork` Klasse, und verwenden sie in der `Course` Controller für den Zugriff auf die `Department` und `Course` Entitätenmengen. Wie bereits erwähnt, sind nicht zur Vereinfachung dieser Teil des Tutorials Sie Schnittstellen für diese Klassen erstellen. Aber wenn Sie diese verwenden, um eine testgesteuerte Entwicklung zu vereinfachen möchten, in der Regel implementieren Sie diese mit Schnittstellen die gleiche Weise wie die `Student` Repository.

### <a name="create-a-generic-repository"></a>Erstellen eines generischen Repositorys

In der *DAL* Ordner erstellen *GenericRepository.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Klassenvariablen deklariert werden, für den Datenbankkontext und für die Entitätenmenge, der für das Repository instanziiert wird:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Der Konstruktor akzeptiert eine datenbankkontextinstanz und initialisiert die Entity-Variable festlegen:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Die `Get` Methode verwendet eine Lambda-Ausdrücke, den aufrufenden Code an eine filterbedingung und eine Spalte zum Sortieren der Ergebnisse von zuzulassen, und ein Zeichenfolgenparameter lässt den Aufrufer, die eine durch Trennzeichen getrennte Liste der Navigationseigenschaften für eager Loading bereitzustellen:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Der Code `Expression<Func<TEntity, bool>> filter` bedeutet, dass der Aufrufer erhalten einen Lambda-Ausdruck, der auf der Grundlage der `TEntity` Typ, und dieser Ausdruck gibt einen booleschen Wert zurück. Z. B., wenn das Repository für instanziiert wird die `Student` Entitätstyp, der Code in der aufrufenden Methode möglicherweise geben `student => student.LastName == "Smith` &quot; für die `filter` Parameter.

Der Code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` bedeutet auch, dass der Aufrufer wird einen Lambda-Ausdruck bereitstellen. Die Eingabe für den Ausdruck in diesem Fall ist jedoch ein `IQueryable` -Objekt für die `TEntity` Typ. Der Ausdruck gibt eine sortierte Version dieses `IQueryable` Objekt. Z. B., wenn das Repository für instanziiert wird die `Student` Entitätstyp, der Code in der aufrufenden Methode möglicherweise geben `q => q.OrderBy(s => s.LastName)` für die `orderBy` Parameter.

Der Code in die `Get` -Methode erstellt eine `IQueryable` Objekt aus, und klicken Sie dann den Filterausdruck gilt, sofern vorhanden:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Als Nächstes wird das Eager-Loading-Ausdrücke nach der Analyse der durch Trennzeichen getrennten Liste angewendet:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Schließlich gilt es die `orderBy` Ausdruck, sofern vorhanden und gibt die Ergebnisse; andernfalls wird von der Abfrage nicht sortiert die Ergebnisse zurückgegeben:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Beim Aufrufen der `Get` -Methode können Sie tun, Filtern und Sortieren auf die `IEnumerable` Auflistung, die von der Methode statt die Parameter für diese Funktionen zurückgegeben. Jedoch werden das Sortieren und Filtern von Arbeitsaufgaben klicken Sie dann im Arbeitsspeicher auf dem Webserver ausgeführt werden. Verwenden Sie diese Parameter an, stellen Sie sicher, dass die Arbeit von der Datenbank statt auf dem Webserver ausgeführt wird. Alternativ kann zum Erstellen von abgeleiteten Klassen für bestimmten Entitätstypen und Hinzufügen von speziellen `Get` Methoden, wie z. B. `GetStudentsInNameOrder` oder `GetStudentsByName`. Allerdings kann in einer komplexen Anwendung kann dadurch eine große Anzahl solcher abgeleiteten Klassen und spezielle Methoden, die mehr Aufgaben zu verwalten sein könnte.

Der Code in die `GetByID`, `Insert`, und `Update` Methoden ist ähnlich wie Sie im Repository nicht generische gesehen haben. (Sie sind nicht bereitstellen, einen Eager Load-Parameter in der `GetByID` Signatur, da Sie eager Loading mit nicht der `Find` Methode.)

Zwei Überladungen stehen für die `Delete` Methode:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Eine der folgenden können Sie übergeben, nur die ID der Entität gelöscht werden soll, und eine Instanz einer Entität verwendet. Wie in der [Behandeln von Parallelität](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial, das für Parallelität, die Sie behandeln muss eine `Delete` -Methode, die eine Instanz einer Entität, die akzeptiert umfasst den ursprünglichen Wert einer Eigenschaft für die nachverfolgung.

Diese generischen Repositorys werden typische CRUD-Anforderungen verarbeiten. Wenn ein bestimmten Entitätstyp besondere Voraussetzungen, z. B. eine komplexere Filterung oder Sortierung, hat können Sie eine abgeleitete Klasse erstellen, die über zusätzliche Methoden für diesen Typ verfügt.

## <a name="creating-the-unit-of-work-class"></a>Erstellen der Komponententestprojekte von Arbeit-Klasse

Die Einheit der Arbeitsaufgaben-Klasse dient einem Zweck: um sicherzustellen, dass bei der Verwendung von mehreren Repositorys, teilen sie einen einzeldatenbank-Kontext. Wenn eine Arbeitseinheit abgeschlossen ist. auf diese Weise können Sie Aufrufen der `SaveChanges` Methode für diese Instanz des Kontexts und sicher sein, dass alle zugehörigen Änderungen koordiniert werden. Alle, die Anforderungen für die Klasse ist eine `Save` -Methode und eine Eigenschaft für jedes Repository. Jedes Repository-Eigenschaft gibt eine Repository-Instanz, die mit der gleichen Kontextinstanz für die Datenbank als die anderen Repository-Instanzen instanziiert wurde.

In der *DAL* Ordner, erstellen Sie eine Klassendatei mit dem Namen *UnitOfWork.cs* , und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Der Code erstellt die Klassenvariablen für den Datenbankkontext und jedes Repository. Für die `context` Variable, ein neuer Kontext instanziiert wird:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Jedes Repository-Eigenschaft überprüft, ob das Repository bereits vorhanden ist. Wenn dies nicht der Fall ist, wird er instanziiert das Repository, in der Context-Instanz übergeben. Alle Repositorys wird daher die gleichen Kontextinstanz freigeben.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Die `Save` Methodenaufrufe `SaveChanges` im Datenbankkontext.

Wie alle Klassen, die einen Datenbankkontext in eine Klassenvariable, instanziiert die `UnitOfWork` -Klasse implementiert `IDisposable` und gibt den Kontext frei.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Ändern den Kurs-Controller, um die UnitOfWork-Klasse und die Repositorys verwenden

Ersetzen Sie den Code, der Sie augenblicklich *CourseController.cs* durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Dieser Code Fügt eine Klassenvariable für den `UnitOfWork` Klasse. (Wenn Sie Schnittstellen hier verwendet haben, wäre nicht, initialisieren Sie die Variable hier; stattdessen, implementieren Sie ein Muster von zwei Konstruktoren verwenden wie für die `Student` Repository.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

In der Rest der Klasse werden alle Verweise auf den Datenbankkontext mit dem Verweis auf das entsprechende Repository ersetzt mit `UnitOfWork` Eigenschaften, die Zugriff auf das Repository. Die `Dispose` -Methode verwirft die `UnitOfWork` Instanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Führen Sie die Website, und klicken Sie auf die **Kurse** Registerkarte.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Die Seite sieht aus, und es funktioniert genauso wie Ihre Änderungen gewohnt, und die andere Kursseiten auch funktionieren auf dieselbe Weise.

## <a name="summary"></a>Zusammenfassung

Sie haben nun das Repository und die arbeitseinheitsmuster implementiert. Sie haben die Lambda-Ausdrücke als Parameter der Methode in der generischen Repositorys verwendet. Weitere Informationen dazu, wie diese Ausdrücke mit einem `IQueryable` Objekt, finden Sie unter [IQueryable(T)-Schnittstelle ("System.Linq")](https://msdn.microsoft.com/library/bb351562.aspx) in der MSDN Library. In den nächsten erweiterte Lernprogramms erfahren Sie, wie einige behandelt Szenarien.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
