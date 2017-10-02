<span data-ttu-id="9f9c3-101">Fügen Sie der Klasse `Movie` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="9f9c3-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="9f9c3-102">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="9f9c3-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="9f9c3-103">Hinzufügen einer Datenbankkontext-Klasse</span><span class="sxs-lookup"><span data-stu-id="9f9c3-103">Add a database context class</span></span>

<span data-ttu-id="9f9c3-104">Fügen Sie dem Ordner *Modelle* eine von `DbContext` abgeleitete Klasse namens *MovieContext.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="9f9c3-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

<span data-ttu-id="9f9c3-105">Der vorangehende Code erstellt eine `DbSet`-Eigenschaft für die Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="9f9c3-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="9f9c3-106">In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle, und eine Entität entspricht einer Zeile in einer Tabelle.</span><span class="sxs-lookup"><span data-stu-id="9f9c3-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="9f9c3-107">Der Name der Eigenschaft `DbSet` ist `Movies`.</span><span class="sxs-lookup"><span data-stu-id="9f9c3-107">The `DbSet` property name is `Movies`.</span></span> <span data-ttu-id="9f9c3-108">Da die Datenbank Namen im Singular verwendet, setzt das Beispiel `OnModelCreating` außer Kraft, um die Form im Singular (`Movie`) als Tabellenname zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9f9c3-108">Since the database uses singular names, the sample overrides `OnModelCreating` to use the singular form (`Movie`) for the table name.</span></span>
