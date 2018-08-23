---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Erstellen Sie eine Datenbank | Microsoft-Dokumentation
author: microsoft
description: Schritt 2 zeigt die Schritte zum Erstellen der Datenbank enthält alle von der Dinner und Antworten Sie Daten für die NerdDinner-Anwendung.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833418"
---
<a name="create-a-database"></a><span data-ttu-id="4dd5b-103">Erstellen Sie eine Datenbank</span><span class="sxs-lookup"><span data-stu-id="4dd5b-103">Create a Database</span></span>
====================
<span data-ttu-id="4dd5b-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4dd5b-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="4dd5b-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="4dd5b-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="4dd5b-106">Dies ist Schritt 2, der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="4dd5b-107">Schritt 2 zeigt die Schritte zum Erstellen der Datenbank enthält alle von der Dinner und Antworten Sie Daten für die NerdDinner-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="4dd5b-108">Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="4dd5b-109">NerdDinner, Schritt 2: Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="4dd5b-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="4dd5b-110">Wir werden eine Datenbank verwenden, um alle Dinner und RSVP Daten für die NerdDinner-Anwendung zu speichern.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="4dd5b-111">Die folgenden Schritte zeigen, der mit der kostenlosen SQL Server Express Edition-Datenbank erstellen (die Sie ganz einfach mit Version 2 der installieren können die [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="4dd5b-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="4dd5b-112">Der gesamte wir schreiben Code funktioniert mit SQL Server Express und die vollständige SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="4dd5b-113">Erstellen einer neuen SQL Server Express-Datenbank</span><span class="sxs-lookup"><span data-stu-id="4dd5b-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="4dd5b-114">Wir beginnen, indem Sie mit der rechten Maustaste auf unsere Webprojekt, und wählen Sie dann die **Add-&gt;neues Element** Menübefehl:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="4dd5b-115">Dialogfeld für Visual Studio-"Neues Element hinzufügen" wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="4dd5b-116">Wir filtern, indem Sie die Kategorie "Data" und wählen die Elementvorlage "SQL-Datenbank":</span><span class="sxs-lookup"><span data-stu-id="4dd5b-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="4dd5b-117">Namen die SQL Server Express-Datenbank, die zum Erstellen von "NerdDinner.mdf", und drücken hier also Okay werden sollten.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="4dd5b-118">Visual Studio wird dann Fragen Sie uns, wenn wir unsere \App dieser Datei hinzufügen möchten\_Verzeichnis "Data" (durch die bereits ein Verzeichnis ist Einrichtung mit Lese- und Schreiben von Sicherheits-ACLs):</span><span class="sxs-lookup"><span data-stu-id="4dd5b-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="4dd5b-119">"Ja" klicken, und der neuen Datenbank wird erstellt und unsere Projektmappen-Explorer hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="4dd5b-120">Erstellen von Tabellen in unserer Datenbank</span><span class="sxs-lookup"><span data-stu-id="4dd5b-120">Creating Tables within our Database</span></span>

<span data-ttu-id="4dd5b-121">Wir haben jetzt eine neue leere Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-121">We now have a new empty database.</span></span> <span data-ttu-id="4dd5b-122">Wir fügen Sie einige Tabellen hinzu.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-122">Let's add some tables to it.</span></span>

<span data-ttu-id="4dd5b-123">Zu diesem Zweck navigieren wir zu der Registerkarte "Server-Explorer" im Fenster in Visual Studio, die wir zum Verwalten von Datenbanken und Server ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="4dd5b-124">SQL Server Express-Datenbanken, die in der \App gespeichert\_Datenordner der Anwendung wird automatisch im Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="4dd5b-125">Wir können optional das Symbol "Verbindung mit Datenbank herstellen" oben auf das Fenster "Server-Explorer" verwenden, um zusätzliche SQL Server-Datenbanken (lokal und remote) sowie der Liste hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="4dd5b-126">Wir werden unsere NerdDinner-Datenbank – einen zum Speichern unserer Dinner, und die andere zum Nachverfolgen von RSVP Annahme werden zwei Tabellen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="4dd5b-127">Wir können neue Tabellen erstellen, indem Sie mit der rechten Maustaste auf den Ordner "Tables" in unserer Datenbank und Auswählen des Menübefehls "Neue Tabelle hinzufügen":</span><span class="sxs-lookup"><span data-stu-id="4dd5b-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="4dd5b-128">Dadurch wird eine Tabellen-Designer geöffnet, die wir das Schema der Tabelle konfigurieren zu können.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="4dd5b-129">Fügen wir für unsere Tabelle "Dinner" 10 Datenspalten hinzu:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="4dd5b-130">Wir möchten die Spalte "DinnerID" einem eindeutigen Primärschlüssel für die Tabelle zu sein.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="4dd5b-131">Wir können dies konfigurieren, indem Sie mit der rechten Maustaste auf die Spalte "DinnerID" und das Menüelement "Primärschlüssel festlegen" auswählen:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="4dd5b-132">Zusätzlich zum DinnerID einen Primärschlüssel, wir sollten es als eine Spalte "Identity" konfigurieren, deren Wert wird automatisch erhöht werden, in der Tabelle neue Datenzeilen hinzugefügt werden (d. h. die erste Zeile der eingefügte Dinner müssen eine DinnerID 1 aus, die zweite eingefügte Zeile hat eine DinnerID 2 usw.).</span><span class="sxs-lookup"><span data-stu-id="4dd5b-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="4dd5b-133">Dazu die Spalte "DinnerID" Sie können und dann mithilfe des "Spalteneigenschaften"-Editors die Eigenschaft "(ist Identity)" in der Spalte auf "Ja" festlegen.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="4dd5b-134">Wir verwenden die Standardwerte für die standard-Identität (beginnen bei 1 und 1 für jede neue Zeile mit Dinner erhöht):</span><span class="sxs-lookup"><span data-stu-id="4dd5b-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="4dd5b-135">Speichern wir auch dann der Tabelle durch Eingabe von STRG + S oder mithilfe der **Dateien&gt;speichern** Menübefehl.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="4dd5b-136">Dies fordert uns, die den Namen der Tabelle.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-136">This will prompt us to name the table.</span></span> <span data-ttu-id="4dd5b-137">Es "Dinner" nennen:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="4dd5b-138">Unsere neue Dinner-Tabelle wird dann in unserer Datenbank im Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="4dd5b-139">Wir dann wiederholen Sie die oben genannten Schritte und erstellen Sie eine Tabelle "Bestätigung".</span><span class="sxs-lookup"><span data-stu-id="4dd5b-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="4dd5b-140">Diese Tabelle mit haben 3 Spalten.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-140">This table with have 3 columns.</span></span> <span data-ttu-id="4dd5b-141">Wir richten die RsvpID-Spalte als Primärschlüssel, und stellen Sie sie außerdem eine Identity-Spalte:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="4dd5b-142">Wir speichern Sie sie und geben Sie ihm den Namen "Bestätigung".</span><span class="sxs-lookup"><span data-stu-id="4dd5b-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="4dd5b-143">Das Einrichten einer Foreign Key-Beziehung zwischen Tabellen</span><span class="sxs-lookup"><span data-stu-id="4dd5b-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="4dd5b-144">Wir haben jetzt zwei Tabellen in unserer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-144">We now have two tables within our database.</span></span> <span data-ttu-id="4dd5b-145">Im letzten Schritt der Schema Design werden Sie eine "1-n-Beziehung zwischen diesen beiden Tabellen – einrichten, damit wir jede Dinner-Zeile mit NULL oder mehr Zeilen von RSVP zuordnen können, die für sie gelten.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="4dd5b-146">Wir tun dies, indem RSVP Spalte der Tabelle "DinnerID" um einen Fremdschlüssel-Beziehung auf die Spalte "DinnerID" in der Tabelle "Dinner" konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="4dd5b-147">Zu diesem Zweck werden wir der RSVP-Tabelle im Tabellen-Designer aktivieren, indem sie im Server-Explorer darauf doppelklicken.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="4dd5b-148">Ich wähle klicken Sie dann die Spalte "DinnerID" darin, mit der rechten Maustaste, und wählen Sie den Befehl im Kontextmenü "Relationshps...":</span><span class="sxs-lookup"><span data-stu-id="4dd5b-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="4dd5b-149">Hierdurch wird ein Dialogfeld angezeigt, die wir zum Setup von Beziehungen zwischen Tabellen verwenden können:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="4dd5b-150">Wir werden auf die Schaltfläche "Hinzufügen", um das Dialogfeld eine neue Beziehung hinzufügen klicken.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="4dd5b-151">Nachdem Sie eine Beziehung hinzugefügt wurde, wir erweitern den Knoten "Tabellen und Spaltenspezifikation" Strukturansicht, innerhalb des eigenschaftsrasters auf der rechten Seite des Dialogfelds, und klicken Sie dann auf die Schaltfläche "...", die sich rechts von:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="4dd5b-152">Klicken Sie auf die Schaltfläche "..." wird ein weiteres Dialogfeld angezeigt, mit der wir angeben, welche Tabellen und Spalten in der Beziehung beteiligt sind, sowie können wir nennen Sie die Beziehung.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="4dd5b-153">Wir ändern die Primärschlüsseltabelle "Dinner" werden, und wählen Sie die Spalte "DinnerID" innerhalb der Dinner-Tabelle als Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="4dd5b-154">Unsere RSVP-Tabelle werden die Fremdschlüsseltabelle, und der Bestätigung. DinnerID-Spalte wird als der fremdschlüsselbeziehung zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="4dd5b-155">Jetzt wird jede Zeile in der RSVP-Tabelle eine Zeile in die Dinner-Tabelle zugeordnet sein.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="4dd5b-156">SQL Server wird referenziellen Integrität für uns – verwalten und zu verhindern, dass wir eine neue RSVP Zeile hinzufügen, wenn es sich nicht auf eine gültige Dinner Zeile verweist.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="4dd5b-157">Es wird auch verhindern, dass uns das Löschen einer Dinner-Zeile, treten immer noch Antworten von Zeilen, die darauf verweist.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="4dd5b-158">Hinzufügen von Daten zu den Tabellen</span><span class="sxs-lookup"><span data-stu-id="4dd5b-158">Adding Data to our Tables</span></span>

<span data-ttu-id="4dd5b-159">Lassen Sie uns durch das Hinzufügen von Beispieldaten mit unserer Tabelle Dinner abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="4dd5b-160">Wir können Daten in eine Tabelle hinzufügen, indem Sie mit der rechten Maustaste auf die sie im Server-Explorer, und wählen den Befehl "Tabellendaten anzeigen":</span><span class="sxs-lookup"><span data-stu-id="4dd5b-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="4dd5b-161">Wir fügen einige Zeilen der Dinner-Daten, die wir später dem Implementieren der Anwendungs verwenden können:</span><span class="sxs-lookup"><span data-stu-id="4dd5b-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="4dd5b-162">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="4dd5b-162">Next Step</span></span>

<span data-ttu-id="4dd5b-163">Wir haben die Erstellung der Datenbank abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-163">We've finished creating our database.</span></span> <span data-ttu-id="4dd5b-164">Als Nächstes erstellen wir ViewModel-Klassen, die wir zum Abfragen und aktualisieren Sie sie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="4dd5b-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4dd5b-165">[Zurück](create-a-new-aspnet-mvc-project.md)
> [Weiter](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="4dd5b-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
