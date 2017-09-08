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
ms.openlocfilehash: 3a7cd83becc570d2f73014b356977fedb16f29bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Einführung in das Erstellen von Projekten mit Yeoman in ASP.NET Core

[Yeoman](http://yeoman.io/) ist ein Gerüst Projektsystem für verschiedene Anwendungen zu erstellen. Die Yeoman-Generator für ASP.NET Core enthält eine Reihe von Projektvorlagen für das Starten einer neuen Web, MVC oder Konsolenanwendung.

## <a name="install-nodejs-npm-and-yeoman"></a>Installieren Sie Yeoman, Node.js und npm

### <a name="prerequisites"></a>Erforderliche Komponenten

Node.js und Npm sind für Yeoman erforderlich. Herunterladen von [Node.js](https://nodejs.org/en/). Der Installer umfasst [Node.js](https://nodejs.org/en/) und [Npm](https://www.npmjs.com/). Bower ist auch für die Installation von Benutzeroberflächen-Frameworks wie Bootstrap erforderlich.

Führen Sie zum Installieren von Yeoman oder Bower den folgenden Befehl ein:

```console
npm install -g yo bower
```

>[!Note]
>Wenn Sie die Fehlermeldung erhalten, `npm ERR! Please try running this command again as root/Administrator.` auf MacOS, führen Sie den folgenden Befehl mit ["sudo"](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Installieren Sie ASP.NET-Generator über eine Eingabeaufforderung:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Wenn Sie ein Berechtigungsfehler erhalten, führen Sie den Befehl unter `sudo` wie oben beschrieben.

Die `–g` Flag wird den Generator Global installiert, sodass sie über einen Pfad verwendet werden kann.

## <a name="create-an-aspnet-app"></a>Erstellen einer ASP.NET-Anwendung

Führen Sie den Generator Yeoman basierende ASP.NET:

```console
yo aspnet
```

Der Generator wird ein Menü angezeigt. Pfeil nach unten, um die **Web Application Basic [ohne Mitgliedschaft und Autorisierung]** Projekt, und tippen Sie auf **EINGABETASTE**:

![Befehlsfenster: welche Art von Anwendung möchten Sie erstellen? Im Menü von Anwendungstypen](yeoman/_static/yeoman-yo-aspnet.png)

Wählen Sie Bootstrap als Benutzeroberflächen-Framework, und tippen Sie auf **EINGABETASTE**.

Mit "**MyWebApp**" für den Anwendungsnamen und tippen Sie dann auf **EINGABETASTE**.

Yeoman wird das Gerüst für das Projekt und die unterstützenden Dateien erstellen. Außerdem werden die empfohlenen ersten Schritte in Form von Befehle bereitgestellt.

![Befehlsfenster: Was ist, dass der Name Ihrer ASP.NET-Anwendung? Eingabeaufforderung](yeoman/_static/yeoman-yo-aspnet-created.png)

Die [ASP.NET Generator](https://www.npmjs.com/package/generator-aspnet) erstellt ASP.NET Core-Projekte, die in Visual Studio-Code, Visual Studio geladen werden oder über die Befehlszeile ausführen können.

## <a name="restore-build-and-run"></a>Stellen Sie wieder her, erstellen und ausführen

Führen Sie die vorgeschlagenen Befehle durch Ändern von Verzeichnissen auf dem `MyWebApp` Verzeichnis. Führen Sie `dotnet restore`.

![Befehlsfenster](yeoman/_static/dotnet-restore.png)

Erstellen und Ausführen der Anwendung mit `dotnet build` und `dotnet run`:

![Befehlsfenster](yeoman/_static/dotnet-build-run.png)

An diesem Punkt können Sie mit der URL angezeigt wird, testen Sie die neu erstellte ASP.NET Core app navigieren.

## <a name="client-side-packages"></a>Die clientseitige Pakete

Die Front-End-Ressourcen werden von den Vorlagen bereitgestellt, von der Yeoman Generator mithilfe der [Bower](xref:client-side/bower) clientseitige Paketmanager hinzufügen *"bower.JSON"* und *".bowerrc"* Dateien, die clientseitige Pakete mithilfe von Bower wiederherzustellen.

Die [BundlerMinifier](xref:client-side/bundling-and-minification) Komponente gehört auch standardmäßig für die erleichterte Verkettung (Bündelung) und Minimierung von CSS, JavaScript und HTML.

## <a name="building-and-running-from-visual-studio"></a>Erstellen und Ausführen von Visual Studio

Laden das generierte Webprojekt von ASP.NET Core direkt in Visual Studio, und klicken Sie dann erstellen, und führen das Projekt von dort aus. Führen Sie die obigen Anweisungen zum Erstellen einer neuen ASP.NET Core-app mithilfe von Yeoman aus. Wählen Sie dieses Mal **Webanwendung** aus dem Menü und die Namen der app `MyWebApp`.

Öffnen Sie Visual Studio. Wählen Sie im Menü Datei öffnen ‣ Projekt/Projektmappe ein.

Navigieren Sie zu, klicken Sie im Dialogfeld "Projekt öffnen" die *csproj* Datei, wählen Sie ihn, und klicken Sie auf die **öffnen** Schaltfläche. Das Projekt sollte etwa der folgende Screenshot aussehen, im Projektmappen-Explorer.

![Dateien und Ordner eines neuen Projekts im Projektmappen-Explorer](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds einer MVC-Webanwendung, sowohl vollständige Server und die clientseitige build-Unterstützung. Server-Side-Abhängigkeiten sind aufgeführt, unter der **Abhängigkeiten/NuGet** Knoten und Client-Side-Abhängigkeiten in der **Abhängigkeiten/Bower** -Knoten des Projektmappen-Explorers. Abhängigkeiten werden automatisch wiederhergestellt, wenn das Projekt geladen wird.

![Unter dem Knoten "Abhängigkeiten" in der Strukturansicht des Projektmappen-Explorer für der Ordner Bower Auflisten der abhängigen Elemente geöffnet ist.](yeoman/_static/yeoman-loading-dependencies.png)

Wenn alle Abhängigkeiten wiederhergestellt werden, drücken Sie die **F5** um das Projekt auszuführen. Standard-Startseite im Browser angezeigt.

![Öffnen Sie in Microsoft Edge-Webanwendung](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Wiederherstellen, erstellen und Hosten der über die Befehlszeile

Sie können vorbereiten und Hosten der Webanwendung mithilfe der .NET Core-CLI.

Ändern Sie das aktuelle Verzeichnis an einer Eingabeaufforderung zum Ordner, der das Projekt (d. h. der Ordner mit der *csproj* Datei):

```console
cd src\MyWebApp
```

Wiederherstellen von Abhängigkeiten für das Projekt NuGet-Pakets:

```console
dotnet restore
```

Führen Sie die Anwendung:

```console
dotnet run
```

Die plattformübergreifenden [Kestrel](xref:fundamentals/servers/kestrel) Webserver Lauschen an Port 5000 beginnt.

Öffnen Sie einen Webbrowser, und navigieren Sie zu `http://localhost:5000`.

![Öffnen Sie in Microsoft Edge-Webanwendung](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Hinzufügen zu Ihrem Projekt mit Sub-Generatoren

Mit Yeoman [sub-Generatoren](https://www.github.com/omnisharp/generator-aspnet#sub-generators), können Sie entweder Hinzufügen einer `nuget.config` oder ein `web.config` nachdem das Projekt erstellt wird. Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die Datei erstellt werden soll:

```console
yo aspnet:nugetconfig
```

Das Ergebnis ist ein NuGet-Konfigurationsdatei mit dem Namen `nuget.config` mit folgendem Inhalt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Server (Kestrel und WebListener)](xref:fundamentals/servers/index)
* [Grundlagen](xref:fundamentals/index)
