---
title: Verwenden Sie die Angular-Projektvorlage, mit ASP.NET Core
author: SteveSandersonMS
description: Informationen Sie zum Einstieg in die Projektvorlage für ASP.NET Core einzelnen Seite Anwendung (SPA) für Angular und Angular CLI.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: e3956bedbc243578f6dfdc09f5f043327de7c66b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Verwenden Sie die Angular-Projektvorlage, mit ASP.NET Core

> [!NOTE]
> Diese Dokumentation ist nicht zur Angular-Projektvorlage in ASP.NET Core 2.0 enthalten. Es geht die neuere Angular-Vorlage, die auf die Sie manuell aktualisieren können. Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.

Die aktualisierte Angular-Projektvorlage stellt einen nützlichen Startpunkt für ASP.NET Core-apps mit Angular und Angular CLI eine reichhaltige, clientseitige Benutzeroberfläche (UI) zu implementieren.

Die Vorlage entspricht dem Erstellen eines ASP.NET Core-Projekts zu fungieren als ein API-Back-End- und ein Angular CLI-Projekt fungiert als eine Benutzeroberfläche zur Verfügung. Die Vorlage bietet die Vorteile des Hostens beide Projekttypen in einem einzelnen app-Projekt. Folglich kann das app-Projekt erstellt und als einzelne Einheit veröffentlicht werden.

## <a name="create-a-new-app"></a>Eine neue app erstellen

Wenn ASP.NET Core 2.0 verwenden, stellen Sie sicher, Sie haben [installiert die aktualisierte Angular-Projektvorlage](xref:spa/index#installation). Wenn Sie ASP.NET Core 2.1 installiert ist, besteht keine Notwendigkeit, es zu installieren.

Erstellen eines neuen Projekts von einer Eingabeaufforderung mit dem Befehl `dotnet new angular` in ein leeres Verzeichnis. Die folgenden Befehle erstellen z. B. die app in eine *Meine-neuen-app* Verzeichnis und wechseln Sie in diesem Verzeichnis:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Führen Sie die app aus Visual Studio oder .NET Core CLI:

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
Öffnen Sie die generierte *csproj* Datei, und führen Sie die app als normale von dort aus.

Während des Erstellungsprozesses stellt Npm Abhängigkeiten bei der ersten Ausführung, die dies einige Minuten dauern kann, wieder her. Nachfolgende Builds sind wesentlich schneller.

#### <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli/)
Sicherzustellen, dass Sie über eine Umgebungsvariable namens `ASPNETCORE_Environment` mit einem Wert von `Development`. Führen Sie für Windows (in nicht-PowerShell-Anweisungen), `SET ASPNETCORE_Environment=Development`. Führen Sie unter Linux oder MacOS `export ASPNETCORE_Environment=Development`.

Führen Sie [Dotnet Build](/dotnet/core/tools/dotnet-build) um zu überprüfen, ob die Anwendung richtig erstellt. Bei der ersten Ausführung stellt während des Erstellungsprozesses Npm Abhängigkeiten, die dies einige Minuten dauern kann, wieder her. Nachfolgende Builds sind wesentlich schneller.

Führen Sie [Dotnet ausführen](/dotnet/core/tools/dotnet-run) für den Anwendungsstart. Es wird eine Meldung ähnlich der folgenden protokolliert:

```console
Now listening on: http://localhost:<port>
```

Navigieren Sie zu dieser URL in einem Browser.

Die app startet eine Instanz des Servers Angular CLI im Hintergrund. Es wird eine Meldung ähnlich der folgenden protokolliert: <em>NG Live Development Server Lauscht auf "localhost":&lt;Otherport&gt;, öffnen Sie in Ihrem Browser auf http://localhost: &lt;Otherport&gt; /</em> . Diese Meldung ignorieren&mdash;kann <strong>nicht</strong> die URL für die kombinierte ASP.NET Core und Angular-CLI-app.

* * *
Die Projektvorlage erstellt eine ASP.NET Core-app und einer Angular-app. Die app ASP.NET Core soll für den Datenzugriff, Autorisierung und Bedenken serverseitige verwendet werden. Die Angular-app in den die *ClientApp* Unterverzeichnis für alle Aspekte der Benutzeroberfläche verwendet werden soll.

## <a name="add-pages-images-styles-modules-etc"></a>Fügen Sie Seiten, Bilder, Stile, Module, usw. an.

Die *ClientApp* Verzeichnis enthält eine standardmäßige Angular-CLI-app. Finden Sie unter den offiziellen [Angular Dokumentation](https://github.com/angular/angular-cli/wiki) für Weitere Informationen.

Es gibt jedoch geringfügige Unterschiede zwischen der Angular app, die anhand dieser Vorlage erstellt und von Angular CLI selbst erstellt (über `ng new`), aber die app-Funktionen unterscheiden sich nicht. Die app, die von der Vorlage erstellten enthält eine [Bootstrap](https://getbootstrap.com/)-basierten Layout und ein einfaches routing-Beispiel.

## <a name="run-ng-commands"></a>Führen Sie Befehle ng

In einem Eingabeaufforderungsfenster, wechseln Sie zu der *ClientApp* Unterverzeichnis:

```console
cd ClientApp
```

Wenn Sie haben die `ng` Tool Global installiert, kann ausgeführt, seine Befehle. Sie können z. B. ausführen `ng lint`, `ng test`, oder eine beliebige andere [Angular-CLI-Befehlen](https://github.com/angular/angular-cli/wiki#additional-commands). Besteht keine Notwendigkeit zur Ausführung `ng serve` , da Ihre app ASP.NET Core befasst sich mit serverseitigen und clientseitigen Teile Ihrer app bedient. Er verwendet intern `ng serve` in der Entwicklung.

Wenn Sie haben die `ng` Tool installiert haben, führen Sie `npm run ng` stattdessen. Sie können z. B. ausführen `npm run ng lint` oder `npm run ng test`.

## <a name="install-npm-packages"></a>NPM-Pakete installieren

Verwenden Sie zum Installieren eines Drittanbieters Npm-Pakete in eine Eingabeaufforderung die *ClientApp* Unterverzeichnis. Zum Beispiel:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Veröffentlichen und Bereitstellen

Führt die app in einem Modus der Einfachheit halber für Entwickler optimiert, bei der Entwicklung. Beispielsweise nehmen JavaScript-Bundle Quelle Zuordnungen (, damit beim Debuggen Sie den ursprünglichen TypeScript-Code sehen können). Die app wird überwacht, ob Änderungen von TypeScript, HTML und CSS-Datei auf dem Datenträger und automatisch erneut kompiliert und lädt, wenn er erkennt, dass diese Dateien zu ändern.

Dienen Sie in der Produktion eine Version der app, die für Leistung optimiert wird. Dies ist für konfiguriert automatisch durchgeführt. Beim Veröffentlichen die Buildkonfiguration eine verkleinerte ausgibt, ahead angegeben (AoT) kompiliert Build des Codes die clientseitige. Im Gegensatz zu den Development Build erfordert die Produktions-Build Node.js auf dem Server installiert werden (es sei denn, Sie aktiviert haben [serverseitige Prerendering](#server-side-rendering)).

Sie können die Standard [ASP.NET Core Hosting- und Bereitstellung Methoden](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>"Ng Serve" unabhängig ausgeführt

Das Projekt konfiguriert wurde, um eine eigene Instanz des Servers Angular CLI im Hintergrund starten, die ASP.NET Core-app in den Entwicklungsmodus gestartet wird. Dies ist nützlich, da Sie nicht auf einen separaten Server manuell ausführen.

Es ist ein Nachteil dieses Standard-Setup aus. Jedes Mal, die Sie ändern den C#-Code und Ihrer ASP.NET Core app neu gestartet werden muss, wird der Angular-CLI-Server neu. Ca. 10 Sekunden ist erforderlich, um die Sicherung starten. Wenn Sie häufige C#-Code bearbeitet machen und, dass nicht gewartet Angular CLI möchte, neu zu starten, führen Sie das Angular CLI extern, unabhängig von der ASP.NET Core-Prozess. Dazu:

1. In einem Eingabeaufforderungsfenster, wechseln Sie zu der *ClientApp* Unterverzeichnis, und starten Sie den Angular CLI Development Server:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Verwendung `npm start` auf den Entwicklungsserver Angular CLI nicht starten `ng serve`, damit die Konfiguration in *"Package.JSON"* gewahrt wird. Um zusätzliche Parameter an den Angular-CLI-Server zu übergeben, fügen Sie sie für das entsprechende `scripts` Zeile in der *"Package.JSON"* Datei.

2. Ändern Sie Ihre app ASP.NET Core, um externe Angular-CLI-Instanz zu verwenden, anstatt eine eigene starten. In Ihrer *Start* -Klasse, ersetzen Sie die `spa.UseAngularCliServer` Aufruf durch Folgendes:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Wenn Sie Ihre app ASP.NET Core starten, wird nicht es einen Angular-CLI-Server zu starten. Die Instanz, die Sie manuell gestartet werden, wird stattdessen verwendet. Dadurch können sie zum Starten und Neustarten schneller. Nicht mehr wartet Angular CLI, um Ihre Client-app erstellen Sie jedes Mal neu auf.

## <a name="server-side-rendering"></a>Serverseitiges rendering

Sie können vorab Rendern die Angular-app auf dem Server, auf dem Client ausführen, als eine Funktion die Leistung. Dies bedeutet, dass Browser HTML-Markup erhalten, anfängliche Ihrer app-Benutzeroberfläche darstellt, sodass sie auch vor dem Herunterladen und Ausführen der JavaScript-Pakete anzeigen. Ein Großteil der Implementierung dieses stammt eine Angular-Funktion wird aufgerufen, [Angular Universal](https://universal.angular.io/).

> [!TIP]
> Aktivieren von serverseitigem Rendering enthält (SSR) einige zusätzliche Komplikationen sowohl während der Entwicklung und Bereitstellung. Lesen [Nachteile SSR](#drawbacks-of-ssr) zu bestimmen, ob SSR für Ihre Anforderungen geeignet ist.

Um SSR zu aktivieren, müssen Sie eine Anzahl von Ergänzungen für das Projekt zu machen.

In der *Start* -Klasse, *nach* die Zeile, die konfiguriert `spa.Options.SourcePath`, und *vor* der Aufruf von `UseAngularCliServer` oder `UseProxyToSpaDevelopmentServer`, fügen Sie die folgenden:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

In den Entwicklungsmodus, dieser Code versucht, auf das SSR-Bundle erstellen, durch Ausführen des Skripts `build:ssr`, definiert *ClientApp\package.json*. Dies erstellt eine Angular app namens `ssr`, ist nicht die noch definiert. 

Am Ende der `apps` array *ClientApp/.angular-cli.json*, definieren Sie eine zusätzliche app mit dem Namen `ssr`. Verwenden Sie die folgenden Optionen:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Diese neue SSR-fähigen app-Konfiguration erfordert zwei weitere Dateien: *tsconfig.server.json* und *main.server.ts*. Die *tsconfig.server.json* -Datei gibt die Optionen für TypeScript-Kompilierung. Die *main.server.ts* Datei dient als Codeeinstiegspunkt während der SSR.

Fügen Sie eine neue Datei namens *tsconfig.server.json* in *ClientApp/Src* (zusammen mit den vorhandenen *tsconfig.app.json*), enthält die folgenden:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Diese Datei wird konfiguriert, des Angular AoT Compiler für ein Modul mit dem Namen gesucht werden soll `app.server.module`. Fügen Sie dies durch Erstellen einer neuen Datei am *ClientApp/src/app/app.server.module.ts* (zusammen mit den vorhandenen *app.module.ts*), die Folgendes enthält: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Dieses Modul erbt von der Clientseite `app.module` und definiert, welche zusätzlichen Angular-Module, die während der SSR verfügbar sind.

Bedenken Sie, dass die neue `ssr` Eintrag im *.angular cli.json* auf die verwiesen wird einer Eintrag Punktdatei mit dem Namen *main.server.ts*. Sie wurden noch nicht die Datei hinzugefügt und ist jetzt tun. Erstellen Sie eine neue Datei am *ClientApp/src/main.server.ts* (zusammen mit den vorhandenen *main.ts*), enthält die folgenden:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Dieser Datei Code wird ASP.NET Core für jede Anforderung bei der Ausführung der `UseSpaPrerendering` -Middleware, die Sie hinzugefügt haben die *Start* Klasse. Er behandelt die empfangenden `params` aus der .NET Code (z. B. der URL angefordert wird) und das Aufrufen Angular SSR-APIs zum Abrufen der resultierenden HTML. 

Streng-sprechen, ist dies ausreichend, um in den Entwicklungsmodus SSR aktivieren. Es ist entscheidend dafür, dass eine endgültige Änderung, damit Ihre app ordnungsgemäß funktioniert, wenn veröffentlicht. In Ihrer app Main *csproj* Datei, legen Sie die `BuildServerSideRenderer` Eigenschaftswert an `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Wird konfiguriert, führen Sie den Buildprozess `build:ssr` während der Veröffentlichung und die SSR-Dateien auf dem Server bereitstellen. Wenn Sie dies nicht aktivieren, SSR in Produktion schlägt fehl.

Wenn Ihre app-Entwicklung oder Produktion-Modus ausgeführt wird, rendert vorab Angular Code als HTML-Code auf dem Server. Der clientseitige Code wird normal ausgeführt.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Übergeben von Daten aus .NET Code in TypeScript-code

Während der SSR empfiehlt es sich Ihre app Angular pro Anforderung Daten aus Ihrer app ASP.NET Core übergeben. Beispielsweise könnten Sie Cookieinformationen übergeben oder etwas aus einer Datenbank lesen. Bearbeiten Sie hierzu die *Start* Klasse. Der Rückruf für `UseSpaPrerendering`, legen Sie einen Wert für `options.SupplyData` z. B. Folgendes:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

Die `SupplyData` Rückruf können Sie beliebige übergeben, die pro Anforderung, die JSON-serialisierbare Daten (z. B. Zeichenfolgen, boolesche Werte oder Zahlen). Ihre *main.server.ts* Code erhält als `params.data`. Im vorangehenden Codebeispiel übergibt z. B. einen booleschen Wert als `params.data.isHttpsRequest` in die `createServerRenderer` Rückruf. Sie können dies auf andere Teile Ihrer App in irgendeiner Weise durch Angular unterstützt übergeben. Beispielsweise finden Sie unter wie *main.server.ts* übergibt die `BASE_URL` Wert, der eine beliebige Komponente, deren Konstruktor für den Empfang deklariert ist.

### <a name="drawbacks-of-ssr"></a>Nachteile der SSR

Nicht alle apps, profitieren von SSR ab. Der Hauptvorteil ist wahrgenommen. Besucher erreichen Ihrer app über eine langsame Netzwerkverbindung oder auf langsamen mobilen Geräten finden Sie unter der anfänglichen Benutzeroberfläche schnell, auch wenn es eine Weile zum Abrufen oder Analysieren der JavaScript-Bundles dauert. Allerdings sind viele SPAs hauptsächlich über schnelle, interne Unternehmensnetzwerken auf schnelle Computern angezeigt, in dem die app nahezu sofort.

Gleichzeitig gibt es erhebliche Nachteile SSR aktivieren. Es erhöht die Komplexität des Entwicklungsprozesses. Der Code muss in zwei verschiedene Umgebungen ausgeführt: clientseitige und serverseitige (in eine Node.js-Umgebung aus ASP.NET Core aufgerufen wurde). Hier sind einige Punkte zu berücksichtigen:

* SSR erfordert eine Node.js-Installation auf Produktionsservern. Dies wird automatisch die Groß-/Kleinschreibung für einige Szenarien, z. B. Azure-App-Dienste, aber nicht für andere, wie Azure Service Fabric.
* Aktivieren der `BuildServerSideRenderer` Flag Ursachen Erstellen Ihrer *Node_modules* Directory zu veröffentlichen. Dieser Ordner enthält 20.000 Dateien, die Bereitstellungszeit erhöht.
* Zum Ausführen von Code in einer Umgebung mit Node.js es nicht verlässlich das Vorhandensein des Browser-spezifische JavaScript-APIs wie z. B. `window` oder `localStorage`. Wenn Ihr Code (oder eine Bibliothek eines Drittanbieters verwenden möchten, auf die Sie verweisen) versucht, die diese APIs verwenden, erhalten Sie einen Fehler während der SSR. Beispielsweise verwenden Sie jQuery nicht mit ein, da sie browserspezifischen APIs an vielen Stellen verweist. Um Fehler zu vermeiden, müssen Sie SSR vermeiden oder Browser-spezifische APIs oder Bibliotheken vermeiden. Sie können alle Aufrufe dieser APIs in Überprüfungen, um sicherzustellen, dass sie während der SSR aufgerufen werden nicht einschließen. Verwenden Sie z. B. eine Überprüfung wie im folgenden in JavaScript oder TypeScript-Code:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
