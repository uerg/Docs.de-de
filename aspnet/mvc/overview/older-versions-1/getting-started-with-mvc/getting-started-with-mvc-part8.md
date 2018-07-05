---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Hinzufügen einer Spalte mit dem Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817200"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="97dde-104">Hinzufügen einer Spalte mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="97dde-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="97dde-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="97dde-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="97dde-106">Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="97dde-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="97dde-107">Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.</span><span class="sxs-lookup"><span data-stu-id="97dde-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="97dde-108">Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="97dde-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="97dde-109">In diesem Abschnitt werden wir erläutert, wie wir können Änderungen vornehmen, um das Schema der Datenbank, und behandeln die Änderungen in unserer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="97dde-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="97dde-110">Fügen Sie eine Spalte "Rating", auf die Tabelle "Movie".</span><span class="sxs-lookup"><span data-stu-id="97dde-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="97dde-111">Wechseln Sie zurück zur IDE, und klicken Sie auf die Datenbank-Explorer.</span><span class="sxs-lookup"><span data-stu-id="97dde-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="97dde-112">Klicken Sie mit der rechten Maustaste auf die Tabelle "Movie" aus, und wählen Sie die Tabellendefinition öffnen.</span><span class="sxs-lookup"><span data-stu-id="97dde-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="97dde-113">Fügen Sie eine Spalte "Rating" hinzu, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="97dde-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="97dde-114">Da wir jetzt nicht über alle Bewertungen verfügen, kann die Spalte NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="97dde-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="97dde-115">Klicken Sie auf Speichern.</span><span class="sxs-lookup"><span data-stu-id="97dde-115">Click Save.</span></span>

<span data-ttu-id="97dde-116">[![Filme Tabelle bearbeiten](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="97dde-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="97dde-117">Klicken Sie dann zurück zum Projektmappen-Explorer, und öffnen Sie die Movies.edmx-Datei (die im Ordner "\Models").</span><span class="sxs-lookup"><span data-stu-id="97dde-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="97dde-118">Klicken Sie mit der rechten Maustaste auf die Entwurfsoberfläche (der weiße Fläche), und wählen Sie Modell aktualisieren, aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="97dde-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="97dde-119">[![Filme – Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="97dde-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="97dde-120">Hierdurch wird der Assistent"Update".</span><span class="sxs-lookup"><span data-stu-id="97dde-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="97dde-121">Klicken Sie auf der Registerkarte "Aktualisieren", darin, und klicken Sie auf "Fertig stellen".</span><span class="sxs-lookup"><span data-stu-id="97dde-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="97dde-122">Unsere Movie-Modell-Klasse wird dann mit der neuen Spalte aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="97dde-122">Our Movie model class will then be updated with the new column.</span></span>

![Update-Assistenten (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="97dde-124">Nach dem Klicken auf "Fertig stellen", sehen Sie sich, dass die Entität "Movie" in unserem Modell der neuen Spalte für die Bewertung hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="97dde-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="97dde-125">[![Film-Entität](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="97dde-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="97dde-126">Wir haben eine Spalte hinzugefügt, in das Datenbankmodell, aber die Ansichten nicht darüber zu informieren.</span><span class="sxs-lookup"><span data-stu-id="97dde-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="97dde-127">Aktualisieren von Sichten mit Änderungen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="97dde-127">Update Views with Model Changes</span></span>

<span data-ttu-id="97dde-128">Es gibt mehrere Möglichkeiten, die wir unsere Vorlagen anzeigen, entsprechend die neue Spalte für die Bewertung aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="97dde-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="97dde-129">Da wir diese Sichten erstellt, indem sie über das Dialogfeld "Ansicht hinzufügen" zu generieren, können wir löschen und wieder neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="97dde-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="97dde-130">Allerdings in der Regel Personen werden Änderungen an ihrer Vorlagen anzeigen, aus der eingerüstete anfangsgenerierung bereits vorgenommen haben und hinzufügen oder Löschen von Feldern manuell, wie wir mit dem ID-Feld erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="97dde-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="97dde-131">Öffnen Sie die \Views\Movies\Index.aspx-Vorlage und fügen eine &lt;th&gt;Bewertung&lt;/th&gt; an den Anfang der Tabelle "Movie".</span><span class="sxs-lookup"><span data-stu-id="97dde-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="97dde-132">Ich habe hinzugefügt mir nach "Genre".</span><span class="sxs-lookup"><span data-stu-id="97dde-132">I added mine after Genre.</span></span> <span data-ttu-id="97dde-133">Fügen Sie dann in die gleiche Spaltenposition aber weiter unten, einer Zeile, um unsere neue Bewertung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="97dde-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="97dde-134">Unsere Vorlage für endgültige Index.aspx wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="97dde-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="97dde-135">Lassen Sie uns dann öffnen Sie die \Views\Movies\Create.aspx-Vorlage und fügen Sie eine Bezeichnung und ein Textfeld für unsere neue Rating-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="97dde-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="97dde-136">Unsere Vorlage für endgültige Create.aspx wird wie folgt aussehen, und lassen Sie uns Titel und die sekundäre Datenbank des Browsers &lt;h2&gt; Titel, z. B. "Erstellen einer Film", während wir hier sind!</span><span class="sxs-lookup"><span data-stu-id="97dde-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="97dde-137">Führen Sie die app, und Sie haben jetzt ein neues Feld in der Datenbank, die auf der Seite "erstellen" hinzugefügt wurden, ist.</span><span class="sxs-lookup"><span data-stu-id="97dde-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="97dde-138">Fügen Sie einen neuen Film - diesmal mit einer Bewertung –, und klicken Sie auf erstellen.</span><span class="sxs-lookup"><span data-stu-id="97dde-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="97dde-139">[![Erstellen Sie einen Film - Windows InternetExplorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="97dde-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="97dde-140">Nachdem Sie auf "erstellen" klicken, werden Sie zur Indexseite gesendet, wo Sie mit neuen Film aufgeführt ist die neue Bewertung-Spalte in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="97dde-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="97dde-141">[![Filmliste – Windows InternetExplorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="97dde-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="97dde-142">Dieses Lernprogramm haben Sie die Schritte der Controller mit Ansichten verknüpft werden, und übergeben hartcodierte Daten vornehmen.</span><span class="sxs-lookup"><span data-stu-id="97dde-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="97dde-143">Anschließend wurde erstellt und eine Datenbank entwickelt und einige Daten legt in.</span><span class="sxs-lookup"><span data-stu-id="97dde-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="97dde-144">Wir die Daten aus der Datenbank abgerufen und diese in eine HTML-Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="97dde-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="97dde-145">Dann haben wir ein Erstellungsformular, mit denen der Benutzer, die Daten in der Datenbank selbst aus der Web-Anwendung hinzufügen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="97dde-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="97dde-146">Wir Überprüfung hinzugefügt, und klicken Sie dann die Überprüfung mithilfe von JavaScript auf der Clientseite vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="97dde-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="97dde-147">Abschließend wir die Datenbank enthält eine neue Spalte mit Daten geändert und unsere zwei Seiten zum Erstellen und zum Anzeigen dieser neuen Daten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="97dde-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="97dde-148">Jetzt sollten Sie in unserem Tutorial für fortgeschrittene zu verschieben, auf "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" sowie die vielen Videos und Ressourcen zu [ https://asp.net/mvc ](https://asp.net/mvc) auch weitere Informationen zur ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="97dde-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="97dde-149">Viel Erfolg!</span><span class="sxs-lookup"><span data-stu-id="97dde-149">Enjoy!</span></span>

- <span data-ttu-id="97dde-150">Scott Hanselman – [ http://hanselman.com ](http://hanselman.com) und [ @shanselman ](http://twitter.com/shanselman) auf Twitter.</span><span class="sxs-lookup"><span data-stu-id="97dde-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="97dde-151">Vorherige</span><span class="sxs-lookup"><span data-stu-id="97dde-151">Previous</span></span>](getting-started-with-mvc-part7.md)
