---
uid: mvc/overview/deployment/docker
title: Migrieren von ASP.NET MVC-Anwendungen zu Windows-Containern
description: Erfahren Sie, wie Sie eine vorhandene ASP.NET MVC-Anwendung in einem Windows Docker-Container ausführen können.
keywords: Windows-Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 1343bd100f521326477ecd831aa627b4394bad44
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795352"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrieren von ASP.NET MVC-Anwendungen zu Windows-Containern

Wird eine vorhandene .NET Framework-basierte Anwendung in einem Windows-Container ausgeführt, sind keine Änderungen an der Anwendung erforderlich. Um die Anwendung in einem Windows-Container auszuführen, erstellen Sie ein Docker-Image, das die Anwendung enthält, und starten Sie den Container. In diesem Thema wird erläutert, wie eine vorhandene [ASP.NET MVC-Anwendung](http://www.asp.net/mvc) übernommen und in einem Windows-Container bereitgestellt wird.

Sie beginnen mit einer vorhandenen ASP.NET MVC-Anwendung und erstellen dann mit Visual Studio die veröffentlichten Objekte. Mithilfe von Docker erstellen Sie das Image, in dem die Anwendung enthalten ist und aus dem sie ausgeführt wird. Sie navigieren zu der Website, die in einem Windows-Container ausgeführt wird, und vergewissern sich, dass die Anwendung funktioniert.

Dieser Artikel setzt Grundkenntnisse in Docker voraus. Ausführlichere Informationen zu Docker finden Sie unter [Docker Overview](https://docs.docker.com/engine/understanding-docker/).

Die Anwendung, die Sie in einem Container ausführen, ist eine einfache Website, die Fragen nach dem Zufallsprinzip beantwortet. Diese Anwendung ist eine grundlegende MVC-Anwendung ohne Authentifizierung oder Datenbankspeicher. Somit wird es Ihnen ermöglicht, sich auf das Verschieben der Webebene in einen Container zu konzentrieren. In zukünftigen Themen wird das Verschieben und Verwalten von beständigem Speicher in Containeranwendungen gezeigt.

Das Verschieben Ihrer Anwendung umfasst folgenden Schritte:

1. [Erstellen einer Veröffentlichungsaufgabe, um die Objekte für ein Image zu erstellen](#publish-script)
1. [Erstellen eines Docker-Images, das die Anwendung ausführt](#build-the-image)
1. [Starten eines Docker-Containers, der das Image ausführt](#start-a-container)
1. [Überprüfen der Anwendung mit Ihrem Browser](#verify-in-the-browser)

Die [fertige Anwendung](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) finden Sie auf GitHub.

## <a name="prerequisites"></a>Vorraussetzungen

Der Entwicklungscomputer benötigen die folgende Software:

- [Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (oder höher) oder [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (oder höher)
- [Docker für Windows](https://docs.docker.com/docker-for-windows/) -Version Stable 1.13.0 oder 1.12 Beta 26 (oder neuere Versionen)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Wenn Sie Windows Server 2016 verwenden, gehen Sie entsprechend den Anweisungen für [Containerhostbereitstellung: Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment) vor.

Nach dem Installieren und starten Docker, mit der Maustaste auf das Taskleistensymbol, und wählen Sie **zu Windows-Containern wechseln**. Dies ist erforderlich, um Docker-Images unter Windows auszuführen. Die Ausführung dieses Befehls dauert ein paar Sekunden:

![Windows-Container][windows-container]

## <a name="publish-script"></a>Veröffentlichen des Skripts

Sammeln Sie alle Objekte, die Sie in ein Docker-Image laden müssen, in einem Speicherort. Sie können den Visual Studio-Befehl **Veröffentlichen** verwenden, um ein Veröffentlichungsprofil für Ihre Anwendung zu erstellen. In diesem Profil sind alle Objekte in einer einzigen Verzeichnisstruktur untergebracht, die Sie später in diesem Tutorial in Ihr Zielimage kopieren.

**Veröffentlichungsschritte**

1. Klicken Sie mit der rechten Maustaste in Visual Studio auf das Webprojekt, und wählen Sie **Veröffentlichen**.
1. Klicken Sie auf die Schaltfläche **Benutzerdefiniertes Profil**, und wählen Sie **Dateisystem** als Methode aus.
1. Wählen Sie das Verzeichnis aus. Konventionsgemäß verwendet das heruntergeladene Beispiel `bin\Release\PublishOutput`.

![Veröffentlichungsverbindung][publish-connection]

Öffnen Sie den Abschnitt **Dateiveröffentlichungsoptionen** der Registerkarte **Einstellungen**. Wählen Sie **Während der Veröffentlichung vorkompilieren**. Diese Optimierung bedeutet, dass Sie beim Kompilieren von Ansichten im Docker-Container die vorkompilierten Ansichten kopieren.

![Veröffentlichungseinstellungen][publish-settings]

Klicken Sie auf **Veröffentlichen**, und Visual Studio kopiert alle erforderlichen Objekte in den Zielordner.

## <a name="build-the-image"></a>Erstellen des Images

Definieren Sie das Docker-Image in einer Dockerfile-Datei. Die Dockerfile-Datei enthält Anweisungen für das Basisimage, zusätzliche Komponenten, die Anwendung, die Sie ausführen möchten, und weitere Konfigurationsimages.  Die Dockerfile-Datei ist die Eingabe für den `docker build`-Befehl, der das Image erstellt.

Sie erstellen ein Image auf der Grundlage des `microsoft/aspnet`-Images, das sich auf dem [Docker-Hub](https://hub.docker.com/r/microsoft/aspnet/) befindet.
Das Basisimage `microsoft/aspnet` ist ein Windows Server-Image. Es enthält Windows Server Core, IIS und ASP.NET 4.6.2. Wenn Sie dieses Image im Container ausführen, werden IIS und die installierten Websites automatisch gestartet.

Die Dockerfile-Datei, die das Image erstellt, sieht folgendermaßen aus:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Es gibt keinen `ENTRYPOINT`-Befehl in dieser Dockerfile-Datei. Sie benötigen ihn nicht. Bei Windows Server mit IIS ausgeführt wird, ist der IIS-Prozess den Einstiegspunkt, der konfiguriert wird, um in das Aspnet-Basisimage zu starten.

Führen Sie den Docker-Erstellungsbefehl aus, um das Image zu erstellen, über das die ASP.NET-Anwendung ausgeführt wird. Zu diesem Zweck öffnen Sie ein PowerShell-Fenster im Verzeichnis des Projekts, und geben Sie den folgenden Befehl im Projektmappenverzeichnis ein:

```console
docker build -t mvcrandomanswers .
```

Dieser Befehl erstellt das neue Image entsprechend den Anweisungen in der dockerfile-Datei benennen (--t tagging) das Bild als Mvcrandomanswers. Dies kann beinhalten, dass Sie das Basisimage aus [Docker Hub](http://hub.docker.com) abrufen und dann Ihre Anwendung zu diesem Image hinzufügen müssen.

Nach Abschluss dieses Befehls können Sie den `docker images`-Befehl ausführen, um Informationen über das neue Image anzuzeigen:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Die IMAGE-ID wird auf Ihrem Computer anders lauten. Führen Sie die Anwendung jetzt aus.

## <a name="start-a-container"></a>Starten eines Containers

Starten Sie mit folgendem `docker run`-Befehl einen Container:

```console
docker run -d --name randomanswers mvcrandomanswers
```

Das `-d`-Argument weist Docker an, das Image im getrennten Modus zu starten. Das bedeutet, dass das Docker-Image getrennt von der aktuellen Shell ausgeführt wird.

In vielen Beispielen für Docker sehen Sie -p, um den Container und Host-Ports zuzuordnen. Das Aspnet-Standardimage wurde bereits konfiguriert, den Container zum Lauschen an Port 80 und verfügbar gemacht.

`--name randomanswers` gibt dem ausgeführten Container einen Namen. Sie können diesen Namen in den meisten Befehlen anstelle der Container-ID verwenden.

`mvcrandomanswers` ist der Name des zu startenden Images.

## <a name="verify-in-the-browser"></a>Überprüfen im Browser

> [!NOTE]
> Mit der aktuellen Version von Windows-Container, die Sie zum navigieren können nicht `http://localhost`.
> Dies ist ein bekanntes Verhalten in WinNAT und wird in Zukunft gelöst. Bis zur Behebung dieses Fehlers müssen Sie die IP-Adresse des Containers verwenden.

Nach dem Start des Containers ermitteln Sie dessen IP-Adresse, damit Sie über einen Browser eine Verbindung mit dem ausgeführten Container herstellen können:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

Verbinden mit dem ausgeführten Container, der die IPv4-Adresse mit `http://172.31.194.61` im gezeigten Beispiel. Wenn Sie diese URL in Ihren Browser eingeben, sollte die ausgeführte Website angezeigt werden.

> [!NOTE]
> Manche VPN- oder Proxysoftware könnte Ihren Wechsel zu Ihrer Website verhindern.
> Sie können diese Software vorübergehend deaktivieren, um sicherzustellen, dass der Container ausgeführt wird.

Das Beispielverzeichnis auf GitHub enthält ein [PowerShell-Skript](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1), das diese Befehle für Sie ausführt. Öffnen Sie ein PowerShell-Fenster, wechseln Sie in das Projektmappenverzeichnis, und geben Sie ein:

```console
./run.ps1
```

Der vorstehende Befehl erstellt das Image, zeigt die Liste der Images auf dem Computer an, startet einen Container und zeigt die IP-Adresse für diesen Container an.

Um den Container zu stoppen, geben Sie einen `docker
stop`-Befehl ein:

```console
docker stop randomanswers
```

Um den Container zu entfernen, geben Sie einen `docker rm`-Befehl ein:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Wechseln zu Windows-Container"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Im Dateisystem veröffentlichen"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Veröffentlichungseinstellungen"
