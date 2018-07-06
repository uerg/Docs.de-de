---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Erste Schritte mit OWIN und Katana | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 58a3d234867821d5e23cce2f01e105dfab88ac33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826658"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="1e359-102">Erste Schritte mit OWIN und Katana</span><span class="sxs-lookup"><span data-stu-id="1e359-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="1e359-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1e359-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1e359-104">[Öffnen Sie Web Interface for .NET (OWIN)](http://owin.org/) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="1e359-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="1e359-105">Durch die Entkopplung des Webserver, aus der Anwendung, erleichtert OWIN-Middleware für .NET Web-Entwicklung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1e359-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="1e359-106">Darüber hinaus OWIN erleichtert es, Port-Webanwendungen zu anderen Hosts&#8212;z. B. Self-hosting in einem Windows-Dienst oder einen anderen Prozess.</span><span class="sxs-lookup"><span data-stu-id="1e359-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="1e359-107">OWIN ist eine Spezifikation Community gehören, nicht auf eine Implementierung.</span><span class="sxs-lookup"><span data-stu-id="1e359-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="1e359-108">Das Katana-Projekt ist eine Reihe von Open Source-OWIN-Komponenten, die von Microsoft entwickelt wurde.</span><span class="sxs-lookup"><span data-stu-id="1e359-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="1e359-109">Eine allgemeine Übersicht über OWIN und Katana, finden Sie unter [eine Übersicht des Katana-Projekt](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="1e359-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="1e359-110">In diesem Artikel werden ich direkt in Code für den Einstieg springen.</span><span class="sxs-lookup"><span data-stu-id="1e359-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="1e359-111">Dieses Tutorial verwendet [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), aber Sie können auch Visual Studio 2012 verwenden.</span><span class="sxs-lookup"><span data-stu-id="1e359-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="1e359-112">Ein paar Schritte unterscheiden sich in Visual Studio 2012, die ich Beachten Sie unten.</span><span class="sxs-lookup"><span data-stu-id="1e359-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="1e359-113">Hosten von OWIN in IIS</span><span class="sxs-lookup"><span data-stu-id="1e359-113">Host OWIN in IIS</span></span>

<span data-ttu-id="1e359-114">In diesem Abschnitt werden wir OWIN in IIS hosten.</span><span class="sxs-lookup"><span data-stu-id="1e359-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="1e359-115">Diese Option bietet Ihnen die Flexibilität und zusammensetzbarkeit von einer OWIN-Pipeline zusammen mit dem ausgereifte Features von IIS.</span><span class="sxs-lookup"><span data-stu-id="1e359-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="1e359-116">Mit dieser Option wird die OWIN-Anwendung in der ASP.NET-Pipeline für die Anforderung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1e359-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="1e359-117">Erstellen Sie zunächst ein neues ASP.NET-Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="1e359-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="1e359-118">(In Visual Studio 2012, verwenden Sie den Projekttyp leere ASP.NET-Webanwendung.)</span><span class="sxs-lookup"><span data-stu-id="1e359-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="1e359-119">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="1e359-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="1e359-120">NuGet-Pakete hinzufügen</span><span class="sxs-lookup"><span data-stu-id="1e359-120">Add NuGet Packages</span></span>

<span data-ttu-id="1e359-121">Als Nächstes fügen Sie die erforderlichen NuGet-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="1e359-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="1e359-122">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="1e359-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1e359-123">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="1e359-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="1e359-124">Fügen Sie eine Startklasse</span><span class="sxs-lookup"><span data-stu-id="1e359-124">Add a Startup Class</span></span>

<span data-ttu-id="1e359-125">Fügen Sie anschließend eine OWIN-Startup-Klasse.</span><span class="sxs-lookup"><span data-stu-id="1e359-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="1e359-126">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **hinzufügen**, und wählen Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="1e359-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="1e359-127">In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Owin-Startklasse**.</span><span class="sxs-lookup"><span data-stu-id="1e359-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="1e359-128">Weitere Informationen zum Konfigurieren der Startup-Klasse finden Sie unter [OWIN-Startup-Klasse Erkennung](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="1e359-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="1e359-129">Fügen Sie der `Startup1.Configuration`-Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="1e359-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="1e359-130">Dieser Code Fügt eine einfache Aufgabe der Middleware OWIN-Pipeline, die als eine Funktion, die empfängt implementiert eine **Microsoft.Owin.IOwinContext** Instanz.</span><span class="sxs-lookup"><span data-stu-id="1e359-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="1e359-131">Wenn der Server eine HTTP-Anforderung empfängt, ruft die OWIN-Pipeline die Middleware.</span><span class="sxs-lookup"><span data-stu-id="1e359-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="1e359-132">Die Middleware legt den Inhaltstyp für die Antwort fest und schreibt den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="1e359-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="1e359-133">Die OWIN-Startup-Klassenvorlage ist in Visual Studio 2013 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="1e359-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="1e359-134">Wenn Sie Visual Studio 2012 verwenden, fügen Sie einfach eine neue leere Klasse, die mit dem Namen `Startup1`, und fügen Sie in den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1e359-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="1e359-135">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="1e359-135">Run the Application</span></span>

<span data-ttu-id="1e359-136">Drücken Sie F5, um das Debuggen zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="1e359-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="1e359-137">Visual Studio öffnet ein Browserfenster mit `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="1e359-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="1e359-138">Die Seite sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="1e359-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="1e359-139">Self-Hosting von OWIN in einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="1e359-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="1e359-140">Es ist einfach, diese Anwendung zu konvertieren, von IIS hosten, selbst in einem benutzerdefinierten Prozess hosten.</span><span class="sxs-lookup"><span data-stu-id="1e359-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="1e359-141">Mit IIS zu hosten fungiert IIS, als der HTTP-Server und den Prozess, der den Dienst hostet.</span><span class="sxs-lookup"><span data-stu-id="1e359-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="1e359-142">Dem Selbsthosting, Ihre Anwendung wird der Prozess erstellt und verwendet die **HttpListener** Klasse wie der HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="1e359-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="1e359-143">Erstellen Sie eine neue Konsolenanwendung in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e359-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="1e359-144">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="1e359-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="1e359-145">Hinzufügen einer `Startup1` Klasse aus Teil 1 dieses Lernprogramms zum Projekt.</span><span class="sxs-lookup"><span data-stu-id="1e359-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="1e359-146">Sie müssen nicht diese Klasse zu ändern.</span><span class="sxs-lookup"><span data-stu-id="1e359-146">You don't need to modify this class.</span></span>

<span data-ttu-id="1e359-147">Implementieren der Anwendung `Main` -Methode wie folgt.</span><span class="sxs-lookup"><span data-stu-id="1e359-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="1e359-148">Wenn Sie die Konsolenanwendung ausführen, wird der Server startet, Lauschen auf `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="1e359-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="1e359-149">Wenn Sie in einem Webbrowser zu dieser Adresse navigieren, sehen Sie die Seite "Hello World".</span><span class="sxs-lookup"><span data-stu-id="1e359-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="1e359-150">Hinzufügen von OWIN-Diagnose</span><span class="sxs-lookup"><span data-stu-id="1e359-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="1e359-151">Das Microsoft.Owin.Diagnostics-Paket enthält Middleware, die nicht behandelte Ausnahmen abfängt und eine HTML-Seite mit Fehlerdetails angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1e359-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="1e359-152">Diese Seite funktioniert ähnlich wie die ASP.NET-Seite für Fehler, die manchmal aufgerufen wird, wird die "[gelben Bildschirm fehl](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="1e359-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="1e359-153">Wie die YSOD die Katana-Fehlerseite wird während der Entwicklung nützlich, aber es wird empfohlen, ihn in den Produktionsmodus deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="1e359-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="1e359-154">Geben Sie den folgenden Befehl im Fenster Paket-Manager-Konsole, um die Diagnose-Paket in Ihrem Projekt zu installieren:</span><span class="sxs-lookup"><span data-stu-id="1e359-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="1e359-155">Ändern Sie den Code in Ihre `Startup1.Configuration` -Methode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1e359-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="1e359-156">Jetzt verwenden Sie STRG + F5, um die Anwendung ohne Debuggen auszuführen, damit Visual Studio nicht auf die Ausnahme unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="1e359-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="1e359-157">Die Anwendung verhält sich wie zuvor, bis Sie zum Navigieren `http://localhost/fail`, wodurch die Anwendung die Ausnahme auslöst.</span><span class="sxs-lookup"><span data-stu-id="1e359-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="1e359-158">Die fehlerseitenmiddleware wird die Ausnahme abzufangen und eine HTML-Seite mit Informationen zum Fehler anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1e359-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="1e359-159">Sie können die Registerkarten, um die Stapel, Abfragezeichenfolgen, Cookies, Anforderungsheader und OWIN-Umgebungsvariablen finden Sie unter klicken.</span><span class="sxs-lookup"><span data-stu-id="1e359-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="1e359-160">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="1e359-160">Next Steps</span></span>

- [<span data-ttu-id="1e359-161">Erkennung der OWIN-Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="1e359-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="1e359-162">Verwenden von OWIN zum Selfhosten von ASP.NET-Web-API</span><span class="sxs-lookup"><span data-stu-id="1e359-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="1e359-163">Verwenden von OWIN zum Selfhosten von SignalR</span><span class="sxs-lookup"><span data-stu-id="1e359-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
