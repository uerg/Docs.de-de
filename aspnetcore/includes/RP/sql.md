# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="1f6d2-101">Arbeiten mit SQLite in einer ASP.NET Core-App mit Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1f6d2-101">Work with SQLite in an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="1f6d2-102">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f6d2-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f6d2-103">Das `MovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="1f6d2-104">Der Datenbankkontext wird mit dem [Dependency Injection](xref:fundamentals/dependency-injection)-Container in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="1f6d2-104">The database context is registered with the [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

<span data-ttu-id="1f6d2-105">Weitere Informationen zur Verwendung von `DbContext` mit Dependency Injection (DI) finden Sie unter [Using DbContext with DI (Verwenden von DbContext mit DI)](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1f6d2-105">For more information on using `DbContext` with DI, see [Using DbContext with DI](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).</span></span>

## <a name="sqlite"></a><span data-ttu-id="1f6d2-106">SQLite</span><span class="sxs-lookup"><span data-stu-id="1f6d2-106">SQLite</span></span>

<span data-ttu-id="1f6d2-107">Auf der [SQLite](https://www.sqlite.org/)-Website ist zu lesen:</span><span class="sxs-lookup"><span data-stu-id="1f6d2-107">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="1f6d2-108">SQLite ist eine eigenständige, sehr zuverlässige, eingebettete, genehmigungsfreie SQL-Datenbank-Engine mit vollem Funktionsumfang.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-108">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="1f6d2-109">SQLite ist die weltweit am häufigsten verwendete Datenbank-Engine.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-109">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="1f6d2-110">Es gibt viele Tools von Drittanbietern, die Sie herunterladen können, um eine SQLite-Datenbank zu verwalten und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-110">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="1f6d2-111">Die folgende Abbildung stammt aus [DB Browser for SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="1f6d2-111">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="1f6d2-112">Wenn Sie ein bestimmtes SQLite-Tool bevorzugen, geben Sie bitte in einem Kommentar dessen Vorteile an.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-112">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser for SQLite mit der Filmdatenbank](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="1f6d2-114">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="1f6d2-114">Seed the database</span></span>

<span data-ttu-id="1f6d2-115">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-115">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="1f6d2-116">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="1f6d2-116">Replace the generated code with the following:</span></span>

[!code-csharp[](code/Models/SeedData.cs)]

<span data-ttu-id="1f6d2-117">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-117">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="1f6d2-118">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="1f6d2-118">Add the seed initializer</span></span>

<span data-ttu-id="1f6d2-119">Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="1f6d2-119">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a><span data-ttu-id="1f6d2-120">Testen der App</span><span class="sxs-lookup"><span data-stu-id="1f6d2-120">Test the app</span></span>

<span data-ttu-id="1f6d2-121">Löschen Sie alle Datensätze in der Datenbank (damit die Seed-Methode ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="1f6d2-121">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="1f6d2-122">Beenden und starten Sie die App, um das Seeding der Datenbank auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-122">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="1f6d2-123">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="1f6d2-123">The app shows the seeded data.</span></span>
