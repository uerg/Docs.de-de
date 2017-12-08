---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Verwenden von AJAX zum Übermitteln von dynamischen | Microsoft Docs"
author: microsoft
description: "Schritt 10 implementiert die Unterstützung für angemeldete Benutzer auf Antwort ihr Interesse an der Teilnahme an einem Essen, mit einem Ajax-basierten Ansatz, der innerhalb der Dinner Detail integriert..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="889ff-103">Mithilfe von AJAX dynamische Updates bereitstellen</span><span class="sxs-lookup"><span data-stu-id="889ff-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="889ff-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="889ff-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="889ff-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="889ff-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="889ff-106">Dies ist Schritt 10 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.</span><span class="sxs-lookup"><span data-stu-id="889ff-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="889ff-107">Schritt 10 implementiert die Unterstützung für angemeldete Benutzer auf Antwort ihr Interesse an der Teilnahme an einem Essen, mit einem Ajax-basierten Ansatz, der innerhalb der Dinner Detailseite integriert.</span><span class="sxs-lookup"><span data-stu-id="889ff-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="889ff-108">Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.</span><span class="sxs-lookup"><span data-stu-id="889ff-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="889ff-109">NerdDinner Schritt 10: AJAX aktivieren Einladung akzeptiert</span><span class="sxs-lookup"><span data-stu-id="889ff-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="889ff-110">Nehmen wir jetzt implementieren die Unterstützung für angemeldete Benutzer auf ihr Interesse an der Teilnahme an einem Dinner RSVP.</span><span class="sxs-lookup"><span data-stu-id="889ff-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="889ff-111">Dadurch wird es mit einem AJAX-basierten Ansatz, der innerhalb der Dinner Detailseite integriert.</span><span class="sxs-lookup"><span data-stu-id="889ff-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="889ff-112">Gibt an, ob der Benutzer auf die Antwort gebeten wird</span><span class="sxs-lookup"><span data-stu-id="889ff-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="889ff-113">Beim Besuch können die */Dinners/Informationen / [Id*] URL, um Details zu einem bestimmten Dinner finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="889ff-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="889ff-114">Die Aktionsmethode wird implementiert Details() wie folgt:</span><span class="sxs-lookup"><span data-stu-id="889ff-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="889ff-115">Unsere erste Schritt beim Implementieren der Unterstützung der Antwort werden unsere Dinner-Objekt (innerhalb der Dinner.cs partiellen Klasse, die wir zuvor erstellt) eine Hilfsmethode "IsUserRegistered(username)" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="889ff-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="889ff-116">Diese Hilfsmethode gibt "true" oder "false", je nachdem, ob der Benutzer derzeit für die Dinner um Antwort gebeten wird:</span><span class="sxs-lookup"><span data-stu-id="889ff-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="889ff-117">Wir können klicken Sie dann den folgenden Code hinzufügen, um unsere "Details.aspx" haben-Ansicht-Vorlage zum Anzeigen einer entsprechenden Meldung gibt an, ob der Benutzer registriert ist oder nicht für das Ereignis:</span><span class="sxs-lookup"><span data-stu-id="889ff-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="889ff-118">Und jetzt Wenn ein Benutzer eine Dinner besucht für die sie registriert werden sie sehen diese Meldung:</span><span class="sxs-lookup"><span data-stu-id="889ff-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="889ff-119">Und wenn sie eine Dinner sie nicht registriert besuchen, sind für die sie sehen die folgende Meldung:</span><span class="sxs-lookup"><span data-stu-id="889ff-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="889ff-120">Implementieren die Register-Aktion-Methode</span><span class="sxs-lookup"><span data-stu-id="889ff-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="889ff-121">Jetzt fügen Sie die Funktionalität zum Aktivieren von Benutzern auf die Antwort für eine Dinner Seite "Details" erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="889ff-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="889ff-122">Um dies zu implementieren, wir erstellen eine neue "RSVPController"-Klasse indem Sie mit der rechten Maustaste auf das Verzeichnis \Controllers und Add -&gt;Controller Menübefehl aus.</span><span class="sxs-lookup"><span data-stu-id="889ff-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="889ff-123">Implementieren wir eine Aktionsmethode "Register" innerhalb der neuen RSVPController-Klasse, die eine Id für eine Dinner als Argument akzeptiert, ruft das entsprechende Dinner-Objekt, geprüft, wenn der angemeldete Benutzer derzeit in der Liste der Benutzer ist, die für sie registriert haben und nicht Fügt ein Objekt für die Antwort für sie:</span><span class="sxs-lookup"><span data-stu-id="889ff-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="889ff-124">Beachten Sie, über wie wir eine einfache Zeichenfolge als Ausgabe der Aktionsmethode zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="889ff-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="889ff-125">Wir konnten diese Nachricht in eine Vorlage anzeigen – eingebettet haben, da es so klein ist. wir verwenden ihn aber nur die Hilfsmethode Content() auf die Basisklasse für Controller und der Rückgabewert eine zeichenfolgenmeldung wie oben.</span><span class="sxs-lookup"><span data-stu-id="889ff-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="889ff-126">Aufrufen der RSVPForEvent-Aktionsmethode, die mithilfe von AJAX</span><span class="sxs-lookup"><span data-stu-id="889ff-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="889ff-127">Wir verwenden AJAX aufzurufenden Aktionsmethode Register aus unserem Detailansicht.</span><span class="sxs-lookup"><span data-stu-id="889ff-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="889ff-128">Implementieren Dies ist recht einfach.</span><span class="sxs-lookup"><span data-stu-id="889ff-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="889ff-129">Zunächst fügen wir zwei Skriptverweise-Bibliothek:</span><span class="sxs-lookup"><span data-stu-id="889ff-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="889ff-130">Die erste Bibliothek verweist auf die ASP.NET AJAX-Skriptdatei auf Clientseite Kernbibliothek.</span><span class="sxs-lookup"><span data-stu-id="889ff-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="889ff-131">Diese Datei ist ca. 24 KB (komprimiert) und die clientseitige AJAX-Kernfunktionalität enthält.</span><span class="sxs-lookup"><span data-stu-id="889ff-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="889ff-132">Die zweite Bibliothek enthält Hilfsfunktionen, die in integrierten ASP.NETs AJAX-Hilfsmethoden integriert (das in Kürze verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="889ff-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="889ff-133">Wir können führt Update der Vorlagencode anzeigen, die wir zuvor hinzugefügt haben, sodass statt Outputing eine Meldung "Sie sind nicht für dieses Ereignis registriert", es stattdessen einem Link, der gerendert werden beim übertragen soll einen AJAX-Aufruf, der unsere RSVPForEvent Aktionsmethode auf unsere Antwort Controller aufruft und den Benutzer RSVPs:</span><span class="sxs-lookup"><span data-stu-id="889ff-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="889ff-134">Die oben verwendete Ajax.ActionLink()-Hilfsmethode wird erstellt in ASP.NET MVC und ähnelt die Hilfsmethode Html.ActionLink(), anstatt einen Standardnavigation einen AJAX-Aufruf an die Aktionsmethode vereinfacht, wenn auf der Link geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="889ff-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="889ff-135">Wir sind über Aufrufen die Aktionsmethode "Registrieren" auf dem Controller "Antwort" und die DinnerID als Parameter "Id" an sie übergibt.</span><span class="sxs-lookup"><span data-stu-id="889ff-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="889ff-136">Der letzte AjaxOptions-Parameter übergeben werden gibt an, dass wir verwenden möchten, nehmen den Inhalt von der Aktionsmethode zurückgegeben und aktualisieren den HTML-Code &lt;Div&gt; Element auf der Seite, deren Id ist "Rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="889ff-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="889ff-137">Und jetzt Wenn ein Benutzer auf eine Dinner durchsucht sie sind nicht registriert, für noch sehen sie einen Link zur Antwort dafür:</span><span class="sxs-lookup"><span data-stu-id="889ff-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="889ff-138">Wenn sie den Link "Antwort für dieses Ereignis" auf treffen sie einen AJAX-Aufruf an die Register-Aktion-Methode auf dem Controller Antwort und bei Abschluss sehen sie eine aktualisierte Meldung wie im folgenden:</span><span class="sxs-lookup"><span data-stu-id="889ff-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="889ff-139">Die Netzwerkbandbreite und der Datenverkehr beteiligt, wenn diese AJAX-Aufruf wirklich einfache ist.</span><span class="sxs-lookup"><span data-stu-id="889ff-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="889ff-140">Klickt der Benutzer auf den Link "Für dieses Ereignis Antwort", kleine HTTP POST-Netzwerk wird eine Anforderung an die */Dinners/Register/1* URL, die bei der Übertragung unten aussieht:</span><span class="sxs-lookup"><span data-stu-id="889ff-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="889ff-141">Und die Antwort vom unsere Aktionsmethode Register ist einfach:</span><span class="sxs-lookup"><span data-stu-id="889ff-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="889ff-142">Diese einfache Aufruf ist schnell und funktioniert auch über ein langsames Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="889ff-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="889ff-143">Hinzufügen von einem jQuery-Animationen</span><span class="sxs-lookup"><span data-stu-id="889ff-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="889ff-144">Die AJAX-Funktionalität, die wir implementiert funktioniert gut und schnell.</span><span class="sxs-lookup"><span data-stu-id="889ff-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="889ff-145">In einigen Fällen kann es so schnell, aber passieren, dass ein Benutzer nicht bemerkt, dass die Antwort-Verknüpfung durch neuen Text ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="889ff-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="889ff-146">Um das Ergebnis etwas deutlicher zu machen, können wir eine einfache Animation, um die Aufmerksamkeit auf die Änderungsnachricht lenken hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="889ff-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="889ff-147">Die Standardeinstellung ASP.NET MVC-Projektvorlage enthält jQuery – eine ausgezeichnete (und sehr beliebte) open Source-JavaScript-Bibliothek, die auch von Microsoft unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="889ff-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="889ff-148">jQuery bietet eine Reihe von Funktionen, einschließlich einer nice HTML-DOM-Auswahl und Effekte-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="889ff-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="889ff-149">Verwendung von jQuery fügen wir zuerst einen Skriptverweis auf ihn.</span><span class="sxs-lookup"><span data-stu-id="889ff-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="889ff-150">Da wir jQuery in unterschiedlichen Stellen in unserer Website verwenden möchten, fügen Skriptverweis in unserer Masterseitendatei Site.master wir damit, dass alle Seiten, die sie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="889ff-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="889ff-151">*Tipp: Stellen Sie sicher, dass Sie den JavaScript-Intellisense-Hotfix für Visual Studio 2008 SP1 installiert haben, die es umfangreichere Intellisense-Unterstützung für JavaScript-Dateien ermöglicht (einschließlich jQuery). Sie können es von herunterladen: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="889ff-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="889ff-152">Unter Verwendung von JQuery häufig geschriebenen Code verwendet eine globale "$ ()" JavaScript-Methode, die eine oder mehrere HTML-Elemente, die mithilfe einer CSS-Auswahl abruft.</span><span class="sxs-lookup"><span data-stu-id="889ff-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="889ff-153">Beispielsweise *$("#rsvpmsg")* wählt alle HTML-Element mit der Id Rsvpmsg, während *$(".something")* wählen alle Elemente mit CSS "etwas" Klassennamen.</span><span class="sxs-lookup"><span data-stu-id="889ff-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="889ff-154">Sie können auch noch umfassendere Abfragen wie alle aktivierten Optionsfelder "return" schreiben mithilfe einer Auswahl Abfrage z. B.: *$("Eingabe [@type= Radio] [@checked]")*.</span><span class="sxs-lookup"><span data-stu-id="889ff-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="889ff-155">Nachdem Sie die Elemente ausgewählt haben, können Sie Methoden aufrufen, damit Maßnahmen, wie sie verbergen: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="889ff-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="889ff-156">Für unser Szenario Antwort definieren wir eine einfache JavaScript-Funktion mit dem Namen "AnimateRSVPMessage", die die "Rsvpmsg" auswählt &lt;Div&gt; und die Größe seines Textinhalts animiert.</span><span class="sxs-lookup"><span data-stu-id="889ff-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="889ff-157">Die folgenden Code startet das kleine Text ein, und Ursachen es innerhalb eines Zeitraums 400 Millisekunden erhöhen:</span><span class="sxs-lookup"><span data-stu-id="889ff-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="889ff-158">Wir können dann über das Netzwerk von diesem JavaScript-Funktion, die aufgerufen wird, nachdem unsere AJAX-Aufruf erfolgreich abgeschlossen wurde, übergeben Sie seinen Namen an unsere Ajax.ActionLink()-Hilfsmethode (über die AjaxOptions "OnSuccess" Ereigniseigenschaft):</span><span class="sxs-lookup"><span data-stu-id="889ff-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="889ff-159">Jetzt bei und der Link "Antwort für dieses Ereignis" geklickt wird, und unsere AJAX-Aufruf erfolgreich abgeschlossen, die Inhalt gesendete Nachricht wird Back wird animieren anwachsen:</span><span class="sxs-lookup"><span data-stu-id="889ff-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="889ff-160">Zusätzlich zur Bereitstellung eines Ereignisses "OnSuccess", stellt der AjaxOptions-Objekt OnBegin OnFailure und OnComplete-Ereignisse, die Sie (zusammen mit einer Vielzahl von anderen Eigenschaften und nützliche Optionen) verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="889ff-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="889ff-161">Cleanup - Umgestaltung out RSVP Teilansicht</span><span class="sxs-lookup"><span data-stu-id="889ff-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="889ff-162">Die Vorlage unsere Details anzeigen wird gestartet, abzurufenden etwas long, welche Überstunden dessen etwas schwieriger zu verstehen, erkennen lässt.</span><span class="sxs-lookup"><span data-stu-id="889ff-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="889ff-163">Um die Lesbarkeit des Codes zu verbessern, wir fertig stellen Sie durch das Erstellen einer Teilansicht – RSVPStatus.ascx –, die gesamte Antwort Ansichtscode für unsere Seite "Details" kapseln.</span><span class="sxs-lookup"><span data-stu-id="889ff-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="889ff-164">Wir können dazu mit der rechten Maustaste auf den Ordner \Views\Dinners und drücken Sie dann die Add -&gt;Menübefehl anzeigen.</span><span class="sxs-lookup"><span data-stu-id="889ff-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="889ff-165">Wir müssen es ein Dinner-Objekt als die stark typisierte ViewModel dauern.</span><span class="sxs-lookup"><span data-stu-id="889ff-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="889ff-166">Wir können dann den Inhalt der Antwort aus unserer Ansicht "Details.aspx" haben hinein kopieren.</span><span class="sxs-lookup"><span data-stu-id="889ff-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="889ff-167">Wenn Sie es fertig auch erstellen wir eine andere partielle Ansicht – EditAndDeleteLinks.ascx - auf, die unsere Code bearbeiten und löschen Link anzeigen kapselt.</span><span class="sxs-lookup"><span data-stu-id="889ff-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="889ff-168">Wir müssen es nehmen Sie ein Dinner-Objekt als seine ViewModel stark typisiert, und die Logik bearbeiten und Löschen von unserer Ansicht "Details.aspx" haben hinein, kopieren und einfügen.</span><span class="sxs-lookup"><span data-stu-id="889ff-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="889ff-169">Unsere Details-Vorlage kann anzeigen, und klicken Sie dann einfach einschließen, zwei Html.RenderPartial() Methodenaufrufe unten:</span><span class="sxs-lookup"><span data-stu-id="889ff-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="889ff-170">Dadurch wird den Code übersichtlicher zu lesen und zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="889ff-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="889ff-171">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="889ff-171">Next Step</span></span>

<span data-ttu-id="889ff-172">Jetzt betrachten wie wir noch weiter verwenden von AJAX und interaktive Unterstützung unserer Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="889ff-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="889ff-173">[Zurück](secure-applications-using-authentication-and-authorization.md)
[Weiter](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="889ff-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
