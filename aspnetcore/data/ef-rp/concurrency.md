---
title: 'Razor-Seiten mit EF Core in ASP.NET Core: Parallelität (8 von 8)'
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie Sie Konflikte behandeln, wenn mehrere Benutzer gleichzeitig dieselbe Entität aktualisieren.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: ff9e52df63f9c9f47ee659a68beb28b773a114a1
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202691"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="2311d-103">Razor-Seiten mit EF Core in ASP.NET Core: Parallelität (8 von 8)</span><span class="sxs-lookup"><span data-stu-id="2311d-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="2311d-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) und [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="2311d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="2311d-105">Dieses Tutorial zeigt, wie Sie Konflikte behandeln, wenn mehrere Benutzer gleichzeitig dieselbe Entität aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2311d-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="2311d-106">Wenn nicht zu lösende Probleme auftreten, laden Sie die [abgeschlossene Anwendung für diese Phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8) herunter.</span><span class="sxs-lookup"><span data-stu-id="2311d-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="2311d-107">Nebenläufigkeitskonflikte</span><span class="sxs-lookup"><span data-stu-id="2311d-107">Concurrency conflicts</span></span>

<span data-ttu-id="2311d-108">Ein Nebenläufigkeitskonflikt tritt auf, wenn:</span><span class="sxs-lookup"><span data-stu-id="2311d-108">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="2311d-109">Ein Benutzer zur Bearbeitungsseite für eine Entität navigiert.</span><span class="sxs-lookup"><span data-stu-id="2311d-109">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="2311d-110">Ein anderer Benutzer dieselbe Entität aktualisiert, bevor die Änderung des ersten Benutzers in die Datenbank geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-110">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="2311d-111">Wenn die Parallelitätserkennung nicht aktiviert ist, während gleichzeitige Updates ausgeführt werden, geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2311d-111">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="2311d-112">Das letzte Update gilt.</span><span class="sxs-lookup"><span data-stu-id="2311d-112">The last update wins.</span></span> <span data-ttu-id="2311d-113">Das bedeutet, dass die neuesten zu aktualisierenden Werte in der Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-113">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="2311d-114">Das erste der aktuellen Updates geht verloren.</span><span class="sxs-lookup"><span data-stu-id="2311d-114">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="2311d-115">Optimistische Nebenläufigkeit</span><span class="sxs-lookup"><span data-stu-id="2311d-115">Optimistic concurrency</span></span>

<span data-ttu-id="2311d-116">Optimistische Nebenläufigkeit lässt Nebenläufigkeitskonflikte zu und reagiert entsprechend, wenn diese auftreten.</span><span class="sxs-lookup"><span data-stu-id="2311d-116">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="2311d-117">Beispielsweise besucht Benutzer1 die Bearbeitungsseite des Fachbereichs und ändert das Budget für den englischen Fachbereich von 350.000 $ in 0 $.</span><span class="sxs-lookup"><span data-stu-id="2311d-117">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Ändern des Budgets in 0 (null)](concurrency/_static/change-budget.png)

<span data-ttu-id="2311d-119">Bevor Benutzer1 auf **Speichern** klickt, besucht Benutzer2 dieselbe Seite und ändert das Feld „Startdatum“ von 9.1.2007 in 9.1.2013.</span><span class="sxs-lookup"><span data-stu-id="2311d-119">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Ändern des Startdatums in 2013](concurrency/_static/change-date.png)

<span data-ttu-id="2311d-121">Benutzer1 klickt zuerst auf **Speichern** und sieht die Änderungen, wenn im Browser die Indexseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-121">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budget in 0 (null) geändert](concurrency/_static/budget-zero.png)

<span data-ttu-id="2311d-123">Benutzer2 klickt auf einer Bearbeitungsseite auf **Speichern**, die weiterhin ein Budget von 350.000 $ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="2311d-123">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="2311d-124">Was daraufhin geschieht ist abhängig davon, wie Sie Nebenläufigkeitskonflikte handhaben.</span><span class="sxs-lookup"><span data-stu-id="2311d-124">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="2311d-125">Die optimistische Nebenläufigkeit umfasst die folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="2311d-125">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="2311d-126">Sie können Nachverfolgen, welche Eigenschaft ein Benutzer geändert hat, und nur die entsprechenden Spalten in der Datenbank aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2311d-126">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="2311d-127">In diesem Szenario sollten keine Daten verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="2311d-127">In the scenario, no data would be lost.</span></span> <span data-ttu-id="2311d-128">Von den beiden Benutzern wurden unterschiedliche Eigenschaften aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="2311d-128">Different properties were updated by the two users.</span></span> <span data-ttu-id="2311d-129">Das nächste Mal, wenn eine Person den englischen Fachbereich durchsucht, sieht diese die Änderungen von Benutzer1 und Benutzer2.</span><span class="sxs-lookup"><span data-stu-id="2311d-129">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="2311d-130">Diese Methode der Aktualisierung kann die Anzahl von Konflikten reduzieren, die zu Datenverlust führen können.</span><span class="sxs-lookup"><span data-stu-id="2311d-130">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="2311d-131">Dieser Ansatz:</span><span class="sxs-lookup"><span data-stu-id="2311d-131">This approach:</span></span>
 
  * <span data-ttu-id="2311d-132">Kann Datenverlust nicht verhindern, wenn konkurrierende Änderungen an der gleichen Eigenschaft gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-132">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="2311d-133">Ist in einer Web-App in der Regel nicht praktisch.</span><span class="sxs-lookup"><span data-stu-id="2311d-133">Is generally not practical in a web app.</span></span> <span data-ttu-id="2311d-134">Erfordert, dass der maßgebliche Zustand beibehalten wird, um alle abgerufenen Werte und neuen Werte nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="2311d-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="2311d-135">Das Verwalten von großen Datenmengen kann den Zustand der App-Leistung beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="2311d-135">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="2311d-136">Kann die Anwendungskomplexität im Vergleich zur Parallelitätsermittlung für eine Entität erhöhen.</span><span class="sxs-lookup"><span data-stu-id="2311d-136">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="2311d-137">Sie können zulassen, dass die Änderungen von Benutzer2 die Änderungen von Benutzer1 überschreiben.</span><span class="sxs-lookup"><span data-stu-id="2311d-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="2311d-138">Das nächste Mal, wenn jemand den englischen Fachbereich durchsucht, wird das Datum 9.1.2013 und der wiederhergestellte Wert von 350.000 $ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2311d-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="2311d-139">Dieses Ansatz wird *Client gewinnt*- oder *Last in Wins*-Szenario (Letzter gewinnt) genannt.</span><span class="sxs-lookup"><span data-stu-id="2311d-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="2311d-140">(Alle Werte des Clients haben Vorrang vor dem Datenspeicher.) Wenn Sie keine Codierung für die Parallelitätsbehandlung durchführen, wird automatisch das „Client gewinnt“-Szenario ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2311d-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="2311d-141">Sie können verhindern, dass die Änderungen von Benutzer2 in die Datenbank aufgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="2311d-142">In der Regel würde die App:</span><span class="sxs-lookup"><span data-stu-id="2311d-142">Typically, the app would:</span></span>

  * <span data-ttu-id="2311d-143">Eine Fehlermeldung anzeigen</span><span class="sxs-lookup"><span data-stu-id="2311d-143">Display an error message.</span></span>
  * <span data-ttu-id="2311d-144">Den aktuellen Datenstatus anzeigen</span><span class="sxs-lookup"><span data-stu-id="2311d-144">Show the current state of the data.</span></span>
  * <span data-ttu-id="2311d-145">Dem Benutzer ermöglichen, die Änderungen erneut anzuwenden</span><span class="sxs-lookup"><span data-stu-id="2311d-145">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="2311d-146">Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt.</span><span class="sxs-lookup"><span data-stu-id="2311d-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="2311d-147">(Die Werte des Datenspeichers haben Vorrang gegenüber den Werten, die vom Client gesendet werden). In diesem Tutorial implementieren Sie das Szenario „Store Wins“ (Speicher gewinnt).</span><span class="sxs-lookup"><span data-stu-id="2311d-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="2311d-148">Diese Methode stellt sicher, dass keine Änderungen überschrieben werden, ohne dass ein Benutzer darüber benachrichtigt wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="2311d-149">Behandlung von Parallelität</span><span class="sxs-lookup"><span data-stu-id="2311d-149">Handling concurrency</span></span> 

<span data-ttu-id="2311d-150">Wenn eine Eigenschaft als ein [Parallelitätstoken](https://docs.microsoft.com/ef/core/modeling/concurrency) konfiguriert ist:</span><span class="sxs-lookup"><span data-stu-id="2311d-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="2311d-151">Stellt EF Core sicher, dass die Eigenschaft nicht geändert wurde, nachdem sie abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="2311d-152">Die Überprüfung findet statt, wenn [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) oder [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-152">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="2311d-153">Wenn die Eigenschaft geändert wurde, nachdem sie abgerufen wurde, wird eine [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2311d-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="2311d-154">Das Datenbank- und Datenmodell müssen konfiguriert sein, um das Auslösen von `DbUpdateConcurrencyException` zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2311d-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="2311d-155">Erkennen von Nebenläufigkeitskonflikten mit Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="2311d-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="2311d-156">Nebenläufigkeitskonflikte können auf der Eigenschaftenebene über das [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)-Attribut erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="2311d-157">Das Attribut kann auf mehrere Eigenschaften für das Modell angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="2311d-158">Weitere Informationen finden Sie unter [Datenanmerkungen-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="2311d-158">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="2311d-159">Das Attribut `[ConcurrencyCheck]` wird in diesem Tutorial nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="2311d-159">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="2311d-160">Erkennen von Nebenläufigkeitskonflikten mit einer Zeile</span><span class="sxs-lookup"><span data-stu-id="2311d-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="2311d-161">Um Nebenläufigkeitskonflikte zu erkennen, wird dem Modell eine [Rowversion](/sql/t-sql/data-types/rowversion-transact-sql)-Nachverfolgungsspalte (Zeilenversion) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="2311d-161">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="2311d-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="2311d-162">`rowversion` :</span></span>

* <span data-ttu-id="2311d-163">Ist SQL Server-spezifisch.</span><span class="sxs-lookup"><span data-stu-id="2311d-163">Is SQL Server specific.</span></span> <span data-ttu-id="2311d-164">Andere Datenbanken enthalten möglicherweise keine ähnlichen Features.</span><span class="sxs-lookup"><span data-stu-id="2311d-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="2311d-165">Wird verwendet, um zu bestimmen, dass eine Entität, seit dem Abruf aus der Datenbank, nicht geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="2311d-166">Die Datenbank generiert eine sequenzielle Anzahl von `rowversion`, die jedes Mal erhöht wird, wenn die Zeile aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="2311d-167">In einem `Update`- oder `Delete`-Befehl enthält die `Where`-Klausel den abgerufenen Wert von `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="2311d-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="2311d-168">Wenn sich die aktualisierte Zeile geändert hat, geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2311d-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="2311d-169">`rowversion` entspricht nicht dem abgerufenen Wert.</span><span class="sxs-lookup"><span data-stu-id="2311d-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="2311d-170">Die Befehle `Update` oder `Delete` finden keine Zeile, da die `Where`-Klausel die abgerufene `rowversion` enthält.</span><span class="sxs-lookup"><span data-stu-id="2311d-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="2311d-171">Es wird eine `DbUpdateConcurrencyException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2311d-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="2311d-172">In EF Core wird eine Parallelitätsausnahme ausgelöst, wenn keine Zeilen durch einen `Update`- oder `Delete`-Befehl aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="2311d-173">Hinzufügen einer Nachverfolgungseigenschaft zur Entität „Department“</span><span class="sxs-lookup"><span data-stu-id="2311d-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="2311d-174">Fügen Sie der Datei *Models/Department.cs* eine Nachverfolgungseigenschaft namens „RowVersion“ hinzu:</span><span class="sxs-lookup"><span data-stu-id="2311d-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="2311d-175">Das [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute)-Attribut gibt an, dass diese Spalte in der `Where`-Klausel der Befehle `Update` und `Delete` enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="2311d-175">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="2311d-176">Das Attribut wird `Timestamp` genannt, weil vorherige Versionen von SQL Server einen SQL-`timestamp`-Datentyp verwendet haben, bevor dieser durch SQL-`rowversion` ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="2311d-177">Die Fluent-API kann auch die Nachverfolgungseigenschaft angeben:</span><span class="sxs-lookup"><span data-stu-id="2311d-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="2311d-178">Der folgende Code zeigt einen Teil von dem T-SQL an, das EF Core generiert, wenn der Name des Fachbereichs aktualisiert wird:</span><span class="sxs-lookup"><span data-stu-id="2311d-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="2311d-179">Das vorherige markierte Codebeispiel zeigt die `WHERE`-Klausel mit `RowVersion` an.</span><span class="sxs-lookup"><span data-stu-id="2311d-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="2311d-180">Wenn die Datenbank `RowVersion` nicht dem `RowVersion`-Parameter (`@p2`) entspricht, werden keine Zeilen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="2311d-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="2311d-181">Der folgende hervorgehobene Code stellt das T-SQL dar, das genau überprüft, ob eine Zeile aktualisiert wurde:</span><span class="sxs-lookup"><span data-stu-id="2311d-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="2311d-182">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) gibt die Anzahl der von der letzten Anweisung betroffenen Zeilen zurück.</span><span class="sxs-lookup"><span data-stu-id="2311d-182">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="2311d-183">Wenn keine Zeilen aktualisiert werden, löst EF Core eine `DbUpdateConcurrencyException` aus.</span><span class="sxs-lookup"><span data-stu-id="2311d-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="2311d-184">Sie können das von EF Core generierte T-SQL im Ausgabefenster von Visual Studio sehen.</span><span class="sxs-lookup"><span data-stu-id="2311d-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="2311d-185">Aktualisieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="2311d-185">Update the DB</span></span>

<span data-ttu-id="2311d-186">Das Hinzufügen der `RowVersion`-Eigenschaft ändert das Datenbankmodell, das eine Migration erfordert.</span><span class="sxs-lookup"><span data-stu-id="2311d-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="2311d-187">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="2311d-187">Build the project.</span></span> <span data-ttu-id="2311d-188">Geben Sie Folgendes in ein Befehlsfenster ein:</span><span class="sxs-lookup"><span data-stu-id="2311d-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="2311d-189">Die obenstehenden Befehle haben folgende Konsequenzen:</span><span class="sxs-lookup"><span data-stu-id="2311d-189">The preceding commands:</span></span>

* <span data-ttu-id="2311d-190">Die Migrationsdatei *Migrations/{Zeitstempel}_RowVersion.cs* wird hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="2311d-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="2311d-191">Es wird ein Update für die Datei *Migrations/SchoolContextModelSnapshot.cs* ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2311d-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="2311d-192">Über dieses Update wird der `BuildModel`-Methode der folgende hervorgehobene Code hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="2311d-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="2311d-193">Migrationen werden durchgeführt, um die Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2311d-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="2311d-194">Erstellen des Gerüsts für das Abteilungsmodell</span><span class="sxs-lookup"><span data-stu-id="2311d-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="2311d-195">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2311d-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="2311d-196">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="2311d-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="2311d-197">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="2311d-197">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="2311d-198">Der vorherige Befehl erstellt ein Gerüst für das `Department`-Modell.</span><span class="sxs-lookup"><span data-stu-id="2311d-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="2311d-199">Öffnen Sie das Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2311d-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="2311d-200">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="2311d-200">Build the project.</span></span> <span data-ttu-id="2311d-201">Der Build generiert z.B. die folgenden Fehler:</span><span class="sxs-lookup"><span data-stu-id="2311d-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="2311d-202">Ändern Sie `_context.Department` allgemein in `_context.Departments` (d.h., fügen Sie ein „s“ zu `Department` hinzu).</span><span class="sxs-lookup"><span data-stu-id="2311d-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="2311d-203">Der Begriff wurde siebenmal gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="2311d-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="2311d-204">Aktualisieren der Indexseite für Abteilungen</span><span class="sxs-lookup"><span data-stu-id="2311d-204">Update the Departments Index page</span></span>

<span data-ttu-id="2311d-205">Die Gerüstbauengine hat eine `RowVersion`-Spalte auf der Indexseite erstellt, jedoch sollte dieses Feld nicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="2311d-206">In diesem Tutorial wird das letzte Byte der `RowVersion` angezeigt, damit Sie ein besseres Verständnis über die Parallelität erlangen.</span><span class="sxs-lookup"><span data-stu-id="2311d-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="2311d-207">Das letzte Byte ist nicht zwingend eindeutig.</span><span class="sxs-lookup"><span data-stu-id="2311d-207">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="2311d-208">Eine echte App würde `RowVersion` oder das letzte Byte von `RowVersion` nicht anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2311d-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="2311d-209">Aktualisieren der Indexseite:</span><span class="sxs-lookup"><span data-stu-id="2311d-209">Update the Index page:</span></span>

* <span data-ttu-id="2311d-210">Ersetzen Sie „Index“ durch „Abteilungen“.</span><span class="sxs-lookup"><span data-stu-id="2311d-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="2311d-211">Ersetzen Sie das Markup mit `RowVersion` durch das letzten Byte von `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2311d-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="2311d-212">Ersetzen Sie „FirstMidName“durch „FullName“.</span><span class="sxs-lookup"><span data-stu-id="2311d-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="2311d-213">Das folgende Markup zeigt die aktualisierte Seite an:</span><span class="sxs-lookup"><span data-stu-id="2311d-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="2311d-214">Aktualisieren des Seitenbearbeitungsmodells</span><span class="sxs-lookup"><span data-stu-id="2311d-214">Update the Edit page model</span></span>

<span data-ttu-id="2311d-215">Aktualisieren Sie die *pages\departments\edit.cshtml.cs*-Datei mithilfe des folgenden Codes:</span><span class="sxs-lookup"><span data-stu-id="2311d-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="2311d-216">Um ein Nebenläufigkeitsproblem zu erkennen, wird die [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)-Eigenschaft mit dem `rowVersion`-Wert aus der Entität aktualisiert, aus der dieser abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-216">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="2311d-217">EF Core generiert einen SQL UPDATE-Befehl mit einer WHERE-Klausel mit dem ursprünglichen `RowVersion`-Wert.</span><span class="sxs-lookup"><span data-stu-id="2311d-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="2311d-218">Wenn keine Zeilen durch den UPDATE-Befehl betroffen sind (keine Zeile enthält den ursprünglichen `RowVersion`-Wert), wird eine `DbUpdateConcurrencyException`-Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2311d-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="2311d-219">Im obenstehenden Code wird der Wert `Department.RowVersion` zurückgegeben, sobald die Entität abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="2311d-220">`OriginalValue` ist der Wert in der Datenbank, wenn `FirstOrDefaultAsync` in dieser Methode aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="2311d-221">Der folgende Code ruft die Clientwerte (die für diese Methode bereitgestellten Werte) und die Datenbankwerte ab:</span><span class="sxs-lookup"><span data-stu-id="2311d-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="2311d-222">Der folgende Code fügt eine benutzerdefinierte Fehlermeldung für jede Spalte ein, deren Datenbankwerte sich von jenen unterscheiden, die auf `OnPostAsync` bereitgestellt wurden:</span><span class="sxs-lookup"><span data-stu-id="2311d-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="2311d-223">Der folgende hervorgehobene Code legt den `RowVersion`-Wert auf den neuen Wert fest, der aus der Datenbank abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="2311d-224">Das nächste Mal, wenn der Benutzer auf **Speichern** klickt, werden nur Parallelitätsfehler abgefangen, die seit der letzten Anzeige der Bearbeitungsseite aufgetreten sind.</span><span class="sxs-lookup"><span data-stu-id="2311d-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="2311d-225">Die Anweisung `ModelState.Remove` ist erforderlich, da `ModelState` über den alten `RowVersion`-Wert verfügt.</span><span class="sxs-lookup"><span data-stu-id="2311d-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="2311d-226">Auf der Razor Page hat der `ModelState`-Wert eines Felds Vorrang gegenüber den Modelleigenschaftswerten, wenn beide vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="2311d-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="2311d-227">Aktualisieren der Seite „Bearbeiten“</span><span class="sxs-lookup"><span data-stu-id="2311d-227">Update the Edit page</span></span>

<span data-ttu-id="2311d-228">Aktualisieren Sie *Pages/Courses/Edit.cshtml* mithilfe des folgenden Markups:</span><span class="sxs-lookup"><span data-stu-id="2311d-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="2311d-229">Das obenstehende Markup:</span><span class="sxs-lookup"><span data-stu-id="2311d-229">The preceding markup:</span></span>

* <span data-ttu-id="2311d-230">Aktualisiert die `page`-Anweisung von `@page` auf `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2311d-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2311d-231">Fügt eine ausgeblendete Zeilenversion hinzu.</span><span class="sxs-lookup"><span data-stu-id="2311d-231">Adds a hidden row version.</span></span> <span data-ttu-id="2311d-232">`RowVersion` muss hinzugefügt werden, damit über ein Postback-Ereignis der Wert gebunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="2311d-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="2311d-233">Zeigt das letzte Byte von `RowVersion` zu Debugzwecken an.</span><span class="sxs-lookup"><span data-stu-id="2311d-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="2311d-234">Ersetzt `ViewData` durch den stark typisierten `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="2311d-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="2311d-235">Testen von Nebenläufigkeitskonflikten mit der Seite „Bearbeiten“</span><span class="sxs-lookup"><span data-stu-id="2311d-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="2311d-236">Öffnen Sie für den englischen Fachbereich zwei Browserinstanzen der Seite „Bearbeiten“:</span><span class="sxs-lookup"><span data-stu-id="2311d-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="2311d-237">Führen Sie die Anwendung aus, und wählen Sie „Abteilungen“ aus.</span><span class="sxs-lookup"><span data-stu-id="2311d-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="2311d-238">Klicken Sie mit der rechten Maustaste auf den Link **Bearbeiten** für den englischen Fachbereich, und klicken Sie auf **In neuer Registerkarte öffnen**.</span><span class="sxs-lookup"><span data-stu-id="2311d-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2311d-239">Klicken Sie in der ersten Registerkarte auf den **Bearbeiten**-Link für den englischen Fachbereich.</span><span class="sxs-lookup"><span data-stu-id="2311d-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="2311d-240">Beide Registerkarten zeigen die gleichen Informationen an.</span><span class="sxs-lookup"><span data-stu-id="2311d-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2311d-241">Ändern Sie den Namen in der ersten Registerkarte, und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="2311d-241">Change the name in the first browser tab and click **Save**.</span></span>

![Seite 1 „Abteilung bearbeiten“ nach der Änderung](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="2311d-243">Der Browser zeigt die Indexseite mit dem geänderten Wert und dem aktualisierten RowVersion-Indikator an.</span><span class="sxs-lookup"><span data-stu-id="2311d-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2311d-244">Beachten Sie, dass der aktualisierte RowVersion-Indikator beim zweiten Postback-Ereignis auf der anderen Registerkarte angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-244">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2311d-245">Ändern Sie ein anderes Feld in der zweiten Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="2311d-245">Change a different field in the second browser tab.</span></span>

![Seite 2 „Abteilung bearbeiten“ nach der Änderung](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="2311d-247">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="2311d-247">Click **Save**.</span></span> <span data-ttu-id="2311d-248">Es werden Fehlermeldungen für alle Felder angezeigt, die nicht mit den Datenbankwerten übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="2311d-248">You see error messages for all fields that don't match the DB values:</span></span>

![Fehlermeldung auf der Seite „Abteilung bearbeiten“](concurrency/_static/edit-error.png)

<span data-ttu-id="2311d-250">In diesem Browserfenster sollte nicht das Namensfeld geändert werden.</span><span class="sxs-lookup"><span data-stu-id="2311d-250">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="2311d-251">Kopieren Sie den aktuellen Wert (Sprachen), und fügen Sie ihn in das Namensfeld ein.</span><span class="sxs-lookup"><span data-stu-id="2311d-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="2311d-252">Wechseln Sie durch Drücken der TAB-Taste zum nächsten Feld. Im Rahmen der clientseitigen Überprüfung wird die Fehlermeldung entfernt.</span><span class="sxs-lookup"><span data-stu-id="2311d-252">Tab out. Client-side validation removes the error message.</span></span>

![Fehlermeldung auf der Seite „Abteilung bearbeiten“](concurrency/_static/cv.png)

<span data-ttu-id="2311d-254">Klicken Sie erneut auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="2311d-254">Click **Save** again.</span></span> <span data-ttu-id="2311d-255">Der Wert, den Sie auf der zweiten Registerkarte eingegeben haben, wird gespeichert.</span><span class="sxs-lookup"><span data-stu-id="2311d-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="2311d-256">Die gespeicherten Werte werden auf der Indexseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2311d-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="2311d-257">Aktualisieren der Seite „Delete“ (Löschen)</span><span class="sxs-lookup"><span data-stu-id="2311d-257">Update the Delete page</span></span>

<span data-ttu-id="2311d-258">Aktualisieren Sie das Seitenmodell „Löschen“ mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2311d-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="2311d-259">Die Seite „Löschen“ erkennt Nebenläufigkeitskonflikte, wenn die Entität geändert wurde, nachdem sie abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="2311d-260">Bei `Department.RowVersion` handelt es sich um die Zeilenversion, nachdem die Entität abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="2311d-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="2311d-261">Wenn EF Core den SQL-DELETE-Befehl erstellt, umfasst dieser eine WHERE-Klausel mit `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2311d-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="2311d-262">Wenn die Ergebnisse des SQL-DELETE-Befehls 0 (null) betroffene Zeilen ergeben:</span><span class="sxs-lookup"><span data-stu-id="2311d-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="2311d-263">Stimmt die `RowVersion` im SQL-DELETE-Befehl nicht mit `RowVersion` in der Datenbank überein.</span><span class="sxs-lookup"><span data-stu-id="2311d-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="2311d-264">Wird eine DbUpdateConcurrencyException-Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2311d-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="2311d-265">Wird `OnGetAsync` mit `concurrencyError` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2311d-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="2311d-266">Aktualisieren der Seite „Delete“ (Löschen)</span><span class="sxs-lookup"><span data-stu-id="2311d-266">Update the Delete page</span></span>

<span data-ttu-id="2311d-267">Aktualisieren Sie die *Pages\Departments\Delete.cshtml*-Datei mithilfe des folgenden Codes:</span><span class="sxs-lookup"><span data-stu-id="2311d-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="2311d-268">Das oben stehende Markup führt die folgenden Änderungen durch:</span><span class="sxs-lookup"><span data-stu-id="2311d-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="2311d-269">Aktualisiert die `page`-Anweisung von `@page` auf `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2311d-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2311d-270">Eine Fehlermeldung wird hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="2311d-270">Adds an error message.</span></span>
* <span data-ttu-id="2311d-271">„FirstMidName“ wird durch „FullName“ im Feld **Administrator** ersetzt.</span><span class="sxs-lookup"><span data-stu-id="2311d-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="2311d-272">`RowVersion` wird geändert, um das letzte Byte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2311d-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="2311d-273">Fügt eine ausgeblendete Zeilenversion hinzu.</span><span class="sxs-lookup"><span data-stu-id="2311d-273">Adds a hidden row version.</span></span> <span data-ttu-id="2311d-274">`RowVersion` muss hinzugefügt werden, damit über ein Postback-Ereignis der Wert gebunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="2311d-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="2311d-275">Überprüfen von Nebenläufigkeitskonflikten mit der Seite „Löschen“</span><span class="sxs-lookup"><span data-stu-id="2311d-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="2311d-276">Erstellen Sie einen Fachbereich zum Testen.</span><span class="sxs-lookup"><span data-stu-id="2311d-276">Create a test department.</span></span>

<span data-ttu-id="2311d-277">Öffnen Sie im Testfachbereich zwei Browserinstanzen zum „Löschen“:</span><span class="sxs-lookup"><span data-stu-id="2311d-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="2311d-278">Führen Sie die Anwendung aus, und wählen Sie „Abteilungen“ aus.</span><span class="sxs-lookup"><span data-stu-id="2311d-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="2311d-279">Klicken Sie mit der rechten Maustaste auf den Link **Löschen** für den Testfachbereich, und klicken Sie auf**In neuer Registerkarte öffnen**.</span><span class="sxs-lookup"><span data-stu-id="2311d-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2311d-280">Klicken Sie auf den Link **Bearbeiten** für den Testfachbereich.</span><span class="sxs-lookup"><span data-stu-id="2311d-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="2311d-281">Beide Registerkarten zeigen die gleichen Informationen an.</span><span class="sxs-lookup"><span data-stu-id="2311d-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2311d-282">Ändern Sie das Budget in der ersten Registerkarte, und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="2311d-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="2311d-283">Der Browser zeigt die Indexseite mit dem geänderten Wert und dem aktualisierten RowVersion-Indikator an.</span><span class="sxs-lookup"><span data-stu-id="2311d-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2311d-284">Beachten Sie, dass der aktualisierte RowVersion-Indikator beim zweiten Postback-Ereignis auf der anderen Registerkarte angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2311d-284">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2311d-285">Löschen Sie den Testfachbereich aus der zweiten Registerkarte. Ein Parallelitätsfehler wird mit den aktuellen Werten aus der Datenbank angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2311d-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="2311d-286">Klicken Sie auf **Löschen**, wird die Entität gelöscht, es sei denn, `RowVersion` wurde aktualisiert und der Fachbereich wurde gelöscht.</span><span class="sxs-lookup"><span data-stu-id="2311d-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="2311d-287">Informationen zum Vererben eines Datenmodells finden Sie unter [Vererbung](xref:data/ef-mvc/inheritance).</span><span class="sxs-lookup"><span data-stu-id="2311d-287">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2311d-288">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2311d-288">Additional resources</span></span>

* [<span data-ttu-id="2311d-289">Concurrency Tokens in EF Core (Parallelitätstoken in EF Core)</span><span class="sxs-lookup"><span data-stu-id="2311d-289">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="2311d-290">Handle concurrency in EF Core (Handhabung von Parallelität in EF Core)</span><span class="sxs-lookup"><span data-stu-id="2311d-290">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="2311d-291">Vorherige</span><span class="sxs-lookup"><span data-stu-id="2311d-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
