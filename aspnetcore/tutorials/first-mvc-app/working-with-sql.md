---
title: Arbeiten mit SQL Server LocalDB
author: rick-anderson
description: Verwenden von SQL Server LocalDB mit einer einfachen MVC-App
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a0aa6fdfa51650628021a4ba6d0533e7e0e39200
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="26e7d-103">Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="26e7d-103">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="26e7d-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26e7d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="26e7d-105">Das `MvcMovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="26e7d-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="26e7d-106">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="26e7d-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="26e7d-107">Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration/index) liest die `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="26e7d-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="26e7d-108">Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen:</span><span class="sxs-lookup"><span data-stu-id="26e7d-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="26e7d-109">Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder eine andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden.</span><span class="sxs-lookup"><span data-stu-id="26e7d-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="26e7d-110">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="26e7d-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="26e7d-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="26e7d-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="26e7d-112">LocalDB ist eine Lightweightversion der SQL Server Express-Datenbank-Engine, die für die Programmentwicklung geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="26e7d-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="26e7d-113">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="26e7d-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="26e7d-114">Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Benutzer/\<Benutzer\>*.</span><span class="sxs-lookup"><span data-stu-id="26e7d-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="26e7d-115">Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="26e7d-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menü Ansicht](working-with-sql/_static/ssox.png)

* <span data-ttu-id="26e7d-117">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Ansicht-Designer**.</span><span class="sxs-lookup"><span data-stu-id="26e7d-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](working-with-sql/_static/dv.png)

<span data-ttu-id="26e7d-120">Beachten Sie das Schlüsselsymbol neben `ID`.</span><span class="sxs-lookup"><span data-stu-id="26e7d-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="26e7d-121">EF macht die Eigenschaft `ID` standardmäßig zum Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="26e7d-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="26e7d-122">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Daten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="26e7d-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/ssox2.png)

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="26e7d-125">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="26e7d-125">Seed the database</span></span>

<span data-ttu-id="26e7d-126">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="26e7d-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="26e7d-127">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="26e7d-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="26e7d-128">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="26e7d-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="26e7d-129">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="26e7d-129">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26e7d-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26e7d-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="26e7d-131">Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="26e7d-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26e7d-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26e7d-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26e7d-133">Fügen Sie den Initialisierer des Seedings in der Datei *Startup.cs* am Ende der `Configure`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="26e7d-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="26e7d-134">Testen der App</span><span class="sxs-lookup"><span data-stu-id="26e7d-134">Test the app</span></span>

* <span data-ttu-id="26e7d-135">Löschen Sie alle Datensätze in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="26e7d-135">Delete all the records in the DB.</span></span> <span data-ttu-id="26e7d-136">Dies ist über die Links „Löschen“ im Browser oder SSOX möglich.</span><span class="sxs-lookup"><span data-stu-id="26e7d-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="26e7d-137">Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seed-Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="26e7d-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="26e7d-138">Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="26e7d-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="26e7d-139">Hierzu können Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="26e7d-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="26e7d-140">Klicken Sie auf der Taskleiste im Infobereich mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus.</span><span class="sxs-lookup"><span data-stu-id="26e7d-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express-Symbol auf der Taskleiste](working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="26e7d-143">Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="26e7d-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="26e7d-144">Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.</span><span class="sxs-lookup"><span data-stu-id="26e7d-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="26e7d-145">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="26e7d-145">The app shows the seeded data.</span></span>

![In Microsoft Edge geöffnete MVC Movie-Anwendung mit Filmdaten](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="26e7d-147">[Zurück](adding-model.md)
[Weiter](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="26e7d-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
