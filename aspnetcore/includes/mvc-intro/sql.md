# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="679c1-101">Arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="679c1-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="679c1-102">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="679c1-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="679c1-103">Das `MvcMovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="679c1-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="679c1-104">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="679c1-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="679c1-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="679c1-105">SQLite</span></span>

<span data-ttu-id="679c1-106">Auf der [SQLite](https://www.sqlite.org/)-Website ist zu lesen:</span><span class="sxs-lookup"><span data-stu-id="679c1-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="679c1-107">SQLite ist eine eigenständige, sehr zuverlässige, eingebettete, genehmigungsfreie SQL-Datenbank-Engine mit vollem Funktionsumfang.</span><span class="sxs-lookup"><span data-stu-id="679c1-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="679c1-108">SQLite ist die weltweit am häufigsten verwendete Datenbank-Engine.</span><span class="sxs-lookup"><span data-stu-id="679c1-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="679c1-109">Es gibt viele Tools von Drittanbietern, die Sie herunterladen können, um eine SQLite-Datenbank zu verwalten und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="679c1-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="679c1-110">Die folgende Abbildung stammt aus [DB Browser for SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="679c1-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="679c1-111">Wenn Sie ein bestimmtes SQLite-Tool bevorzugen, geben Sie bitte in einem Kommentar dessen Vorteile an.</span><span class="sxs-lookup"><span data-stu-id="679c1-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser for SQLite mit der Filmdatenbank](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="679c1-113">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="679c1-113">Seed the database</span></span>

<span data-ttu-id="679c1-114">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="679c1-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="679c1-115">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="679c1-115">Replace the generated code with the following:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="679c1-116">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="679c1-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="679c1-117">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="679c1-117">Add the seed initializer</span></span>

<span data-ttu-id="679c1-118">Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="679c1-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a><span data-ttu-id="679c1-119">Testen der App</span><span class="sxs-lookup"><span data-stu-id="679c1-119">Test the app</span></span>

<span data-ttu-id="679c1-120">Löschen Sie alle Datensätze in der Datenbank (damit die Seed-Methode ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="679c1-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="679c1-121">Beenden und starten Sie die App, um das Seeding der Datenbank auszuführen.</span><span class="sxs-lookup"><span data-stu-id="679c1-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="679c1-122">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="679c1-122">The app shows the seeded data.</span></span>

![Im Browser geöffnete MVC Movie-Anwendung mit Filmdaten](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
