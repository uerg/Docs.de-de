---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Verwenden Sie Code First-Migrationen, um die Datenbank ein Seeding | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910498"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="10dbf-102">Verwenden Sie Code First-Migrationen, um die Datenbank ein Seeding</span><span class="sxs-lookup"><span data-stu-id="10dbf-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="10dbf-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="10dbf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="10dbf-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="10dbf-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="10dbf-105">In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF das Seeding der Datenbank mit Testdaten.</span><span class="sxs-lookup"><span data-stu-id="10dbf-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="10dbf-106">Von der **Tools** , wählen Sie im Menü **NuGet Package Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="10dbf-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="10dbf-107">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="10dbf-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="10dbf-108">Dieser Befehl fügt einen Ordner namens Migrationen zu Ihrem Projekt sowie eine Codedatei namens Configuration.cs in den Ordner "Migrations".</span><span class="sxs-lookup"><span data-stu-id="10dbf-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="10dbf-109">Öffnen Sie die Configuration.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="10dbf-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="10dbf-110">Fügen Sie die folgenden **mit** Anweisung.</span><span class="sxs-lookup"><span data-stu-id="10dbf-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="10dbf-111">Klicken Sie dann fügen Sie den folgenden Code der **Configuration.Seed** Methode:</span><span class="sxs-lookup"><span data-stu-id="10dbf-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="10dbf-112">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="10dbf-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="10dbf-113">Der erste Befehl generiert Code, der die Datenbank erstellt, und mit dem zweite Befehl wird dieser Code ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="10dbf-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="10dbf-114">Die Datenbank wird lokal mit erstellt [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="10dbf-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="10dbf-115">Erkunden der API (Optional)</span><span class="sxs-lookup"><span data-stu-id="10dbf-115">Explore the API (Optional)</span></span>

<span data-ttu-id="10dbf-116">Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="10dbf-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="10dbf-117">Visual Studio startet IIS Express und Ihrer Web-app ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="10dbf-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="10dbf-118">Visual Studio klicken Sie dann einen Browser und der app-Startseite wird geöffnet.</span><span class="sxs-lookup"><span data-stu-id="10dbf-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="10dbf-119">Wenn Visual Studio ein Webprojekt ausgeführt wird, weist sie eine Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="10dbf-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="10dbf-120">In der folgenden Abbildung ist die Nummer des Ports 50524.</span><span class="sxs-lookup"><span data-stu-id="10dbf-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="10dbf-121">Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="10dbf-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="10dbf-122">Auf der Startseite wird die Verwendung von ASP.NET MVC implementiert.</span><span class="sxs-lookup"><span data-stu-id="10dbf-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="10dbf-123">Bei den oberen Rand der Seite wird eine Verknüpfung mit dem Text "API".</span><span class="sxs-lookup"><span data-stu-id="10dbf-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="10dbf-124">Diesen Link gelangen Sie zu einer automatisch generierten Hilfeseite an, für die Web-API.</span><span class="sxs-lookup"><span data-stu-id="10dbf-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="10dbf-125">(Um zu erfahren, wie diese Hilfeseite generiert wird und wie Sie Ihre eigene Dokumentation auf der Seite hinzufügen können, finden Sie unter [für ASP.NET Web API Help Pages erstellen](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können Seite enthält Links, um die Details der API, einschließlich der Anforderung und Antwort-Format finden auf die Hilfe klicken.</span><span class="sxs-lookup"><span data-stu-id="10dbf-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="10dbf-126">Der API können CRUD-Vorgänge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="10dbf-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="10dbf-127">Im folgenden werden die API zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="10dbf-127">The following summarizes the API.</span></span>

| <span data-ttu-id="10dbf-128">Authors</span><span class="sxs-lookup"><span data-stu-id="10dbf-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="10dbf-129">Abrufen der api/authors</span><span class="sxs-lookup"><span data-stu-id="10dbf-129">GET api/authors</span></span> | <span data-ttu-id="10dbf-130">Rufen Sie alle Autoren.</span><span class="sxs-lookup"><span data-stu-id="10dbf-130">Get all authors.</span></span> |
| <span data-ttu-id="10dbf-131">GET-api/Authors / {Id}</span><span class="sxs-lookup"><span data-stu-id="10dbf-131">GET api/authors/{id}</span></span> | <span data-ttu-id="10dbf-132">Erhalten Sie einen Autor anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="10dbf-132">Get an author by ID.</span></span> |
| <span data-ttu-id="10dbf-133">POST/api/authors</span><span class="sxs-lookup"><span data-stu-id="10dbf-133">POST /api/authors</span></span> | <span data-ttu-id="10dbf-134">Erstellen Sie einen neuen Autor.</span><span class="sxs-lookup"><span data-stu-id="10dbf-134">Create a new author.</span></span> |
| <span data-ttu-id="10dbf-135">PUT/API/Authors / {Id}</span><span class="sxs-lookup"><span data-stu-id="10dbf-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="10dbf-136">Aktualisieren eines vorhandenen Autors an.</span><span class="sxs-lookup"><span data-stu-id="10dbf-136">Update an existing author.</span></span> |
| <span data-ttu-id="10dbf-137">Löschen Sie/API/Authors / {Id}</span><span class="sxs-lookup"><span data-stu-id="10dbf-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="10dbf-138">Löschen eines Autors an.</span><span class="sxs-lookup"><span data-stu-id="10dbf-138">Delete an author.</span></span> |

| <span data-ttu-id="10dbf-139">Bücher</span><span class="sxs-lookup"><span data-stu-id="10dbf-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="10dbf-140">/Api/books abrufen</span><span class="sxs-lookup"><span data-stu-id="10dbf-140">GET /api/books</span></span> | <span data-ttu-id="10dbf-141">Alle Bücher zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="10dbf-141">Get all books.</span></span> |
| <span data-ttu-id="10dbf-142">Abrufen Sie/API/Books / {Id}</span><span class="sxs-lookup"><span data-stu-id="10dbf-142">GET /api/books/{id}</span></span> | <span data-ttu-id="10dbf-143">Erhalten Sie ein Buch anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="10dbf-143">Get a book by ID.</span></span> |
| <span data-ttu-id="10dbf-144">POST/api/Bücher</span><span class="sxs-lookup"><span data-stu-id="10dbf-144">POST /api/books</span></span> | <span data-ttu-id="10dbf-145">Erstellen Sie ein neues Buch.</span><span class="sxs-lookup"><span data-stu-id="10dbf-145">Create a new book.</span></span> |
| <span data-ttu-id="10dbf-146">PUT/API/Books / {Id}</span><span class="sxs-lookup"><span data-stu-id="10dbf-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="10dbf-147">Aktualisieren Sie ein vorhandenes Buch.</span><span class="sxs-lookup"><span data-stu-id="10dbf-147">Update an existing book.</span></span> |
| <span data-ttu-id="10dbf-148">Löschen Sie/API/Books / {Id}</span><span class="sxs-lookup"><span data-stu-id="10dbf-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="10dbf-149">Löschen Sie ein Buch.</span><span class="sxs-lookup"><span data-stu-id="10dbf-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="10dbf-150">Zeigen Sie die Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="10dbf-150">View the Database (Optional)</span></span>

<span data-ttu-id="10dbf-151">Wenn Sie den Update-Database-Befehl ausgeführt haben, EF die Datenbank erstellt und wird aufgerufen, die `Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="10dbf-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="10dbf-152">Wenn Sie die Anwendung lokal ausführen, verwendet EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="10dbf-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="10dbf-153">Sie können die Datenbank in Visual Studio anzeigen.</span><span class="sxs-lookup"><span data-stu-id="10dbf-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="10dbf-154">Von der **Ansicht** , wählen Sie im Menü **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="10dbf-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="10dbf-155">In der **Herstellen einer Verbindung mit Server** Dialogfeld, in der **Servernamen** "Bearbeiten", geben Sie "(Localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="10dbf-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="10dbf-156">Lassen Sie die **Authentifizierung** Option als "Windows-Authentifizierung".</span><span class="sxs-lookup"><span data-stu-id="10dbf-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="10dbf-157">Klicken Sie auf **Verbinden**.</span><span class="sxs-lookup"><span data-stu-id="10dbf-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="10dbf-158">Visual Studio eine Verbindung mit LocalDB her und zeigt Ihre vorhandenen Datenbanken im Objekt-Explorer von SQL Server-Fenster.</span><span class="sxs-lookup"><span data-stu-id="10dbf-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="10dbf-159">Sie können die Knoten, um die Tabellen anzuzeigen, die EF erstellt erweitern.</span><span class="sxs-lookup"><span data-stu-id="10dbf-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="10dbf-160">Klicken Sie zum Anzeigen der Daten, mit der rechten Maustaste in einer Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="10dbf-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="10dbf-161">Der folgende Screenshot zeigt die Ergebnisse für die Bücher-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="10dbf-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="10dbf-162">Beachten Sie, dass EF, die Datenbank mit der Seed-Daten aufgefüllt, und die Tabelle die Fremdschlüssel für die Authors-Tabelle enthält.</span><span class="sxs-lookup"><span data-stu-id="10dbf-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="10dbf-163">[Zurück](part-2.md)
> [Weiter](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="10dbf-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
