---
title: Arbeiten mit SQL Server LocalDB und ASP.NET Core
author: rick-anderson
description: Informationen zum Arbeiten mit SQL Server LocalDB und ASP.NET Core.
keywords: ASP.NET Core, Razor-Seiten, Razor, MVC, SQL, LocalDB
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 173bdcca80a599ec2d87ff4158614727b35f984a
ms.sourcegitcommit: d02d90b6272372178723ff932e8a9b9566afedb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="f0f01-104">Arbeiten mit SQL Server LocalDB und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0f01-104">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="f0f01-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="f0f01-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="f0f01-106">Das `MovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f0f01-106">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f0f01-107">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="f0f01-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="f0f01-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="f0f01-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]</span></span>

<span data-ttu-id="f0f01-109">Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration) liest die `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="f0f01-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f0f01-110">Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen:</span><span class="sxs-lookup"><span data-stu-id="f0f01-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

<span data-ttu-id="f0f01-111">[!code-javascript[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]</span><span class="sxs-lookup"><span data-stu-id="f0f01-111">[!code-javascript[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]</span></span>

<span data-ttu-id="f0f01-112">Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0f01-112">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="f0f01-113">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="f0f01-113">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="f0f01-114">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f0f01-114">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f0f01-115">LocalDB ist eine Basisversion des SQL Server Express-Datenbankmoduls, die für die Programmentwicklung geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="f0f01-115">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="f0f01-116">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="f0f01-116">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="f0f01-117">Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="f0f01-117">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="f0f01-118">Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="f0f01-118">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menü Ansicht](sql/_static/ssox.png)

* <span data-ttu-id="f0f01-120">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Ansicht-Designer**.</span><span class="sxs-lookup"><span data-stu-id="f0f01-120">Right click on the `Movie` table **> View Designer**</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](sql/_static/dv.png)

<span data-ttu-id="f0f01-123">Beachten Sie das Schlüsselsymbol neben `ID`.</span><span class="sxs-lookup"><span data-stu-id="f0f01-123">Note the key icon next to `ID`.</span></span> <span data-ttu-id="f0f01-124">EF macht die Eigenschaft `ID` standardmäßig zum Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="f0f01-124">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="f0f01-125">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Daten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="f0f01-125">Right click on the `Movie` table **> View Data**</span></span>

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="f0f01-127">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="f0f01-127">Seed the database</span></span>

<span data-ttu-id="f0f01-128">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="f0f01-128">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="f0f01-129">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="f0f01-129">Replace the generated code with the following:</span></span>

<span data-ttu-id="f0f01-130">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="f0f01-130">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

<span data-ttu-id="f0f01-131">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f0f01-131">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="f0f01-132">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="f0f01-132">Add the seed initializer</span></span>

<span data-ttu-id="f0f01-133">Fügen Sie den Initialisierers des Seedings in der Datei *Program.cs* am Ende der `Main`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="f0f01-133">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="f0f01-134">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]</span><span class="sxs-lookup"><span data-stu-id="f0f01-134">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]</span></span>

<span data-ttu-id="f0f01-135">Testen der App</span><span class="sxs-lookup"><span data-stu-id="f0f01-135">Test the app</span></span>

* <span data-ttu-id="f0f01-136">Löschen Sie alle Datensätze in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f0f01-136">Delete all the records in the DB.</span></span> <span data-ttu-id="f0f01-137">Dies ist über die Links „Löschen“ im Browser oder [SSOX](xref:tutorials/razor-pages/new-field#ssox) möglich.</span><span class="sxs-lookup"><span data-stu-id="f0f01-137">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="f0f01-138">Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seedmethode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f0f01-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="f0f01-139">Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="f0f01-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="f0f01-140">Hierzu können Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="f0f01-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="f0f01-141">Klicken Sie auf der Taskleiste im Infobereich mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus.</span><span class="sxs-lookup"><span data-stu-id="f0f01-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express-Symbol auf der Taskleiste](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](sql/_static/stopIIS.png)

   * <span data-ttu-id="f0f01-144">Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f0f01-144">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="f0f01-145">Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.</span><span class="sxs-lookup"><span data-stu-id="f0f01-145">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="f0f01-146">Die App zeigt die per Seeding hinzugefügten Daten.</span><span class="sxs-lookup"><span data-stu-id="f0f01-146">The app shows the seeded data.</span></span>

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

<span data-ttu-id="f0f01-148">Im nächsten Tutorial wird die Präsentation der Daten bereinigt.</span><span class="sxs-lookup"><span data-stu-id="f0f01-148">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f0f01-149">[Zurück: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages/page) </span><span class="sxs-lookup"><span data-stu-id="f0f01-149">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page) </span></span>  
[<span data-ttu-id="f0f01-150">Weiter: Aktualisieren der Seiten</span><span class="sxs-lookup"><span data-stu-id="f0f01-150">Next: Updating the pages</span></span>](xref:tutorials/razor-pages/da1)
