---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Hinzufügen einer Spalte mit dem Modell | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
ms.locfileid: "30879256"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="1e415-104">Hinzufügen einer Spalte mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="1e415-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="1e415-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="1e415-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="1e415-106">Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1e415-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="1e415-107">Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1e415-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="1e415-108">Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="1e415-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="1e415-109">In diesem Abschnitt werden wir exemplarisch erklärt, wie wir nehmen Sie Änderungen an das Schema der unsere Datenbank und die Änderungen innerhalb der Anwendung behandeln können.</span><span class="sxs-lookup"><span data-stu-id="1e415-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="1e415-110">Fügen Sie einen Columnstore "Bewertung" der Film-Tabelle ein.</span><span class="sxs-lookup"><span data-stu-id="1e415-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="1e415-111">Wechseln Sie zurück in die IDE, und klicken Sie auf die Datenbank-Explorer.</span><span class="sxs-lookup"><span data-stu-id="1e415-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="1e415-112">Klicken Sie mit der mit der rechten Maustaste auf die Film-Tabelle aus, und wählen Sie die Tabellendefinition öffnen.</span><span class="sxs-lookup"><span data-stu-id="1e415-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="1e415-113">Fügen Sie eine Bewertungsspalte "" aus, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="1e415-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="1e415-114">Da wir nun alle Bewertungen haben, kann die Spalte NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="1e415-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="1e415-115">Klicken Sie auf Speichern.</span><span class="sxs-lookup"><span data-stu-id="1e415-115">Click Save.</span></span>

<span data-ttu-id="1e415-116">[![Filme Tabelle bearbeiten](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1e415-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="1e415-117">Klicken Sie dann zurück zum Projektmappen-Explorer, und öffnen Sie die Movies.edmx-Datei (die im Ordner "\Models").</span><span class="sxs-lookup"><span data-stu-id="1e415-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="1e415-118">Klicken Sie mit der rechten Maustaste auf die Entwurfsoberfläche (die weiße Fläche), und wählen Sie Updatemodell aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1e415-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="1e415-119">[![Filme – Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1e415-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="1e415-120">Hierdurch wird der Assistent"Update".</span><span class="sxs-lookup"><span data-stu-id="1e415-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="1e415-121">Klicken Sie auf der Registerkarte "Aktualisierung" darin, und klicken Sie auf ' Fertig stellen '.</span><span class="sxs-lookup"><span data-stu-id="1e415-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="1e415-122">Unsere Film Modellklasse wird dann mit der neuen Spalte aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1e415-122">Our Movie model class will then be updated with the new column.</span></span>

![Update-Assistenten (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="1e415-124">Nach dem Klicken auf "Fertig stellen" können Sie sehen, dass die neue Spalte für die Bewertung für die Entität Film in unserem Modell hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="1e415-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="1e415-125">[![Film-Entität](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="1e415-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="1e415-126">Wir haben eine Spalte hinzugefügt, in das Datenbankmodell, aber die Sichten nicht kennt.</span><span class="sxs-lookup"><span data-stu-id="1e415-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="1e415-127">Aktualisieren von Sichten mit Modelländerungen</span><span class="sxs-lookup"><span data-stu-id="1e415-127">Update Views with Model Changes</span></span>

<span data-ttu-id="1e415-128">Es gibt einige Möglichkeiten, die wir unsere Ansichtsvorlagen entsprechend die neue Spalte für die Bewertung aktualisieren konnte.</span><span class="sxs-lookup"><span data-stu-id="1e415-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="1e415-129">Da wir diese Sichten generiert werden, über das Dialogfeld "Ansicht hinzufügen" erstellt haben, konnten wir löschen und erneut neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1e415-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="1e415-130">Allerdings in der Regel Personen werden Änderungen an ihrer Ansichtsvorlagen aus der scaffolded anfangsgenerierung bereits vorgenommen haben und möchten hinzufügen oder Löschen von Felder manuell, genau wie mit dem ID-Feld zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1e415-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="1e415-131">Öffnen Sie die Vorlage \Views\Movies\Index.aspx und Hinzufügen einer &lt;ten&gt;Bewertung&lt;/th&gt; an den Anfang der Film-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="1e415-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="1e415-132">Ich hinzugefügt Meine nach "Genre".</span><span class="sxs-lookup"><span data-stu-id="1e415-132">I added mine after Genre.</span></span> <span data-ttu-id="1e415-133">Fügen Sie Sie dann in der gleichen Position der Spalte jedoch weiter unten einer Zeile, um unsere neue Bewertung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="1e415-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="1e415-134">Unsere letzte Index.aspx Vorlage wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="1e415-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="1e415-135">Wir dann öffnen Sie die Vorlage \Views\Movies\Create.aspx und fügen Sie eine Bezeichnung und ein Textfeld für unsere neue Eigenschaft für die Bewertung:</span><span class="sxs-lookup"><span data-stu-id="1e415-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="1e415-136">Unsere letzte Create.aspx Vorlage wie folgt aussehen soll, und Ändern des Browsers Titel und sekundäre &lt;h2&gt; Titel, z. B. "Erstellen eines Films", während wir hier sind!</span><span class="sxs-lookup"><span data-stu-id="1e415-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="1e415-137">Ausführen der app, und jetzt haben Sie ein neues Feld in der Datenbank, die auf der Seite "erstellen" hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="1e415-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="1e415-138">Fügen Sie einen neuen Film - diesmal mit einer Bewertung –, und klicken Sie auf erstellen.</span><span class="sxs-lookup"><span data-stu-id="1e415-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="1e415-139">[![Erstellen Sie einen Film - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="1e415-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="1e415-140">Nachdem Sie auf "erstellen" klicken, werden Sie auf der Seite "Index" gesendet, wo Sie neue Film mit aufgeführt ist die neue Spalte Bewertung in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="1e415-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="1e415-141">[![Film List – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="1e415-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="1e415-142">Dieses Lernprogramm wurde Ihnen den Einstieg, Controller, Ansichten zuordnen, und übergeben hartcodierte Daten vornehmen.</span><span class="sxs-lookup"><span data-stu-id="1e415-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="1e415-143">Dann wird erstellt, und eine Datenbank entworfen, und fügen Sie einige Daten in.</span><span class="sxs-lookup"><span data-stu-id="1e415-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="1e415-144">Wir die Daten aus der Datenbank abgerufen und in einer HTML-Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1e415-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="1e415-145">Klicken Sie dann hinzugefügt wird ein Erstellungsformular, mit denen den Benutzer, die Daten in die Datenbank selbst aus der Web-Anwendung hinzufügen kann.</span><span class="sxs-lookup"><span data-stu-id="1e415-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="1e415-146">Wir Validierung hinzugefügt, und die Überprüfung auf der Clientseite JavaScript zu verwenden versucht.</span><span class="sxs-lookup"><span data-stu-id="1e415-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="1e415-147">Schließlich die Datenbank um eine neue Spalte mit Daten enthalten geändert, anschließend führen wir unsere zwei Seiten zum Erstellen und zeigen Sie diese neuen Daten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1e415-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="1e415-148">Jetzt sollten Sie auf unser Tutorial aus, der Intermediate-Stufe zu verschieben, auf "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" sowie den zahlreichen Videos und Ressourcen zur [ https://asp.net/mvc ](https://asp.net/mvc) , sogar noch stärker ASP.NET MVC vertraut zu machen!</span><span class="sxs-lookup"><span data-stu-id="1e415-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="1e415-149">Viel Erfolg!</span><span class="sxs-lookup"><span data-stu-id="1e415-149">Enjoy!</span></span>

- <span data-ttu-id="1e415-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) und [ @shanselman ](http://twitter.com/shanselman) auf Twitter.</span><span class="sxs-lookup"><span data-stu-id="1e415-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1e415-151">Vorherige</span><span class="sxs-lookup"><span data-stu-id="1e415-151">Previous</span></span>](getting-started-with-mvc-part7.md)
