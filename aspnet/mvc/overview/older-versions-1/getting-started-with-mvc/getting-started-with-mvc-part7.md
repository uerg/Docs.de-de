---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Hinzufügen einer Validierung zum Modell | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871947"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="239de-104">Hinzufügen einer Validierung zum Modell</span><span class="sxs-lookup"><span data-stu-id="239de-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="239de-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="239de-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="239de-106">Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="239de-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="239de-107">Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="239de-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="239de-108">Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="239de-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="239de-109">In diesem Abschnitt werden wir implementieren die Unterstützung für die Validierung von Benutzereingaben innerhalb der Anwendung aktivieren.</span><span class="sxs-lookup"><span data-stu-id="239de-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="239de-110">Wir müssen sicherstellen, dass unsere Datenbankinhalte immer richtig ist und hilfreiche Fehlermeldungen für Endbenutzer bereitzustellen, wenn sie versuchen, und geben Sie die Filmdaten, die was nicht zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="239de-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="239de-111">Wir beginnen, indem Sie ein wenig Validierungslogik auf die Film-Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="239de-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="239de-112">Klicken Sie mit der rechten Maustaste auf den Ordner Modell, und wählen Sie die Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="239de-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="239de-113">Benennen Sie Ihre Film-Klasse.</span><span class="sxs-lookup"><span data-stu-id="239de-113">Name your class Movie.</span></span>

<span data-ttu-id="239de-114">Wenn wir das Entitätsmodell Film vorher erstellt haben, erstellt die IDE eine Film-Klasse.</span><span class="sxs-lookup"><span data-stu-id="239de-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="239de-115">In der Tat kann Teil der Film-Klasse in eine Datei und in einer anderen.</span><span class="sxs-lookup"><span data-stu-id="239de-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="239de-116">Dies ist eine partielle Klasse aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="239de-116">This is called a Partial Class.</span></span> <span data-ttu-id="239de-117">Wir werden die Film-Klasse aus einer anderen Datei erweitern.</span><span class="sxs-lookup"><span data-stu-id="239de-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="239de-118">Wir erstellen eine partielle Film-Klasse, verweist auf eine "Buddy-Klasse" für einige Attribute, die Überprüfung Hinweise mit dem System bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="239de-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="239de-119">Wir Titel und Preis nach Bedarf zu markieren, und außerdem bestehen, dass der Preis innerhalb eines bestimmten Bereichs darstellen.</span><span class="sxs-lookup"><span data-stu-id="239de-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="239de-120">Klicken Sie mit der mit der rechten Maustaste auf den Ordner Models, und wählen Sie die Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="239de-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="239de-121">Benennen Sie Ihre Film-Klasse, und klicken Sie auf die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="239de-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="239de-122">Hier ist unsere partielle Film Klasse-ähnlichen.</span><span class="sxs-lookup"><span data-stu-id="239de-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="239de-123">Führen Sie die Anwendung erneut aus, und versuchen Sie, einen Film mit einem Preis über 100 eingeben.</span><span class="sxs-lookup"><span data-stu-id="239de-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="239de-124">Nachdem Sie das Formular gesendet haben, erhalten Sie einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="239de-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="239de-125">Der Fehler abgefangen wird, auf der Serverseite und tritt nach dem Bereitstellen des Formulars gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="239de-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="239de-126">Beachten Sie, wie die integrierten HTML-Hilfsmethoden des ASP.NET intelligent genug, um die Fehlermeldung angezeigt und verwalten Sie die Werte für uns innerhalb der Textbox-Elemente wurden:</span><span class="sxs-lookup"><span data-stu-id="239de-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="239de-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="239de-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="239de-128">Dies funktioniert hervorragend, aber es wäre schön, wenn den Benutzer auf der Clientseite sofort mitgeteilt konnte vor der Server beteiligt ruft.</span><span class="sxs-lookup"><span data-stu-id="239de-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="239de-129">Wir einige clientseitige Validierung mit JavaScript zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="239de-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="239de-130">Die clientseitige Validierung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="239de-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="239de-131">Da unsere Film-Klasse bereits einige Validierungsattribute aufweist, müssen wir nur unsere Create.aspx Ansicht Vorlage einige JavaScript-Dateien hinzu, und fügen eine Codezeile aktivieren Sie die clientseitige Überprüfung stattfinden.</span><span class="sxs-lookup"><span data-stu-id="239de-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="239de-132">Wechseln Sie von innerhalb VWD unsere Ansichten/Film-Ordner, und öffnen Sie Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="239de-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="239de-133">Öffnen Sie den Skriptordner im Projektmappen-Explorer, und ziehen Sie die folgenden drei Skripts innerhalb der &lt;Head&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="239de-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="239de-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="239de-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="239de-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="239de-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="239de-136">Diese Skriptdateien werden in dieser Reihenfolge angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="239de-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="239de-137">Darüber hinaus fügen Sie dieser einzelnen Zeile oberhalb der Html.BeginForm hinzu:</span><span class="sxs-lookup"><span data-stu-id="239de-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="239de-138">Hier ist der Code in der IDE angezeigt.</span><span class="sxs-lookup"><span data-stu-id="239de-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="239de-139">[![Filme – Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="239de-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="239de-140">Führen Sie die Anwendung und besuchen Sie /Movies/Create erneut, und klicken Sie auf erstellen, ohne Eingabe von Daten.</span><span class="sxs-lookup"><span data-stu-id="239de-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="239de-141">Die Fehlermeldungen werden sofort ohne die Seite flash, ordnen wir Senden von Daten alle fort, bis der Server.</span><span class="sxs-lookup"><span data-stu-id="239de-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="239de-142">Grund hierfür ist ASP.NET MVC jetzt die Eingabe der sowohl der Client (JavaScript) überprüft wird und auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="239de-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="239de-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="239de-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="239de-144">Dies ist gute!</span><span class="sxs-lookup"><span data-stu-id="239de-144">This is looking good!</span></span> <span data-ttu-id="239de-145">Nehmen wir jetzt eine zusätzliche Spalte mit der Datenbank hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="239de-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="239de-146">[Zurück](getting-started-with-mvc-part6.md)
> [Weiter](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="239de-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
