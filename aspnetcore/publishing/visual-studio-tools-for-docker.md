---
title: "Visual Studio-Tools für Docker mit ASP.NET Core"
description: "Dieser Artikel beschreibt die Verwendung von Visual Studio 2017-Tools und Docker für Windows, um eine ASP.NET Core-Anwendung in Container zu packen."
keywords: Docker,ASP.NET Core,Visual Studio,Container
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="9d02c-104">Visual Studio-Tools für Docker</span><span class="sxs-lookup"><span data-stu-id="9d02c-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="9d02c-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) mit [Docker für Windows](https://docs.docker.com/docker-for-windows/install/) unterstützt das Erstellen, Debuggen und Ausführen von .NET Framework und .NET Core-Web- und Konsolenanwendungen mithilfe von Windows- und Linux-Containern.</span><span class="sxs-lookup"><span data-stu-id="9d02c-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d02c-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="9d02c-106">Prerequisites</span></span>

- <span data-ttu-id="9d02c-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) mit der .NET Core-Workload</span><span class="sxs-lookup"><span data-stu-id="9d02c-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="9d02c-108">Docker für Windows</span><span class="sxs-lookup"><span data-stu-id="9d02c-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="9d02c-109">Installation und Einrichtung</span><span class="sxs-lookup"><span data-stu-id="9d02c-109">Installation and setup</span></span>

<span data-ttu-id="9d02c-110">Installieren Sie [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) mit der .NET Core-Arbeitsauslastung.</span><span class="sxs-lookup"><span data-stu-id="9d02c-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span> <span data-ttu-id="9d02c-111">Wenn Sie Visual Studio bereits installiert haben, können Sie [die Visual Studio-Installation ändern](https://docs.microsoft.com/visualstudio/install/modify-visual-studio), um die .NET Core-Workload hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-111">If you already have Visual Studio installed, you can [modify your Visual Studio installation](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) to add the .NET Core workload.</span></span>

<span data-ttu-id="9d02c-112">Informationen zur Docker-Installation entnehmen Sie dem Artikel [Docker for Windows: What to know before you install (Docker für Windows: Was Sie vor der Installation wissen müssen)](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install), und installieren Sie [Docker für Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="9d02c-112">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="9d02c-113">Es ist erforderlich, dass Sie in Docker für Windows **[freigegebene Laufwerke](https://docs.docker.com/docker-for-windows/#shared-drives)** einrichten.</span><span class="sxs-lookup"><span data-stu-id="9d02c-113">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="9d02c-114">Die Einstellung ist für die Volumezuordnung und die Debugunterstützung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9d02c-114">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="9d02c-115">Klicken Sie in der Taskleiste mit der rechten Maustaste auf das Docker-Symbol. Klicken Sie auf **Einstellungen**, und wählen Sie **Freigegebene Laufwerke** aus.</span><span class="sxs-lookup"><span data-stu-id="9d02c-115">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="9d02c-116">Wählen Sie das Laufwerk aus, in dem Docker Ihre Dateien speichern soll, und übernehmen Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-116">Select the drive where Docker will store your files and apply changes.</span></span>

![Freigegebene Laufwerke](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="9d02c-118">Erstellen einer ASP.NET-Webanwendung und Hinzufügen der Docker-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="9d02c-118">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="9d02c-119">Erstellen Sie mithilfe von Visual Studio eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="9d02c-119">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="9d02c-120">Wenn die Anwendung geladen wurde, wählen Sie entweder im Menü **Projekt** die Option **Docker-Unterstützung hinzufügen** aus, oder klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt und wählen **Hinzufügen** > **Docker-Unterstützung** aus.</span><span class="sxs-lookup"><span data-stu-id="9d02c-120">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="9d02c-121">*Menü „Projekt“*</span><span class="sxs-lookup"><span data-stu-id="9d02c-121">*Project Menu*</span></span>

![Projekt > Add Docker Support (Docker-Unterstützung hinzufügen)](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="9d02c-123">*Kontextmenü „Projekt“*</span><span class="sxs-lookup"><span data-stu-id="9d02c-123">*Project Context Menu*</span></span>

![Rechtsklick auf „Docker-Unterstützung hinzufügen“](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="9d02c-125">Wenn Sie Docker-Unterstützung zu Ihrem Projekt hinzufügen, können Sie zwischen Windows- und Linux-Containern wählen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-125">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="9d02c-126">(Der Docker-Host muss den gleichen Containertyp ausführen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-126">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="9d02c-127">Wenn Sie den Containertyp in der ausgeführten Docker-Instanz ändern müssen, klicken Sie mit der rechten Maustaste in der Taskleiste auf das **Docker**-Symbol, und wählen Sie **Zu Windows-Containern wechseln** oder **Zu Linux-Containern wechseln**.)</span><span class="sxs-lookup"><span data-stu-id="9d02c-127">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="9d02c-128">Die folgenden Dateien werden zum Projekt hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9d02c-128">The following files are added to the project:</span></span>

- <span data-ttu-id="9d02c-129">**Dockerfile**: Die Docker-Datei für ASP.NET Core-Anwendungen basiert auf dem [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore)-Image.</span><span class="sxs-lookup"><span data-stu-id="9d02c-129">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="9d02c-130">Dieses Image enthält die ASP.NET Core NuGet-Pakete, die zur Verbesserung der Leistung beim Starten vorab mit JIT kompiliert wurden.</span><span class="sxs-lookup"><span data-stu-id="9d02c-130">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="9d02c-131">Beim Erstellen von .NET Core-Konsolenanwendungen verweist die Docker-Datei auf das aktuellste [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet)-Image.</span><span class="sxs-lookup"><span data-stu-id="9d02c-131">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="9d02c-132">**docker-compose.yml**: Docker Compose-Basisdatei zum Definieren der Sammlung von Images, die mit „docker-compose build/run“ erstellt und ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="9d02c-132">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="9d02c-133">**docker-compose.dev.debug.yml**: zusätzliche „docker-compose“-Datei für iterative Änderungen, wenn die Konfiguration zum Debuggen festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="9d02c-133">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="9d02c-134">Visual Studio ruft „-f docker-compose.yml“ und „-f docker-compose.dev.debug.yml“ auf und führt beide zusammen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-134">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="9d02c-135">Diese compose-Datei wird von den Visual Studio-Entwicklungstools verwendet.</span><span class="sxs-lookup"><span data-stu-id="9d02c-135">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="9d02c-136">**docker-compose.dev.release.yml**: zusätzliche Docker Compose-Datei zum Debuggen der Releasedefinition.</span><span class="sxs-lookup"><span data-stu-id="9d02c-136">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="9d02c-137">Sie dient der Volumebereitstellung für den Debugger, sodass der Inhalt des Produktionsimages nicht verändert wird.</span><span class="sxs-lookup"><span data-stu-id="9d02c-137">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="9d02c-138">Die Datei *docker-compose.yml* enthält den Namen des Images, das beim Ausführen des Projekts erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9d02c-138">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="9d02c-139">In diesem Beispiel erstellt `image: user/hellodockertools${TAG}` das Image `user/hellodockertools:dev`, wenn die Anwendung im **Debugmodus** und `user/hellodockertools:latest` im **Releasemodus** ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9d02c-139">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="9d02c-140">Sie sollten für `user` den Benutzernamen Ihres [Docker-Hubs](https://hub.docker.com/) angeben, wenn Sie das Image per Push in die Registrierung laden möchten.</span><span class="sxs-lookup"><span data-stu-id="9d02c-140">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="9d02c-141">Beispiel: `spboyer/hellodockertools`. Oder geben Sie je nach Konfiguration Ihre private Registrierungs-URL an: `privateregistry.domain.com/`.</span><span class="sxs-lookup"><span data-stu-id="9d02c-141">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="9d02c-142">Debuggen</span><span class="sxs-lookup"><span data-stu-id="9d02c-142">Debugging</span></span>

<span data-ttu-id="9d02c-143">Wählen Sie in der Symbolleiste im Dropdownmenü „Debuggen“ die Option **Docker** aus, und drücken Sie die Taste F5, um mit dem Debuggen der Anwendung zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-143">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="9d02c-144">Das Image *microsoft/aspnetcore* wird geladen (falls es sich nicht bereits im Cache befindet).</span><span class="sxs-lookup"><span data-stu-id="9d02c-144">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="9d02c-145">*ASPNETCORE_ENVIRONMENT* wird im Container auf „Development“ festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9d02c-145">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="9d02c-146">PORT 80 wird VERFÜGBAR GEMACHT und einem dynamisch zugewiesenen Port für localhost zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="9d02c-146">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="9d02c-147">Der Port wird vom Docker-Host bestimmt und kann mit „docker ps“ abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="9d02c-147">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="9d02c-148">Ihre Anwendung wird in den Container kopiert.</span><span class="sxs-lookup"><span data-stu-id="9d02c-148">Your application is copied to the container</span></span>
- <span data-ttu-id="9d02c-149">Der Standardbrowser wird mit dem dynamisch zugewiesenen Port gestartet, wobei der Debugger an den Container angehängt wurde.</span><span class="sxs-lookup"><span data-stu-id="9d02c-149">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="9d02c-150">Das resultierende Docker-Image ist das *dev*-Image Ihrer Anwendung mit den *microsoft/aspnetcore*-Images als Basisimage.</span><span class="sxs-lookup"><span data-stu-id="9d02c-150">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="9d02c-151">**Hinweis**: Das dev-Image enthält keine App-Inhalte, da Debugkonfigurationen mittels Volumebereitstellung für die iterative Funktionalität sorgen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-151">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="9d02c-152">Verwenden Sie zum Verschieben eines Images die Releasekonfiguration.</span><span class="sxs-lookup"><span data-stu-id="9d02c-152">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="9d02c-153">Die Anwendung wird mit dem Container ausgeführt. Diesen können Sie mit dem Befehl `docker ps` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-153">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="9d02c-154">Bearbeiten und Fortfahren</span><span class="sxs-lookup"><span data-stu-id="9d02c-154">Edit and Continue</span></span>

<span data-ttu-id="9d02c-155">Änderungen an statischen Dateien und/oder Razor-Vorlagendateien (*.cshtml*) werden automatisch aktualisiert, ohne dass ein Kompilierungsschritt erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="9d02c-155">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="9d02c-156">Nehmen Sie die Änderung vor, speichern Sie die Datei, und aktualisieren Sie die Browseransicht, um die Aktualisierung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-156">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="9d02c-157">Wenn Sie Änderungen an Codedateien vornehmen, müssen Sie die Dateien kompilieren und Kestrel im Container neu starten.</span><span class="sxs-lookup"><span data-stu-id="9d02c-157">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="9d02c-158">Nachdem Sie die Änderung vorgenommen haben, drücken Sie die Tastenkombination STRG + F5, um den Prozess durchzuführen und die Anwendung im Container zu starten.</span><span class="sxs-lookup"><span data-stu-id="9d02c-158">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="9d02c-159">Der Docker-Container wird weder neu erstellt noch beendet. Wenn Sie `docker ps` in die Befehlszeile eingeben, können Sie sehen, dass der ursprüngliche Container seit 10 Minuten ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9d02c-159">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="9d02c-160">Veröffentlichen von Docker-Images</span><span class="sxs-lookup"><span data-stu-id="9d02c-160">Publishing Docker images</span></span>

<span data-ttu-id="9d02c-161">Nachdem Sie den Entwicklungs- und Debug-Zyklus für Ihre Anwendung abgeschlossen haben, können Sie mit den Visual Studio-Tools für Docker das Produktionsimage Ihrer Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-161">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="9d02c-162">Wählen Sie im Dropdownmenü „Debuggen“ die Option **Release**, und erstellen Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9d02c-162">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="9d02c-163">Das Tool erstellt das Image mit dem Tag `:latest`, das Sie mittels Push an Ihre private Registrierung oder den Docker-Hub übertragen können.</span><span class="sxs-lookup"><span data-stu-id="9d02c-163">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="9d02c-164">Mit dem Befehl `docker images` können Sie die Liste der Images anzeigen.</span><span class="sxs-lookup"><span data-stu-id="9d02c-164">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="9d02c-165">Es wäre möglicherweise zu erwarten, dass das Produktions- bzw. Release-Image kleiner ist als das **dev**-Image. Aufgrund der Volumezuordnung werden Debugger und Anwendung jedoch auf dem lokalen Computer und nicht im Container ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9d02c-165">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="9d02c-166">Beim **neuesten** Image wurde der gesamte Anwendungscode, der zum Ausführen der Anwendung auf einem Hostcomputer benötigt wird, gepackt. Daher entspricht die Größe der Deltaversion der Größe des Anwendungscodes.</span><span class="sxs-lookup"><span data-stu-id="9d02c-166">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
