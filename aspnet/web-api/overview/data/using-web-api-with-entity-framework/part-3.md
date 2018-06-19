---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Code First-Migrationen verwenden, um das Seeding der Datenbank | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 33bc6d82daa9ca5f46452a1adf4e2eebea04fa6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869932"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="b93e2-102">Verwenden Sie Code First-Migrationen, um das Seeding der Datenbank</span><span class="sxs-lookup"><span data-stu-id="b93e2-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="b93e2-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b93e2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b93e2-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="b93e2-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b93e2-105">In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF zum Ausgangswert für der Datenbank mit Testdaten.</span><span class="sxs-lookup"><span data-stu-id="b93e2-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="b93e2-106">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b93e2-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b93e2-107">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b93e2-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="b93e2-108">Dieser Befehl fügt einen Ordner namens Migrationen zu Ihrem Projekt plus eine Codedatei mit dem Namen "Configuration.cs" in den Ordner.</span><span class="sxs-lookup"><span data-stu-id="b93e2-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="b93e2-109">Öffnen Sie die Datei "Configuration.cs".</span><span class="sxs-lookup"><span data-stu-id="b93e2-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="b93e2-110">Fügen Sie die folgenden **mit** Anweisung.</span><span class="sxs-lookup"><span data-stu-id="b93e2-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="b93e2-111">Fügen Sie folgenden Code, der **Configuration.Seed** Methode:</span><span class="sxs-lookup"><span data-stu-id="b93e2-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="b93e2-112">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="b93e2-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="b93e2-113">Der erste Befehl generiert Code, der die Datenbank erstellt und mit dem zweite Befehl wird dieser Code ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b93e2-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="b93e2-114">Die Datenbank wird lokal mit erstellt [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="b93e2-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="b93e2-115">Untersuchen Sie die API (Optional)</span><span class="sxs-lookup"><span data-stu-id="b93e2-115">Explore the API (Optional)</span></span>

<span data-ttu-id="b93e2-116">Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b93e2-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="b93e2-117">Visual Studio startet IIS Express und Ihre Web-app ausführt.</span><span class="sxs-lookup"><span data-stu-id="b93e2-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="b93e2-118">Visual Studio dann öffnet einen Browser und Startseite der app geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b93e2-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="b93e2-119">Wenn Visual Studio ein Webprojekt ausgeführt wird, weist es eine Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="b93e2-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="b93e2-120">In der folgenden Abbildung ist die Nummer des Ports 50524.</span><span class="sxs-lookup"><span data-stu-id="b93e2-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="b93e2-121">Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="b93e2-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="b93e2-122">Die Startseite wird mithilfe von ASP.NET MVC implementiert.</span><span class="sxs-lookup"><span data-stu-id="b93e2-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="b93e2-123">Klicken Sie oben auf der Seite besteht eine Verknüpfung, die besagt, dass "API" zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="b93e2-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="b93e2-124">Diesen Link gelangen Sie zur einer automatisch generierten Hilfeseite an, für die Web-API.</span><span class="sxs-lookup"><span data-stu-id="b93e2-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="b93e2-125">(Um zu erfahren, wie diese Hilfeseite generiert wird und wie Sie Ihre eigenen Dokumentation auf der Seite hinzufügen können, finden Sie unter [Hilfeseiten für ASP.NET Web-API erstellen](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können auf "Hilfe" die Seite enthält Links, um Details über die API, einschließlich der Anforderung und Antwort-Format anzuzeigen klicken.</span><span class="sxs-lookup"><span data-stu-id="b93e2-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="b93e2-126">Die API ermöglicht CRUD-Vorgänge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b93e2-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="b93e2-127">Im folgenden werden die API zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="b93e2-127">The following summarizes the API.</span></span>

| <span data-ttu-id="b93e2-128">Authors</span><span class="sxs-lookup"><span data-stu-id="b93e2-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="b93e2-129">Api-Autoren abrufen</span><span class="sxs-lookup"><span data-stu-id="b93e2-129">GET api/authors</span></span> | <span data-ttu-id="b93e2-130">Rufen Sie aller Autoren ab.</span><span class="sxs-lookup"><span data-stu-id="b93e2-130">Get all authors.</span></span> |
| <span data-ttu-id="b93e2-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="b93e2-131">GET api/authors/{id}</span></span> | <span data-ttu-id="b93e2-132">Einen Autor-ID abrufen</span><span class="sxs-lookup"><span data-stu-id="b93e2-132">Get an author by ID.</span></span> |
| <span data-ttu-id="b93e2-133">POST/api/Autoren</span><span class="sxs-lookup"><span data-stu-id="b93e2-133">POST /api/authors</span></span> | <span data-ttu-id="b93e2-134">Erstellen Sie eine neue erstellen.</span><span class="sxs-lookup"><span data-stu-id="b93e2-134">Create a new author.</span></span> |
| <span data-ttu-id="b93e2-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="b93e2-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="b93e2-136">Aktualisieren eines vorhandenen Autors.</span><span class="sxs-lookup"><span data-stu-id="b93e2-136">Update an existing author.</span></span> |
| <span data-ttu-id="b93e2-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="b93e2-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="b93e2-138">Löschen eines Autors an.</span><span class="sxs-lookup"><span data-stu-id="b93e2-138">Delete an author.</span></span> |

| <span data-ttu-id="b93e2-139">Bücher</span><span class="sxs-lookup"><span data-stu-id="b93e2-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="b93e2-140">/Api/books abrufen</span><span class="sxs-lookup"><span data-stu-id="b93e2-140">GET /api/books</span></span> | <span data-ttu-id="b93e2-141">Rufen Sie aller Bücher an ab.</span><span class="sxs-lookup"><span data-stu-id="b93e2-141">Get all books.</span></span> |
| <span data-ttu-id="b93e2-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="b93e2-142">GET /api/books/{id}</span></span> | <span data-ttu-id="b93e2-143">Ein Buch-ID abrufen</span><span class="sxs-lookup"><span data-stu-id="b93e2-143">Get a book by ID.</span></span> |
| <span data-ttu-id="b93e2-144">Bereitstellen Sie/api/Bücher</span><span class="sxs-lookup"><span data-stu-id="b93e2-144">POST /api/books</span></span> | <span data-ttu-id="b93e2-145">Erstellen Sie ein neues Buch.</span><span class="sxs-lookup"><span data-stu-id="b93e2-145">Create a new book.</span></span> |
| <span data-ttu-id="b93e2-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="b93e2-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="b93e2-147">Aktualisieren Sie ein vorhandenes Buch.</span><span class="sxs-lookup"><span data-stu-id="b93e2-147">Update an existing book.</span></span> |
| <span data-ttu-id="b93e2-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="b93e2-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="b93e2-149">Löschen Sie ein Buch.</span><span class="sxs-lookup"><span data-stu-id="b93e2-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="b93e2-150">Anzeigen der Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="b93e2-150">View the Database (Optional)</span></span>

<span data-ttu-id="b93e2-151">Beim Ausführen den Update-Database-Befehl wurde die Datenbank erstellt und aufgerufen EF die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="b93e2-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="b93e2-152">Wenn Sie die Anwendung lokal ausführen, EF verwendet [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="b93e2-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="b93e2-153">Sie können die Datenbank in Visual Studio anzeigen.</span><span class="sxs-lookup"><span data-stu-id="b93e2-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="b93e2-154">Aus der **Ansicht** klicken Sie im Menü **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="b93e2-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="b93e2-155">In der **Verbindung mit Server herstellen** Dialogfeld in der **Servernamen** Bearbeitungsfeld, geben Sie "(Localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="b93e2-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="b93e2-156">Lassen Sie die **Authentifizierung** Option als "Windows-Authentifizierung".</span><span class="sxs-lookup"><span data-stu-id="b93e2-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="b93e2-157">Klicken Sie auf **verbinden**.</span><span class="sxs-lookup"><span data-stu-id="b93e2-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="b93e2-158">Visual Studio eine Verbindung mit LocalDB her und zeigt Ihre vorhandenen Datenbanken im Objekt-Explorer von SQL Server-Fenster.</span><span class="sxs-lookup"><span data-stu-id="b93e2-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="b93e2-159">Sie können die Knoten, um die Tabellen anzuzeigen, die EF erstellt erweitern.</span><span class="sxs-lookup"><span data-stu-id="b93e2-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="b93e2-160">Klicken Sie zum Anzeigen der Daten mit der rechten Maustaste in einer Tabellenstatus, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="b93e2-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="b93e2-161">Der folgende Screenshot zeigt die Ergebnisse für die Bücher-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="b93e2-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="b93e2-162">Beachten Sie, dass EF, die Datenbank mit den Daten Ausgangswert aufgefüllt und die Tabelle die Fremdschlüssel für die Authors-Tabelle enthält.</span><span class="sxs-lookup"><span data-stu-id="b93e2-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b93e2-163">[Zurück](part-2.md)
> [Weiter](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="b93e2-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
