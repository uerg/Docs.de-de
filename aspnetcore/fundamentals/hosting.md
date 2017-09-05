---
title: Hosten in ASP.NET Core | Microsoft Docs
author: ardalis
description: "Einführung in Web-Hosts in ASP.NET Core."
keywords: ASP.NET Core, Webhost, IWebHost
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>Einführung zum Hosten der in ASP.NET Core

Durch [Steve Smith](http://ardalis.com)

Um eine ASP.NET Core-app auszuführen, müssen Sie zum Konfigurieren und starten Sie einen Host mit `WebHostBuilder`.

## <a name="what-is-a-host"></a>Was ist ein Host?

ASP.NET Core apps erfordern eine *Host* in den ausgeführt. Ein Host muss implementieren die `IWebHost` -Schnittstelle, die Sammlungen von Funktionen und Diensten verfügbar macht, und eine `Start` Methode. Der Host ist in der Regel erstellt eine Instanz von einem `WebHostBuilder`, die erstellt und zurückgibt eine `WebHost` Instanz. Die `WebHost` verweist auf den Server, die Anforderungen behandelt. Erfahren Sie mehr über [Server](servers/index.md).

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>Was ist der Unterschied zwischen einem Host und einem Server?

Der Host ist verantwortlich für die anwendungsverwaltung starten sowie seine Lebensdauer. Der Server ist verantwortlich für das HTTP-Anforderungen akzeptieren. Teil des Hosts Verantwortung umfasst das sicherstellen, dass die Anwendung Dienste und der Server verfügbar und ordnungsgemäß konfiguriert sind. Sie können den Host als Wrapper um den Server vorstellen. Der Host für die Verwendung ein bestimmtes Servers konfiguriert; der Server ist der Host nicht bewusst.

## <a name="setting-up-a-host"></a>Einrichten eines Hosts

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Erstellen Sie einen Host mit einer Instanz von `WebHostBuilder`. Dies erfolgt in der Regel in der app-Einstiegspunkt: `public static void Main` (in Projektvorlagen befindet sich eine *"Program.cs"* Datei). Eine typische *"Program.cs"*wie unten veranschaulicht, wie eine `WebHostBuilder` auf einen Host zu erstellen.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

Die `WebHostBuilder` ist für den Host erstellen, die den Server für die app Bootstrapping verantwortlich. `WebHostBuilder`erfordert, geben Sie einen Server, der implementiert `IServer` (`UseKestrel` im obigen Code). `UseKestrel`Gibt an, dass die Kestrel Server von der app verwendet wird.

Des Servers *Content Stamm* bestimmt, wo er für Inhaltsdateien wie MVC-Ansicht-Dateien sucht. Der standardmäßige Content-Stamm ist der Ordner, von dem die Anwendung ausgeführt wird.

> [!NOTE]
> Angeben von `Directory.GetCurrentDirectory` wie den Inhalt Stamm Stammordner für das Webprojekt als Inhalt der app-Stammverzeichnis, beim Starten der app aus diesem Ordner verwenden (z. B. durch Aufruf von `dotnet run` aus dem Projektordner Web). Dies ist die Standardeinstellung in Visual Studio verwendet und `dotnet new` Vorlagen.

Rufen Sie zum Verwenden von IIS als Reverseproxy `UseIISIntegration` im Rahmen der Erstellung des Hosts. 

Beachten Sie, dass `UseIISIntegration` nicht Konfigurieren einer *Server*, z. B. `UseKestrel` verfügt. Um IIS mit ASP.NET Core verwenden zu können, müssen Sie beide angeben `UseKestrel` und `UseIISIntegration`. `UseKestrel`erstellt den Webserver und hostet die app. `UseIISIntegration`überprüft Umgebungsvariablen, die von IIS/IIS Express verwendet und konfiguriert die Einstellungen wie z. B. den Port zu Lauschen und die Header verwendet wird.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Erstellen Sie einen Host mit einer Instanz von `WebHostBuilder`. Dies erfolgt in der Regel in der app-Einstiegspunkt: `public static void Main` (in Projektvorlagen befindet sich eine *"Program.cs"* Datei). Eine typische *"Program.cs"*, wie unten Aufrufe gezeigt `CreateDefaultbuilder` auf einen Host zu erstellen:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`erstellt eine Instanz des `WebHostBuilder` auf den Host zu erstellen, die den Server für die app gestartet wird. Der Host erfordert eine [Server, die Iserver-c# implementiert](servers/index.md). Die integrierten Server sind [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` Kestrel wird standardmäßig verwendet.

`CreateDefaultbuilder`führt Installationsaufgaben zusätzlich zum Konfigurieren von Kestrel wie des Webservers:

* Legt den Content-Stamm auf `Directory.GetCurrentDirectory`.
* Lädt die Konfiguration aus:
  * *AppSettings.JSON*
  * *"appSettings". \<EnvironmentName > JSON*.
  * Benutzer geheime Schlüssel, wenn die app in der Entwicklungsumgebung ausgeführt wird.
  * Umgebungsvariablen
  * angegebene Befehlszeile args
* Protokollierung für Konsole und Debug-Ausgabe konfiguriert mit Filtern in einem Konfigurationsabschnitt für die Protokollierung angegebene Regeln.
* Ermöglicht die Integration von IIS.
* Fügt die Developer-Ausnahme-Seite hinzu, wenn die app in der Entwicklungsumgebung ausgeführt wird.

Des Servers *Content Stamm* bestimmt, wo er für Inhaltsdateien wie MVC-Ansicht-Dateien sucht. Der standardmäßige Content-Stamm ist der Ordner, von dem die Anwendung ausgeführt wird.

> [!NOTE]
> Angeben von `Directory.GetCurrentDirectory` wie den Inhalt Stamm Stammordner für das Webprojekt als Inhalt der app-Stammverzeichnis, beim Starten der app aus diesem Ordner verwenden (z. B. durch Aufruf von `dotnet run` aus dem Projektordner Web). Dies ist die Standardeinstellung in Visual Studio verwendet und `dotnet new` Vorlagen.

Wenn Sie IIS als Reverseproxy verwenden, ruft ASP.NET Core automatisch `UseIISIntegration` im Rahmen der Erstellung des Hosts. Weitere Informationen finden Sie unter [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).

Beachten Sie, dass `UseIISIntegration` nicht Konfigurieren einer *Server*, z. B. `UseKestrel` verfügt. `UseKestrel`erstellt den Webserver und hostet die app. `UseIISIntegration`überprüft Umgebungsvariablen, die von IIS/IIS Express verwendet und konfiguriert die Einstellungen wie z. B. den Port zu Lauschen und die Header verwendet wird.

---

Eine minimale Implementierung der Konfiguration eines Hosts (und ASP.NET Core-app) würde nur einen Server und die Konfiguration der Anforderungspipeline die app einschließen:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> Beim Einrichten von eines Hosts können Sie angeben `Configure` und `ConfigureServices` Methoden stattdessen oder zusätzlich zum Angeben einer `Startup` Klasse (die müssen auch diese Methoden - definieren finden Sie unter [Anwendungsstart](startup.md)). Mehrere Aufrufe `ConfigureServices` untereinander angefügt wird; Aufrufe von `Configure` oder `UseStartup` ersetzt die vorherige Einstellungen.

## <a name="configuring-a-host"></a>Konfigurieren eines Hosts

Die `WebHostBuilder` stellt Methoden zum Festlegen der Großteil der verfügbaren Konfigurationswerte für den Host, die auch direkt festgelegt werden kann `UseSetting` und den zugehörigen Schlüssel.

### <a name="host-configuration-values"></a>Host-Konfigurationswerte

**Erfassen von Startfehlern**`bool`

Schlüssel: `captureStartupErrors`. Wird standardmäßig auf `false` festgelegt. Wenn `false`, Fehler während des Starts Ergebnis auf dem Host beendet. Wenn `true`, der Host werden keine Ausnahmen aus erfasst die `Startup` Klasse, und versuchen, den Server zu starten. Es wird eine Fehlerseite angezeigt (allgemeines oder detaillierte, basierend auf der Einstellung der detaillierte Fehler unten) für jede Anforderung. Legen Sie mithilfe der `CaptureStartupErrors` Methode.

Hinweis: Wenn Ihre app mit Kestrel und IIS ausgeführt wird, ist das Standardverhalten von Startfehlern zu erfassen. 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**Inhalts-Stamm**`string`

Schlüssel: `contentRoot`. Der Standardwert ist der Ordner, in dem die Assembly (für Kestrel; befindet IIS wird dem Projektstamm Web wird standardmäßig verwendet). Diese Einstellung bestimmt, in dem ASP.NET Core für Inhaltsdateien, z. B. MVC-Ansichten Suche begonnen wird. Verwendet auch als den Basispfad für den [Root Webeinstellung](#web-root-setting). Legen Sie mithilfe der `UseContentRoot` Methode. Pfad muss vorhanden sein, oder Host kann nicht gestartet.

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**Detaillierte Fehler**`bool`

Schlüssel: `detailedErrors`. Wird standardmäßig auf `false` festgelegt. Wenn `true` (oder wenn die Umgebung auf "Entwicklung" festgelegt ist), die app werden Details zu Ausnahmen aus, starten, anstatt nur eine generische Fehlerseite angezeigt. Legen Sie mithilfe von `UseSetting`.

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

Wenn ausführliche Fehler festgelegt ist, um `false` und Erfassen von Startfehlern `true`, eine generische Fehlerseite wird als Antwort auf jede Anforderung an den Server angezeigt.

![Generische Fehlerseite angezeigt.](hosting/_static/generic-error-page.png)

Wenn ausführliche Fehler festgelegt ist, um `true` und Erfassen von Startfehlern `true`, eine ausführliche Fehlermeldung-Seite wird als Antwort auf jede Anforderung an den Server angezeigt.

![Detaillierte Fehler (Seite)](hosting/_static/detailed-error-page.png)

**Umgebung**`string`

Schlüssel: `environment`. Der Standardwert ist "Produktion". Kann auf einen beliebigen Wert festgelegt werden. Framework definierte Werte sind "Entwicklung", "Staging" und "Produktion" definieren. Werte sind nicht in der Groß-/Kleinschreibung beachtet. Finden Sie unter [arbeiten mit mehreren Umgebungen](environments.md). Legen Sie mithilfe der `UseEnvironment` Methode.

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> Standardmäßig wird die Umgebung aus der gelesen der `ASPNETCORE_ENVIRONMENT` -Umgebungsvariablen angegeben. Wenn Sie Visual Studio verwenden, können Umgebungsvariablen festgelegt werden, der *launchSettings.json* Datei.

<a id="server-urls"></a>

**Server-URLs**`string`

Schlüssel: `urls`. Legen Sie auf ein Semikolon (;) getrennte Liste von URL-Präfixen, die auf der Server reagieren soll. Beispielsweise `http://localhost:123`. Der Domäne/Hostname kann ersetzt werden, mit "\*" an, dass der Server Lauscht auf Anforderungen für eine beliebige IP-Adresse oder host mit dem angegebenen Port und Protokoll sollten (z. B. `http://*:5000` oder `https://*:5001`). Das Protokoll (`http://` oder `https://`) für jede URL eingeschlossen werden müssen. Die Präfixe werden durch den konfigurierten Server interpretiert; zu den unterstützten Bildformaten variiert zwischen Servern.

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

In ASP.NET Core 2.0 Kestrel verfügt über einen eigenen Endpunkt-Konfigurations-API und unterstützt keine `https://` in die `urls` Zeichenfolge. Weitere Informationen finden Sie unter [Einführung in Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

**Startassembly**`string`

Schlüssel: `startupAssembly`. Die Assembly zu suchende bestimmt die `Startup` Klasse. Legen Sie mithilfe der `UseStartup` Methode. Kann stattdessen mit bestimmten Typ verweisen `WebHostBuilder.UseStartup<StartupType>`. Wenn mehrere `UseStartup` Methoden aufgerufen werden, das letzte Lesezeichen hat Vorrang vor.

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**Webseiten Stammverzeichnis**`string`

Schlüssel: `webroot`. Wenn nicht angegeben wird, ist die Standardeinstellung `(Content Root Path)\wwwroot`, sofern vorhanden. Wenn dieser Pfad nicht vorhanden ist, wird ein ohne-Op-dateisystemanbieter verwendet. Legen Sie mithilfe von `UseWebRoot`.

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>Überschreiben die Konfiguration

Verwendung [Konfiguration](configuration.md) zum Festlegen der Konfigurationswerte, die vom Host verwendet werden. Diese Werte können später überschrieben werden. Dadurch wird angegeben, mit `UseConfiguration`.

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

Im obigen Beispiel Befehlszeilenargumente übergeben so konfigurieren Sie den Host oder Konfigurationseinstellungen können optional angegeben werden, einem *hosting.json* Datei. Um den Host aus, führen Sie auf eine bestimmte URL angeben, könnten Sie den gewünschten Wert aus der Eingabeaufforderung übergeben:

```console
dotnet run --urls "http://*:5000"
```

Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird.

```csharp
host.Run();
```

Sie können den Host in eine nicht blockierende Weise ausführen, durch Aufrufen seiner `Start` Methode:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Übergeben Sie eine Liste von URLs für die `Start` -Methode, und es werden auf die angegebenen URLs überwachen:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

Die URL-Formaten, die hier gelten abhängig sind, auf dem Server, den Sie verwenden. Weitere Informationen finden Sie unter [Server URLs](#server-urls) weiter oben in diesem Artikel.

> [!NOTE]
> Die `UseConfiguration` Erweiterungsmethode ist zurzeit der Analyse eines zurückgegebenen Konfigurationsabschnitts kann nicht `GetSection` (z. B. `.UseConfiguration(Configuration.GetSection("section"))`. Die `GetSection` Methode filtert die Schlüssel der Konfiguration im Abschnitt angefordert, sondern bleibt der Name des Abschnitts auf die Schlüssel (z. B. `section:urls`, `section:environment`). Die `UseConfiguration` Methode erwartet die Schlüssel entsprechend dem `WebHostBuilder` Schlüssel (z. B. `urls`, `environment`). Das Vorhandensein der Abschnittsname den Schlüsseln wird verhindert, dass Werte im Abschnitt konfigurieren den Host. Dieses Problem wird in einer bevorstehenden Version behoben. Weitere Informationen und problemumgehungen finden Sie unter [vollständige Schlüssel übergeben Konfigurationsabschnitt in WebHostBuilder.UseConfiguration verwendet](https://github.com/aspnet/Hosting/issues/839).

### <a name="ordering-importance"></a>Reihenfolge der Wichtigkeit

`WebHostBuilder`Einstellungen werden zuerst vom bestimmte Umgebungsvariablen, gelesen, wenn festgelegt. Verwenden Sie diese Umgebungsvariablen müssen das Format `ASPNETCORE_{configurationKey}`, also beispielsweise um URLs festgelegt, die der Server wird standardmäßig auf lauschen, Sie legen würde `ASPNETCORE_URLS`.

Sie können eine der folgenden Werte für Umgebungsvariablen überschreiben, indem Konfiguration angeben (mithilfe von `UseConfiguration`) oder indem Sie den Wert explizit festlegen (mit `UseUrls` für die Instanz). Der Host wird verwendet, unabhängig davon, welche Option zuletzt legt den Wert fest. Aus diesem Grund `UseIISIntegration` muss nach angezeigt werden `UseUrls`, da er die URL mit einem dynamisch von IIS bereitgestellten ersetzt. Wenn Sie programmgesteuert die Standard-URL auf einen Wert festgelegt, aber lassen sie bei der Konfiguration außer Kraft gesetzt werden soll, konnte Sie den Host wie folgt konfigurieren:

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Veröffentlichen Sie in Windows unter Verwendung von IIS](../publishing/iis.md)
* [Veröffentlichen Sie in Linux mit Nginx](../publishing/linuxproduction.md)
* [Veröffentlichen Sie in Linux mit Apache](../publishing/apache-proxy.md)
* [Host in einem Windowsdienst](xref:hosting/windows-service)

