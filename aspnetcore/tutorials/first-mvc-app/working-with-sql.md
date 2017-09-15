---
title: Arbeiten mit SQL Server LocalDB
author: rick-anderson
description: Verwenden von SQL Server LocalDB mit einer einfachen MVC-App
keywords: ASP.NET Core, SQL Server LocalDB, SQL Server, LocalDB
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: ff8fd9b8-7c98-424d-8641-7524e23bf541
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: dd8b8603d8444c95f086fd593aabe86d60f93ad4
ms.sourcegitcommit: eb025f2166023e1c394a0213c7ed8a9ca7190da5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2017
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="5c21f-104">Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="5c21f-104">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="5c21f-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5c21f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5c21f-106">Das `MvcMovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5c21f-106">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="5c21f-107">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="5c21f-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="5c21f-108">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="5c21f-108">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>

<span data-ttu-id="5c21f-109">Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration) liest die `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="5c21f-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="5c21f-110">Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen:</span><span class="sxs-lookup"><span data-stu-id="5c21f-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

<span data-ttu-id="5c21f-111">[!code-javascript[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]</span><span class="sxs-lookup"><span data-stu-id="5c21f-111">[!code-javascript[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]</span></span>

<span data-ttu-id="5c21f-112">Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder eine andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden.</span><span class="sxs-lookup"><span data-stu-id="5c21f-112">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="5c21f-113">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="5c21f-113">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="5c21f-114">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="5c21f-114">SQL Server Express LocalDB</span></span>

<span data-ttu-id="5c21f-115">LocalDB ist eine Basisversion des SQL Server Express-Datenbankmoduls, die für die Programmentwicklung geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="5c21f-115">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="5c21f-116">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="5c21f-116">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="5c21f-117">Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Benutzer/\<Benutzer\>*.</span><span class="sxs-lookup"><span data-stu-id="5c21f-117">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="5c21f-118">Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="5c21f-118">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menü Ansicht](working-with-sql/_static/ssox.png)

* <span data-ttu-id="5c21f-120">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Ansicht-Designer**.</span><span class="sxs-lookup"><span data-stu-id="5c21f-120">Right click on the `Movie` table **> View Designer**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](working-with-sql/_static/dv.png)

<span data-ttu-id="5c21f-123">Beachten Sie das Schlüsselsymbol neben `ID`.</span><span class="sxs-lookup"><span data-stu-id="5c21f-123">Note the key icon next to `ID`.</span></span> <span data-ttu-id="5c21f-124">EF macht die Eigenschaft `ID` standardmäßig zum Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="5c21f-124">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="5c21f-125">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Daten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="5c21f-125">Right click on the `Movie` table **> View Data**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/ssox2.png)

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="5c21f-128">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="5c21f-128">Seed the database</span></span>

<span data-ttu-id="5c21f-129">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="5c21f-129">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="5c21f-130">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="5c21f-130">Replace the generated code with the following:</span></span>

<span data-ttu-id="5c21f-131">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="5c21f-131">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

<span data-ttu-id="5c21f-132">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5c21f-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="5c21f-133">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="5c21f-133">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c21f-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c21f-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c21f-135">Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="5c21f-135">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="5c21f-136">[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span><span class="sxs-lookup"><span data-stu-id="5c21f-136">[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c21f-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c21f-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c21f-138">Fügen Sie den Initialisierer des Seedings in der Datei *Startup.cs* am Ende der `Configure`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="5c21f-138">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

<span data-ttu-id="5c21f-139">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span><span class="sxs-lookup"><span data-stu-id="5c21f-139">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span></span>

---

<span data-ttu-id="5c21f-140">Testen der App</span><span class="sxs-lookup"><span data-stu-id="5c21f-140">Test the app</span></span>

* <span data-ttu-id="5c21f-141">Löschen Sie alle Datensätze in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5c21f-141">Delete all the records in the DB.</span></span> <span data-ttu-id="5c21f-142">Dies ist über die Links „Löschen“ im Browser oder SSOX möglich.</span><span class="sxs-lookup"><span data-stu-id="5c21f-142">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="5c21f-143">Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seed-Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5c21f-143">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="5c21f-144">Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="5c21f-144">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="5c21f-145">Hierzu können Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="5c21f-145">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="5c21f-146">Klicken Sie auf der Taskleiste im Infobereich mit der mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus.</span><span class="sxs-lookup"><span data-stu-id="5c21f-146">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express-Symbol in der Taskleiste](working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="5c21f-149">Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5c21f-149">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="5c21f-150">Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.</span><span class="sxs-lookup"><span data-stu-id="5c21f-150">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="5c21f-151">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="5c21f-151">The app shows the seeded data.</span></span>

![In Microsoft Edge geöffnete MVC Movie-Anwendung mit Filmdaten](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="5c21f-153">[Zurück](adding-model.md)
[Weiter](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="5c21f-153">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
