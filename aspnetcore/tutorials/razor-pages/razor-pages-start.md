---
title: Erste Schritte mit Razor Pages in ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Diese Reihe von Tutorials zeigt, wie Sie Razor Pages in ASP.NET Core verwenden können. Erfahren Sie, wie Sie ein Modell erstellen, Code für Razor-Seiten generieren, Entity Framework Core und SQL Server für den Datenzugriff verwenden, Suchfunktionen hinzufügen, Eingabeüberprüfung hinzufügen und Migrationen verwenden, um das Modell zu aktualisieren.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861627"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="b112a-104">Tutorial: Erste Schritte mit Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b112a-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b112a-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b112a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b112a-106">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b112a-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="b112a-107">Die App verwaltet eine Datenbank mit Filmtiteln.</span><span class="sxs-lookup"><span data-stu-id="b112a-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="b112a-108">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b112a-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b112a-109">Erstellen einer Razor Pages-Web-App</span><span class="sxs-lookup"><span data-stu-id="b112a-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="b112a-110">Hinzufügen eines Modells und Erstellen eines Gerüsts für das Modell</span><span class="sxs-lookup"><span data-stu-id="b112a-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="b112a-111">Arbeiten mit einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="b112a-111">Work with a database.</span></span>
> * <span data-ttu-id="b112a-112">Hinzufügen von Such- und Überprüfungsfunktionen</span><span class="sxs-lookup"><span data-stu-id="b112a-112">Add search and validation.</span></span>

<span data-ttu-id="b112a-113">Am Ende verfügen Sie über eine App, die Filmtitel verwalten und anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="b112a-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="b112a-114">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b112a-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="b112a-115">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="b112a-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b112a-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b112a-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b112a-117">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b112a-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b112a-118">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="b112a-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="b112a-119">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="b112a-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="b112a-120">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="b112a-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="b112a-121">![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="b112a-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="b112a-122">Wählen Sie in der Dropdownliste **ASP.NET Core 2.2** und anschließend **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="b112a-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="b112a-124">Das folgende Startprojekt wird erstellt:</span><span class="sxs-lookup"><span data-stu-id="b112a-124">The following starter project is created:</span></span>

  ![Projektmappen-Explorer](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="b112a-126">Drücken Sie **STRG+F5**, um die Ausführung ohne den Debugger zu starten.</span><span class="sxs-lookup"><span data-stu-id="b112a-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b112a-127">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus.</span><span class="sxs-lookup"><span data-stu-id="b112a-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="b112a-128">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b112a-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b112a-129">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für den lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="b112a-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="b112a-130">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="b112a-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="b112a-131">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="b112a-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b112a-132">In der vorherigen Abbildung ist die Portnummer 5001.</span><span class="sxs-lookup"><span data-stu-id="b112a-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="b112a-133">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b112a-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="b112a-134">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="b112a-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b112a-135">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die Seite zu aktualisieren und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b112a-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b112a-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b112a-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b112a-137">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b112a-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b112a-138">Wechseln Sie mit `cd` zu einem Ordner, der das Projekt enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="b112a-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b112a-139">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b112a-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="b112a-140">Es wird ein Dialogfeld mit folgender Meldung angezeigt: **Die erforderlichen Objekte zum Erstellen und Debuggen sind in "RazorPagesMovie" nicht vorhanden. Sollen sie hinzugefügt werden?**</span><span class="sxs-lookup"><span data-stu-id="b112a-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="b112a-141">Wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="b112a-141">Select **Yes**</span></span>

  * <span data-ttu-id="b112a-142">`dotnet new webapp -o RazorPagesMovie`: Erstellt ein neues Razor Pages-Projekt im Ordner *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="b112a-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="b112a-143">`code -r RazorPagesMovie`: Lädt die Projektdatei *RazorPagesMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b112a-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="b112a-144">Starten der App</span><span class="sxs-lookup"><span data-stu-id="b112a-144">Launch the app</span></span>

* <span data-ttu-id="b112a-145">Drücken Sie **STRG+F5**, um die Ausführung ohne den Debugger zu starten.</span><span class="sxs-lookup"><span data-stu-id="b112a-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b112a-146">Visual Studio Code startet [Kestrel](xref:fundamentals/servers/kestrel) und einen Browser und navigiert zu `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b112a-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="b112a-147">Die Adressleiste zeigt `localhost:port:5001` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b112a-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="b112a-148">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für den lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="b112a-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="b112a-149">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="b112a-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="b112a-150">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="b112a-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b112a-151">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die Seite zu aktualisieren und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b112a-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b112a-152">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="b112a-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b112a-153">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="b112a-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="b112a-154">Diese Befehle verwenden die [.NET Core-CLI](/dotnet/core/tools/dotnet), um ein Projekt mit Razor Pages zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b112a-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="b112a-155">Öffnen Sie http://localhost:5000 in einem Browser, um sich die Anwendung anzeigen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="b112a-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="b112a-156">Öffnen des Projekts</span><span class="sxs-lookup"><span data-stu-id="b112a-156">Open the project</span></span>

<span data-ttu-id="b112a-157">Drücken Sie STRG+C, um die Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="b112a-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="b112a-158">Klicken Sie in Visual Studio auf **Datei > Öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.</span><span class="sxs-lookup"><span data-stu-id="b112a-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="b112a-159">Starten der App</span><span class="sxs-lookup"><span data-stu-id="b112a-159">Launch the app</span></span>

<span data-ttu-id="b112a-160">Klicken Sie in Visual Studio auf **Ausführen > Ohne Debuggen starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="b112a-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b112a-161">Visual Studio startet dann [Kestrel](xref:fundamentals/servers/kestrel) und einen Browser und navigiert zu `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b112a-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="b112a-162">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="b112a-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="b112a-163">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="b112a-163">This app doesn't track personal information.</span></span> <span data-ttu-id="b112a-164">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="b112a-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Start- oder Indexseite](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="b112a-166">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b112a-166">The following image shows the app after accepting tracking:</span></span>

  ![Start- oder Indexseite](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="b112a-168">Projektdateien und -ordner</span><span class="sxs-lookup"><span data-stu-id="b112a-168">Project files and folders</span></span>

<span data-ttu-id="b112a-169">Die folgende Tabelle listet die Dateien und Ordner im Projekt auf.</span><span class="sxs-lookup"><span data-stu-id="b112a-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="b112a-170">An diesem Punkt des Tutorials ist die Datei *Startup.cs*, die wichtigste, die Sie kennen müssen.</span><span class="sxs-lookup"><span data-stu-id="b112a-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="b112a-171">Sie müssen nicht jeden der unten angegebenen Links ansehen.</span><span class="sxs-lookup"><span data-stu-id="b112a-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="b112a-172">Die Links werden als Referenz bereitgestellt, wenn Sie weitere Informationen zu einer Datei oder einem Ordner im Projekt benötigen.</span><span class="sxs-lookup"><span data-stu-id="b112a-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="b112a-173">Datei oder Ordner</span><span class="sxs-lookup"><span data-stu-id="b112a-173">File or folder</span></span>              | <span data-ttu-id="b112a-174">Zweck</span><span class="sxs-lookup"><span data-stu-id="b112a-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="b112a-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="b112a-175">*wwwroot*</span></span> | <span data-ttu-id="b112a-176">Enthält statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="b112a-176">Contains static files.</span></span> <span data-ttu-id="b112a-177">Weitere Informationen finden Sie unter [Statische Dateien](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b112a-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="b112a-178">*Seiten*</span><span class="sxs-lookup"><span data-stu-id="b112a-178">*Pages*</span></span> | <span data-ttu-id="b112a-179">Ordner für [Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="b112a-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="b112a-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="b112a-180">*appsettings.json*</span></span> | [<span data-ttu-id="b112a-181">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b112a-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="b112a-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="b112a-182">*Program.cs*</span></span> | <span data-ttu-id="b112a-183">[Hostet](xref:fundamentals/host/index) die ASP.NET Core-App.</span><span class="sxs-lookup"><span data-stu-id="b112a-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="b112a-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="b112a-184">*Startup.cs*</span></span> | <span data-ttu-id="b112a-185">Konfiguriert Dienste und die Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="b112a-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="b112a-186">Siehe [Startup](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="b112a-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="b112a-187">Der Seitenordner</span><span class="sxs-lookup"><span data-stu-id="b112a-187">The Pages folder</span></span>

<span data-ttu-id="b112a-188">Die Datei *_Layout.cshtml* enthält häufige HTML-Elemente (Skripts und Stylesheets) und legt das Layout für die Anwendung fest.</span><span class="sxs-lookup"><span data-stu-id="b112a-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="b112a-189">Wenn Sie beispielsweise auf **RazorPagesMovie**, **Home** (Startseite) oder **Privacy** (Datenschutz) klicken, werden dieselben Elemente angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b112a-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="b112a-190">Die häufigen Elemente umfassen das Navigationsmenü am oberen Rand und den Header am unteren Rand des Fensters.</span><span class="sxs-lookup"><span data-stu-id="b112a-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="b112a-191">Weitere Informationen finden Sie unter [Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="b112a-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="b112a-192">Die Datei *_ViewImports.cshtml* enthält Razor-Anweisungen, die in jede Razor Page importiert werden.</span><span class="sxs-lookup"><span data-stu-id="b112a-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="b112a-193">Weitere Informationen finden Sie unter [Importing Shared Directives (Importieren gemeinsamer Anweisungen)](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="b112a-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="b112a-194">Die Datei *_ViewStart.cshtml* legt die Eigenschaft `Layout` der Razor Pages auf die Verwendung der Datei *_Layout.cshtml* fest.</span><span class="sxs-lookup"><span data-stu-id="b112a-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="b112a-195">Weitere Informationen finden Sie unter [Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="b112a-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="b112a-196">Die Datei *_ValidationScriptsPartial.cshtml* enthält einen Verweis auf [jQuery](https://jquery.com/)-Validierungsskripts.</span><span class="sxs-lookup"><span data-stu-id="b112a-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="b112a-197">Wenn Sie im späteren Verlauf des Tutorials die Seiten `Create` und `Edit` hinzufügen, wird dazu die Datei *_ValidationScriptsPartial.cshtml* verwendet.</span><span class="sxs-lookup"><span data-stu-id="b112a-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="b112a-198">Die Seiten `Index`, `Error` und `Privacy` werden zu folgenden Zwecken bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="b112a-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="b112a-199">`Index`: Starten einer App</span><span class="sxs-lookup"><span data-stu-id="b112a-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="b112a-200">`Error`: Anzeigen von Fehlerinformationen</span><span class="sxs-lookup"><span data-stu-id="b112a-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="b112a-201">`Privacy`: Angeben von Details zur Datenschutzrichtlinie der Website</span><span class="sxs-lookup"><span data-stu-id="b112a-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="b112a-202">Für das vorliegende Tutorial werden diese Seiten nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="b112a-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b112a-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b112a-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="b112a-204">Verwenden von F7 zum Umschalten zwischen einer Razor-Seite und dem PageModel</span><span class="sxs-lookup"><span data-stu-id="b112a-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="b112a-205">Mit F7 wechseln Sie zwischen einer Razor Page-Datei (*\*.cshtml*) und der C#-Datei (*\*. cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b112a-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b112a-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b112a-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="b112a-207">Per Konvention weisen die Razor Page-Datei (*\*.cshtml*) und das zugehörige `PageModel` den gleichen Stammdateinamen auf.</span><span class="sxs-lookup"><span data-stu-id="b112a-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b112a-208">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="b112a-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b112a-209">Per Konvention weisen die Razor Page-Datei (*\*.cshtml*) und das zugehörige `PageModel` den gleichen Stammdateinamen auf.</span><span class="sxs-lookup"><span data-stu-id="b112a-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="b112a-210">Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor Pages in ASP.NET Core mit Visual Studio für Mac)</span><span class="sxs-lookup"><span data-stu-id="b112a-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)