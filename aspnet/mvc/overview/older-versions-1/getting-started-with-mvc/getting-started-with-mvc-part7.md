---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Hinzufügen der Überprüfung zum Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837159"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="01796-104">Hinzufügen der Überprüfung zum Modell</span><span class="sxs-lookup"><span data-stu-id="01796-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="01796-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="01796-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="01796-106">Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="01796-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="01796-107">Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.</span><span class="sxs-lookup"><span data-stu-id="01796-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="01796-108">Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.</span><span class="sxs-lookup"><span data-stu-id="01796-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="01796-109">In diesem Abschnitt werden wir implementieren Sie die notwendige Unterstützung für die Validierung von Benutzereingaben in unserer Anwendung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="01796-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="01796-110">Wir stellen sicher, dass unsere Datenbankinhalte immer richtig ist und hilfreiche Fehlermeldungen für Endbenutzer bereit, wenn sie versuchen, und geben Sie die Daten ungültig ist.</span><span class="sxs-lookup"><span data-stu-id="01796-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="01796-111">Wir beginnen, indem die Movie-Klasse ein wenig Validierungslogik hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="01796-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="01796-112">Klicken Sie mit der rechten Maustaste auf den Modell-Ordner, und wählen Sie die Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="01796-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="01796-113">Benennen Sie die Movie-Klasse.</span><span class="sxs-lookup"><span data-stu-id="01796-113">Name your class Movie.</span></span>

<span data-ttu-id="01796-114">Wenn wir das Entitätsmodell Film zuvor erstellt haben, erstellt die IDE eine Movie-Klasse.</span><span class="sxs-lookup"><span data-stu-id="01796-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="01796-115">Tatsächlich können die Movie-Klasse in einer Datei und Teil an einer anderen angehören.</span><span class="sxs-lookup"><span data-stu-id="01796-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="01796-116">Dies ist eine partielle Klasse bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="01796-116">This is called a Partial Class.</span></span> <span data-ttu-id="01796-117">Wir werden die Movie-Klasse aus einer anderen Datei zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="01796-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="01796-118">Wir erstellen eine partielle Movie-Klasse, die auf "Buddy-Klasse" zeigt für einige Attribute, die Validierung Hinweise auf das System erhalten.</span><span class="sxs-lookup"><span data-stu-id="01796-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="01796-119">Wir markieren den Titel und der Preis nach Bedarf, und auch bestehen, dass der Preis innerhalb eines bestimmten Bereichs liegen.</span><span class="sxs-lookup"><span data-stu-id="01796-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="01796-120">Klicken Sie mit der rechten Maustaste auf den Ordner "Models", und wählen Sie die Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="01796-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="01796-121">Benennen Sie die Movie-Klasse, und klicken Sie auf die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="01796-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="01796-122">Hier ist unsere teilweise Movie-Klasse-ähnlichen.</span><span class="sxs-lookup"><span data-stu-id="01796-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="01796-123">Führen Sie die Anwendung erneut aus, und versuchen Sie, einen Film mit einem Preis über 100 einzugeben.</span><span class="sxs-lookup"><span data-stu-id="01796-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="01796-124">Sie erhalten eine Fehlermeldung, nachdem Sie das Formular gesendet haben.</span><span class="sxs-lookup"><span data-stu-id="01796-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="01796-125">Der Fehler abgefangen wird, auf dem Server, und es tritt auf, nachdem das Formular gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="01796-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="01796-126">Beachten Sie, wie integrierte HTML-Hilfsprogramme des ASP.NET intelligent genug, um die Fehlermeldung angezeigt, und behalten die Werte für uns in der Textbox-Elemente wurden:</span><span class="sxs-lookup"><span data-stu-id="01796-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="01796-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="01796-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="01796-128">Dies funktioniert hervorragend, aber es wäre schön, wenn wir den Benutzer auf der Clientseite sofort mitteilen könnten, bevor der Server beteiligt ruft.</span><span class="sxs-lookup"><span data-stu-id="01796-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="01796-129">Lassen Sie uns einige clientseitige Validierung mit JavaScript zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="01796-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="01796-130">Die clientseitige Validierung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="01796-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="01796-131">Da unsere Movie-Klasse bereits einige Validierungsattribute aufweist, müssen wir nur unseren Create.aspx ansichtsvorlage einige JavaScript-Dateien hinzu, und fügen eine Codezeile aktivieren Sie die clientseitige Validierung durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="01796-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="01796-132">In VWD wechseln Sie den Ordner Views/Film, und öffnen Sie Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="01796-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="01796-133">Öffnen Sie im Projektmappen-Explorer den Ordner "Scripts", und ziehen Sie die folgenden drei Skripts innerhalb der &lt;Head&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="01796-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="01796-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="01796-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="01796-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="01796-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="01796-136">Diese Skriptdateien in dieser Reihenfolge angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="01796-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="01796-137">Darüber hinaus fügen Sie dieser einzelnen Zeile oberhalb der Html.BeginForm hinzu:</span><span class="sxs-lookup"><span data-stu-id="01796-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="01796-138">Hier ist der Code in der IDE angezeigt.</span><span class="sxs-lookup"><span data-stu-id="01796-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="01796-139">[![Filme – Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="01796-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="01796-140">Führen Sie die Anwendung und besuchen Sie /Movies/Create erneut, und klicken Sie auf erstellen, ohne Eingabe von Daten.</span><span class="sxs-lookup"><span data-stu-id="01796-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="01796-141">Die Fehlermeldungen werden sofort ohne die Seite flash, ordnen wir Senden von Daten zurück an den Server.</span><span class="sxs-lookup"><span data-stu-id="01796-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="01796-142">Dies ist, da ASP.NET MVC jetzt die Eingabe der sowohl der Client (JavaScript) eine Überprüfung durchgeführt wird und auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="01796-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="01796-143">[![Erstellen Sie – Windows InternetExplorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="01796-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="01796-144">Dies ist gut suchen!</span><span class="sxs-lookup"><span data-stu-id="01796-144">This is looking good!</span></span> <span data-ttu-id="01796-145">Nun fügen Sie eine zusätzliche Spalte in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="01796-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01796-146">[Zurück](getting-started-with-mvc-part6.md)
> [Weiter](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="01796-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
