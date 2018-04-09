---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Erste Schritte mit OWIN und Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="90bce-102">Erste Schritte mit OWIN und Katana</span><span class="sxs-lookup"><span data-stu-id="90bce-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="90bce-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="90bce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="90bce-104">[Öffnen Sie die Weboberfläche für .NET (OWIN)](http://owin.org/) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="90bce-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="90bce-105">Durch die Trennung des Servers aus der Anwendung, erleichtert OWIN Middleware für die Webentwicklung .NET zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="90bce-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="90bce-106">Darüber hinaus OWIN erleichtert es, Port-Webanwendungen zu anderen Hosts&#8212;z. B. Selbsthosting in einer Windows-Dienst oder ein anderer Prozess.</span><span class="sxs-lookup"><span data-stu-id="90bce-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="90bce-107">OWIN ist eine Spezifikation Community gehören, nicht um eine Implementierung.</span><span class="sxs-lookup"><span data-stu-id="90bce-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="90bce-108">Das Katana-Projekt ist eine Reihe von Open Source-OWIN-Komponenten von Microsoft entwickelt wurde.</span><span class="sxs-lookup"><span data-stu-id="90bce-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="90bce-109">Eine allgemeine Übersicht über OWIN und Katana finden Sie unter [eine Übersicht über Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="90bce-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="90bce-110">In diesem Artikel springt ich rechts in Code, um zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="90bce-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="90bce-111">Dieses Lernprogramm verwendet [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), aber Sie können auch Visual Studio 2012 verwenden.</span><span class="sxs-lookup"><span data-stu-id="90bce-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="90bce-112">Nur einige der Schritte unterscheiden sich in Visual Studio 2012, die ich Beachten Sie unten.</span><span class="sxs-lookup"><span data-stu-id="90bce-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="90bce-113">Host OWIN in IIS</span><span class="sxs-lookup"><span data-stu-id="90bce-113">Host OWIN in IIS</span></span>

<span data-ttu-id="90bce-114">In diesem Abschnitt werden OWIN in IIS gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="90bce-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="90bce-115">Diese Option bietet Ihnen die Flexibilität und Composability von einer OWIN-Pipeline zusammen mit den ausgereifte Funktionsumfang von IIS.</span><span class="sxs-lookup"><span data-stu-id="90bce-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="90bce-116">Bei Verwendung dieser Option wird die OWIN-Anwendung in der ASP.NET-Pipeline für die Anforderung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="90bce-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="90bce-117">Erstellen Sie zunächst ein neues ASP.NET Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="90bce-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="90bce-118">(In Visual Studio 2012, verwenden Sie den Projekttyp leere ASP.NET-Webanwendung.)</span><span class="sxs-lookup"><span data-stu-id="90bce-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="90bce-119">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="90bce-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="90bce-120">Hinzufügen von NuGet-Paketen</span><span class="sxs-lookup"><span data-stu-id="90bce-120">Add NuGet Packages</span></span>

<span data-ttu-id="90bce-121">Fügen Sie anschließend die erforderlichen NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="90bce-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="90bce-122">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="90bce-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="90bce-123">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="90bce-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="90bce-124">Fügen Sie eine Startklasse</span><span class="sxs-lookup"><span data-stu-id="90bce-124">Add a Startup Class</span></span>

<span data-ttu-id="90bce-125">Fügen Sie anschließend eine OWIN-Start-Klasse.</span><span class="sxs-lookup"><span data-stu-id="90bce-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="90bce-126">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen**, und wählen Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="90bce-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="90bce-127">In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Owin Startklasse**.</span><span class="sxs-lookup"><span data-stu-id="90bce-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="90bce-128">Weitere Informationen zum Konfigurieren der Startklasse finden Sie unter [OWIN Start Klasse Erkennung](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="90bce-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="90bce-129">Fügen Sie der `Startup1.Configuration`-Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="90bce-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="90bce-130">Dieser Code Fügt einen einfachen Teil Middleware an die OWIN-Pipeline, die als eine Funktion, die empfängt implementiert eine **Microsoft.Owin.IOwinContext** Instanz.</span><span class="sxs-lookup"><span data-stu-id="90bce-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="90bce-131">Wenn der Server über eine HTTP-Anforderung empfängt, ruft die OWIN-Pipeline die Middleware.</span><span class="sxs-lookup"><span data-stu-id="90bce-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="90bce-132">Die Middleware legt den Inhaltstyp für die Antwort und schreibt den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="90bce-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="90bce-133">Die OWIN-Start-Klassenvorlage ist in Visual Studio 2013 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="90bce-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="90bce-134">Wenn Sie Visual Studio 2012 verwenden, hinzufügen eine neue leere Klasse mit dem Namen `Startup1`, und fügen Sie in den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="90bce-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="90bce-135">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="90bce-135">Run the Application</span></span>

<span data-ttu-id="90bce-136">Drücken Sie F5, um das Debuggen zu starten.</span><span class="sxs-lookup"><span data-stu-id="90bce-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="90bce-137">Visual Studio öffnet ein Browserfenster mit `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="90bce-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="90bce-138">Die Seite sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="90bce-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="90bce-139">Selbsthosting OWIN in einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="90bce-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="90bce-140">Es ist einfach, diese Anwendung zu konvertieren, von IIS-hosting Selbsthosting in einem benutzerdefinierten Prozess.</span><span class="sxs-lookup"><span data-stu-id="90bce-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="90bce-141">Mit IIS-hosting fungiert IIS als den HTTP-Server und der Prozess, der den Dienst hostet.</span><span class="sxs-lookup"><span data-stu-id="90bce-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="90bce-142">Mit Selbsthosting, Ihre Anwendung den Prozess erstellt und verwendet die **HttpListener** Klasse wie der HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="90bce-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="90bce-143">Erstellen Sie in Visual Studio eine neue Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="90bce-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="90bce-144">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="90bce-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="90bce-145">Hinzufügen einer `Startup1` Klasse aus Teil 1 dieses Lernprogramms zum Projekt.</span><span class="sxs-lookup"><span data-stu-id="90bce-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="90bce-146">Sie müssen diese Klasse zu ändern.</span><span class="sxs-lookup"><span data-stu-id="90bce-146">You don't need to modify this class.</span></span>

<span data-ttu-id="90bce-147">Implementieren Sie der Anwendungsverzeichnis `Main` Methode wie folgt.</span><span class="sxs-lookup"><span data-stu-id="90bce-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="90bce-148">Wenn Sie die Konsolenanwendung ausführen, Start des Servers überwachen `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="90bce-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="90bce-149">Wenn Sie an diese Adresse in einem Webbrowser navigieren, sehen Sie die Seite "Hello World".</span><span class="sxs-lookup"><span data-stu-id="90bce-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="90bce-150">Hinzufügen von OWIN Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="90bce-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="90bce-151">Das Paket "Microsoft.owin.Diagnostics" enthält die Middleware, die nicht behandelte Ausnahmen abfängt und eine HTML-Seite mit Fehlerdetails angezeigt.</span><span class="sxs-lookup"><span data-stu-id="90bce-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="90bce-152">Diese Seite funktioniert ähnlich wie die ASP.NET-Seite für Fehler, die in einigen Fällen aufgerufen wird, wird der "[gelben Bildschirm eines Computers zum Tod](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="90bce-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="90bce-153">Wie die YSOD Katana-Fehlerseite eignet sich während der Entwicklung, aber es wird empfohlen, ihn in den Produktionsmodus zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="90bce-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="90bce-154">Geben Sie den folgenden Befehl zum Installieren des Pakets für die Diagnose in Ihrem Projekt in der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="90bce-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="90bce-155">Ändern Sie den Code in Ihrem `Startup1.Configuration` Methode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="90bce-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="90bce-156">Jetzt verwenden Sie STRG + F5, um die Anwendung ohne Debuggen auszuführen, damit Visual Studio nicht auf die Ausnahme unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="90bce-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="90bce-157">Die Anwendung verhält sich wie zuvor, bis Sie zu navigieren, `http://localhost/fail`, an welchem Punkt die Anwendung die Ausnahme auslöst.</span><span class="sxs-lookup"><span data-stu-id="90bce-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="90bce-158">Die fehlerseitenmiddleware wird die Ausnahme abfangen und eine HTML-Seite mit Informationen zum Fehler anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="90bce-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="90bce-159">Sie können die Registerkarten, um den Stapel, Abfragezeichenfolge, Cookies, Anforderungsheader und OWIN-Umgebungsvariablen finden Sie unter klicken.</span><span class="sxs-lookup"><span data-stu-id="90bce-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="90bce-160">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="90bce-160">Next Steps</span></span>

- [<span data-ttu-id="90bce-161">Erkennung der OWIN-Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="90bce-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="90bce-162">Mit der ASP.NET Web API Self Host OWIN</span><span class="sxs-lookup"><span data-stu-id="90bce-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="90bce-163">Mit der SignalR Selbsthosting OWIN</span><span class="sxs-lookup"><span data-stu-id="90bce-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
