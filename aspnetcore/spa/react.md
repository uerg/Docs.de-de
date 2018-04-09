---
title: Verwenden Sie die Projektvorlage reagieren mit ASP.NET Core
author: SteveSandersonMS
description: Informationen Sie zum Einstieg in die Projektvorlage für ASP.NET Core einzelnen Seite Anwendung (SPA) für reagieren und erstellen-reagieren-app.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 4dcfef2bbb99873a9d716a4942f39123944f495c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="6145a-103">Verwenden Sie die Projektvorlage reagieren mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6145a-103">Use the React project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="6145a-104">Diese Dokumentation ist nicht zur reagieren-Projektvorlage in ASP.NET Core 2.0 enthalten.</span><span class="sxs-lookup"><span data-stu-id="6145a-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="6145a-105">Es geht die neuere reagieren-Vorlage, die Sie manuell aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="6145a-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="6145a-106">Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="6145a-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="6145a-107">Die aktualisierte reagieren-Projektvorlage stellt einen nützlichen Startpunkt für ASP.NET Core-apps mit reagieren und [erstellen-reagieren-app](https://github.com/facebookincubator/create-react-app) Konventionen (CRA), um eine umfassende, clientseitige Benutzeroberfläche (UI) zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="6145a-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="6145a-108">Die Vorlage ist äquivalent zu erstellen, sowohl ein ASP.NET Core-Projekt fungiert als ein API-Back-End- und ein standard-CRA reagieren Projekt fungieren, wie eine Benutzeroberfläche, jedoch mit den Komfort Hosten in einer einzelnen app-Projekt, das erstellt und als einzelne Einheit veröffentlicht werden kann.</span><span class="sxs-lookup"><span data-stu-id="6145a-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="6145a-109">Eine neue app erstellen</span><span class="sxs-lookup"><span data-stu-id="6145a-109">Create a new app</span></span>

<span data-ttu-id="6145a-110">Wenn ASP.NET Core 2.0 verwenden, stellen Sie sicher, Sie haben [installiert die aktualisierte reagieren-Projektvorlage](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="6145a-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="6145a-111">Wenn Sie ASP.NET Core 2.1 installiert ist, besteht keine Notwendigkeit, es zu installieren.</span><span class="sxs-lookup"><span data-stu-id="6145a-111">If you have ASP.NET Core 2.1, there's no need to install it.</span></span>

<span data-ttu-id="6145a-112">Erstellen eines neuen Projekts von einer Eingabeaufforderung mit dem Befehl `dotnet new react` in ein leeres Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="6145a-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="6145a-113">Die folgenden Befehle erstellen z. B. die app in eine *Meine-neuen-app* Verzeichnis und wechseln Sie in diesem Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="6145a-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="6145a-114">Führen Sie die app aus Visual Studio oder .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="6145a-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6145a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6145a-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6145a-116">Öffnen Sie die generierte *csproj* Datei, und führen Sie die app als normale von dort aus.</span><span class="sxs-lookup"><span data-stu-id="6145a-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="6145a-117">Während des Erstellungsprozesses stellt Npm Abhängigkeiten bei der ersten Ausführung, die dies einige Minuten dauern kann, wieder her.</span><span class="sxs-lookup"><span data-stu-id="6145a-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="6145a-118">Nachfolgende Builds sind wesentlich schneller.</span><span class="sxs-lookup"><span data-stu-id="6145a-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6145a-119">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="6145a-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6145a-120">Sicherzustellen, dass Sie über eine Umgebungsvariable namens `ASPNETCORE_Environment` mit dem Wert des `Development`.</span><span class="sxs-lookup"><span data-stu-id="6145a-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="6145a-121">Führen Sie für Windows (in nicht-PowerShell-Anweisungen), `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="6145a-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="6145a-122">Führen Sie unter Linux oder MacOS `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="6145a-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="6145a-123">Führen Sie [Dotnet Build](/dotnet/core/tools/dotnet-build) um zu überprüfen, ob die app ordnungsgemäß erstellt.</span><span class="sxs-lookup"><span data-stu-id="6145a-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="6145a-124">Bei der ersten Ausführung stellt während des Erstellungsprozesses Npm Abhängigkeiten, die dies einige Minuten dauern kann, wieder her.</span><span class="sxs-lookup"><span data-stu-id="6145a-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="6145a-125">Nachfolgende Builds sind wesentlich schneller.</span><span class="sxs-lookup"><span data-stu-id="6145a-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="6145a-126">Führen Sie [Dotnet ausführen](/dotnet/core/tools/dotnet-run) für den Anwendungsstart.</span><span class="sxs-lookup"><span data-stu-id="6145a-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="6145a-127">Die Projektvorlage erstellt eine ASP.NET Core-app und einer app reagieren.</span><span class="sxs-lookup"><span data-stu-id="6145a-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="6145a-128">Die app ASP.NET Core soll für den Datenzugriff, Autorisierung und Bedenken serverseitige verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6145a-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="6145a-129">Die reagieren-app, die in den die *ClientApp* Unterverzeichnis für alle Aspekte der Benutzeroberfläche verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6145a-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="6145a-130">Fügen Sie Seiten, Bilder, Stile, Module, usw. an.</span><span class="sxs-lookup"><span data-stu-id="6145a-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="6145a-131">Die *ClientApp* Verzeichnis ist eine standard-app, der auf der CRA zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="6145a-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="6145a-132">Finden Sie unter den offiziellen [CRA Dokumentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="6145a-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="6145a-133">Es gibt jedoch geringfügige Unterschiede zwischen der reagieren-app, die anhand dieser Vorlage erstellt und von CRA selbst erstellt; die app-Funktionen sind jedoch unverändert.</span><span class="sxs-lookup"><span data-stu-id="6145a-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="6145a-134">Die app, die von der Vorlage erstellten enthält eine [Bootstrap](https://getbootstrap.com/)-basierten Layout und ein einfaches routing-Beispiel.</span><span class="sxs-lookup"><span data-stu-id="6145a-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="6145a-135">NPM-Pakete installieren</span><span class="sxs-lookup"><span data-stu-id="6145a-135">Install npm packages</span></span>

<span data-ttu-id="6145a-136">Verwenden Sie zum Installieren eines Drittanbieters Npm-Pakete in eine Eingabeaufforderung die *ClientApp* Unterverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="6145a-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="6145a-137">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6145a-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="6145a-138">Veröffentlichen und Bereitstellen</span><span class="sxs-lookup"><span data-stu-id="6145a-138">Publish and deploy</span></span>

<span data-ttu-id="6145a-139">Führt die app in einem Modus der Einfachheit halber für Entwickler optimiert, bei der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="6145a-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="6145a-140">JavaScript-Bundle enthalten z. B. Quelle Maps, (, damit beim Debuggen Sie den ursprünglichen Quellcode ersichtlich).</span><span class="sxs-lookup"><span data-stu-id="6145a-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="6145a-141">Die app wird überwacht, ob JavaScript, HTML und CSS-Datei von Änderungen auf dem Datenträger und automatisch erneut kompiliert und lädt, wenn er erkennt, dass diese Dateien zu ändern.</span><span class="sxs-lookup"><span data-stu-id="6145a-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="6145a-142">Dienen Sie in der Produktion eine Version der app, die für Leistung optimiert wird.</span><span class="sxs-lookup"><span data-stu-id="6145a-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="6145a-143">Dies ist für konfiguriert automatisch durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="6145a-143">This is configured to happen automatically.</span></span> <span data-ttu-id="6145a-144">Beim Veröffentlichen, gibt die Buildkonfiguration eine verkleinerte, Transpiled Build des Codes die clientseitige aus.</span><span class="sxs-lookup"><span data-stu-id="6145a-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="6145a-145">Im Gegensatz zu den Development Build erfordert die Produktions-Build Node.js auf dem Server installiert werden.</span><span class="sxs-lookup"><span data-stu-id="6145a-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="6145a-146">Sie können die Standard [ASP.NET Core Hosting- und Bereitstellung Methoden](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="6145a-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="6145a-147">Führen Sie das CRA unabhängig</span><span class="sxs-lookup"><span data-stu-id="6145a-147">Run the CRA server independently</span></span>

<span data-ttu-id="6145a-148">Das Projekt konfiguriert wurde, um eine eigene Instanz von den CRA Development Server im Hintergrund starten, die ASP.NET Core-app in den Entwicklungsmodus gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="6145a-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="6145a-149">Dies ist nützlich, da es bedeutet, dass Sie keinen separaten Server manuell ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="6145a-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="6145a-150">Es ist ein Nachteil dieses Standard-Setup aus.</span><span class="sxs-lookup"><span data-stu-id="6145a-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="6145a-151">Jedes Mal, die Sie ändern den C#-Code und Ihrer ASP.NET Core app neu gestartet werden muss, wird der CRA-Server neu.</span><span class="sxs-lookup"><span data-stu-id="6145a-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="6145a-152">Ein paar Sekunden sind erforderlich, zu sichern.</span><span class="sxs-lookup"><span data-stu-id="6145a-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="6145a-153">Wenn Sie häufige C#-Code bearbeitet machen und nicht für einen Neustart des CRA Servers warten möchten, führen Sie das CRA extern, unabhängig von der ASP.NET Core-Prozess.</span><span class="sxs-lookup"><span data-stu-id="6145a-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="6145a-154">Dazu:</span><span class="sxs-lookup"><span data-stu-id="6145a-154">To do so:</span></span>

1. <span data-ttu-id="6145a-155">In einem Eingabeaufforderungsfenster, wechseln Sie zu der *ClientApp* Unterverzeichnis, und starten Sie den CRA Development Server:</span><span class="sxs-lookup"><span data-stu-id="6145a-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="6145a-156">Ändern Sie die ASP.NET Core Anwendung die externen CRA Server-Instanz verwenden, anstatt eine eigene starten.</span><span class="sxs-lookup"><span data-stu-id="6145a-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="6145a-157">In Ihrer *Start* -Klasse, ersetzen Sie die `spa.UseReactDevelopmentServer` Aufruf durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6145a-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="6145a-158">Wenn Sie Ihre app ASP.NET Core starten, wird nicht es einen CRA-Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="6145a-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="6145a-159">Die Instanz, die Sie manuell gestartet werden, wird stattdessen verwendet.</span><span class="sxs-lookup"><span data-stu-id="6145a-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="6145a-160">Dadurch können sie zum Starten und Neustarten schneller.</span><span class="sxs-lookup"><span data-stu-id="6145a-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="6145a-161">Er wartet nicht mehr für Ihre app reagieren, um jedes Mal neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6145a-161">It's no longer waiting for your React app to rebuild each time.</span></span>
