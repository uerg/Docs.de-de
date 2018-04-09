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
# <a name="use-the-react-project-template-with-aspnet-core"></a>Verwenden Sie die Projektvorlage reagieren mit ASP.NET Core

> [!NOTE]
> Diese Dokumentation ist nicht zur reagieren-Projektvorlage in ASP.NET Core 2.0 enthalten. Es geht die neuere reagieren-Vorlage, die Sie manuell aktualisieren können. Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.

Die aktualisierte reagieren-Projektvorlage stellt einen nützlichen Startpunkt für ASP.NET Core-apps mit reagieren und [erstellen-reagieren-app](https://github.com/facebookincubator/create-react-app) Konventionen (CRA), um eine umfassende, clientseitige Benutzeroberfläche (UI) zu implementieren.

Die Vorlage ist äquivalent zu erstellen, sowohl ein ASP.NET Core-Projekt fungiert als ein API-Back-End- und ein standard-CRA reagieren Projekt fungieren, wie eine Benutzeroberfläche, jedoch mit den Komfort Hosten in einer einzelnen app-Projekt, das erstellt und als einzelne Einheit veröffentlicht werden kann.

## <a name="create-a-new-app"></a>Eine neue app erstellen

Wenn ASP.NET Core 2.0 verwenden, stellen Sie sicher, Sie haben [installiert die aktualisierte reagieren-Projektvorlage](xref:spa/index#installation). Wenn Sie ASP.NET Core 2.1 installiert ist, besteht keine Notwendigkeit, es zu installieren.

Erstellen eines neuen Projekts von einer Eingabeaufforderung mit dem Befehl `dotnet new react` in ein leeres Verzeichnis. Die folgenden Befehle erstellen z. B. die app in eine *Meine-neuen-app* Verzeichnis und wechseln Sie in diesem Verzeichnis:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Führen Sie die app aus Visual Studio oder .NET Core CLI:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Öffnen Sie die generierte *csproj* Datei, und führen Sie die app als normale von dort aus.

Während des Erstellungsprozesses stellt Npm Abhängigkeiten bei der ersten Ausführung, die dies einige Minuten dauern kann, wieder her. Nachfolgende Builds sind wesentlich schneller.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Sicherzustellen, dass Sie über eine Umgebungsvariable namens `ASPNETCORE_Environment` mit dem Wert des `Development`. Führen Sie für Windows (in nicht-PowerShell-Anweisungen), `SET ASPNETCORE_Environment=Development`. Führen Sie unter Linux oder MacOS `export ASPNETCORE_Environment=Development`.

Führen Sie [Dotnet Build](/dotnet/core/tools/dotnet-build) um zu überprüfen, ob die app ordnungsgemäß erstellt. Bei der ersten Ausführung stellt während des Erstellungsprozesses Npm Abhängigkeiten, die dies einige Minuten dauern kann, wieder her. Nachfolgende Builds sind wesentlich schneller.

Führen Sie [Dotnet ausführen](/dotnet/core/tools/dotnet-run) für den Anwendungsstart.

---

Die Projektvorlage erstellt eine ASP.NET Core-app und einer app reagieren. Die app ASP.NET Core soll für den Datenzugriff, Autorisierung und Bedenken serverseitige verwendet werden. Die reagieren-app, die in den die *ClientApp* Unterverzeichnis für alle Aspekte der Benutzeroberfläche verwendet werden soll.

## <a name="add-pages-images-styles-modules-etc"></a>Fügen Sie Seiten, Bilder, Stile, Module, usw. an.

Die *ClientApp* Verzeichnis ist eine standard-app, der auf der CRA zu reagieren. Finden Sie unter den offiziellen [CRA Dokumentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) für Weitere Informationen.

Es gibt jedoch geringfügige Unterschiede zwischen der reagieren-app, die anhand dieser Vorlage erstellt und von CRA selbst erstellt; die app-Funktionen sind jedoch unverändert. Die app, die von der Vorlage erstellten enthält eine [Bootstrap](https://getbootstrap.com/)-basierten Layout und ein einfaches routing-Beispiel.

## <a name="install-npm-packages"></a>NPM-Pakete installieren

Verwenden Sie zum Installieren eines Drittanbieters Npm-Pakete in eine Eingabeaufforderung die *ClientApp* Unterverzeichnis. Zum Beispiel:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Veröffentlichen und Bereitstellen

Führt die app in einem Modus der Einfachheit halber für Entwickler optimiert, bei der Entwicklung. JavaScript-Bundle enthalten z. B. Quelle Maps, (, damit beim Debuggen Sie den ursprünglichen Quellcode ersichtlich). Die app wird überwacht, ob JavaScript, HTML und CSS-Datei von Änderungen auf dem Datenträger und automatisch erneut kompiliert und lädt, wenn er erkennt, dass diese Dateien zu ändern.

Dienen Sie in der Produktion eine Version der app, die für Leistung optimiert wird. Dies ist für konfiguriert automatisch durchgeführt. Beim Veröffentlichen, gibt die Buildkonfiguration eine verkleinerte, Transpiled Build des Codes die clientseitige aus. Im Gegensatz zu den Development Build erfordert die Produktions-Build Node.js auf dem Server installiert werden.

Sie können die Standard [ASP.NET Core Hosting- und Bereitstellung Methoden](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Führen Sie das CRA unabhängig

Das Projekt konfiguriert wurde, um eine eigene Instanz von den CRA Development Server im Hintergrund starten, die ASP.NET Core-app in den Entwicklungsmodus gestartet wird. Dies ist nützlich, da es bedeutet, dass Sie keinen separaten Server manuell ausführen müssen.

Es ist ein Nachteil dieses Standard-Setup aus. Jedes Mal, die Sie ändern den C#-Code und Ihrer ASP.NET Core app neu gestartet werden muss, wird der CRA-Server neu. Ein paar Sekunden sind erforderlich, zu sichern. Wenn Sie häufige C#-Code bearbeitet machen und nicht für einen Neustart des CRA Servers warten möchten, führen Sie das CRA extern, unabhängig von der ASP.NET Core-Prozess. Dazu:

1. In einem Eingabeaufforderungsfenster, wechseln Sie zu der *ClientApp* Unterverzeichnis, und starten Sie den CRA Development Server:

    ```console
    cd ClientApp
    npm start
    ```

2. Ändern Sie die ASP.NET Core Anwendung die externen CRA Server-Instanz verwenden, anstatt eine eigene starten. In Ihrer *Start* -Klasse, ersetzen Sie die `spa.UseReactDevelopmentServer` Aufruf durch Folgendes:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Wenn Sie Ihre app ASP.NET Core starten, wird nicht es einen CRA-Server zu starten. Die Instanz, die Sie manuell gestartet werden, wird stattdessen verwendet. Dadurch können sie zum Starten und Neustarten schneller. Er wartet nicht mehr für Ihre app reagieren, um jedes Mal neu zu erstellen.
