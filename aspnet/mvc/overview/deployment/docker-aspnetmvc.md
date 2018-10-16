---
uid: mvc/overview/deployment/docker
title: Migrieren von ASP.NET MVC-Anwendungen zu Windows-Containern
description: Erfahren Sie, wie Sie eine vorhandene ASP.NET MVC-Anwendung in einem Windows Docker-Container ausführen können.
keywords: Windows-Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 2e7b2a2e9d915aec0814def56daab860e1efa8af
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348415"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="1ac22-104">Migrieren von ASP.NET MVC-Anwendungen zu Windows-Containern</span><span class="sxs-lookup"><span data-stu-id="1ac22-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="1ac22-105">Wird eine vorhandene .NET Framework-basierte Anwendung in einem Windows-Container ausgeführt, sind keine Änderungen an der Anwendung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1ac22-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="1ac22-106">Um die Anwendung in einem Windows-Container auszuführen, erstellen Sie ein Docker-Image, das die Anwendung enthält, und starten Sie den Container.</span><span class="sxs-lookup"><span data-stu-id="1ac22-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="1ac22-107">In diesem Thema wird erläutert, wie eine vorhandene [ASP.NET MVC-Anwendung](http://www.asp.net/mvc) übernommen und in einem Windows-Container bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="1ac22-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="1ac22-108">Sie beginnen mit einer vorhandenen ASP.NET MVC-Anwendung und erstellen dann mit Visual Studio die veröffentlichten Objekte.</span><span class="sxs-lookup"><span data-stu-id="1ac22-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="1ac22-109">Mithilfe von Docker erstellen Sie das Image, in dem die Anwendung enthalten ist und aus dem sie ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1ac22-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="1ac22-110">Sie navigieren zu der Website, die in einem Windows-Container ausgeführt wird, und vergewissern sich, dass die Anwendung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="1ac22-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="1ac22-111">Dieser Artikel setzt Grundkenntnisse in Docker voraus.</span><span class="sxs-lookup"><span data-stu-id="1ac22-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="1ac22-112">Ausführlichere Informationen zu Docker finden Sie unter [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="1ac22-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="1ac22-113">Die Anwendung, die Sie in einem Container ausführen, ist eine einfache Website, die Fragen nach dem Zufallsprinzip beantwortet.</span><span class="sxs-lookup"><span data-stu-id="1ac22-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="1ac22-114">Diese Anwendung ist eine grundlegende MVC-Anwendung ohne Authentifizierung oder Datenbankspeicher. Somit wird es Ihnen ermöglicht, sich auf das Verschieben der Webebene in einen Container zu konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="1ac22-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="1ac22-115">In zukünftigen Themen wird das Verschieben und Verwalten von beständigem Speicher in Containeranwendungen gezeigt.</span><span class="sxs-lookup"><span data-stu-id="1ac22-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="1ac22-116">Das Verschieben Ihrer Anwendung umfasst folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="1ac22-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="1ac22-117">Erstellen einer Veröffentlichungsaufgabe, um die Objekte für ein Image zu erstellen</span><span class="sxs-lookup"><span data-stu-id="1ac22-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="1ac22-118">Erstellen eines Docker-Images, das die Anwendung ausführt</span><span class="sxs-lookup"><span data-stu-id="1ac22-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="1ac22-119">Starten eines Docker-Containers, der das Image ausführt</span><span class="sxs-lookup"><span data-stu-id="1ac22-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="1ac22-120">Überprüfen der Anwendung mit Ihrem Browser</span><span class="sxs-lookup"><span data-stu-id="1ac22-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="1ac22-121">Die [fertige Anwendung](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="1ac22-121">The [finished application](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ac22-122">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="1ac22-122">Prerequisites</span></span>

<span data-ttu-id="1ac22-123">Der Entwicklungscomputer benötigen die folgende Software:</span><span class="sxs-lookup"><span data-stu-id="1ac22-123">The development machine must have the following software:</span></span>

- <span data-ttu-id="1ac22-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (oder höher) oder [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (oder höher)</span><span class="sxs-lookup"><span data-stu-id="1ac22-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher)</span></span>
- <span data-ttu-id="1ac22-125">[Docker für Windows](https://docs.docker.com/docker-for-windows/) -Version Stable 1.13.0 oder 1.12 Beta 26 (oder neuere Versionen)</span><span class="sxs-lookup"><span data-stu-id="1ac22-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- [<span data-ttu-id="1ac22-126">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1ac22-126">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> <span data-ttu-id="1ac22-127">Wenn Sie Windows Server 2016 verwenden, gehen Sie entsprechend den Anweisungen für [Containerhostbereitstellung: Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment) vor.</span><span class="sxs-lookup"><span data-stu-id="1ac22-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="1ac22-128">Nach dem Installieren und starten Docker, mit der Maustaste auf das Taskleistensymbol, und wählen Sie **zu Windows-Containern wechseln**.</span><span class="sxs-lookup"><span data-stu-id="1ac22-128">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="1ac22-129">Dies ist erforderlich, um Docker-Images unter Windows auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1ac22-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="1ac22-130">Die Ausführung dieses Befehls dauert ein paar Sekunden:</span><span class="sxs-lookup"><span data-stu-id="1ac22-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="1ac22-131">![Windows-Container][windows-container]</span><span class="sxs-lookup"><span data-stu-id="1ac22-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="1ac22-132">Veröffentlichen des Skripts</span><span class="sxs-lookup"><span data-stu-id="1ac22-132">Publish script</span></span>

<span data-ttu-id="1ac22-133">Sammeln Sie alle Objekte, die Sie in ein Docker-Image laden müssen, in einem Speicherort.</span><span class="sxs-lookup"><span data-stu-id="1ac22-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="1ac22-134">Sie können den Visual Studio-Befehl **Veröffentlichen** verwenden, um ein Veröffentlichungsprofil für Ihre Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1ac22-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="1ac22-135">In diesem Profil sind alle Objekte in einer einzigen Verzeichnisstruktur untergebracht, die Sie später in diesem Tutorial in Ihr Zielimage kopieren.</span><span class="sxs-lookup"><span data-stu-id="1ac22-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="1ac22-136">**Veröffentlichungsschritte**</span><span class="sxs-lookup"><span data-stu-id="1ac22-136">**Publish Steps**</span></span>

1. <span data-ttu-id="1ac22-137">Klicken Sie mit der rechten Maustaste in Visual Studio auf das Webprojekt, und wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1ac22-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="1ac22-138">Klicken Sie auf die Schaltfläche **Benutzerdefiniertes Profil**, und wählen Sie **Dateisystem** als Methode aus.</span><span class="sxs-lookup"><span data-stu-id="1ac22-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="1ac22-139">Wählen Sie das Verzeichnis aus.</span><span class="sxs-lookup"><span data-stu-id="1ac22-139">Choose the directory.</span></span> <span data-ttu-id="1ac22-140">Konventionsgemäß verwendet das heruntergeladene Beispiel `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="1ac22-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="1ac22-141">![Veröffentlichungsverbindung][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="1ac22-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="1ac22-142">Öffnen Sie den Abschnitt **Dateiveröffentlichungsoptionen** der Registerkarte **Einstellungen**. Wählen Sie **Während der Veröffentlichung vorkompilieren**.</span><span class="sxs-lookup"><span data-stu-id="1ac22-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="1ac22-143">Diese Optimierung bedeutet, dass Sie beim Kompilieren von Ansichten im Docker-Container die vorkompilierten Ansichten kopieren.</span><span class="sxs-lookup"><span data-stu-id="1ac22-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="1ac22-144">![Veröffentlichungseinstellungen][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="1ac22-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="1ac22-145">Klicken Sie auf **Veröffentlichen**, und Visual Studio kopiert alle erforderlichen Objekte in den Zielordner.</span><span class="sxs-lookup"><span data-stu-id="1ac22-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="1ac22-146">Erstellen des Images</span><span class="sxs-lookup"><span data-stu-id="1ac22-146">Build the image</span></span>

<span data-ttu-id="1ac22-147">Definieren Sie das Docker-Image in einer Dockerfile-Datei.</span><span class="sxs-lookup"><span data-stu-id="1ac22-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="1ac22-148">Die Dockerfile-Datei enthält Anweisungen für das Basisimage, zusätzliche Komponenten, die Anwendung, die Sie ausführen möchten, und weitere Konfigurationsimages.</span><span class="sxs-lookup"><span data-stu-id="1ac22-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="1ac22-149">Die Dockerfile-Datei ist die Eingabe für den `docker build`-Befehl, der das Image erstellt.</span><span class="sxs-lookup"><span data-stu-id="1ac22-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="1ac22-150">Sie erstellen ein Image auf der Grundlage des `microsoft/aspnet`-Images, das sich auf dem [Docker-Hub](https://hub.docker.com/r/microsoft/aspnet/) befindet.</span><span class="sxs-lookup"><span data-stu-id="1ac22-150">You will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="1ac22-151">Das Basisimage `microsoft/aspnet` ist ein Windows Server-Image.</span><span class="sxs-lookup"><span data-stu-id="1ac22-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="1ac22-152">Es enthält Windows Server Core, IIS und ASP.NET 4.6.2.</span><span class="sxs-lookup"><span data-stu-id="1ac22-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="1ac22-153">Wenn Sie dieses Image im Container ausführen, werden IIS und die installierten Websites automatisch gestartet.</span><span class="sxs-lookup"><span data-stu-id="1ac22-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="1ac22-154">Die Dockerfile-Datei, die das Image erstellt, sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="1ac22-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="1ac22-155">Es gibt keinen `ENTRYPOINT`-Befehl in dieser Dockerfile-Datei.</span><span class="sxs-lookup"><span data-stu-id="1ac22-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="1ac22-156">Sie benötigen ihn nicht.</span><span class="sxs-lookup"><span data-stu-id="1ac22-156">You don't need one.</span></span> <span data-ttu-id="1ac22-157">Bei Windows Server mit IIS ausgeführt wird, ist der IIS-Prozess den Einstiegspunkt, der konfiguriert wird, um in das Aspnet-Basisimage zu starten.</span><span class="sxs-lookup"><span data-stu-id="1ac22-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="1ac22-158">Führen Sie den Docker-Erstellungsbefehl aus, um das Image zu erstellen, über das die ASP.NET-Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1ac22-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="1ac22-159">Zu diesem Zweck öffnen Sie ein PowerShell-Fenster im Verzeichnis des Projekts, und geben Sie den folgenden Befehl im Projektmappenverzeichnis ein:</span><span class="sxs-lookup"><span data-stu-id="1ac22-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="1ac22-160">Dieser Befehl erstellt das neue Image entsprechend den Anweisungen in der dockerfile-Datei benennen (--t tagging) das Bild als Mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="1ac22-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="1ac22-161">Dies kann beinhalten, dass Sie das Basisimage aus [Docker Hub](http://hub.docker.com) abrufen und dann Ihre Anwendung zu diesem Image hinzufügen müssen.</span><span class="sxs-lookup"><span data-stu-id="1ac22-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="1ac22-162">Nach Abschluss dieses Befehls können Sie den `docker images`-Befehl ausführen, um Informationen über das neue Image anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="1ac22-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="1ac22-163">Die IMAGE-ID wird auf Ihrem Computer anders lauten.</span><span class="sxs-lookup"><span data-stu-id="1ac22-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="1ac22-164">Führen Sie die Anwendung jetzt aus.</span><span class="sxs-lookup"><span data-stu-id="1ac22-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="1ac22-165">Starten eines Containers</span><span class="sxs-lookup"><span data-stu-id="1ac22-165">Start a container</span></span>

<span data-ttu-id="1ac22-166">Starten Sie mit folgendem `docker run`-Befehl einen Container:</span><span class="sxs-lookup"><span data-stu-id="1ac22-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="1ac22-167">Das `-d`-Argument weist Docker an, das Image im getrennten Modus zu starten.</span><span class="sxs-lookup"><span data-stu-id="1ac22-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="1ac22-168">Das bedeutet, dass das Docker-Image getrennt von der aktuellen Shell ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1ac22-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="1ac22-169">In vielen Beispielen für Docker sehen Sie -p, um den Container und Host-Ports zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="1ac22-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="1ac22-170">Das Aspnet-Standardimage wurde bereits konfiguriert, den Container zum Lauschen an Port 80 und verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="1ac22-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span>

<span data-ttu-id="1ac22-171">`--name randomanswers` gibt dem ausgeführten Container einen Namen.</span><span class="sxs-lookup"><span data-stu-id="1ac22-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="1ac22-172">Sie können diesen Namen in den meisten Befehlen anstelle der Container-ID verwenden.</span><span class="sxs-lookup"><span data-stu-id="1ac22-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="1ac22-173">`mvcrandomanswers` ist der Name des zu startenden Images.</span><span class="sxs-lookup"><span data-stu-id="1ac22-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="1ac22-174">Überprüfen im Browser</span><span class="sxs-lookup"><span data-stu-id="1ac22-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="1ac22-175">Mit der aktuellen Version von Windows-Container, die Sie zum navigieren können nicht `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1ac22-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="1ac22-176">Dies ist ein bekanntes Verhalten in WinNAT und wird in Zukunft gelöst.</span><span class="sxs-lookup"><span data-stu-id="1ac22-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="1ac22-177">Bis zur Behebung dieses Fehlers müssen Sie die IP-Adresse des Containers verwenden.</span><span class="sxs-lookup"><span data-stu-id="1ac22-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="1ac22-178">Nach dem Start des Containers ermitteln Sie dessen IP-Adresse, damit Sie über einen Browser eine Verbindung mit dem ausgeführten Container herstellen können:</span><span class="sxs-lookup"><span data-stu-id="1ac22-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="1ac22-179">Verbinden mit dem ausgeführten Container, der die IPv4-Adresse mit `http://172.31.194.61` im gezeigten Beispiel.</span><span class="sxs-lookup"><span data-stu-id="1ac22-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="1ac22-180">Wenn Sie diese URL in Ihren Browser eingeben, sollte die ausgeführte Website angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1ac22-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="1ac22-181">Manche VPN- oder Proxysoftware könnte Ihren Wechsel zu Ihrer Website verhindern.</span><span class="sxs-lookup"><span data-stu-id="1ac22-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="1ac22-182">Sie können diese Software vorübergehend deaktivieren, um sicherzustellen, dass der Container ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1ac22-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="1ac22-183">Das Beispielverzeichnis auf GitHub enthält ein [PowerShell-Skript](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1), das diese Befehle für Sie ausführt.</span><span class="sxs-lookup"><span data-stu-id="1ac22-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="1ac22-184">Öffnen Sie ein PowerShell-Fenster, wechseln Sie in das Projektmappenverzeichnis, und geben Sie ein:</span><span class="sxs-lookup"><span data-stu-id="1ac22-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="1ac22-185">Der vorstehende Befehl erstellt das Image, zeigt die Liste der Images auf dem Computer an, startet einen Container und zeigt die IP-Adresse für diesen Container an.</span><span class="sxs-lookup"><span data-stu-id="1ac22-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="1ac22-186">Um den Container zu stoppen, geben Sie einen `docker
stop`-Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="1ac22-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="1ac22-187">Um den Container zu entfernen, geben Sie einen `docker rm`-Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="1ac22-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Wechseln zu Windows-Container"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Im Dateisystem veröffentlichen"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Veröffentlichungseinstellungen"
