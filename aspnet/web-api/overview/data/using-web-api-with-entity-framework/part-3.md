---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Verwenden Sie Code First-Migrationen, um die Datenbank ein Seeding | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: bce662110fa63a9aee8e23e7b9fcc9ba9a8d2857
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805456"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="92b1f-102">Verwenden Sie Code First-Migrationen, um die Datenbank ein Seeding</span><span class="sxs-lookup"><span data-stu-id="92b1f-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="92b1f-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="92b1f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="92b1f-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="92b1f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="92b1f-105">In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF das Seeding der Datenbank mit Testdaten.</span><span class="sxs-lookup"><span data-stu-id="92b1f-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="92b1f-106">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="92b1f-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="92b1f-107">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="92b1f-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="92b1f-108">Dieser Befehl fügt einen Ordner namens Migrationen zu Ihrem Projekt sowie eine Codedatei namens Configuration.cs in den Ordner "Migrations".</span><span class="sxs-lookup"><span data-stu-id="92b1f-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="92b1f-109">Öffnen Sie die Configuration.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="92b1f-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="92b1f-110">Fügen Sie die folgenden **mit** Anweisung.</span><span class="sxs-lookup"><span data-stu-id="92b1f-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="92b1f-111">Klicken Sie dann fügen Sie den folgenden Code der **Configuration.Seed** Methode:</span><span class="sxs-lookup"><span data-stu-id="92b1f-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="92b1f-112">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="92b1f-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="92b1f-113">Der erste Befehl generiert Code, der die Datenbank erstellt, und mit dem zweite Befehl wird dieser Code ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="92b1f-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="92b1f-114">Die Datenbank wird lokal mit erstellt [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="92b1f-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="92b1f-115">Erkunden der API (Optional)</span><span class="sxs-lookup"><span data-stu-id="92b1f-115">Explore the API (Optional)</span></span>

<span data-ttu-id="92b1f-116">Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="92b1f-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="92b1f-117">Visual Studio startet IIS Express und Ihrer Web-app ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="92b1f-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="92b1f-118">Visual Studio klicken Sie dann einen Browser und der app-Startseite wird geöffnet.</span><span class="sxs-lookup"><span data-stu-id="92b1f-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="92b1f-119">Wenn Visual Studio ein Webprojekt ausgeführt wird, weist sie eine Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="92b1f-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="92b1f-120">In der folgenden Abbildung ist die Nummer des Ports 50524.</span><span class="sxs-lookup"><span data-stu-id="92b1f-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="92b1f-121">Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="92b1f-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="92b1f-122">Auf der Startseite wird die Verwendung von ASP.NET MVC implementiert.</span><span class="sxs-lookup"><span data-stu-id="92b1f-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="92b1f-123">Bei den oberen Rand der Seite wird eine Verknüpfung mit dem Text "API".</span><span class="sxs-lookup"><span data-stu-id="92b1f-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="92b1f-124">Diesen Link gelangen Sie zu einer automatisch generierten Hilfeseite an, für die Web-API.</span><span class="sxs-lookup"><span data-stu-id="92b1f-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="92b1f-125">(Um zu erfahren, wie diese Hilfeseite generiert wird und wie Sie Ihre eigene Dokumentation auf der Seite hinzufügen können, finden Sie unter [für ASP.NET Web API Help Pages erstellen](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können Seite enthält Links, um die Details der API, einschließlich der Anforderung und Antwort-Format finden auf die Hilfe klicken.</span><span class="sxs-lookup"><span data-stu-id="92b1f-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="92b1f-126">Der API können CRUD-Vorgänge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="92b1f-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="92b1f-127">Im folgenden werden die API zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="92b1f-127">The following summarizes the API.</span></span>

| <span data-ttu-id="92b1f-128">Authors</span><span class="sxs-lookup"><span data-stu-id="92b1f-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="92b1f-129">Abrufen der api/authors</span><span class="sxs-lookup"><span data-stu-id="92b1f-129">GET api/authors</span></span> | <span data-ttu-id="92b1f-130">Rufen Sie alle Autoren.</span><span class="sxs-lookup"><span data-stu-id="92b1f-130">Get all authors.</span></span> |
| <span data-ttu-id="92b1f-131">GET-api/Authors / {Id}</span><span class="sxs-lookup"><span data-stu-id="92b1f-131">GET api/authors/{id}</span></span> | <span data-ttu-id="92b1f-132">Erhalten Sie einen Autor anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="92b1f-132">Get an author by ID.</span></span> |
| <span data-ttu-id="92b1f-133">POST/api/authors</span><span class="sxs-lookup"><span data-stu-id="92b1f-133">POST /api/authors</span></span> | <span data-ttu-id="92b1f-134">Erstellen Sie einen neuen Autor.</span><span class="sxs-lookup"><span data-stu-id="92b1f-134">Create a new author.</span></span> |
| <span data-ttu-id="92b1f-135">PUT/API/Authors / {Id}</span><span class="sxs-lookup"><span data-stu-id="92b1f-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="92b1f-136">Aktualisieren eines vorhandenen Autors an.</span><span class="sxs-lookup"><span data-stu-id="92b1f-136">Update an existing author.</span></span> |
| <span data-ttu-id="92b1f-137">Löschen Sie/API/Authors / {Id}</span><span class="sxs-lookup"><span data-stu-id="92b1f-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="92b1f-138">Löschen eines Autors an.</span><span class="sxs-lookup"><span data-stu-id="92b1f-138">Delete an author.</span></span> |

| <span data-ttu-id="92b1f-139">Bücher</span><span class="sxs-lookup"><span data-stu-id="92b1f-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="92b1f-140">/Api/books abrufen</span><span class="sxs-lookup"><span data-stu-id="92b1f-140">GET /api/books</span></span> | <span data-ttu-id="92b1f-141">Alle Bücher zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="92b1f-141">Get all books.</span></span> |
| <span data-ttu-id="92b1f-142">Abrufen Sie/API/Books / {Id}</span><span class="sxs-lookup"><span data-stu-id="92b1f-142">GET /api/books/{id}</span></span> | <span data-ttu-id="92b1f-143">Erhalten Sie ein Buch anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="92b1f-143">Get a book by ID.</span></span> |
| <span data-ttu-id="92b1f-144">POST/api/Bücher</span><span class="sxs-lookup"><span data-stu-id="92b1f-144">POST /api/books</span></span> | <span data-ttu-id="92b1f-145">Erstellen Sie ein neues Buch.</span><span class="sxs-lookup"><span data-stu-id="92b1f-145">Create a new book.</span></span> |
| <span data-ttu-id="92b1f-146">PUT/API/Books / {Id}</span><span class="sxs-lookup"><span data-stu-id="92b1f-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="92b1f-147">Aktualisieren Sie ein vorhandenes Buch.</span><span class="sxs-lookup"><span data-stu-id="92b1f-147">Update an existing book.</span></span> |
| <span data-ttu-id="92b1f-148">Löschen Sie/API/Books / {Id}</span><span class="sxs-lookup"><span data-stu-id="92b1f-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="92b1f-149">Löschen Sie ein Buch.</span><span class="sxs-lookup"><span data-stu-id="92b1f-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="92b1f-150">Zeigen Sie die Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="92b1f-150">View the Database (Optional)</span></span>

<span data-ttu-id="92b1f-151">Wenn Sie den Update-Database-Befehl ausgeführt haben, EF die Datenbank erstellt und wird aufgerufen, die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="92b1f-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="92b1f-152">Wenn Sie die Anwendung lokal ausführen, verwendet EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="92b1f-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="92b1f-153">Sie können die Datenbank in Visual Studio anzeigen.</span><span class="sxs-lookup"><span data-stu-id="92b1f-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="92b1f-154">Von der **Ansicht** , wählen Sie im Menü **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="92b1f-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="92b1f-155">In der **Herstellen einer Verbindung mit Server** Dialogfeld, in der **Servernamen** "Bearbeiten", geben Sie "(Localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="92b1f-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="92b1f-156">Lassen Sie die **Authentifizierung** Option als "Windows-Authentifizierung".</span><span class="sxs-lookup"><span data-stu-id="92b1f-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="92b1f-157">Klicken Sie auf **Verbinden**.</span><span class="sxs-lookup"><span data-stu-id="92b1f-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="92b1f-158">Visual Studio eine Verbindung mit LocalDB her und zeigt Ihre vorhandenen Datenbanken im Objekt-Explorer von SQL Server-Fenster.</span><span class="sxs-lookup"><span data-stu-id="92b1f-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="92b1f-159">Sie können die Knoten, um die Tabellen anzuzeigen, die EF erstellt erweitern.</span><span class="sxs-lookup"><span data-stu-id="92b1f-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="92b1f-160">Klicken Sie zum Anzeigen der Daten, mit der rechten Maustaste in einer Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="92b1f-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="92b1f-161">Der folgende Screenshot zeigt die Ergebnisse für die Bücher-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="92b1f-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="92b1f-162">Beachten Sie, dass EF, die Datenbank mit der Seed-Daten aufgefüllt, und die Tabelle die Fremdschlüssel für die Authors-Tabelle enthält.</span><span class="sxs-lookup"><span data-stu-id="92b1f-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="92b1f-163">[Zurück](part-2.md)
> [Weiter](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="92b1f-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
