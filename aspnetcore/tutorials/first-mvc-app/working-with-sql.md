---
title: Arbeiten mit SQL Server LocalDB in einer ASP.NET Core MVC-App
author: rick-anderson
description: Erfahren Sie mehr über die Verwendung von SQL Server LocalDB in einer einfachen ASP.NET Core MVC-App.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: fadd7be793b1ff6e863b549271acd5b6b2cc9305
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011848"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="d6382-103">Arbeiten mit SQL Server LocalDB in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6382-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="d6382-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d6382-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d6382-105">Das `MvcMovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d6382-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d6382-106">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="d6382-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

<span data-ttu-id="d6382-107">Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration/index) liest die `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="d6382-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="d6382-108">Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen:</span><span class="sxs-lookup"><span data-stu-id="d6382-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="d6382-109">Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder eine andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden.</span><span class="sxs-lookup"><span data-stu-id="d6382-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="d6382-110">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d6382-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="d6382-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d6382-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d6382-112">LocalDB ist eine Lightweightversion der SQL Server Express-Datenbank-Engine, die für die Programmentwicklung geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="d6382-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="d6382-113">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="d6382-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="d6382-114">Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Benutzer/\<Benutzer\>*.</span><span class="sxs-lookup"><span data-stu-id="d6382-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="d6382-115">Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="d6382-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menü Ansicht](working-with-sql/_static/ssox.png)

* <span data-ttu-id="d6382-117">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Ansicht-Designer**.</span><span class="sxs-lookup"><span data-stu-id="d6382-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](working-with-sql/_static/dv.png)

<span data-ttu-id="d6382-120">Beachten Sie das Schlüsselsymbol neben `ID`.</span><span class="sxs-lookup"><span data-stu-id="d6382-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="d6382-121">EF macht die Eigenschaft `ID` standardmäßig zum Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="d6382-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="d6382-122">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Daten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="d6382-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/ssox2.png)

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="d6382-125">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="d6382-125">Seed the database</span></span>

<span data-ttu-id="d6382-126">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="d6382-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="d6382-127">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="d6382-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="d6382-128">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d6382-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="d6382-129">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="d6382-129">Add the seed initializer</span></span>

<span data-ttu-id="d6382-130">Ersetzen Sie den Inhalt von *Program.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d6382-130">Replace the contents of *Program.cs* with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d6382-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d6382-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="d6382-132">Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6382-132">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d6382-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d6382-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="d6382-134">Fügen Sie den Initialisierer des Seedings in der Datei *Startup.cs* am Ende der `Configure`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6382-134">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

::: moniker-end

<span data-ttu-id="d6382-135">Testen der App</span><span class="sxs-lookup"><span data-stu-id="d6382-135">Test the app</span></span>

* <span data-ttu-id="d6382-136">Löschen Sie alle Datensätze in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d6382-136">Delete all the records in the DB.</span></span> <span data-ttu-id="d6382-137">Dies ist über die Links „Löschen“ im Browser oder SSOX möglich.</span><span class="sxs-lookup"><span data-stu-id="d6382-137">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="d6382-138">Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seed-Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d6382-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="d6382-139">Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="d6382-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="d6382-140">Hierzu können Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="d6382-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="d6382-141">Klicken Sie auf der Taskleiste im Infobereich mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus.</span><span class="sxs-lookup"><span data-stu-id="d6382-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express-Symbol auf der Taskleiste](working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="d6382-144">Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6382-144">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="d6382-145">Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.</span><span class="sxs-lookup"><span data-stu-id="d6382-145">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="d6382-146">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="d6382-146">The app shows the seeded data.</span></span>

![In Microsoft Edge geöffnete MVC Movie-Anwendung mit Filmdaten](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="d6382-148">[Zurück](adding-model.md)
> [Weiter](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="d6382-148">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
