---
title: Arbeiten mit SQL Server LocalDB und ASP.NET Core
author: rick-anderson
description: Informationen zum Arbeiten mit SQL Server LocalDB und ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 255faf12064aa424d51fb6faa801884c474bd288
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889482"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="e6e48-103">Arbeiten mit SQL Server LocalDB und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6e48-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="e6e48-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="e6e48-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="e6e48-105">Das `MovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e6e48-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e6e48-106">Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:</span><span class="sxs-lookup"><span data-stu-id="e6e48-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="e6e48-107">Weitere Informationen über die in `ConfigureServices` verwendeten Methoden finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="e6e48-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="e6e48-108">[Unterstützung für die Datenschutz-Grundverordnung (DSGVO) in ASP.NET Core](xref:security/gdpr) für `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="e6e48-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="e6e48-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="e6e48-109">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="e6e48-110">Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration/index) liest die `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e6e48-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e6e48-111">Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen.</span><span class="sxs-lookup"><span data-stu-id="e6e48-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="e6e48-112">Der Name-Wert für die Datenbank (`Database={Database name}`) ist für den generierten Code anders.</span><span class="sxs-lookup"><span data-stu-id="e6e48-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="e6e48-113">Beim Name-Wert handelt es sich um einen beliebigen Wert.</span><span class="sxs-lookup"><span data-stu-id="e6e48-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="e6e48-114">Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder eine andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden.</span><span class="sxs-lookup"><span data-stu-id="e6e48-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="e6e48-115">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e6e48-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e6e48-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e6e48-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e6e48-117">LocalDB ist eine Lightweightversion der SQL Server Express-Datenbank-Engine, die für die Programmentwicklung geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="e6e48-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="e6e48-118">LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt.</span><span class="sxs-lookup"><span data-stu-id="e6e48-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e6e48-119">Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Benutzer/\<Benutzer\>*.</span><span class="sxs-lookup"><span data-stu-id="e6e48-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="e6e48-120">Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e6e48-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menü Ansicht](sql/_static/ssox.png)

* <span data-ttu-id="e6e48-122">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie`, und wählen Sie **Designer anzeigen** aus:</span><span class="sxs-lookup"><span data-stu-id="e6e48-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](sql/_static/dv.png)

<span data-ttu-id="e6e48-125">Beachten Sie das Schlüsselsymbol neben `ID`.</span><span class="sxs-lookup"><span data-stu-id="e6e48-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e6e48-126">EF erstellt standardmäßig eine Eigenschaft namens `ID` für den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="e6e48-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="e6e48-127">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie`, und wählen Sie **Daten anzeigen** aus:</span><span class="sxs-lookup"><span data-stu-id="e6e48-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="e6e48-129">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="e6e48-129">Seed the database</span></span>

<span data-ttu-id="e6e48-130">Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`.</span><span class="sxs-lookup"><span data-stu-id="e6e48-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="e6e48-131">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="e6e48-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="e6e48-132">Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e6e48-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="e6e48-133">Hinzufügen des Initialisierers des Seedings</span><span class="sxs-lookup"><span data-stu-id="e6e48-133">Add the seed initializer</span></span>

<span data-ttu-id="e6e48-134">Ändern Sie in der *Program.cs*-Datei die `Main`-Methode, um die folgenden Vorgänge auszuführen:</span><span class="sxs-lookup"><span data-stu-id="e6e48-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="e6e48-135">Rufen Sie eine Datenbankkontextinstanz aus dem Dependency Injection-Container ab.</span><span class="sxs-lookup"><span data-stu-id="e6e48-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e6e48-136">Rufen Sie die Seedmethode auf, indem Sie den Kontext an diese übergeben.</span><span class="sxs-lookup"><span data-stu-id="e6e48-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e6e48-137">Löschen Sie den Kontext, wenn die Seedmethode abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="e6e48-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="e6e48-138">Der folgende Code zeigt die aktualisierte *Program.cs*-Datei.</span><span class="sxs-lookup"><span data-stu-id="e6e48-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="e6e48-139">Eine Produktions-App würde `Database.Migrate` nicht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="e6e48-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="e6e48-140">Es wird zum vorangehenden Code hinzugefügt, um die folgende Ausnahme zu verhindern, wenn `Update-Database` nicht ausgeführt wurde:</span><span class="sxs-lookup"><span data-stu-id="e6e48-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="e6e48-141">SqlException: Die bei der Anmeldung angeforderte Datenbank „RazorPagesMovieContext-21“ kann nicht geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="e6e48-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="e6e48-142">Die Anmeldung ist fehlgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="e6e48-142">The login failed.</span></span>
<span data-ttu-id="e6e48-143">Die Anmeldung des Benutzers „Benutzername“ ist fehlgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="e6e48-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="e6e48-144">Testen der App</span><span class="sxs-lookup"><span data-stu-id="e6e48-144">Test the app</span></span>

* <span data-ttu-id="e6e48-145">Löschen Sie alle Datensätze in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e6e48-145">Delete all the records in the DB.</span></span> <span data-ttu-id="e6e48-146">Dies ist über die Links „Löschen“ im Browser oder [SSOX](xref:tutorials/razor-pages/new-field#ssox) möglich.</span><span class="sxs-lookup"><span data-stu-id="e6e48-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="e6e48-147">Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seedmethode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e6e48-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e6e48-148">Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="e6e48-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e6e48-149">Hierzu können Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="e6e48-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e6e48-150">Klicken Sie auf der Taskleiste im Infobereich mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus:</span><span class="sxs-lookup"><span data-stu-id="e6e48-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express-Symbol auf der Taskleiste](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](sql/_static/stopIIS.png)

    * <span data-ttu-id="e6e48-153">Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e6e48-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="e6e48-154">Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.</span><span class="sxs-lookup"><span data-stu-id="e6e48-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="e6e48-155">Die App zeigt die per Seeding hinzugefügten Daten:</span><span class="sxs-lookup"><span data-stu-id="e6e48-155">The app shows the seeded data:</span></span>

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

<span data-ttu-id="e6e48-157">Im nächsten Tutorial wird die Präsentation der Daten bereinigt.</span><span class="sxs-lookup"><span data-stu-id="e6e48-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e6e48-158">[Zurück: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages/page)
> [Weiter: Aktualisieren der Seiten](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="e6e48-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
