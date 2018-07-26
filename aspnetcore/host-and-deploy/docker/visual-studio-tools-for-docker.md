---
title: Visual Studio-Tools für Docker mit ASP.NET Core
author: spboyer
description: Informationen zum Verwenden der Visual Studio 2017-Tools und Docker für Windows, um Ihre ASP.NET Core-App in Container zu packen.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146883"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio-Tools für Docker mit ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) unterstützt das Erstellen, Debuggen und Ausführen von ASP.NET Core-Container-Apps, die für .NET Core entwickelt wurden. Sowohl Windows- als auch Linux-Container werden unterstützt.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Visual Studio 2017](https://www.visualstudio.com/) mit der Workload für die **plattformübergreifende .NET Core-Entwicklung**.
* [Docker für Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Installation und Einrichtung

Informationen zur Docker-Installation entnehmen Sie dem Artikel [Docker for Windows: What to know before you install (Docker für Windows: Was Sie vor der Installation wissen müssen)](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install), und installieren Sie [Docker für Windows](https://docs.docker.com/docker-for-windows/install/).

**[Freigegebene Laufwerke](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker für Windows müssen so konfiguriert sein, dass sie die Volumezuordnung und das Debuggen unterstützen. Klicken Sie mit der rechten Maustaste in der Taskleiste auf das Docker-Symbol, und wählen Sie **Einstellungen...** und anschließend **Freigegebene Laufwerke** aus. Wählen Sie das Laufwerk aus, auf dem Docker Dateien speichert. Klicken Sie auf **Übernehmen**.

![Freigegebene Laufwerke](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 Version 15.6 und höher zeigt später eine Eingabeaufforderung an, wenn die **Freigegebenen Laufwerke** nicht konfiguriert sind.

## <a name="add-docker-support-to-an-app"></a>Hinzufügen der Docker-Unterstützung zu einer App

Um einem ASP.NET Core-Projekt Docker-Unterstützung hinzuzufügen, muss das Projekt .NET Core als Ziel haben. Sowohl Linux- als auch Windows-Container werden unterstützt.

Wenn Sie Docker-Unterstützung zu einem Projekt hinzufügen, können Sie zwischen einem Windows- oder einem Linux-Container auswählen. Der Docker-Host muss den gleichen Containertyp ausführen. Wenn Sie den Containertyp in der ausgeführten Docker-Instanz ändern möchten, klicken Sie mit der rechten Maustaste auf der Taskleiste auf das Docker-Symbol, und wählen Sie **Switch to Windows containers** (Zu Windows-Containern wechseln) oder **Switch to Linux container** (Zu Linux-Containern wechseln) aus.

### <a name="new-app"></a>Neue App

Wenn Sie mithilfe der Projektvorlage **ASP.NET Core-Webanwendung** eine neue App erstellen, aktivieren Sie das Kontrollkästchen **Enable Docker Support** (Docker-Unterstützung aktivieren):

![Kontrollkästchen „Enable Docker Support“ (Docker-Unterstützung aktivieren)](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

Wenn .NET Core das Zielframework ist, können Sie in der Dropdownliste **OS** (Betriebssystem) einen Containertyp auswählen.

### <a name="existing-app"></a>Vorhandene App

Mit den Visual Studio-Tools für Docker können Sie Docker nicht zu einem vorhandenen ASP.NET Core-Projekt hinzufügen, das auf .NET Framework abzielt. Es gibt für ASP.NET Core-Projekte, die auf .NET Core abzielen, zwei Möglichkeiten, Docker-Unterstützung über Tools hinzuzufügen. Öffnen Sie das Projekt in Visual Studio, und wählen Sie eine der folgenden Optionen aus:

* Wählen Sie aus dem **Projektmenü** **Docker-Unterstützung** aus.
* Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Docker-Unterstützung** aus.

## <a name="docker-assets-overview"></a>Übersicht über Docker-Objekte

Die Visual Studio-Tools für Docker fügen der Lösung ein *docker-compose*-Projekt mit den folgenden Dateien hinzu:

* *.dockerignore*: Enthält eine Liste mit Mustern für Dateien und Verzeichnisse, die beim Generieren eines Buildkontexts ausgeschlossen werden sollen.
* *docker-compose.yml*: Die [Docker Compose](https://docs.docker.com/compose/overview/)-Basisdatei zum Definieren der Sammlung von Images, die mit `docker-compose build` und `docker-compose run` erstellt bzw. ausgeführt werden soll.
* *docker-compose.override.yml*: Eine optionale Datei, die von Docker Compose gelesen wird und Konfigurationsüberschreibungen für Dienste enthält. Visual Studio führt `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` aus, um die Dateien zusammenzuführen.

Eine *Dockerfile*-Datei, der wichtigste Bestandteil beim Erstellen eines endgültigen Docker-Images, wird dem Projektstamm hinzugefügt. Verweisen Sie auf einen [Dockerfile-Verweis](https://docs.docker.com/engine/reference/builder/), damit Sie einen Überblick über die darin enthaltenen Befehle erlangen. Diese spezielle *Dockerfile*-Datei verwendet einen [mehrstufigen Build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/), der vier unterschiedlich benannte Buildschritte enthält:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

Die *Dockerfile*-Datei basiert auf dem [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore)-Image. Dieses Image enthält die ASP.NET Core NuGet-Pakete, die zur Verbesserung der Leistung beim Starten vorab mit JIT kompiliert wurden.

Die Datei *docker-compose.yml* enthält den Namen des Images, das beim Ausführen des Projekts erstellt wird:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

In dem zuvor genannten Beispiel generiert `image: hellodockertools` das Image `hellodockertools:dev`, wenn die App im Modus **Debuggen** ausgeführt wird. Das `hellodockertools:latest`-Image wird generiert, wenn die App im Modus **Release** ausgeführt wird.

Stellen Sie dem Imagenamen den Benutzernamen für [Docker Hub](https://hub.docker.com/) voran (z.B. `dockerhubusername/hellodockertools`), wenn das Image per Push in die Registrierung übertragen werden soll. Stattdessen können Sie auch den Imagenamen ändern, sodass er die private Registrierungs-URL, die von der Konfiguration abhängig ist (z.B. `privateregistry.domain.com/hellodockertools`) enthält.

## <a name="debug"></a>Debug

Wählen Sie in der Symbolleiste im Dropdownmenü „Debuggen“ die Option **Docker** aus, und starten Sie das Debuggen der Anwendung. In der Ansicht **Docker** im Fenster **Ausgabe** werden die folgenden Aktionen angezeigt:

* Das Runtime-Image *microsoft/aspnetcore* wird geladen (falls es sich nicht bereits im Cache befindet).
* Das *microsoft/aspnetcore-build* compile/publish image wird geladen (falls es sich nicht bereits im Cache befindet).
* Die Umgebungsvariable *ASPNETCORE_ENVIRONMENT* wird im Container auf `Development` festgelegt.
* Port 80 wird verfügbar gemacht und einem dynamisch zugewiesenen Port für Localhost zugewiesen. Der Port wird vom Docker-Host bestimmt und kann mit dem Befehl `docker ps` abgefragt werden.
* Die App wird in den Container kopiert.
* Der Standardbrowser wird mit dem dynamisch zugewiesenen Port gestartet, wobei der Debugger an den Container angehängt wird.

Das resultierende Docker-Image ist das *dev*-Image der Anwendung mit den *microsoft/aspnetcore*-Images als Basisimage. Führen Sie im Fenster **Paket-Manager-Konsole** den Befehl `docker images` aus. Die Images auf dem Computer werden angezeigt:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Das dev-Image enthält keine App-Inhalte, da **Debugkonfigurationen** mittels Volumebereitstellung für die iterative Funktionalität sorgen. Verwenden Sie zum Verschieben eines Images die Konfiguration **Release**.

Führen Sie in der Paket-Manager-Konsole den Befehl `docker ps` aus. Beachten Sie, dass die App mithilfe des Containers ausgeführt wird:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Bearbeiten und Fortfahren

Änderungen an statischen Dateien und Razor-Ansichten werden automatisch aktualisiert, wobei kein Kompilierungsschritt erforderlich ist. Nehmen Sie die Änderung vor, speichern Sie die Datei, und aktualisieren Sie die Browseransicht, um die Aktualisierung anzuzeigen.

Änderungen an Codedateien erfordern eine Kompilierung und einen Neustart von Kestrel im Container. Nachdem Sie die Änderung vorgenommen haben, drücken Sie `CTRL+F5`, um den Prozess durchzuführen und die App im Container zu starten. Der Docker-Container wird nicht erneut erstellt oder angehalten. Führen Sie in der Paket-Manager-Konsole den Befehl `docker ps` aus. Beachten Sie, dass der ursprüngliche Container immer noch genau so ausgeführt wird wie vor zehn Minuten:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Veröffentlichen von Docker-Images

Nachdem der Entwicklungs- und Debugzyklus für die App abgeschlossen wurde, kann mit den Visual Studio-Tools für Docker das Produktionsimage der App erstellt werden. Wählen Sie im Dropdownmenü „Konfiguration“ die Option **Release** aus, und erstellen Sie die App. Das Tool erstellt das Image mit dem Tag *latest*, das mittels Push an die private Registrierung oder den Docker-Hub übertragen werden kann.

Mit dem Befehl `docker images` können Sie die Liste der Images in der Paket-Manager-Konsole anzeigen:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Der Befehl `docker images` gibt Vermittlerimages mit Repositorynamen und -tags zurück, die als *\<none>* gekennzeichnet sind (obenstehend nicht aufgeführt). Diese unbenannten Images werden von der [mehrstufig erstellten](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*-Datei erstellt. Sie verbessern die Effizienz der Erstellung des endgültigen Images. D.h., nur die notwendigen Ebenen werden erneut erstellt, wenn Änderungen auftreten. Wenn die Vermittlerimages nicht mehr benötigt werden, löschen Sie sie über den Befehl [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/).

Möglicherweise wird angenommen, dass das Produktions- oder Releaseimage im Vergleich zum *dev*-Image kleiner ist. Aufgrund der Volumezuordnung wurden der Debugger und die App über den lokalen Computer und nicht innerhalb des Containers ausgeführt. Im *neusten* Image wurde der benötigte App-Code gepackt, um die App auf einem Hostcomputer auszuführen. Aus diesem Grund ist das Delta die Größe des App-Codes.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Problembehandlung bei der Entwicklung von Visual Studio 2017 mit Docker](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Visual Studio-Tools für Docker – GitHub-Repository](https://github.com/Microsoft/DockerTools)
