---
title: "Visual Studio-Tools für Docker mit ASP.NET Core"
author: spboyer
description: "Informationen zum Verwenden der Visual Studio 2017-Tools und Docker für Windows, um Ihre ASP.NET Core-App in Container zu packen."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio-Tools für Docker mit ASP.NET Core

[Visual Studio-2017](https://www.visualstudio.com/) erstellen, Debuggen und Ausführen von apps, die .NET Core als Ziel dient für die Datenvolumes ASP.NET Core unterstützt. Sowohl Windows- als auch Linux-Container werden unterstützt.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Visual Studio 2017](https://www.visualstudio.com/) mit der Workload für die **plattformübergreifende .NET Core-Entwicklung**.
* [Docker für Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Installation und Einrichtung

Informationen zur Docker-Installation entnehmen Sie dem Artikel [Docker for Windows: What to know before you install (Docker für Windows: Was Sie vor der Installation wissen müssen)](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install), und installieren Sie [Docker für Windows](https://docs.docker.com/docker-for-windows/install/).

**[Freigegebene Laufwerke](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker für Windows müssen so konfiguriert sein, dass sie die Volumezuordnung und das Debuggen unterstützen. Mit der rechten Maustaste in der Taskleiste Docker-Symbol, wählen Sie **Einstellungen...** , und wählen Sie **freigegebene Laufwerke**. Wählen Sie das Laufwerk, auf dem Docker Dateien speichert. Wählen Sie **gelten**.

![Freigegebene Laufwerke](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 Versionen 15.6 und höher aufgefordert, wenn **freigegebene Laufwerke** nicht konfiguriert.

## <a name="add-docker-support-to-an-app"></a>Hinzufügen der Docker-Unterstützung zu einer App

Damit Unterstützung für Docker zu einem ASP.NET Core-Projekt hinzufügen können, muss das Projekt .NET Core als Ziel. Linux und Windows-Containern werden unterstützt.

Wenn Sie Docker-Unterstützung zu einem Projekt hinzufügen möchten, wählen Sie ein Windows- oder Linux-Container. Der Docker-Host muss den gleichen Containertyp ausführen. Wenn Sie den Containertyp in der ausgeführten Docker-Instanz ändern möchten, klicken Sie mit der rechten Maustaste auf der Taskleiste auf das Docker-Symbol, und wählen Sie **Switch to Windows containers** (Zu Windows-Containern wechseln) oder **Switch to Linux container** (Zu Linux-Containern wechseln) aus.

### <a name="new-app"></a>Neue App

Wenn Sie mithilfe der Projektvorlage **ASP.NET Core-Webanwendung** eine neue App erstellen, aktivieren Sie das Feld **Enable Docker Support** (Docker-Unterstützung aktivieren):

![Aktivieren des Felds „Docker-Unterstützung“](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Wenn das Zielframework .NET Core, ist die **OS** Dropdown-ermöglicht die Auswahl eines Containers.

### <a name="existing-app"></a>Vorhandene App

Mit den Visual Studio-Tools für Docker können Sie Docker nicht zu einem vorhandenen ASP.NET Core-Projekt hinzufügen, das auf .NET Framework abzielt. Es gibt für ASP.NET Core-Projekte, die auf .NET Core abzielen, zwei Möglichkeiten, Docker-Unterstützung über Tools hinzuzufügen. Öffnen Sie das Projekt in Visual Studio, und wählen Sie eine der folgenden Optionen aus:

* Wählen Sie aus dem **Projektmenü** **Docker-Unterstützung** aus.
* Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Docker-Unterstützung** aus.

## <a name="docker-assets-overview"></a>Übersicht über Docker-Objekte

Die Visual Studio-Tools für Docker fügen der Projektmappe ein *Docker Compose*-Projekt mit folgendem Inhalt hinzu:

* *.dockerignore*: Enthält eine Liste mit Mustern für Dateien und Verzeichnisse, die beim Generieren eines Buildkontexts ausgeschlossen werden sollen.
* *docker-compose.yml*: Die [Docker Compose](https://docs.docker.com/compose/overview/)-Basisdatei zum Definieren der Sammlung von Images, die mit `docker-compose build` und `docker-compose run` erstellt bzw. ausgeführt werden soll.
* *docker-compose.override.yml*: Eine optionale Datei, die von Docker Compose gelesen wird und Konfigurationsüberschreibungen für Dienste enthält. Visual Studio führt `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` aus, um die Dateien zusammenzuführen.

Eine *Dockerfile*-Datei, der wichtigste Bestandteil beim Erstellen eines endgültigen Docker-Images, wird dem Projektstamm hinzugefügt. Verweisen Sie auf einen [Dockerfile-Verweis](https://docs.docker.com/engine/reference/builder/), damit Sie einen Überblick über die darin enthaltenen Befehle erlangen. Diese spezielle *Dockerfile*-Datei verwendet einen [mehrstufigen Build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/), der vier unterschiedlich benannte Buildschritte enthält:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

Die *Dockerfile*-Datei basiert auf dem [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore)-Image. Dieses Image enthält die ASP.NET Core NuGet-Pakete, die zur Verbesserung der Leistung beim Starten vorab mit JIT kompiliert wurden.

Die *Docker compose.yml* Datei enthält den Namen des Bilds, das erstellt wird, wenn das Projekt ausgeführt wird:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

In dem zuvor genannten Beispiel generiert `image: hellodockertools` das Image `hellodockertools:dev`, wenn die App im Modus **Debuggen** ausgeführt wird. Das `hellodockertools:latest`-Image wird generiert, wenn die App im Modus **Release** ausgeführt wird.

Als Präfix den Namen des Abbilds mit der [Docker Hub](https://hub.docker.com/) Benutzernamen (z. B. `dockerhubusername/hellodockertools`), wenn das Bild wird der Registrierung vorgenommen werden. Ändern Sie alternativ den Namen des Abbilds auf die privaten Registrierungs-URL enthalten (z. B. `privateregistry.domain.com/hellodockertools`) abhängig von der Konfiguration.

## <a name="debug"></a>Debug

Wählen Sie in der Symbolleiste im Dropdownmenü „Debuggen“ die Option **Docker** aus, und starten Sie das Debuggen der Anwendung. In der Ansicht **Docker** im Fenster **Ausgabe** werden die folgenden Aktionen angezeigt:

* Die *Microsoft/Aspnetcore* Laufzeit-Image wird (wenn nicht bereits im Cache) abgerufen.
* Die *Microsoft/Aspnetcore-Build* kompilieren/veröffentlichen-Bild wird (wenn nicht bereits im Cache) abgerufen.
* Die Umgebungsvariable *ASPNETCORE_ENVIRONMENT* wird im Container auf `Development` festgelegt.
* PORT 80 wird verfügbar gemacht und einem dynamisch zugewiesenen Port für Localhost zugeordnet. Der Port wird vom Docker-Host bestimmt und kann mit dem Befehl `docker ps` abgefragt werden.
* Die app wird auf den Container kopiert.
* Default-Browsers wird mit dem auf den Container über den Port dynamisch zugewiesene angefügten Debugger gestartet. 

Das resultierende Image mit Docker wird der *Dev* Bild der app mit der *Microsoft/Aspnetcore* Bilder als Basis-Image. Führen Sie im Fenster **Paket-Manager-Konsole** den Befehl `docker images` aus. Die Bilder auf dem Computer werden angezeigt:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Das Bild Dev verfügt nicht über den Inhalt der app als **Debuggen** Konfigurationen verwenden Volume bereitstellen, um die iterative zu ermöglichen. Verwenden Sie zum Verschieben eines Images die Konfiguration **Release**.

Führen Sie in der Paket-Manager-Konsole den Befehl `docker ps` aus. Beachten Sie, dass die App mithilfe des Containers ausgeführt wird:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Bearbeiten und Fortfahren

Änderungen an statischen Dateien und Razor-Ansichten werden automatisch aktualisiert, wobei kein Kompilierungsschritt erforderlich ist. Nehmen Sie die Änderung vor, speichern Sie die Datei, und aktualisieren Sie die Browseransicht, um die Aktualisierung anzuzeigen.  

Wenn Sie Änderungen an Codedateien vornehmen, müssen Sie die Dateien kompilieren und Kestrel im Container neu starten. Nachdem Sie die Änderung vorgenommen haben, drücken Sie die Tastenkombination STRG+F5, um den Prozess durchzuführen und die App im Container zu starten. Der Docker-Container ist nicht neu erstellt oder beendet. Führen Sie in der Paket-Manager-Konsole den Befehl `docker ps` aus. Beachten Sie, dass der ursprüngliche Container immer noch genau so ausgeführt wird wie vor zehn Minuten:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Veröffentlichen von Docker-Images

Bei Abschluss der Zyklus entwickeln und Debuggen der app ist bei der Visual Studio-Tools für Docker der Produktions-Image der app zu erstellen. Wählen Sie im Dropdownmenü „Konfiguration“ die Option **Release** aus, und erstellen Sie die App. Die Tools erzeugt das Image mit der *neueste* Tags, das auf die privaten Registrierung oder Docker Hub übertragen werden kann. 

Mit dem Befehl `docker images` können Sie die Liste der Images in der Paket-Manager-Konsole anzeigen:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Der Befehl `docker images` gibt Vermittlerimages mit Repositorynamen und -tags zurück, die als *\<none>* gekennzeichnet sind (obenstehend nicht aufgeführt). Diese unbenannten Images werden von der [mehrstufig erstellten](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*-Datei erstellt. Sie verbessern die Effizienz der Erstellung des endgültigen Images. D.h., nur die notwendigen Ebenen werden erneut erstellt, wenn Änderungen auftreten. Wenn die zwischengeschaltete Bilder nicht mehr benötigt werden, löschen Sie sie mithilfe der [Docker Rmi](https://docs.docker.com/engine/reference/commandline/rmi/) Befehl.

Möglicherweise wird angenommen, dass das Produktions- oder Releaseimage im Vergleich zum *dev*-Image kleiner ist. Aufgrund der volumezuordnung zwischen wurden dem Debugger und der app auf dem lokalen Computer und nicht innerhalb des Containers ausgeführt. Im *neusten* Image wurde der benötigte App-Code gepackt, um die App auf einem Hostcomputer auszuführen. Aus diesem Grund ist die Differenz der Größe der app-Code.
