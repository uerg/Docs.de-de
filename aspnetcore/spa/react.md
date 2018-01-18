---
title: Verwenden Sie die Projektvorlage reagieren
author: SteveSandersonMS
description: "Informationen Sie zum Einstieg in ASP.NET Core Einzelseiten-Anwendung (SPA) Release Candidate-Projektvorlage für reagieren und erstellen-reagieren-app."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: a63d81633a0f37d24ad5e05de293e3c41004eba1
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="36675-103">Verwenden Sie die Projektvorlage reagieren (RC)</span><span class="sxs-lookup"><span data-stu-id="36675-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="36675-104">Diese Dokumentation ist nicht zur endgültigen reagieren-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="36675-104">This documentation is not about the released React project template.</span></span> <span data-ttu-id="36675-105">**Diese Dokumentation ist über dem Release Candidate von der Vorlage reagieren.**</span><span class="sxs-lookup"><span data-stu-id="36675-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="36675-106">Wir hoffen, dass die endgültige Produktversion in frühen 2018 ausgeliefert werden.</span><span class="sxs-lookup"><span data-stu-id="36675-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="36675-107">Die aktualisierte reagieren-Projektvorlage stellt einen nützlichen Startpunkt für ASP.NET Core-apps mit reagieren und [erstellen-reagieren-app](https://github.com/facebookincubator/create-react-app) Konventionen (CRA), um eine umfassende, clientseitige Benutzeroberfläche (UI) zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="36675-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="36675-108">Die Vorlage ist äquivalent zu erstellen, sowohl ein ASP.NET Core-Projekt fungiert als ein API-Back-End- und ein standard-CRA reagieren Projekt fungieren, wie eine Benutzeroberfläche, jedoch mit den Komfort Hosten in einer einzelnen app-Projekt, das erstellt und als einzelne Einheit veröffentlicht werden kann.</span><span class="sxs-lookup"><span data-stu-id="36675-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="36675-109">Eine neue app erstellen</span><span class="sxs-lookup"><span data-stu-id="36675-109">Create a new app</span></span>

<span data-ttu-id="36675-110">Um zu beginnen, stellen Sie sicher haben Sie [installiert die aktualisierte reagieren-Projektvorlage](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="36675-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="36675-111">Diese Anweisungen nicht gelten, für die vorherigen reagieren-Projektvorlage, die in der .NET Core enthalten 2.0.x SDK.</span><span class="sxs-lookup"><span data-stu-id="36675-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="36675-112">Erstellen eines neuen Projekts von einer Eingabeaufforderung mit dem Befehl `dotnet new react` in ein leeres Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="36675-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="36675-113">Die folgenden Befehle erstellen z. B. die app in eine *Meine-neuen-app* Verzeichnis und wechseln Sie in diesem Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="36675-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new -o my-new-app
cd my-new-app
```

<span data-ttu-id="36675-114">Führen Sie die app aus Visual Studio oder .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="36675-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="36675-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36675-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="36675-116">Öffnen Sie die generierte *csproj* Datei, und führen Sie die app als normale von dort aus.</span><span class="sxs-lookup"><span data-stu-id="36675-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="36675-117">Während des Erstellungsprozesses stellt Npm Abhängigkeiten bei der ersten Ausführung, die dies einige Minuten dauern kann, wieder her.</span><span class="sxs-lookup"><span data-stu-id="36675-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="36675-118">Nachfolgende Builds sind wesentlich schneller.</span><span class="sxs-lookup"><span data-stu-id="36675-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="36675-119">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="36675-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="36675-120">Sicherzustellen, dass Sie über eine Umgebungsvariable namens `ASPNETCORE_Environment` mit dem Wert des `Development`.</span><span class="sxs-lookup"><span data-stu-id="36675-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="36675-121">Führen Sie für Windows (in nicht-PowerShell-Anweisungen), `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="36675-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="36675-122">Führen Sie unter Linux oder MacOS `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="36675-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="36675-123">Führen Sie `dotnet build` um zu überprüfen, ob die app ordnungsgemäß erstellt.</span><span class="sxs-lookup"><span data-stu-id="36675-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="36675-124">Bei der ersten Ausführung stellt während des Erstellungsprozesses Npm Abhängigkeiten, die dies einige Minuten dauern kann, wieder her.</span><span class="sxs-lookup"><span data-stu-id="36675-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="36675-125">Nachfolgende Builds sind wesentlich schneller.</span><span class="sxs-lookup"><span data-stu-id="36675-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="36675-126">Führen Sie `dotnet run` für den Anwendungsstart.</span><span class="sxs-lookup"><span data-stu-id="36675-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="36675-127">Die Projektvorlage erstellt eine ASP.NET Core-app und einer app reagieren.</span><span class="sxs-lookup"><span data-stu-id="36675-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="36675-128">Die app ASP.NET Core soll für den Datenzugriff, Autorisierung und Bedenken serverseitige verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="36675-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="36675-129">Die reagieren-app, die in den die *ClientApp* Unterverzeichnis für alle Aspekte der Benutzeroberfläche verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="36675-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="36675-130">Fügen Sie Seiten, Bilder, Stile, Module, usw. an.</span><span class="sxs-lookup"><span data-stu-id="36675-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="36675-131">Die *ClientApp* Verzeichnis ist eine standard-app, der auf der CRA zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="36675-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="36675-132">Finden Sie unter den offiziellen [CRA Dokumentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="36675-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="36675-133">Es gibt jedoch geringfügige Unterschiede zwischen der reagieren-app, die anhand dieser Vorlage erstellt und von CRA selbst erstellt; die app-Funktionen sind jedoch unverändert.</span><span class="sxs-lookup"><span data-stu-id="36675-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="36675-134">Die app, die von der Vorlage erstellten enthält eine [Bootstrap](https://getbootstrap.com/)-basierten Layout und ein einfaches routing-Beispiel.</span><span class="sxs-lookup"><span data-stu-id="36675-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="36675-135">Installieren Sie Npm-Pakete</span><span class="sxs-lookup"><span data-stu-id="36675-135">Install npm packages</span></span>

<span data-ttu-id="36675-136">Verwenden Sie zum Installieren eines Drittanbieters Npm-Pakete in eine Eingabeaufforderung die *ClientApp* Unterverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="36675-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="36675-137">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="36675-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="36675-138">Veröffentlichen und Bereitstellen</span><span class="sxs-lookup"><span data-stu-id="36675-138">Publish and deploy</span></span>

<span data-ttu-id="36675-139">Führt die app in einem Modus der Einfachheit halber für Entwickler optimiert, bei der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="36675-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="36675-140">JavaScript-Bundle enthalten z. B. Quelle Maps, (, damit beim Debuggen Sie den ursprünglichen Quellcode ersichtlich).</span><span class="sxs-lookup"><span data-stu-id="36675-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="36675-141">Die app wird überwacht, ob JavaScript, HTML und CSS-Datei von Änderungen auf dem Datenträger und automatisch erneut kompiliert und lädt, wenn er erkennt, dass diese Dateien zu ändern.</span><span class="sxs-lookup"><span data-stu-id="36675-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="36675-142">Dienen Sie in der Produktion eine Version der app, die für Leistung optimiert wird.</span><span class="sxs-lookup"><span data-stu-id="36675-142">In production, serve a version of your app that is optimized for performance.</span></span> <span data-ttu-id="36675-143">Dies ist für konfiguriert automatisch durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="36675-143">This is configured to happen automatically.</span></span> <span data-ttu-id="36675-144">Beim Veröffentlichen, gibt die Buildkonfiguration eine verkleinerte, Transpiled Build des Codes die clientseitige aus.</span><span class="sxs-lookup"><span data-stu-id="36675-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="36675-145">Im Gegensatz zu den Development Build erfordert die Produktions-Build Node.js auf dem Server installiert werden.</span><span class="sxs-lookup"><span data-stu-id="36675-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="36675-146">Sie können die Standard [ASP.NET Core Hosting- und Bereitstellung Methoden](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="36675-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="36675-147">Führen Sie das CRA unabhängig</span><span class="sxs-lookup"><span data-stu-id="36675-147">Run the CRA server independently</span></span>

<span data-ttu-id="36675-148">Das Projekt konfiguriert wurde, um eine eigene Instanz von den CRA Development Server im Hintergrund starten, die ASP.NET Core-app in den Entwicklungsmodus gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="36675-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="36675-149">Dies ist nützlich, da es bedeutet, dass Sie keinen separaten Server manuell ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="36675-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="36675-150">Es ist ein Nachteil dieses Standard-Setup aus.</span><span class="sxs-lookup"><span data-stu-id="36675-150">There is a drawback to this default setup.</span></span> <span data-ttu-id="36675-151">Jedes Mal, die Sie ändern den C#-Code und Ihrer ASP.NET Core app neu gestartet werden muss, wird der CRA-Server neu.</span><span class="sxs-lookup"><span data-stu-id="36675-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="36675-152">Ein paar Sekunden sind erforderlich, zu sichern.</span><span class="sxs-lookup"><span data-stu-id="36675-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="36675-153">Wenn Sie häufige C#-Code bearbeitet machen und nicht für einen Neustart des CRA Servers warten möchten, führen Sie das CRA extern, unabhängig von der ASP.NET Core-Prozess.</span><span class="sxs-lookup"><span data-stu-id="36675-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="36675-154">Dazu:</span><span class="sxs-lookup"><span data-stu-id="36675-154">To do so:</span></span>

1. <span data-ttu-id="36675-155">In einem Eingabeaufforderungsfenster, wechseln Sie zu der *ClientApp* Unterverzeichnis, und starten Sie den CRA Development Server:</span><span class="sxs-lookup"><span data-stu-id="36675-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="36675-156">Ändern Sie die ASP.NET Core Anwendung die externen CRA Server-Instanz verwenden, anstatt eine eigene starten.</span><span class="sxs-lookup"><span data-stu-id="36675-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="36675-157">In Ihrer *Start* -Klasse, ersetzen Sie die `spa.UseReactDevelopmentServer` Aufruf durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="36675-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="36675-158">Wenn Sie Ihre app ASP.NET Core starten, wird nicht es einen CRA-Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="36675-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="36675-159">Die Instanz, die Sie manuell gestartet werden, wird stattdessen verwendet.</span><span class="sxs-lookup"><span data-stu-id="36675-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="36675-160">Dadurch können sie zum Starten und Neustarten schneller.</span><span class="sxs-lookup"><span data-stu-id="36675-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="36675-161">Er wartet nicht mehr für Ihre app reagieren, um jedes Mal neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="36675-161">It's no longer waiting for your React app to rebuild each time.</span></span>
