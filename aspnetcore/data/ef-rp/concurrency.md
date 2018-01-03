---
title: "Razor-Seiten mit EF - Parallelität - 8 von 8-Kern"
author: rick-anderson
description: "Dieses Lernprogramm zeigt, wie Konflikte zu behandeln, wenn mehrere Benutzer gleichzeitig derselben Entität aktualisieren."
keywords: "ASP.NET Core, Entity Framework Core, Parallelität"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 8862c6b9a5eb7ac3b6889071e4ce9ff6f02512c9
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
<span data-ttu-id="1b67a-104">En-us /</span><span class="sxs-lookup"><span data-stu-id="1b67a-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="1b67a-105">Parallelitätskonflikte - EF-Core mit Razor-Seiten (8 von 8)</span><span class="sxs-lookup"><span data-stu-id="1b67a-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="1b67a-106">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), und [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="1b67a-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="1b67a-107">Dieses Lernprogramm veranschaulicht, wie Konflikte behandelt wird, wenn mehrere Benutzer gleichzeitig (gleichzeitig) eine Entität aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="1b67a-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="1b67a-108">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="1b67a-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="1b67a-109">Parallelitätskonflikte</span><span class="sxs-lookup"><span data-stu-id="1b67a-109">Concurrency conflicts</span></span>

<span data-ttu-id="1b67a-110">Parallelitätskonflikt tritt auf, wenn:</span><span class="sxs-lookup"><span data-stu-id="1b67a-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="1b67a-111">Ein Benutzer navigiert zu der Seite "Bearbeiten" für eine Entität.</span><span class="sxs-lookup"><span data-stu-id="1b67a-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="1b67a-112">Ein anderer Benutzer aktualisiert dieselbe Entität, bevor die erste Änderung des Benutzers mit der Datenbank geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="1b67a-113">Wenn die Erkennung der Parallelität nicht aktiviert ist, beim Auftreten von gleichzeitige Updates:</span><span class="sxs-lookup"><span data-stu-id="1b67a-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="1b67a-114">Das letzte Update gewinnt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-114">The last update wins.</span></span> <span data-ttu-id="1b67a-115">D. h. werden die letzten Aktualisieren von Werten mit der Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="1b67a-116">Die erste von der aktuellen Updates verloren.</span><span class="sxs-lookup"><span data-stu-id="1b67a-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="1b67a-117">Optimistische Nebenläufigkeit</span><span class="sxs-lookup"><span data-stu-id="1b67a-117">Optimistic concurrency</span></span>

<span data-ttu-id="1b67a-118">Vollständige Parallelität ermöglicht Parallelitätskonflikte durchgeführt werden soll, und klicken Sie dann reagiert entsprechend Wenn sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="1b67a-119">Z. B. Andrea besucht die Abteilung bearbeiten (Seite) und ändert sich das Budget für die englischen Abteilung von $350,000.00 in 0,00.</span><span class="sxs-lookup"><span data-stu-id="1b67a-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Ändern Sie das Budget auf 0](concurrency/_static/change-budget.png)

<span data-ttu-id="1b67a-121">Bevor Andrea klickt **speichern**, John derselben Seite besuchen, und ändert sich der Start Date-Felds von 9/1/2007, 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="1b67a-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Ändern von Startdatum in 2013](concurrency/_static/change-date.png)

<span data-ttu-id="1b67a-123">Andrea klickt **speichern** erste und sieht ihr ändern, wenn der Browser die Indexseite anzeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budget in NULL geändert](concurrency/_static/budget-zero.png)

<span data-ttu-id="1b67a-125">John klickt **speichern** auf eine Bearbeitungsseite, die ein Budget von $350,000.00 weiterhin angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="1b67a-126">Was daraufhin geschieht richtet sich nach der Behandlung von Parallelitätskonflikten.</span><span class="sxs-lookup"><span data-stu-id="1b67a-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="1b67a-127">Vollständige Parallelität umfasst die folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="1b67a-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="1b67a-128">Sie können Nachverfolgen von ein Benutzer geändert hat, dessen Eigenschaft und nur die entsprechenden Spalten in der Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="1b67a-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="1b67a-129">In diesem Szenario würden keine Daten verloren.</span><span class="sxs-lookup"><span data-stu-id="1b67a-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="1b67a-130">Unterschiedliche Eigenschaften wurden durch die beiden Benutzer aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="1b67a-131">Das nächste Mal, das eine Person die englische Abteilung durchsucht, werden Janes und Peters Änderungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="1b67a-132">Diese Methode zur Aktualisierung reduzieren die Anzahl der Konflikte, die zu Datenverlusten führen können.</span><span class="sxs-lookup"><span data-stu-id="1b67a-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="1b67a-133">Dieser Ansatz: * Datenverlust nicht vermieden werden, wenn die gleiche Eigenschaft konkurrierende geändert werden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="1b67a-134">* Wird im Allgemeinen nicht besonders praktisch in einer Web-app.</span><span class="sxs-lookup"><span data-stu-id="1b67a-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="1b67a-135">Sie erfordert erhebliche Zustand beibehalten, um nachzuverfolgen, um alle abgerufenen Werte und neue Werte.</span><span class="sxs-lookup"><span data-stu-id="1b67a-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="1b67a-136">Verwalten von großen Datenmengen Zustand kann die app-Leistung beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="1b67a-137">* Können app-Komplexität, die im Vergleich zur Erkennung der Parallelität für eine Entität zu erhöhen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="1b67a-138">Sie können Peters Änderung Janes Änderung überschreiben lassen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="1b67a-139">Das nächste Mal eine Person durchsucht die englische Abteilung, sehen sie, 9/1/2013 und die abgerufenen $350,000.00-Wert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="1b67a-140">Dieser Ansatz nennt man eine *Client gewinnt* oder *Last in Wins* Szenario.</span><span class="sxs-lookup"><span data-stu-id="1b67a-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="1b67a-141">(Alle Werte aus dem Client haben Vorrang vor was im Datenspeicher ist.) Wenn Sie die Codierung für Parallelitätsbehandlung nicht tun, Wins-Client wird automatisch durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="1b67a-142">Sie können verhindern, dass Peters Änderung in der Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="1b67a-143">Die app in der Regel würden: * Zeigt eine Fehlermeldung an.</span><span class="sxs-lookup"><span data-stu-id="1b67a-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="1b67a-144">* Zeigen Sie den aktuellen Zustand der Daten.</span><span class="sxs-lookup"><span data-stu-id="1b67a-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="1b67a-145">* Ermöglicht dem Benutzer, die Änderungen erneut anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="1b67a-146">Hierbei spricht einen *Store Wins* Szenario.</span><span class="sxs-lookup"><span data-stu-id="1b67a-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="1b67a-147">(Der Datenspeicher Werte haben Vorrang vor den Werten, die vom Client gesendet.) In diesem Lernprogramm implementieren Sie die Wins-Store-Szenario.</span><span class="sxs-lookup"><span data-stu-id="1b67a-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="1b67a-148">Diese Methode wird sichergestellt, dass keine Änderungen überschrieben werden, ohne dass ein Benutzer wird benachrichtigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="1b67a-149">Behandeln von Parallelität</span><span class="sxs-lookup"><span data-stu-id="1b67a-149">Handling concurrency</span></span> 

<span data-ttu-id="1b67a-150">Wenn eine Eigenschaft konfiguriert ist, als ein [parallelitätstoken](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="1b67a-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="1b67a-151">EF Core stellt sicher, dass die Eigenschaft nicht geändert wurde, nachdem es abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="1b67a-152">Die Überprüfung tritt auf, wenn [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) oder [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="1b67a-153">Wenn die Eigenschaft geändert wurde, nachdem es abgerufen wurde, eine [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="1b67a-154">Die DB- und Datenmodell must be configured auslösen unterstützen `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="1b67a-155">Erkennen von Konflikten bei der Parallelität für eine Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="1b67a-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="1b67a-156">Parallelitätskonflikte erkannt werden können, auf der Eigenschaftenebene mit der [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) Attribut.</span><span class="sxs-lookup"><span data-stu-id="1b67a-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="1b67a-157">Das Attribut kann mehreren Eigenschaften für das Modell angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="1b67a-158">Weitere Informationen finden Sie unter [Daten Anmerkungen-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="1b67a-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="1b67a-159">Die `[ConcurrencyCheck]` Attribut wird in diesem Lernprogramm nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="1b67a-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="1b67a-160">Erkennen von Konflikten bei der Parallelität in einer Zeile</span><span class="sxs-lookup"><span data-stu-id="1b67a-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="1b67a-161">Erkennen der Parallelitätskonflikte, eine [Rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) nachverfolgung Spalte mit dem Modell hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="1b67a-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="1b67a-162">`rowversion` :</span></span>

* <span data-ttu-id="1b67a-163">SQL Server bezieht.</span><span class="sxs-lookup"><span data-stu-id="1b67a-163">Is SQL Server specific.</span></span> <span data-ttu-id="1b67a-164">Andere Datenbanken bietet eine ähnliche Funktion möglicherweise nicht.</span><span class="sxs-lookup"><span data-stu-id="1b67a-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="1b67a-165">Wird verwendet, um zu bestimmen, dass eine Entität nicht geändert wurde, seit dem Abruf aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1b67a-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="1b67a-166">Die Datenbank generiert einen sequenziellen `rowversion` Anzahl, die jede Zeile erhöht wurde aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="1b67a-167">In einer `Update` oder `Delete` Befehl, der `Where` -Klausel enthält den abgerufenen Wert des `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="1b67a-168">Wenn die Zeile aktualisiert wird, hat sich geändert:</span><span class="sxs-lookup"><span data-stu-id="1b67a-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="1b67a-169">`rowversion`entspricht nicht den abgerufenen Wert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="1b67a-170">Die `Update` oder `Delete` Befehle eine Zeile nicht finden kann, da die `Where` -Klausel enthält das abgerufene `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="1b67a-171">Ein `DbUpdateConcurrencyException` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="1b67a-172">In EF Kern ausgeführt, wenn keine Zeilen durch aktualisiert wurden eine `Update` oder `Delete` Befehls, eine Parallelitätsausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="1b67a-173">Hinzufügen einer Überwachung-Eigenschaft auf die Entität Department</span><span class="sxs-lookup"><span data-stu-id="1b67a-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="1b67a-174">In *Models/Department.cs*, Hinzufügen einer Überwachung-Eigenschaft, die mit dem Namen RowVersion:</span><span class="sxs-lookup"><span data-stu-id="1b67a-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="1b67a-175">Die [Zeitstempel](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) Attribut gibt an, dass diese Spalte, in enthalten ist der `Where` -Klausel der `Update` und `Delete` Befehle.</span><span class="sxs-lookup"><span data-stu-id="1b67a-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="1b67a-176">Das Attribut heißt `Timestamp` da frühere Versionen von SQL Server eine SQL verwendet `timestamp` -Datentyp, bevor SQL `rowversion` Typ ersetzt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="1b67a-177">Die fluent-API kann auch die Tracking-Eigenschaft angeben:</span><span class="sxs-lookup"><span data-stu-id="1b67a-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="1b67a-178">Der folgende Code zeigt einen Teil der T-SQL-von EF Core generiert, wenn der Name der Abteilung aktualisiert wird:</span><span class="sxs-lookup"><span data-stu-id="1b67a-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="1b67a-179">Das vorherige Codebeispiel markiert die `WHERE` Klausel mit `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="1b67a-180">Wenn der DB `RowVersion` entspricht nicht der `RowVersion` Parameter (`@p2`), keine Zeilen aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="1b67a-181">Die folgende hervorgehobene Code zeigt das T-SQL, die überprüft, dass genau eine Zeile aktualisiert wurde:</span><span class="sxs-lookup"><span data-stu-id="1b67a-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="1b67a-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) gibt die Anzahl der von der letzten Anweisung betroffenen Zeilen zurück.</span><span class="sxs-lookup"><span data-stu-id="1b67a-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="1b67a-183">In keine Zeilen aktualisiert werden, EF Core löst eine `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="1b67a-184">Sie können sehen, dass die T-SQL EF Core im Ausgabefenster von Visual Studio generiert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="1b67a-185">Aktualisieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="1b67a-185">Update the DB</span></span>

<span data-ttu-id="1b67a-186">Hinzufügen der `RowVersion` Eigenschaft ändert das DB-Modell, die eine Migration erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="1b67a-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="1b67a-187">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-187">Build the project.</span></span> <span data-ttu-id="1b67a-188">Geben Sie Folgendes in einem Befehlsfenster aus:</span><span class="sxs-lookup"><span data-stu-id="1b67a-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="1b67a-189">Die vorherigen Befehle:</span><span class="sxs-lookup"><span data-stu-id="1b67a-189">The preceding commands:</span></span>

* <span data-ttu-id="1b67a-190">Fügt der *Migrationen / {Time stamp}_RowVersion.cs* Migrationsdatei.</span><span class="sxs-lookup"><span data-stu-id="1b67a-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="1b67a-191">Updates der *Migrations/SchoolContextModelSnapshot.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="1b67a-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="1b67a-192">Das Update fügt die folgenden hervorgehobenen Code in die `BuildModel` Methode:</span><span class="sxs-lookup"><span data-stu-id="1b67a-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="1b67a-193">Führt Migrationen, um die Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="1b67a-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="1b67a-194">Das Gerüst für die Abteilungen-Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="1b67a-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="1b67a-195">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b67a-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="1b67a-196">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="1b67a-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1b67a-197">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1b67a-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="1b67a-198">Die vorherigen Befehl Gerüste der `Department` Modell.</span><span class="sxs-lookup"><span data-stu-id="1b67a-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="1b67a-199">Öffnen Sie das Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b67a-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="1b67a-200">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-200">Build the project.</span></span> <span data-ttu-id="1b67a-201">Der Build generiert Fehler wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1b67a-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="1b67a-202">Globales ändern `_context.Department` auf `_context.Departments` (d. h. hinzufügen ein "s" `Department`).</span><span class="sxs-lookup"><span data-stu-id="1b67a-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="1b67a-203">7 Vorkommen gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="1b67a-204">Aktualisieren Sie die Abteilungen Indexseite</span><span class="sxs-lookup"><span data-stu-id="1b67a-204">Update the Departments Index page</span></span>

<span data-ttu-id="1b67a-205">Das Gerüst Modul erstellt eine `RowVersion` Spalte für die Seite "Index", aber dieses Feld darf nicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1b67a-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="1b67a-206">In diesem Lernprogramm, das letzte Byte der `RowVersion` wird angezeigt, um die Parallelität zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="1b67a-207">Das letzte Byte ist nicht unbedingt eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="1b67a-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="1b67a-208">Eine wirkliche app wäre nicht anzeigen `RowVersion` oder das letzte Byte der `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="1b67a-209">Aktualisieren Sie die Seite "Index":</span><span class="sxs-lookup"><span data-stu-id="1b67a-209">Update the Index page:</span></span>

* <span data-ttu-id="1b67a-210">Abteilungen Index ersetzt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="1b67a-211">Ersetzen Sie das Markup mit `RowVersion` mit das letzte Byte der `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="1b67a-212">FullName FirstMidName ersetzt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="1b67a-213">Das folgende Markup zeigt aktualisierte Seite:</span><span class="sxs-lookup"><span data-stu-id="1b67a-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="1b67a-214">Aktualisieren Sie das Modell bearbeiten</span><span class="sxs-lookup"><span data-stu-id="1b67a-214">Update the Edit page model</span></span>

<span data-ttu-id="1b67a-215">Update *pages\departments\edit.cshtml.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1b67a-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="1b67a-216">Um ein Problem Parallelität erkennen die [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) wird aktualisiert und enthält die `rowVersion` Wert aus der Entität, die es abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="1b67a-217">EF Core generiert einen SQL-Befehl UPDATE mit einer WHERE-Klausel mit der ursprünglichen `RowVersion` Wert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="1b67a-218">Wenn keine Zeilen, durch den Befehl UPDATE betroffen sind (keine Zeilen vorhanden sind, die ursprüngliche `RowVersion` Wert), wird eine `DbUpdateConcurrencyException` Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="1b67a-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="1b67a-219">Im vorangehenden Code `Department.RowVersion` ist der Wert die Entität abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="1b67a-220">`OriginalValue`ist der Wert in der DB bei `FirstOrDefaultAsync` in diese Methode aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="1b67a-221">Der folgende Code Ruft die Client-Werte (die Werte für diese Methode bereitgestellten) und die DB-Werte:</span><span class="sxs-lookup"><span data-stu-id="1b67a-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="1b67a-222">Der folgende Code Fügt eine benutzerdefinierte Fehlermeldung für jede Spalte, die DB wurde unterscheiden, was Werte zu anweisungsbereitstellung `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="1b67a-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="1b67a-223">Die folgenden hervorgehobenen Code legt die `RowVersion` Wert in den neuen Wert aus der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="1b67a-224">Das nächste Mal, die der Benutzer klickt auf **speichern**, nur Parallelitätsfehlern, die auftreten, seit die letzten Anzeige Bearbeitungsseite abgefangen wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="1b67a-225">Die `ModelState.Remove` Anweisung ist erforderlich, da `ModelState` hat die alte `RowVersion` Wert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="1b67a-226">Der Razor-Seite der `ModelState` Wert für ein Feld Vorrang vor den Modell-Eigenschaftswerte hat, wenn beide vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="1b67a-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="1b67a-227">Aktualisieren Sie die Seite "Bearbeiten"</span><span class="sxs-lookup"><span data-stu-id="1b67a-227">Update the Edit page</span></span>

<span data-ttu-id="1b67a-228">Update *Pages/Departments/Edit.cshtml* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="1b67a-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="1b67a-229">Die vorangehenden Markup:</span><span class="sxs-lookup"><span data-stu-id="1b67a-229">The preceding markup:</span></span>

* <span data-ttu-id="1b67a-230">Updates der `page` -Direktive aus `@page` auf `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="1b67a-231">Fügt eine ausgeblendete Zeilenversion.</span><span class="sxs-lookup"><span data-stu-id="1b67a-231">Adds a hidden row version.</span></span> <span data-ttu-id="1b67a-232">`RowVersion`muss hinzugefügt werden, damit Postback den Wert gebunden wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="1b67a-233">Zeigt das letzte Byte der `RowVersion` zu Debugzwecken.</span><span class="sxs-lookup"><span data-stu-id="1b67a-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="1b67a-234">Ersetzt `ViewData` mit starker Typisierung `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="1b67a-235">Testen von Parallelitätskonflikten der Seite "Bearbeiten"</span><span class="sxs-lookup"><span data-stu-id="1b67a-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="1b67a-236">Öffnen Sie zwei Instanzen von Browsern Bearbeitung auf Englisch-Abteilung:</span><span class="sxs-lookup"><span data-stu-id="1b67a-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="1b67a-237">Führen Sie die app, und wählen Sie Abteilungen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="1b67a-238">Mit der rechten Maustaste die **bearbeiten** Link für die englischen Abteilung, und wählen **in neuer Registerkarte öffnen**.</span><span class="sxs-lookup"><span data-stu-id="1b67a-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="1b67a-239">Klicken Sie in der ersten Registerkarte auf die **bearbeiten** Link für die englischen Abteilung.</span><span class="sxs-lookup"><span data-stu-id="1b67a-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="1b67a-240">Die zwei browserregisterkarten werden dieselben Informationen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="1b67a-241">Ändern Sie den Namen in der ersten Registerkarte ' Browser ', und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="1b67a-241">Change the name in the first browser tab and click **Save**.</span></span>

![Abteilung bearbeiten Seite 1 nach der Änderung](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="1b67a-243">Der Browser zeigt die Indexseite mit den geänderten Wert und die aktualisierte RowVersion-Indikator.</span><span class="sxs-lookup"><span data-stu-id="1b67a-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="1b67a-244">Beachten Sie die aktualisierte RowVersion-Indikator, wird er auf das zweite Postback auf der anderen Registerkarte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="1b67a-245">Ändern Sie ein anderes Feld in der zweiten Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="1b67a-245">Change a different field in the second browser tab.</span></span>

![Abteilung bearbeiten Seite 2 nach der Änderung](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="1b67a-247">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="1b67a-247">Click **Save**.</span></span> <span data-ttu-id="1b67a-248">Fehlermeldungen für alle Felder, die die DB-Werte nicht übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="1b67a-248">You see error messages for all fields that don't match the DB values:</span></span>

![Abteilung bearbeiten Seite Fehlermeldung](concurrency/_static/edit-error.png)

<span data-ttu-id="1b67a-250">Dieses Browserfenster wollten nicht so ändern Sie das Feld Name.</span><span class="sxs-lookup"><span data-stu-id="1b67a-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="1b67a-251">Kopieren Sie den aktuellen Wert (Sprachen) in das Feld "Name".</span><span class="sxs-lookup"><span data-stu-id="1b67a-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="1b67a-252">Registerkarte "aus. Die clientseitige Validierung entfernt die Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-252">Tab out. Client-side validation removes the error message.</span></span>

![Abteilung bearbeiten Seite Fehlermeldung](concurrency/_static/cv.png)

<span data-ttu-id="1b67a-254">Klicken Sie auf **speichern** erneut aus.</span><span class="sxs-lookup"><span data-stu-id="1b67a-254">Click **Save** again.</span></span> <span data-ttu-id="1b67a-255">Der Wert in der zweiten Registerkarte eingegebene wird gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1b67a-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="1b67a-256">Daraufhin werden die gespeicherten Werte in der Indexseite.</span><span class="sxs-lookup"><span data-stu-id="1b67a-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="1b67a-257">Aktualisieren Sie die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="1b67a-257">Update the Delete page</span></span>

<span data-ttu-id="1b67a-258">Aktualisieren Sie das Modell löschen, durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1b67a-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="1b67a-259">Die Seite "löschen" erkennt Parallelitätskonflikte, wenn die Entität geändert wurde, nachdem es abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="1b67a-260">`Department.RowVersion`ist die Zeilenversion an, wenn es sich bei die Entität abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="1b67a-261">Wenn EF Kern den SQL-DELETE-Befehl erstellt, enthält Sie eine WHERE-Klausel mit `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="1b67a-262">Wenn wirkt sich die Ergebnisse der SQL-DELETE-Befehl in 0 (null) Zeilen:</span><span class="sxs-lookup"><span data-stu-id="1b67a-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="1b67a-263">Die `RowVersion` in den SQL Delete-passt nicht Befehl `RowVersion` in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1b67a-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="1b67a-264">Eine DbUpdateConcurrencyException Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="1b67a-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="1b67a-265">`OnGetAsync`wird aufgerufen, mit der `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="1b67a-266">Aktualisieren Sie die Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="1b67a-266">Update the Delete page</span></span>

<span data-ttu-id="1b67a-267">Update *Pages/Departments/Delete.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1b67a-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="1b67a-268">Das vorhergehende Markup nimmt folgende Änderungen:</span><span class="sxs-lookup"><span data-stu-id="1b67a-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="1b67a-269">Updates der `page` -Direktive aus `@page` auf `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1b67a-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="1b67a-270">Fügt eine Fehlermeldung an.</span><span class="sxs-lookup"><span data-stu-id="1b67a-270">Adds an error message.</span></span>
* <span data-ttu-id="1b67a-271">Ersetzt FirstMidName mit FullName in den **Administrator** Feld.</span><span class="sxs-lookup"><span data-stu-id="1b67a-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="1b67a-272">Änderungen `RowVersion` das letzte Byte an.</span><span class="sxs-lookup"><span data-stu-id="1b67a-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="1b67a-273">Fügt eine ausgeblendete Zeilenversion.</span><span class="sxs-lookup"><span data-stu-id="1b67a-273">Adds a hidden row version.</span></span> <span data-ttu-id="1b67a-274">`RowVersion`muss hinzugefügt werden, damit Postback den Wert gebunden wird.</span><span class="sxs-lookup"><span data-stu-id="1b67a-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="1b67a-275">Testen von Parallelitätskonflikten der Seite "löschen"</span><span class="sxs-lookup"><span data-stu-id="1b67a-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="1b67a-276">Erstellen Sie eine Test-Abteilung.</span><span class="sxs-lookup"><span data-stu-id="1b67a-276">Create a test department.</span></span>

<span data-ttu-id="1b67a-277">Öffnen Sie zwei Instanzen von Browsern von Delete für die Test-Abteilung:</span><span class="sxs-lookup"><span data-stu-id="1b67a-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="1b67a-278">Führen Sie die app, und wählen Sie Abteilungen.</span><span class="sxs-lookup"><span data-stu-id="1b67a-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="1b67a-279">Mit der rechten Maustaste die **löschen** Link für den Test-Abteilung, und wählen **in neuer Registerkarte öffnen**.</span><span class="sxs-lookup"><span data-stu-id="1b67a-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="1b67a-280">Klicken Sie auf die **bearbeiten** Link für die Test-Abteilung.</span><span class="sxs-lookup"><span data-stu-id="1b67a-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="1b67a-281">Die zwei browserregisterkarten werden dieselben Informationen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="1b67a-282">Ändern Sie das Budget in der ersten Registerkarte ' Browser ', und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="1b67a-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="1b67a-283">Der Browser zeigt die Indexseite mit den geänderten Wert und die aktualisierte RowVersion-Indikator.</span><span class="sxs-lookup"><span data-stu-id="1b67a-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="1b67a-284">Beachten Sie die aktualisierte RowVersion-Indikator, wird er auf das zweite Postback auf der anderen Registerkarte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="1b67a-285">Löschen Sie die Test-Abteilung, auf der zweiten Registerkarte. Ein Parallelitätsfehler ist, werden mit den aktuellen Werten aus der Datenbank angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1b67a-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="1b67a-286">Auf **löschen** wird die Entität gelöscht, es sei denn, `RowVersion` updated.department wurde gelöscht wurde.</span><span class="sxs-lookup"><span data-stu-id="1b67a-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="1b67a-287">Finden Sie unter [Vererbung](xref:data/ef-mvc/inheritance) Anweisungen zur Vererbung im Datenmodell.</span><span class="sxs-lookup"><span data-stu-id="1b67a-287">See [Inheritance](xref:data/ef-mvc/inheritance) for instruction on how to inheritance in the data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1b67a-288">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1b67a-288">Additional resources</span></span>

* [<span data-ttu-id="1b67a-289">Parallelitätstoken in EF Core</span><span class="sxs-lookup"><span data-stu-id="1b67a-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="1b67a-290">Behandeln von Parallelität in EF Core</span><span class="sxs-lookup"><span data-stu-id="1b67a-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="1b67a-291">Vorherige</span><span class="sxs-lookup"><span data-stu-id="1b67a-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
