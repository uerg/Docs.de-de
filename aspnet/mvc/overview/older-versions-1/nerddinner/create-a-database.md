---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Erstellen Sie eine Datenbank | Microsoft Docs
author: microsoft
description: Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, die alle von der Dinner und RSVP Daten für unsere NerdDinner-Anwendung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="create-a-database"></a><span data-ttu-id="71538-103">Erstellen Sie eine Datenbank</span><span class="sxs-lookup"><span data-stu-id="71538-103">Create a Database</span></span>
====================
<span data-ttu-id="71538-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="71538-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="71538-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="71538-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="71538-106">Dies ist Schritt 2 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.</span><span class="sxs-lookup"><span data-stu-id="71538-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="71538-107">Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, die alle von der Dinner und RSVP Daten für unsere NerdDinner-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="71538-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="71538-108">Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.</span><span class="sxs-lookup"><span data-stu-id="71538-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="71538-109">NerdDinner, Schritt 2: Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="71538-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="71538-110">Wir müssen eine Datenbank verwenden, um alle Daten Dinner und die Antwort für unsere NerdDinner-Anwendung speichern.</span><span class="sxs-lookup"><span data-stu-id="71538-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="71538-111">Die folgenden Schritte veranschaulichen das Erstellen der Datenbank mithilfe der kostenlosen SQL Server Express Edition (die Sie leicht installieren können mithilfe von V2 der [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="71538-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="71538-112">Der gesamte Code wir schreiben funktioniert mit SQL Server Express und die Vollversion von SQL Server.</span><span class="sxs-lookup"><span data-stu-id="71538-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="71538-113">Erstellen einer neuen SQL Server Express-Datenbank</span><span class="sxs-lookup"><span data-stu-id="71538-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="71538-114">Wir beginnen, indem Sie mit der rechten Maustaste auf unserem Webprojekt und wählen Sie dann die **Add -&gt;neues Element** Menübefehl:</span><span class="sxs-lookup"><span data-stu-id="71538-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="71538-115">Hierdurch wird Visual Studio das Dialogfeld "Neues Element hinzufügen" "angezeigt.</span><span class="sxs-lookup"><span data-stu-id="71538-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="71538-116">Wir filtern, indem Sie die "Datenkategorie" fort und wählen die Elementvorlage "SQL Server-Datenbank":</span><span class="sxs-lookup"><span data-stu-id="71538-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="71538-117">Namen die SQL Server Express-Datenbank zu erstellen "NerdDinner.mdf", und klicken Sie auf ok.</span><span class="sxs-lookup"><span data-stu-id="71538-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="71538-118">Visual Studio wird dann Fragen Sie uns, wenn wir unsere \App dieser Datei hinzufügen möchten\_Datenverzeichnis (durch die bereits ein Verzeichnis ist setup mit Lese- und Sicherheits-ACLs schreiben):</span><span class="sxs-lookup"><span data-stu-id="71538-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="71538-119">Wir müssen auf "Ja" klicken und die neue Datenbank erstellt und unsere Projektmappen-Explorer hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="71538-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="71538-120">Erstellen von Tabellen in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="71538-120">Creating Tables within our Database</span></span>

<span data-ttu-id="71538-121">Wir haben jetzt eine neue leere Datenbank.</span><span class="sxs-lookup"><span data-stu-id="71538-121">We now have a new empty database.</span></span> <span data-ttu-id="71538-122">Fügen Sie nun einige Tabellen hinzu.</span><span class="sxs-lookup"><span data-stu-id="71538-122">Let's add some tables to it.</span></span>

<span data-ttu-id="71538-123">Zu diesem Zweck wechseln wir zu "Verwaltung" die "Server-Explorer" in Visual Studio die uns zum Verwalten von Datenbanken und Servern ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="71538-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="71538-124">SQL Server Express-Datenbanken, die in der \App gespeichert\_Datenordner der Anwendung wird automatisch innerhalb des Server-Explorers angezeigt.</span><span class="sxs-lookup"><span data-stu-id="71538-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="71538-125">Wir können optional das Symbol "Verbindung mit Datenbank herstellen" oben auf das Fenster "Server-Explorer" verwenden, um zusätzliche SQL Server-Datenbanken (lokal und remote) als auch der Liste hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="71538-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="71538-126">Wir werden unsere NerdDinner-Datenbank – einen zum Speichern von unseren Abendessen angezeigt, und die andere um Antwort Akzepte nachzuverfolgen zwei Tabellen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="71538-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="71538-127">Wir können neue Tabellen erstellen, indem Sie mit der rechten Maustaste auf den Ordner "Tables" innerhalb der Datenbank und des Menübefehls "Neue Tabelle hinzufügen":</span><span class="sxs-lookup"><span data-stu-id="71538-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="71538-128">Dadurch wird eine Tabellen-Designer geöffnet, die uns so konfigurieren Sie das Schema der Tabelle ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="71538-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="71538-129">Für unsere Tabelle "Abendessen" werden wir 10 Datenspalten hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="71538-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="71538-130">Wir möchten die Spalte "DinnerID" zu einem eindeutigen Primärschlüssel für die Tabelle.</span><span class="sxs-lookup"><span data-stu-id="71538-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="71538-131">Wir können dies konfigurieren, indem Sie mit der rechten Maustaste auf die Spalte "DinnerID" und das Menüelement "Primärschlüssel festlegen":</span><span class="sxs-lookup"><span data-stu-id="71538-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="71538-132">Zusätzlich zu den vorgenommen DinnerID ein Primärschlüssel ist, möchten wir auch ebenfalls eine Spalte "Identity" zu konfigurieren, deren Wert wird automatisch erhöht, wenn neue Zeilen von Daten zur Tabelle hinzugefügt werden (d. h., die erste Zeile der eingefügte Dinner müssen eine DinnerID 1, das zweite eingefügte Zeile weist eine DinnerID 2 usw.).</span><span class="sxs-lookup"><span data-stu-id="71538-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="71538-133">Wir können dazu die Spalte "DinnerID" und dann mithilfe des "Spalteneigenschaften"-Editors die Eigenschaft "(ist Identity)" für die Spalte auf "Ja" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="71538-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="71538-134">Wir verwenden die Standardidentität Standardwerte (beginnen bei 1 und 1 für jede neue Dinner Zeile erhöht):</span><span class="sxs-lookup"><span data-stu-id="71538-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="71538-135">Wir werden unsere Tabelle dann speichern, durch Eingabe von STRG + S oder mithilfe der **Datei -&gt;speichern** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="71538-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="71538-136">Daraufhin werden wir den Namen der Tabelle aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="71538-136">This will prompt us to name the table.</span></span> <span data-ttu-id="71538-137">Er "Abendessen" den Namen:</span><span class="sxs-lookup"><span data-stu-id="71538-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="71538-138">Unsere neue Abendessen Tabelle wird dann in die Datenbank im Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="71538-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="71538-139">Wir dann wiederholen Sie die oben genannten Schritte und erstellen Sie eine Tabelle "Antwort".</span><span class="sxs-lookup"><span data-stu-id="71538-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="71538-140">Diese Tabelle mit 3 Spalten erhalten</span><span class="sxs-lookup"><span data-stu-id="71538-140">This table with have 3 columns.</span></span> <span data-ttu-id="71538-141">Wir werden die RsvpID-Spalte als Primärschlüssel einrichten, und auch eine Identity-Spalte:</span><span class="sxs-lookup"><span data-stu-id="71538-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="71538-142">Wir müssen speichern, und geben Sie ihm den Namen "Antwort".</span><span class="sxs-lookup"><span data-stu-id="71538-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="71538-143">Einrichten einer Foreign Key-Beziehung zwischen Tabellen</span><span class="sxs-lookup"><span data-stu-id="71538-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="71538-144">Wir haben jetzt die beiden Tabellen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="71538-144">We now have two tables within our database.</span></span> <span data-ttu-id="71538-145">Unsere letzte Schritt der Schema-Entwurf werden eine "eins zu viele"-Beziehung zwischen diesen beiden Tabellen – einrichten, damit wir jede Zeile Dinner NULL oder mehr Zeilen von Antwort zuordnen können, die für sie gelten.</span><span class="sxs-lookup"><span data-stu-id="71538-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="71538-146">Es wird dazu konfigurieren die Antwort Spalte der Tabelle "DinnerID" um einen Fremdschlüssel-Beziehung auf die Spalte "DinnerID" in der Tabelle "Abendessen" haben.</span><span class="sxs-lookup"><span data-stu-id="71538-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="71538-147">Zu diesem Zweck müssen wir die Antwort-Tabelle im Tabellen-Designer öffnen, durch Doppelklick im Server-Explorer.</span><span class="sxs-lookup"><span data-stu-id="71538-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="71538-148">Wir wählen dann die Spalte "DinnerID" darin, mit der rechten Maustaste, und wählen Sie "Relationshps..."</span><span class="sxs-lookup"><span data-stu-id="71538-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="71538-149">Der Kontextmenübefehl:</span><span class="sxs-lookup"><span data-stu-id="71538-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="71538-150">Hierdurch wird ein Dialogfeld angezeigt, die wir zum Setup-Beziehungen zwischen Tabellen verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="71538-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="71538-151">Die Schaltfläche "Hinzufügen", um das Dialogfeld eine neue Beziehung hinzufügen klicken.</span><span class="sxs-lookup"><span data-stu-id="71538-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="71538-152">Sobald eine Beziehung hinzugefügt wurde, müssen wir Knoten "Tabellen und Spaltenspezifikation" Strukturansicht im Eigenschaftenraster auf der rechten Seite des Dialogfelds und klicken Sie dann auf die Schaltfläche "…" rechts davon:</span><span class="sxs-lookup"><span data-stu-id="71538-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="71538-153">Klicken auf die Schaltfläche "…" wird ein weiteres Dialogfeld angezeigt, die uns ermöglicht, die angeben, welche Tabellen und Spalten in der Beziehung beteiligt sind, sowie ermöglichen es uns, um die Beziehung zu benennen.</span><span class="sxs-lookup"><span data-stu-id="71538-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="71538-154">Wir ändern die Primärschlüsseltabelle "Abendessen" angezeigt werden, und wählen Sie die Spalte "DinnerID" innerhalb der Tabelle Abendessen als Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="71538-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="71538-155">Unsere Antwort-Tabelle werden die Fremdschlüssel-Tabelle und die Antwort. DinnerID-Spalte wird als der Fremdschlüssel zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="71538-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="71538-156">Jetzt wird jede Zeile in der Antwort-Tabelle eine Zeile in der Tabelle Dinner zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="71538-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="71538-157">SQL Server wird die referenziellen Integrität für uns – verwalten und verhindern, dass wir eine neue Antwort Zeile hinzufügen, wenn er nicht auf eine gültige Dinner Zeile verweist.</span><span class="sxs-lookup"><span data-stu-id="71538-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="71538-158">Es wird auch verhindert, dass uns Löschen einer Zeile Dinner treten weiterhin RSVP von Zeilen, die darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="71538-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="71538-159">Hinzufügen von Daten zu den Tabellen</span><span class="sxs-lookup"><span data-stu-id="71538-159">Adding Data to our Tables</span></span>

<span data-ttu-id="71538-160">Fertig stellen wir einige Beispieldaten mit unserer Tabelle Abendessen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="71538-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="71538-161">Wir können die Daten in eine Tabelle hinzufügen, indem Sie mit der rechten Maustaste auf die sie in Server-Explorer und den Befehl "Tabellendaten anzeigen":</span><span class="sxs-lookup"><span data-stu-id="71538-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="71538-162">Wir fügen einige Dinner Datenzeilen, die wir später wie beginnen wir implementieren die Anwendung verwenden können:</span><span class="sxs-lookup"><span data-stu-id="71538-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="71538-163">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="71538-163">Next Step</span></span>

<span data-ttu-id="71538-164">Wir haben die Erstellung der Datenbank ist abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="71538-164">We've finished creating our database.</span></span> <span data-ttu-id="71538-165">Jetzt erstellen wir Modellklassen, die wir zum Abfragen und aktualisieren Sie ihn verwenden können.</span><span class="sxs-lookup"><span data-stu-id="71538-165">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71538-166">[Zurück](create-a-new-aspnet-mvc-project.md)
> [Weiter](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="71538-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
