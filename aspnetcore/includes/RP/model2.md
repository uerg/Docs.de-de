<span data-ttu-id="ea522-101">Fügen Sie der Klasse `Movie` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="ea522-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="ea522-102">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="ea522-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="ea522-103">Hinzufügen einer Datenbankkontext-Klasse</span><span class="sxs-lookup"><span data-stu-id="ea522-103">Add a database context class</span></span>

<span data-ttu-id="ea522-104">Fügen Sie dem Ordner *Modelle* die folgende von `DbContext` abgeleitete Klasse namens *MovieContext.cs* hinzu: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="ea522-104">Add the following `DbContext` derived class named *MovieContext.cs* to the *Models* folder: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="ea522-105">Der vorangehende Code erstellt eine `DbSet`-Eigenschaft für die Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="ea522-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="ea522-106">In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle, und eine Entität entspricht einer Zeile in einer Tabelle.</span><span class="sxs-lookup"><span data-stu-id="ea522-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
