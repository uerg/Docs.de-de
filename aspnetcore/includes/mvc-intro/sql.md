# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="c0afb-101">Arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="c0afb-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="c0afb-102">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0afb-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0afb-103">Das `MvcMovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c0afb-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="c0afb-104">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="c0afb-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="c0afb-105">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]</span><span class="sxs-lookup"><span data-stu-id="c0afb-105">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]</span></span>

## <a name="sqlite"></a><span data-ttu-id="c0afb-106">SQLite</span><span class="sxs-lookup"><span data-stu-id="c0afb-106">SQLite</span></span>

<span data-ttu-id="c0afb-107">Auf der [SQLite](https://www.sqlite.org/)-Website ist zu lesen:</span><span class="sxs-lookup"><span data-stu-id="c0afb-107">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="c0afb-108">SQLite ist ein eigenständiges, sehr zuverlässiges, eingebettetes, genehmigungsfreies SQL-Datenbankmodul mit vollen Funktionsumfang.</span><span class="sxs-lookup"><span data-stu-id="c0afb-108">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="c0afb-109">SQLite ist das weltweit am häufigsten verwendete Datenbankmodul.</span><span class="sxs-lookup"><span data-stu-id="c0afb-109">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="c0afb-110">Es gibt viele Tools von Drittanbietern, die Sie herunterladen können, um eine SQLite-Datenbank zu verwalten und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c0afb-110">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="c0afb-111">Die folgende Abbildung stammt aus [DB Browser for SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="c0afb-111">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="c0afb-112">Wenn Sie ein bestimmtes SQLite-Tool bevorzugen, geben Sie bitte in einem Kommentar dessen Vorteile an.</span><span class="sxs-lookup"><span data-stu-id="c0afb-112">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser for SQLite mit der Filmdatenbank](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="c0afb-114">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="c0afb-114">Seed the database</span></span>

<span data-ttu-id="c0afb-115">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="c0afb-115">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="c0afb-116">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="c0afb-116">Replace the generated code with the following:</span></span>

<span data-ttu-id="c0afb-117">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="c0afb-117">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

<span data-ttu-id="c0afb-118">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="c0afb-118">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="c0afb-119">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="c0afb-119">Add the seed initializer</span></span>

<span data-ttu-id="c0afb-120">Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="c0afb-120">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="c0afb-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span><span class="sxs-lookup"><span data-stu-id="c0afb-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span></span>

### <a name="test-the-app"></a><span data-ttu-id="c0afb-122">Testen der App</span><span class="sxs-lookup"><span data-stu-id="c0afb-122">Test the app</span></span>

<span data-ttu-id="c0afb-123">Löschen Sie alle Datensätze in der Datenbank (damit die Seed-Methode ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="c0afb-123">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="c0afb-124">Beenden und starten Sie die App, um das Seeding der Datenbank auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c0afb-124">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="c0afb-125">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="c0afb-125">The app shows the seeded data.</span></span>

![Im Browser geöffnete MVC Movie-Anwendung mit Filmdaten](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
