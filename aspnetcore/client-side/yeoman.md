---
title: Erstellen von Projekten mit Yeoman in ASP.NET Core
author: spboyer
description: "Dieser Artikel führt durch Erstellen einer ASP.NET Core Webanwendung, die mithilfe der Yeoman Generator auf MacOS."
keywords: "ASP.NET Core, Yeoman, plattformübergreifend, Handelsversion Aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="36978-104">Einführung in das Erstellen von Projekten mit Yeoman in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36978-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="36978-105">[Yeoman](http://yeoman.io/) ist ein Gerüst Projektsystem für verschiedene Anwendungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="36978-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="36978-106">Die Yeoman-Generator für ASP.NET Core enthält eine Reihe von Projektvorlagen für das Starten einer neuen Web, MVC oder Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="36978-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="36978-107">Installieren Sie Yeoman, Node.js und npm</span><span class="sxs-lookup"><span data-stu-id="36978-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="36978-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="36978-108">Prerequisites</span></span>

<span data-ttu-id="36978-109">Node.js und Npm sind für Yeoman erforderlich.</span><span class="sxs-lookup"><span data-stu-id="36978-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="36978-110">Herunterladen von [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="36978-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="36978-111">Der Installer umfasst [Node.js](https://nodejs.org/) und [Npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="36978-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="36978-112">Bower ist auch für die Installation von Benutzeroberflächen-Frameworks wie Bootstrap erforderlich.</span><span class="sxs-lookup"><span data-stu-id="36978-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="36978-113">Führen Sie zum Installieren von Yeoman oder Bower den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="36978-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="36978-114">Wenn Sie die Fehlermeldung erhalten, `npm ERR! Please try running this command again as root/Administrator.` auf MacOS, führen Sie den folgenden Befehl mit ["sudo"](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="36978-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="36978-115">Installieren Sie ASP.NET-Generator über eine Eingabeaufforderung:</span><span class="sxs-lookup"><span data-stu-id="36978-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="36978-116">Wenn Sie ein Berechtigungsfehler erhalten, führen Sie den Befehl unter `sudo` wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="36978-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="36978-117">Die `–g` Flag wird den Generator Global installiert, sodass sie über einen Pfad verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="36978-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="36978-118">Erstellen einer ASP.NET-Anwendung</span><span class="sxs-lookup"><span data-stu-id="36978-118">Create an ASP.NET app</span></span>

<span data-ttu-id="36978-119">Führen Sie den Generator Yeoman basierende ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="36978-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="36978-120">Der Generator wird ein Menü angezeigt.</span><span class="sxs-lookup"><span data-stu-id="36978-120">The generator displays a menu.</span></span> <span data-ttu-id="36978-121">Pfeil nach unten, um die **Web Application Basic [ohne Mitgliedschaft und Autorisierung]** Projekt, und tippen Sie auf **EINGABETASTE**:</span><span class="sxs-lookup"><span data-stu-id="36978-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Befehlsfenster: welche Art von Anwendung möchten Sie erstellen?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="36978-124">Wählen Sie Bootstrap als Benutzeroberflächen-Framework, und tippen Sie auf **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="36978-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="36978-125">Mit "**MyWebApp**" für den Anwendungsnamen und tippen Sie dann auf **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="36978-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="36978-126">Yeoman wird das Gerüst für das Projekt und die unterstützenden Dateien erstellen.</span><span class="sxs-lookup"><span data-stu-id="36978-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="36978-127">Außerdem werden die empfohlenen ersten Schritte in Form von Befehle bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="36978-127">Suggested next steps are also provided in the form of commands.</span></span>

![Befehlsfenster: Was ist, dass der Name Ihrer ASP.NET-Anwendung?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="36978-130">Die [ASP.NET Generator](https://www.npmjs.com/package/generator-aspnet) erstellt ASP.NET Core-Projekte, die in Visual Studio-Code, Visual Studio geladen werden oder über die Befehlszeile ausführen können.</span><span class="sxs-lookup"><span data-stu-id="36978-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="36978-131">Stellen Sie wieder her, erstellen und ausführen</span><span class="sxs-lookup"><span data-stu-id="36978-131">Restore, build, and run</span></span>

<span data-ttu-id="36978-132">Führen Sie die vorgeschlagenen Befehle durch Ändern von Verzeichnissen auf dem `MyWebApp` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="36978-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="36978-133">Führen Sie `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="36978-133">Then run `dotnet restore`.</span></span>

![Befehlsfenster](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="36978-135">Erstellen und Ausführen der Anwendung mit `dotnet build` und `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="36978-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![Befehlsfenster](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="36978-137">An diesem Punkt können Sie mit der URL angezeigt wird, testen Sie die neu erstellte ASP.NET Core app navigieren.</span><span class="sxs-lookup"><span data-stu-id="36978-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="36978-138">Die clientseitige Pakete</span><span class="sxs-lookup"><span data-stu-id="36978-138">Client-side packages</span></span>

<span data-ttu-id="36978-139">Die Front-End-Ressourcen werden von den Vorlagen bereitgestellt, von der Yeoman Generator mithilfe der [Bower](xref:client-side/bower) clientseitige Paketmanager hinzufügen *"bower.JSON"* und *".bowerrc"* Dateien, die clientseitige Pakete mithilfe von Bower wiederherzustellen.</span><span class="sxs-lookup"><span data-stu-id="36978-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="36978-140">Die [BundlerMinifier](xref:client-side/bundling-and-minification) Komponente gehört auch standardmäßig für die erleichterte Verkettung (Bündelung) und Minimierung von CSS, JavaScript und HTML.</span><span class="sxs-lookup"><span data-stu-id="36978-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="36978-141">Erstellen und Ausführen von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36978-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="36978-142">Laden das generierte Webprojekt von ASP.NET Core direkt in Visual Studio, und klicken Sie dann erstellen, und führen das Projekt von dort aus.</span><span class="sxs-lookup"><span data-stu-id="36978-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="36978-143">Führen Sie die obigen Anweisungen zum Erstellen einer neuen ASP.NET Core-app mithilfe von Yeoman aus.</span><span class="sxs-lookup"><span data-stu-id="36978-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="36978-144">Wählen Sie dieses Mal **Webanwendung** aus dem Menü und die Namen der app `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="36978-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="36978-145">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36978-145">Open Visual Studio.</span></span> <span data-ttu-id="36978-146">Wählen Sie im Menü Datei öffnen ‣ Projekt/Projektmappe ein.</span><span class="sxs-lookup"><span data-stu-id="36978-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="36978-147">Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" die *csproj* Datei, wählen Sie ihn, und klicken Sie auf die **öffnen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="36978-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="36978-148">Das Projekt sollte etwa der folgende Screenshot aussehen, im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="36978-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Dateien und Ordner eines neuen Projekts im Projektmappen-Explorer](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="36978-150">Yeoman scaffolds einer MVC-Webanwendung, sowohl vollständige Server und die clientseitige build-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="36978-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="36978-151">Server-Side-Abhängigkeiten sind aufgeführt, unter der **Abhängigkeiten/NuGet** Knoten und Client-Side-Abhängigkeiten in der **Abhängigkeiten/Bower** -Knoten des Projektmappen-Explorers.</span><span class="sxs-lookup"><span data-stu-id="36978-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="36978-152">Abhängigkeiten werden automatisch wiederhergestellt, wenn das Projekt geladen wird.</span><span class="sxs-lookup"><span data-stu-id="36978-152">Dependencies are restored automatically when the project is loaded.</span></span>

![Unter dem Knoten "Abhängigkeiten" in der Strukturansicht des Projektmappen-Explorer für der Ordner Bower Auflisten der abhängigen Elemente geöffnet ist.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="36978-154">Wenn alle Abhängigkeiten wiederhergestellt werden, drücken Sie die **F5** um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="36978-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="36978-155">Standard-Startseite im Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="36978-155">The default home page displays in the browser.</span></span>

![Öffnen Sie in Microsoft Edge-Webanwendung](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="36978-157">Wiederherstellen, erstellen und Hosten der über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="36978-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="36978-158">Sie können vorbereiten und Hosten der Webanwendung mithilfe der .NET Core-CLI.</span><span class="sxs-lookup"><span data-stu-id="36978-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="36978-159">Ändern Sie das aktuelle Verzeichnis an einer Eingabeaufforderung zum Ordner, der das Projekt (d. h. der Ordner mit der *csproj* Datei):</span><span class="sxs-lookup"><span data-stu-id="36978-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="36978-160">Wiederherstellen von Abhängigkeiten für das Projekt NuGet-Pakets:</span><span class="sxs-lookup"><span data-stu-id="36978-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="36978-161">Führen Sie die Anwendung:</span><span class="sxs-lookup"><span data-stu-id="36978-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="36978-162">Die plattformübergreifenden [Kestrel](xref:fundamentals/servers/kestrel) Webserver Lauschen an Port 5000 beginnt.</span><span class="sxs-lookup"><span data-stu-id="36978-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="36978-163">Öffnen Sie einen Webbrowser, und navigieren Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="36978-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Öffnen Sie in Microsoft Edge-Webanwendung](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="36978-165">Hinzufügen zu Ihrem Projekt mit Sub-Generatoren</span><span class="sxs-lookup"><span data-stu-id="36978-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="36978-166">Mit Yeoman [sub-Generatoren](https://github.com/omnisharp/generator-aspnet), können Sie entweder Hinzufügen einer `nuget.config` oder ein `web.config` nachdem das Projekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="36978-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="36978-167">Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die Datei erstellt werden soll:</span><span class="sxs-lookup"><span data-stu-id="36978-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="36978-168">Das Ergebnis ist ein NuGet-Konfigurationsdatei mit dem Namen `nuget.config` mit folgendem Inhalt:</span><span class="sxs-lookup"><span data-stu-id="36978-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="36978-169">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="36978-169">Additional resources</span></span>

* [<span data-ttu-id="36978-170">Server (Kestrel und WebListener)</span><span class="sxs-lookup"><span data-stu-id="36978-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="36978-171">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="36978-171">Fundamentals</span></span>](xref:fundamentals/index)
