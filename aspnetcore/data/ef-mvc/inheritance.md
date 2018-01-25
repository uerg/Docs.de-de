---
title: ASP.NET Core MVC mit EF-Kern - Vererbung - 9 von 10
author: tdykstra
description: In diesem Lernprogramm erfahren Sie, wie Vererbung in das Datenmodell mithilfe von Entity Framework Core in einer ASP.NET Core-Anwendung implementiert.
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a4ae696bdd114ab9c36d1218f753fa3d515f2300
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="961f8-103">Vererbung - EF-Core mit ASP.NET Core MVC-Lernprogramm (9 von 10)</span><span class="sxs-lookup"><span data-stu-id="961f8-103">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="961f8-104">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="961f8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="961f8-105">Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="961f8-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="961f8-106">Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="961f8-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="961f8-107">Im vorherigen Lernprogramm behandelt Sie Parallelitätsausnahmen.</span><span class="sxs-lookup"><span data-stu-id="961f8-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="961f8-108">In diesem Lernprogramm erfahren Sie, wie Vererbung im Datenmodell implementiert.</span><span class="sxs-lookup"><span data-stu-id="961f8-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="961f8-109">In einer objektorientierten Programmierung können Sie Vererbung verwenden, um die Wiederverwendung von Code zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="961f8-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="961f8-110">In diesem Lernprogramm ändern Sie die `Instructor` und `Student` Klassen, damit sie ableiten eine `Person` Basisklasse, die Eigenschaften, z. B. enthält `LastName` , könne Dozenten und Studenten gemeinsam sind.</span><span class="sxs-lookup"><span data-stu-id="961f8-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="961f8-111">Sie wird nicht hinzufügen oder Ändern von Webseiten, aber ändern Sie Teil des Codes und diese Änderungen werden automatisch in der Datenbank übernommen.</span><span class="sxs-lookup"><span data-stu-id="961f8-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="961f8-112">Optionen für die Zuordnung von Vererbung zu Datenbanktabellen</span><span class="sxs-lookup"><span data-stu-id="961f8-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="961f8-113">Die `Instructor` und `Student` Klassen im Modell "School" Daten wurden mehrere Eigenschaften, die identisch sind:</span><span class="sxs-lookup"><span data-stu-id="961f8-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Student "und" Instructor-Klassen](inheritance/_static/no-inheritance.png)

<span data-ttu-id="961f8-115">Angenommen, Sie möchten, für die Eigenschaften der redundanten Code vermeiden, die von gemeinsam genutzt werden die `Instructor` und `Student` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="961f8-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="961f8-116">Oder Sie möchten einen Dienst schreiben, der Formatnamen können ohne eine Rolle spielt, ob der Name einer Dozenten oder eines Teilnehmers stammt.</span><span class="sxs-lookup"><span data-stu-id="961f8-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="961f8-117">Sie erstellen eine `Person` Basisklasse, die nur diejenigen Eigenschaften gemeinsame enthält, muss sich die `Instructor` und `Student` Klassen erben von dieser Basisklasse auf, wie in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="961f8-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Student "und" Instructor-Klassen ableiten von Person-Klasse](inheritance/_static/inheritance.png)

<span data-ttu-id="961f8-119">Es gibt mehrere Möglichkeiten, die diese Vererbungsstruktur in der Datenbank dargestellt werden konnte.</span><span class="sxs-lookup"><span data-stu-id="961f8-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="961f8-120">Sie möglicherweise eine Person-Tabelle, die Informationen zu Studenten und Lehrkräfte in einer einzelnen Tabelle enthält.</span><span class="sxs-lookup"><span data-stu-id="961f8-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="961f8-121">Einige Spalten könnte gelten nur für Lehrkräfte (Einstellungsdatum), einige nur für Studenten (EnrollmentDate), einige beide (Nachname, Vorname).</span><span class="sxs-lookup"><span data-stu-id="961f8-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="961f8-122">Normalerweise müssten Sie, um anzugeben, welcher Typ jede Zeile stellt eine Unterscheidungsspalte.</span><span class="sxs-lookup"><span data-stu-id="961f8-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="961f8-123">Z. B. möglicherweise Unterscheidungsspalte für Studenten "Instructor" für Dozenten und "Student" haben.</span><span class="sxs-lookup"><span data-stu-id="961f8-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Beispiel für eine Tabelle pro Hierarchie](inheritance/_static/tph.png)

<span data-ttu-id="961f8-125">Dieses Muster für eine Entität Vererbungsstruktur aus einer einzelnen Datenbanktabelle generiert wird Tabelle pro Hierarchie (TPH) Vererbung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="961f8-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="961f8-126">Eine Alternative besteht darin, von der Datenbank eher wie die Vererbungsstruktur.</span><span class="sxs-lookup"><span data-stu-id="961f8-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="961f8-127">Sie konnten z. B. haben nur die Felder in den Person-Tabelle und separate Dozenten und Student-Tabellen mit den Datumsfeldern aufweisen.</span><span class="sxs-lookup"><span data-stu-id="961f8-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

!["Tabelle pro Typ"-Vererbung](inheritance/_static/tpt.png)

<span data-ttu-id="961f8-129">Dieses Muster versteht man eine Datenbanktabelle für jede Entitätsklasse heißt Tabelle pro Typ (TPT) Vererbung.</span><span class="sxs-lookup"><span data-stu-id="961f8-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="961f8-130">Ist noch eine weitere Option, um einzelne Tabellen alle nicht abstrakten Typen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="961f8-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="961f8-131">Alle Eigenschaften einer Klasse, einschließlich der geerbte Eigenschaften sind Spalten der entsprechenden Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="961f8-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="961f8-132">Dieses Muster wird Tabelle pro konkrete klassenvererbung (TPC) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="961f8-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="961f8-133">Wenn Sie vorher gezeigten TPC-Vererbung für die Person, Studenten und Dozenten Klassen implementiert, sieht Student "und" Instructor-Tabellen unterscheidet sich nach dem Implementieren der Vererbung als vor der funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="961f8-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="961f8-134">TPC und TPH Muster der methodenvererbung aufgeführt übermitteln im Allgemeinen eine bessere Leistung als TPT Muster der methodenvererbung aufgeführt, da TPT Muster komplexer Join-Abfragen führen können.</span><span class="sxs-lookup"><span data-stu-id="961f8-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="961f8-135">Dieses Lernprogramm veranschaulicht die TPH-Vererbung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="961f8-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="961f8-136">TPH ist die einzige Vererbungsmuster, die den Kern des Entity Framework unterstützt.</span><span class="sxs-lookup"><span data-stu-id="961f8-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="961f8-137">Was Sie tun müssen ist, erstellen Sie eine `Person` Klasse, Ändern der `Instructor` und `Student` Klassen ableiten `Person`, fügen Sie die neue Klasse, die `DbContext`, und erstellen Sie eine Migration.</span><span class="sxs-lookup"><span data-stu-id="961f8-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="961f8-138">Sollten Sie eine Kopie des Projekts speichern, bevor Sie die folgenden Änderungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="961f8-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="961f8-139">Klicken Sie dann werden Probleme und müssen über starten auftreten, einfacher aus dem gespeicherten Projekt anstelle von "Fertig" für dieses Lernprogramm Schritte umkehren oder wechseln zurück zum Anfang die gesamte Reihe gestartet.</span><span class="sxs-lookup"><span data-stu-id="961f8-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="961f8-140">Erstellen Sie die Person-Klasse</span><span class="sxs-lookup"><span data-stu-id="961f8-140">Create the Person class</span></span>

<span data-ttu-id="961f8-141">Klicken Sie im Ordner Models erstellen Sie Person.cs, und Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="961f8-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="961f8-142">Stellen Sie Student "und" Instructor-Klassen, die von der Person erben</span><span class="sxs-lookup"><span data-stu-id="961f8-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="961f8-143">In *Instructor.cs*, leiten Sie die Instructor-Klasse, von der Person-Klasse, und entfernen Sie die Schlüssel und den Namen der Felder.</span><span class="sxs-lookup"><span data-stu-id="961f8-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="961f8-144">Der Code wird wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="961f8-144">The code will look like the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="961f8-145">Nehmen Sie die gleichen Änderungen in *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="961f8-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="961f8-146">Fügen Sie des Person-Entität vom Typs des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="961f8-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="961f8-147">Fügen Sie den Person-Entität auf *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="961f8-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="961f8-148">Die neuen Zeilen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="961f8-148">The new lines are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="961f8-149">Dies ist das Entity Framework muss lediglich um eine Tabelle pro Hierarchie Vererbung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="961f8-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="961f8-150">Wie Sie sehen werden, wenn die Datenbank aktualisiert wird, müssen sie eine Personentabelle anstelle der Student "und" Instructor-Tabellen.</span><span class="sxs-lookup"><span data-stu-id="961f8-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="961f8-151">Erstellen und Anpassen von Code der migration</span><span class="sxs-lookup"><span data-stu-id="961f8-151">Create and customize migration code</span></span>

<span data-ttu-id="961f8-152">Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="961f8-152">Save your changes and build the project.</span></span> <span data-ttu-id="961f8-153">Klicken Sie dann öffnen Sie das Befehlsfenster zu, in den Projektordner, und geben Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="961f8-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="961f8-154">Führen Sie nicht die `database update` noch Befehl.</span><span class="sxs-lookup"><span data-stu-id="961f8-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="961f8-155">Dieser Befehl führt zu Datenverlust, da die Instructor-Tabelle löschen und benennen Sie die Student-Tabelle, die Person.</span><span class="sxs-lookup"><span data-stu-id="961f8-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="961f8-156">Sie müssen zum Bereitstellen von benutzerdefiniertem Code, um vorhandene Daten nicht beibehalten.</span><span class="sxs-lookup"><span data-stu-id="961f8-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="961f8-157">Open *Migrationen /\<Zeitstempel > _Inheritance.cs* , und Ersetzen Sie die `Up` -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="961f8-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="961f8-158">Dieser Code übernimmt die folgenden Aufgaben der Datenbank aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="961f8-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="961f8-159">Entfernt foreign Key-Einschränkungen und Indizes, die auf die Student-Tabelle verweisen.</span><span class="sxs-lookup"><span data-stu-id="961f8-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="961f8-160">Benennt die Instructor-Tabelle als Person und nimmt Änderungen, die zum Speichern von Daten Student erforderlich:</span><span class="sxs-lookup"><span data-stu-id="961f8-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="961f8-161">Fügt NULL-Werte zulassen EnrollmentDate für Studenten hinzu.</span><span class="sxs-lookup"><span data-stu-id="961f8-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="961f8-162">Fügt Unterscheidungsspalte, um anzugeben, ob eine Zeile für ein Student oder einen Kursleiter bestimmt ist.</span><span class="sxs-lookup"><span data-stu-id="961f8-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="961f8-163">Macht HireDate seit Student Zeilen Einstellungsdaten keine NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="961f8-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="961f8-164">Fügt ein temporäres Feld, das zum Aktualisieren von Fremdschlüsseln, die auf Studenten verweisen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="961f8-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="961f8-165">Beim Kopieren von Studenten in die Person-Tabelle erhalten sie neue primäre Schlüsselwerte.</span><span class="sxs-lookup"><span data-stu-id="961f8-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="961f8-166">Kopiert Daten aus der Tabelle "Student" in der Person-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="961f8-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="961f8-167">Dies bewirkt, dass Studenten neue Primärschlüsselwerte zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="961f8-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="961f8-168">Behebt Fremdschlüsselwerte, die auf Studenten verweisen.</span><span class="sxs-lookup"><span data-stu-id="961f8-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="961f8-169">Neu erstellt, foreign Key-Einschränkungen und Indizes, die Sie sie jetzt auf der Person-Tabelle verweisen.</span><span class="sxs-lookup"><span data-stu-id="961f8-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="961f8-170">(Wenn Sie GUID nicht ganze Zahl als den Typ des primären Schlüssels verwendet haben, Student Primärschlüsselwerte würde nicht ändern müssen und mehrere Schritte wurde konnte ausgelassen.)</span><span class="sxs-lookup"><span data-stu-id="961f8-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="961f8-171">Führen Sie die `database update` Befehl:</span><span class="sxs-lookup"><span data-stu-id="961f8-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="961f8-172">(In einem Produktionssystem Sie entsprechende Änderungen vornehmen würden die `Down` Methode im Fall schon zurückdatieren, um die frühere Datenbankversion verwendet.</span><span class="sxs-lookup"><span data-stu-id="961f8-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="961f8-173">In diesem Lernprogramm nicht benötigte die `Down` Methode.)</span><span class="sxs-lookup"><span data-stu-id="961f8-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="961f8-174">Es ist möglich, andere Fehler auftreten, wenn schemaänderungen in einer Datenbank zu bestimmen, die vorhandene Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="961f8-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="961f8-175">Wenn Sie Fehler bei der Migration, die Sie nicht beheben können erhalten, können Sie den Datenbanknamen in der Verbindungszeichenfolge ändern oder löschen Sie die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="961f8-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="961f8-176">Mit einer neuen Datenbank es sind keine Daten zu migrieren, und der Update-Database-Befehl ist wahrscheinlicher ohne Fehler abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="961f8-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="961f8-177">Klicken Sie zum Löschen der Datenbank SSOX verwenden, oder führen Sie die `database drop` CLI-Befehl.</span><span class="sxs-lookup"><span data-stu-id="961f8-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="961f8-178">Testen mit Vererbung implementiert</span><span class="sxs-lookup"><span data-stu-id="961f8-178">Test with inheritance implemented</span></span>

<span data-ttu-id="961f8-179">Führen Sie die app, und wiederholen Sie den verschiedenen Seiten.</span><span class="sxs-lookup"><span data-stu-id="961f8-179">Run the app and try various pages.</span></span> <span data-ttu-id="961f8-180">Alles funktioniert genauso wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="961f8-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="961f8-181">In **Objekt-Explorer von SQL Server**, erweitern Sie **Daten Verbindungen/SchoolContext** und dann **Tabellen**, und Sie sehen, dass die Tabellen Student "und" Dozenten durch ersetzt wurden eine Person-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="961f8-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="961f8-182">Die Person-Tabellen-Designer öffnen und sehen Sie, dass sie alle Spalten hat, die in den Tabellen Student "und" Dozenten werden verwendet.</span><span class="sxs-lookup"><span data-stu-id="961f8-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Person-Tabelle in SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="961f8-184">Mit der rechten Maustaste in den Person-Tabelle, und klicken Sie dann auf **Tabellendaten anzeigen** Unterscheidungsspalte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="961f8-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Person-Tabelle in SSOX - Tabellendaten](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="961f8-186">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="961f8-186">Summary</span></span>

<span data-ttu-id="961f8-187">Sie haben die Tabelle pro Hierarchie Vererbung für implementiert die `Person`, `Student`, und `Instructor` Klassen.</span><span class="sxs-lookup"><span data-stu-id="961f8-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="961f8-188">Weitere Informationen zu Vererbung in Entity Framework Core, finden Sie unter [Vererbung](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="961f8-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="961f8-189">In den nächsten Lernprogrammen sehen Sie, wie eine Vielzahl von relativ erweiterte Entity Framework-Szenarien behandelt.</span><span class="sxs-lookup"><span data-stu-id="961f8-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="961f8-190">[Zurück](concurrency.md)
[Weiter](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="961f8-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
