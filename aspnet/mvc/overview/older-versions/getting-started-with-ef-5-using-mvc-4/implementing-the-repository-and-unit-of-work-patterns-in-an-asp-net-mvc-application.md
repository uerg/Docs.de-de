---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementieren das Repository und die Einheit der Arbeit Muster in einer ASP.NET MVC-Anwendung (9 von 10) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f870b61658686769304a7809bde62e66da3bd0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementieren das Repository und die Einheit der Arbeit Muster in einer ASP.NET MVC-Anwendung (9 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm verwenden Vererbung, reduzieren Sie redundanten Code in der `Student` und `Instructor` Entitätsklassen. In diesem Lernprogramm sehen Sie einige Möglichkeiten, um das Repository und die Einheit der Arbeit Muster für CRUD-Vorgänge verwenden. Wie das vorherige Lernprogramm in diesem ändern wie Sie der Code mit Seiten funktioniert Sie bereits erstellt und nicht als neue Seiten erstellen.

## <a name="the-repository-and-unit-of-work-patterns"></a>Das Repository und die Einheit der Arbeit-Muster

Das Repository und die Einheit der Arbeit Muster, soll eine Abstraktionsebene zwischen der Datenzugriffsebene und den Geschäftslogikschicht einer Anwendung zu erstellen. Die Implementierung dieser Muster unterstützt die Isolation Ihrer Anwendung vor Änderungen im Datenspeicher und kann automatisierte Komponententests oder eine testgesteuerte Entwicklung (Test-Driven Development, TDD) erleichtern.

In diesem Lernprogramm implementieren Sie eine Repository-Klasse für jeden Entitätstyp. Für die `Student` Entitätstyp erstellen Sie ein Repository-Schnittstelle und eine Repository-Klasse. Wenn Sie das Repository in Ihr Controller instanziieren, verwenden Sie die Schnittstelle, damit der Controller einen Verweis auf ein Objekt akzeptiert, der Repository-Schnittstelle implementiert. Wenn der Controller unter einer Webserver ausgeführt wird, erhält sie ein Repository, das mit dem Entity Framework zusammenarbeitet. Wenn der Controller unter einer Testklasse Einheit ausgeführt wird, erhält sie ein Repository, das zusammen mit Daten in einer Weise, die Sie, zum Testen bearbeiten können, wie z. B. eine Auflistung im Arbeitsspeicher gespeichert.

Später in diesem Lernprogramm verwenden Sie mehrere Repositorys und eine Einheit der Arbeit-Klasse für die `Course` und `Department` Entität Typen in den `Course` Controller. Die Einheit der Arbeit Klasse koordiniert die Arbeit von mehreren Repositorys durch das Erstellen einer einzelnen Datenbank Context-Klasse, die von allen von ihnen freigegebenen. Falls gewünscht, um automatisierte Komponententests ausführen kann, würde Sie erstellen und für diese Klassen Schnittstellen verwenden, auf die gleiche Weise, die Sie ausgeführt, für haben die `Student` Repository. Um das Lernprogramm einfach zu halten, müssen Sie jedoch erstellen und verwenden Sie diese Klassen ohne Schnittstellen.

Die folgende Abbildung zeigt eine Möglichkeit, die Beziehungen zwischen dem Controller und Kontextklassen, die im Vergleich zu nicht mit dem Repository oder die Einheit der Arbeit Muster überhaupt beschrieben vorzustellen.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Sie wird nicht in diesem Lernprogramm Reihe Komponententests erstellen. Eine Einführung in die testgesteuerte Entwicklung mit einer MVC-Anwendung, des Repositorymusters verwendet, werden soll, finden Sie unter [Exemplarische Vorgehensweise: Verwenden von TDD mit ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Weitere Informationen über das Repositorymuster finden Sie unter den folgenden Ressourcen:

- [Das Repositorymuster](https://msdn.microsoft.com/library/ff649690.aspx) auf MSDN.
- [Entity Framework 4.0-Repository und Unit of Work Muster mit](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) im Entity Framework-Team-Blog.
- [Agile Entity Framework 4-Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) Reihe von Beiträge zu Julie Lerman Blog.
- [Erstellen das Konto auf einen Blick HTML5/jQuery-Anwendung](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) Dan Wahlin Blog.

> [!NOTE]
> Es gibt viele Möglichkeiten, das Repository und die Einheit der Arbeit Muster zu implementieren. Sie können Repositoryklassen mit oder ohne eine Einheit der Arbeit-Klasse. Sie können ein einzelnes Repository für alle Entitätstypen oder one für jeden Typ implementieren. Wenn Sie einen für jeden Typ implementieren, können Sie separate Klassen, eine generische Basisklasse und abgeleiteten Klassen oder einer abstrakten Klasse und abgeleitete Klassen. Sie Geschäftslogik in Ihrem Repository ein- bzw. um Datenzugriffslogik zu beschränken. Sie können auch eine Abstraktionsebene in Ihrer Datenbank Context-Klasse erstellen, mit [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) es anstelle von Schnittstellen [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) -Typen für Ihre Entitätenmengen. Der Ansatz für die Implementierung einer Abstraktionsebene, die in diesem Lernprogramm gezeigt ist eine Option, die Sie berücksichtigen müssen, nicht auf eine Empfehlung für alle Szenarien und Umgebungen.


## <a name="creating-the-student-repository-class"></a>Erstellen der Student-Repository-Klasse

In der *DAL* Ordner, erstellen Sie eine Klassendatei namens *IStudentRepository.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code deklariert eine Reihe von CRUD-Methoden, einschließlich zwei schreibgeschützte Methoden – eine, die alle zurückgibt `Student` Entitäten, und eine, die eine einzelne findet `Student` Entität nach ID auf.

In der *DAL* Ordner, erstellen Sie eine Klassendatei namens *StudentRepository.cs* Datei. Ersetzen Sie den vorhandenen Code durch den folgenden Code, der implementiert die `IStudentRepository` Schnittstelle:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der Datenbankkontext in einer Klassenvariablen definiert ist und der Konstruktor erwartet das aufrufende Objekt um eine Instanz des Kontexts zu übergeben:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Instanziieren Sie einen neuen Kontext in das Repository, aber dann, wenn Sie mehrere Repositorys in einen Controller verwendet, jede würde am Ende mit einem separaten Kontext. Später verwenden Sie mehrere Repositorys in den `Course` Controller, und Sie sehen wie eine Einheit der Arbeit Klasse sicherstellen kann, dass alle Repositorys, die denselben Kontext verwenden.

Implementiert das Repository [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) und verwirft den Datenbankkontext aus, wie Sie gesehen, weiter oben in der Controller haben und deren CRUD-Methoden Aufrufe an den Datenbankkontext auf die gleiche Weise, den Sie zuvor gesehen haben.

## <a name="change-the-student-controller-to-use-the-repository"></a>Ändern Sie den Student-Controller, um das Repository

In *StudentController.cs*, ersetzen Sie den Code derzeit in der Klasse, durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Deklariert der Controller jetzt eine Klassenvariable für ein Objekt, implementiert die `IStudentRepository` Schnittstelle anstelle der Context-Klasse:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Der (parameterlose) Standardkonstruktor erstellt eine neue Kontextinstanz und ein optionaler Konstruktor ermöglicht den Aufrufer die Übergabe in einem Kontextinstanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Wenn Sie verwendeten *Abhängigkeitsinjektion*, oder DI, wäre nicht Sie den Standardkonstruktor erforderlich, da die Software DI müssen sicherstellen, dass, dass das richtige Repositoryobjekt immer angegeben wird.)

In den CRUD-Methoden heißt das Repository jetzt anstelle des Kontexts

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Und die `Dispose` Methode jetzt das Repository anstelle des Kontexts entfernt:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Führen Sie den Standort aus, und klicken Sie auf die **Studenten** Registerkarte.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Die Seite sieht und funktioniert genauso wie vor der Sie den Code, um das Repository geändert, und die anderen Student-Seiten arbeiten auch identisch. Es ist jedoch ein wichtiger Unterschied bei der Datenerfassung die `Index` -Methode des Controllers verfügt, Filterung und Sortierung. Die ursprüngliche Version dieser Methode enthält den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Die aktualisierte `Index` -Methode enthält den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Nur der hervorgehobene Code hat sich geändert.

In der ursprünglichen Version des Codes `students` als typisiert ist, ein `IQueryable` Objekt. Die Abfrage wird nicht an die Datenbank gesendet, bis er in eine Auflistung mit einer Methode wie konvertiert wird `ToList`, dem tritt nicht auf, bis die Indexansicht Student Modell zugreift. Die `Where` Methode in den ursprünglichen Code oben wird eine `WHERE` -Klausel in der SQL-Abfrage, die an die Datenbank gesendet wird. Dies bedeutet wiederum, dass nur die ausgewählten Entitäten von der Datenbank zurückgegeben werden. Allerdings Folge der Änderung `context.Students` auf `studentRepository.GetStudents()`, `students` Variablen, nachdem diese Anweisung ist ein `IEnumerable` -Auflistung, die alle Teilnehmer in der Datenbank enthält. Das Ergebnis des Anwendens der `Where` Methode ist identisch, aber jetzt die Arbeit im Arbeitsspeicher auf dem Webserver und nicht von der Datenbank erfolgt. Für Abfragen, die große Mengen an Daten zurückgeben, kann dies ineffizient sein.

> [!TIP]
> 
> **IQueryable-Objekt im Vergleich zu IEnumerable**
> 
> Nachdem Sie das Repository implementiert wie hier gezeigt, selbst wenn Sie ein Element im eingeben der **Suche** Feld der Abfrage an SQL Server gesendet werden alle Student Zeilen zurückgegeben, da Ihre Suchkriterien nicht berücksichtigt:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Diese Abfrage gibt alle Student Daten zurück, da Repository die Abfrage ausgeführt, ohne zu wissen zu den Suchkriterien entsprechen. Der Prozess der Sortierung, Suchkriterien angewendet werden und eine Teilmenge der Daten auswählen, für Auslagerungsdatei (anzeigt, in diesem Fall nur 3 Zeilen) im Arbeitsspeicher erfolgt höher bei der `ToPagedList` Methode wird aufgerufen, auf der `IEnumerable` Auflistung.
> 
> In der vorherigen Version des Codes (bevor Sie das Repository implementiert), die Abfrage wird nicht an die Datenbank gesendet erst nach dem Anwenden der Suchkriterien beim `ToPagedList` aufgerufen wird, auf die `IQueryable` Objekt.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Wenn ToPagedList aufgerufen wird, auf ein `IQueryable` Objekt, an SQL Server gesendete Abfrage gibt die Suchzeichenfolge daher nur Zeilen zurückgegeben, die den Suchkriterien entsprechen, und keine Filterung im Arbeitsspeicher ausgeführt werden muss.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Im folgende Lernprogramm wird erläutert, wie Abfragen an SQL Server gesendet zu untersuchen.)


Der folgende Abschnitt zeigt, wie Repository-Methoden implementiert, mit denen Sie angeben, dass diese Arbeit von der Datenbank ausgeführt werden soll.

Sie haben jetzt eine Abstraktionsebene zwischen dem Controller und den Kontext der Entity Framework-Datenbank erstellt. Bei einer geplanten wurden, um automatisierte Komponententests, die mit dieser Anwendung ausführen konnten Sie eine alternative Repository-Klasse erstellen, in ein Komponententestprojekt, die implementiert `IStudentRepository` *.* Anstelle von aufrufen und den Kontext zum Lesen und Schreiben von Daten, konnte dieser Klasse pseudorepository Auflistungen im Arbeitsspeicher bearbeiten, zum Testen der Controllerfunktionen.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementieren Sie eine generische Repository und eine Einheit der Arbeit-Klasse

Erstellen einer Klasse Repository für jeden Entitätstyp kann zu viele redundanter Code, und es könnte die teilweise Updates zu. Angenommen Sie, Sie verfügen über zwei verschiedene Entitätstypen als Teil der gleichen Transaktion zu aktualisieren. Wenn jeweils eine separate Datenbank Kontextinstanz verwendet wird, eine zwar erfolgreich sein und den anderen möglicherweise nicht. Eine Möglichkeit zum Minimieren von redundanten Codes ist die Verwendung einer generischen Repository und eine Möglichkeit, sicherzustellen, dass alle Repositorys den gleichen Datenbankkontext verwenden (und somit alle Updates zu koordinieren) ist die Verwendung eine Einheit der Arbeit-Klasse.

In diesem Abschnitt des Lernprogramms erstellen Sie eine `GenericRepository` Klasse und ein `UnitOfWork` Klasse, und verwenden sie in der `Course` Controller aus, um den Zugriff auf die `Department` und `Course` Entitätenmengen. Wie zuvor erläutert, sind nicht zur Vereinfachung der in diesem Teil des Lernprogramms Sie Schnittstellen für diese Klassen erstellen. Aber bei einer geplanten wurden, deren Verwendung zur testgesteuerten Entwicklung zu ermöglichen würden normalerweise implementieren Sie diese mit Schnittstellen die gleiche Weise wie Sie verwendet haben die `Student` Repository.

### <a name="create-a-generic-repository"></a>Erstellen Sie eine generische Repository

In der *DAL* Ordner erstellen *GenericRepository.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Klassenvariablen deklariert werden, für den Datenbankkontext und der Entitätenmenge, die im Repository für die instanziiert wird:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Der Konstruktor akzeptiert eine Kontext-Datenbankinstanz und der Entität Set-Variable initialisiert:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Die `Get` Methode Lambda-Ausdrücke verwendet, um eine filterbedingung und eine Spalte zum Sortieren der Ergebnisse von an den aufrufenden Code zu ermöglichen und ein Zeichenfolgenparameter ermöglicht den Aufrufer, geben Sie eine durch Trennzeichen getrennte Liste der Navigationseigenschaften für unverzüglichem Laden:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Der Code `Expression<Func<TEntity, bool>> filter` bedeutet, dass der Aufrufer gebe ein Lambda-Ausdrucks auf der Grundlage der `TEntity` Typ, und dieser Ausdruck gibt einen booleschen Wert zurück. Z. B., wenn das Repository für die instanziiert wird die `Student` Entitätstyp, der Code in der aufrufenden Methode möglicherweise geben `student => student.LastName == "Smith` &quot; für die `filter` Parameter.

Der Code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` bedeutet auch, dass der Aufrufer wird einen Lambda-Ausdruck bereitstellen. In diesem Fall die Eingabe für den Ausdruck ist jedoch ein `IQueryable` -Objekt für die `TEntity` Typ. Der Ausdruck wird eine sortierte Version dieses zurückgeben `IQueryable` Objekt. Z. B., wenn das Repository für die instanziiert wird die `Student` Entitätstyp, der Code in der aufrufenden Methode möglicherweise geben `q => q.OrderBy(s => s.LastName)` für die `orderBy` Parameter.

Der Code in der `Get` Methode erstellt ein `IQueryable` Objekt, und klicken Sie dann den Filterausdruck gilt, sofern vorhanden:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Als Nächstes wird die Eager Automatisches Laden von Ausdrücke nach der Analyse der durch Trennzeichen getrennte Liste angewendet:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Schließlich gilt es die `orderBy` Ausdruck, sofern vorhanden und gibt die Ergebnisse; andernfalls wird von der Abfrage nicht sortiert die Ergebnisse zurückgegeben:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Beim Aufrufen der `Get` -Methode, könnten Sie tun, Filterung und Sortierung auf die `IEnumerable` Auflistung zurückgegeben, die von der Methode, anstatt den Parameter für diese Funktionen bereitzustellen. Aber das Sortieren und Filtern von Arbeitsaufgaben würden dann im Arbeitsspeicher auf dem Webserver ausgeführt werden. Verwenden Sie diese Parameter an, stellen Sie sicher, dass die selbst von der Datenbank statt auf dem Webserver ausgeführt wird. Eine Alternative besteht darin, erstellen Sie abgeleitete Klassen für bestimmte Entitätstypen und fügen spezielle `Get` Methoden, wie z. B. `GetStudentsInNameOrder` oder `GetStudentsByName`. Allerdings kann in einer komplexen Anwendung kann dies dazu führen eine große Anzahl solcher abgeleitete Klassen und spezielle Methoden, die Arbeit zu verwalten sein könnte.

Der Code in der `GetByID`, `Insert`, und `Update` Methoden ähnelt der im Repository nicht generische gesehen haben. (Sie sind nicht zur Verfügung eines unverzüglichem Laden-Parameters in der `GetByID` Signatur, da Sie mit unverzüglichem Laden nicht möglich die `Find` Methode.)

Zwei Überladungen werden bereitgestellt, für die `Delete` Methode:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Übergeben eines diese können in der nur die ID der Entität gelöscht werden soll, und eine Überladung einer Entitätsinstanz. In haben Sie gesehen der [Behandeln von Parallelität](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial, Parallelität, die Sie behandeln müssen eine `Delete` Methode, die einer Entitätsinstanz, die akzeptiert enthält den ursprünglichen Wert einer Eigenschaft für die Überwachung.

Dieses generischen Repository werden typische CRUD-Anforderungen verarbeitet. Bei bestimmten Entitätstyps besondere Anforderungen, z. B. komplexer Filterung oder Sortierung, hat können Sie eine abgeleitete Klasse erstellen, die zusätzliche Methoden für diesen Typ enthält.

## <a name="creating-the-unit-of-work-class"></a>Erstellen die Einheit der Arbeit-Klasse

Die Einheit der Arbeit Klasse dient ein bestimmter Zweck: um sicherzustellen, dass bei der Verwendung von mehreren Repositorys, teilen sie einen einzelnen Datenbankkontext. Auf diese Weise, wenn eine Arbeitseinheit abgeschlossen ist Sie erreichen die `SaveChanges` Methode für diese Instanz des Kontexts und sicher sein, dass alle zugehörigen Änderungen koordiniert werden. Die Klasse erfordert, wird eine `Save` -Methode und eine Eigenschaft für jedes Repository. Jede Repository-Eigenschaft gibt eine Repository-Instanz, die dieselbe Datenbank Kontextinstanz als die anderen Repository-Instanzen mit instanziiert wurde.

In der *DAL* Ordner, erstellen Sie eine Klassendatei namens *UnitOfWork.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Der Code erstellt Klassenvariablen für den Datenbankkontext als auch jedes Repository. Für die `context` Variable, ein neuer Kontext instanziiert wird:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Jede Repository-Eigenschaft überprüft, ob das Repository ist bereits vorhanden. Wenn dies nicht der Fall ist, wird das Repository, übergeben die Kontextinstanz instanziiert. Daher verwenden alle Repositorys dieselbe Kontextinstanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Die `Save` Methodenaufrufe `SaveChanges` im Kontext Datenbank.

Wie bei jeder Klasse, die einen Datenbankkontext in einer Klassenvariablen instanziiert die `UnitOfWork` -Klasse implementiert `IDisposable` und gibt den Kontext frei.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Ändern den Kurs-Controller, um die UnitOfWork-Klasse und Repositorys verwenden

Ersetzen Sie den Code, Sie derzeit im haben *CourseController.cs* durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Dieser Code Fügt eine Klassenvariablen für die `UnitOfWork` Klasse. (Wenn Sie hier Schnittstellen verwendet haben, wäre nicht, initialisieren Sie die Variable hier; stattdessen würden Sie ein Muster von zwei Konstruktoren implementiert wird, wie Sie für die `Student` Repository.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

In den Rest der Klasse, werden alle Verweise auf den Datenbankkontext durch Verweise auf die entsprechenden Repository ersetzt mit `UnitOfWork` Eigenschaften auf das Repository zugreifen. Die `Dispose` -Methode verwirft die `UnitOfWork` Instanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Führen Sie den Standort aus, und klicken Sie auf die **Kurse** Registerkarte.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Die Seite sieht und funktioniert genauso wie Ihre Änderungen gewohnt und die anderen Seiten der Kurs auch identisch arbeiten.

## <a name="summary"></a>Zusammenfassung

Sie haben nun das Repository und die Einheit der Arbeit Muster implementiert. Sie haben die Lambda-Ausdrücke als Parameter im Repository generischen Methode verwendet. Weitere Informationen zur Verwendung von diesen Ausdrücken mit einem `IQueryable` Objekt, finden Sie unter [IQueryable(T)-Schnittstelle ("System.Linq")](https://msdn.microsoft.com/library/bb351562.aspx) in der MSDN Library. In den nächsten erweiterte Lernprogramm erfahren Sie, wie einige behandelt datentransformationsschritte Szenarien.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
