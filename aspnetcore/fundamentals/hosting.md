---
title: Hosten in ASP.NET Core
author: guardrex
description: "Informationen Sie zu den Webhost in ASP.NET Core, die für die app starten und die Verwaltung zuständig ist."
keywords: ASP.NET Core, web Host IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a>Hosten in ASP.NET Core

Durch [Luke Latham](https://github.com/guardrex)

ASP.NET Core apps konfigurieren und starten Sie eine *Host*, die für die app starten und Verwaltung der Lebensdauer verantwortlich ist. Mindestens wird der Host einem Server und einem Anforderungsverarbeitungspipeline konfiguriert.

## <a name="setting-up-a-host"></a>Einrichten eines Hosts

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Dies erfolgt in der Regel am Einstiegspunkt für Ihre app, die `Main` Methode. In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*. Eine typische *"Program.cs"* Aufrufe [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zum Einrichten eines Hosts zu starten:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`führt die folgenden Aufgaben:

* Konfiguriert die [Kestrel](servers/kestrel.md) wie der Webserver.
* Legt den Content-Stamm auf [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Lädt die optionale Konfiguration aus:
  * *AppSettings.JSON*.
  * *"appSettings". {Umgebung} JSON*.
  * [Vertrauliche Benutzerdaten](xref:security/app-secrets) Wenn die app ausgeführt wird, der `Development` Umgebung.
  * Umgebungsvariablen.
  * Befehlszeilenargumente.
* Konfiguriert [Protokollierung](xref:fundamentals/logging) für die Konsole und Debug-Ausgabe mit [Protokoll filtern](xref:fundamentals/logging#log-filtering) Regeln in einem Konfigurationsabschnitt Protokollierung ein *appsettings.json* oder *"appSettings". {Umgebung} JSON* Datei.
* Wenn hinter dem IIS ausgeführt wird, ermöglicht die IIS-Integration durch Konfigurieren der Basispfad und der Port, auf der Server überwacht werden soll bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module). Das Modul erstellt einen Reverse-Proxy zwischen Kestrel und IIS STATT. Außerdem konfiguriert die app, [Erfassen von Startfehlern](#capture-startup-errors).

Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht. Der Standard-Content-Stamm ist [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory). Dies führt zu Stammordner für das Webprojekt als Inhalt Stamm verwenden, wenn die app aus dem Stammordner gestartet wird (z. B. durch Aufruf von [Dotnet ausführen](/dotnet/core/tools/dotnet-run) aus dem Projektordner). Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).

Finden Sie unter [Konfiguration in ASP.NET Core](xref:fundamentals/configuration) für Weitere Informationen zu app-Konfiguration.

> [!NOTE]
> Als Alternative zur Verwendung der statischen `CreateDefaultBuilder` -Methode, erstellen einen Host aus [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ist ein unterstützten Ansatz mit ASP.NET Core 2.x. Finden Sie unter der Registerkarte "1.x ASP.NET Core" für Weitere Informationen.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Dies erfolgt in der Regel am Einstiegspunkt für Ihre app, die `Main` Methode. In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*. Die folgenden *"Program.cs"* veranschaulicht, wie `WebHostBuilder` auf den Host zu erstellen:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`erfordert eine [Server, die Iserver-c# implementiert](servers/index.md). Die integrierten Server sind [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md) (vor der Veröffentlichung von ASP.NET Core 2.0, HTTP.sys hieß [WebListener](xref:fundamentals/servers/weblistener)). In diesem Beispiel wird die [UseKestrel Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) gibt den Kestrel-Server.

Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht. Des Standardstammverzeichnisses Content bereitgestellten `UseContentRoot` ist [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Dies führt zu Stammordner für das Webprojekt als Inhalt Stamm verwenden, wenn die app aus dem Stammordner gestartet wird (z. B. durch Aufruf von [Dotnet ausführen](/dotnet/core/tools/dotnet-run) aus dem Projektordner). Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).

Rufen Sie zum Verwenden von IIS als Reverseproxy [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts. `UseIISIntegration`Konfigurieren Sie keine *Server*, z. B. [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) verfügt. `UseIISIntegration`konfiguriert den Basispfad und der Port, auf der Server überwacht werden soll bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) einen Reverse-Proxy zwischen Kestrel und IIS zu erstellen. Um IIS mit ASP.NET Core verwenden zu können, müssen Sie beide angeben `UseKestrel` und `UseIISIntegration`. `UseIISIntegration`nur aktiviert, wenn hinter der IIS- oder IIS Express ausführen. Weitere Informationen finden Sie unter [Einführung in ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und [ASP.NET Core-Modul Konfigurationsverweis](xref:hosting/aspnet-core-module).

Eine minimale Implementierung, die einen Host (und ASP.NET Core-app) konfiguriert, enthält einen Server und die Konfiguration der Anforderungspipeline die app angeben:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

Beim Einrichten von eines Hosts bieten [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) Methoden. Bei Angabe einer `Startup` -Klasse, müssen sie definieren eine `Configure` Methode. Weitere Informationen finden Sie unter [Start der Anwendung in ASP.NET Core](startup.md). Mehrere Aufrufe `ConfigureServices` untereinander anfügen. Mehrere Aufrufe `Configure` oder `UseStartup` auf die `WebHostBuilder` ersetzen die vorherige Einstellungen.

## <a name="host-configuration-values"></a>Host-Konfigurationswerte

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) stellt Methoden zum Festlegen der Großteil der verfügbaren Konfigurationswerte, damit des Hosts die direkt mit auch festgelegt werden können [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und den zugehörigen Schlüssel. Beim Festlegen eines Werts mit `UseSetting`, der Wert wird als Zeichenfolge (in Anführungszeichen) unabhängig vom Typ festgelegt.

### <a name="capture-startup-errors"></a>Erfassen von Startfehlern

Diese Einstellung steuert die Erfassung von Startfehlern.

**Schlüssel**: CaptureStartupErrors  
**Typ**: *Bool* (`true` oder `1`)  
**Standardmäßige**: standardmäßig `false` , wenn die app mit Kestrel hinter dem IIS ausgeführt wird, in dem die Standardeinstellung ist `true`.  
**Legen Sie mithilfe von**:`CaptureStartupErrors`

Wenn `false`, Fehler während des Starts Ergebnis auf dem Host beendet. Wenn `true`, der Host erfasst Ausnahmen während des Starts und versucht, den Server zu starten.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Inhalts-Stamm

Diese Einstellung bestimmt, in dem ASP.NET Core beginnt die Suche nach Inhaltsdateien gespeichert, z. B. MVC-Ansichten. 

**Schlüssel**: ContentRoot  
**Typ**: *Zeichenfolge*  
**Standard**: standardmäßig auf den Ordner, in dem die app-Assembly gespeichert ist.  
**Legen Sie mithilfe von**:`UseContentRoot`

Die Inhalte-Stamm dient außerdem als den Basispfad für den [Stammweb-Einstellung](#web-root). Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Ausführliche Fehler

Bestimmt, ob ausführliche Fehler erfasst werden sollen.

**Schlüssel**: DetailedErrors  
**Typ**: *Bool* (`true` oder `1`)  
**Standard**: "false"  
**Legen Sie mithilfe von**:`UseSetting`

Wenn aktiviert (oder wenn die <a href="#environment">Umgebung</a> auf festgelegt ist `Development`), die app erfasst ausführliche Ausnahmen.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Umgebung

Legt fest, die app-Umgebung.

**Schlüssel**: Umgebung  
**Typ**: *Zeichenfolge*  
**Standard**: Produktion  
**Legen Sie mithilfe von**:`UseEnvironment`

Sie können festlegen, die *Umgebung* auf einen beliebigen Wert. Framework definierte Werte sind `Development`, `Staging`, und `Production`. Werte sind keine Groß-/Kleinschreibung beachtet. Wird standardmäßig die *Umgebung* auslesen ist die `ASPNETCORE_ENVIRONMENT` -Umgebungsvariablen angegeben. Bei Verwendung [Visual Studio](https://www.visualstudio.com/), Umgebungsvariablen festgelegt werden können, der *launchSettings.json* Datei. Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Hosten Startassemblys

Legt die app hosten Startassemblys fest.

**Schlüssel**: HostingStartupAssemblies  
**Typ**: *Zeichenfolge*  
**Standard**: leere Zeichenfolge  
**Legen Sie mithilfe von**:`UseSetting`

Eine durch Semikolons getrennte Zeichenfolge hosten Startassemblys beim Start geladen werden soll. Dieses Feature ist neu in ASP.NET Core 2.0.

Obwohl die Konfiguration auf eine leere Zeichenfolge wird als Standardwert, enthalten hosting Autostart-Assemblys immer der app-Assembly. Wenn Sie hosting Startassemblys angeben, werden diese hinzugefügt auf die app-Assembly für das Laden, wenn die app während des Starts der gemeinsamen Dienste erstellt.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Diese Funktion steht in ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Bevorzugen Sie Hosten von URLs

Gibt an, ob der Host werden, auf die URLs abgehört soll mit konfiguriert die `WebHostBuilder` nicht die dafür konfigurierten mit der `IServer` Implementierung.

**Schlüssel**: PreferHostingUrls  
**Typ**: *Bool* (`true` oder `1`)  
**Standard**: "true"  
**Legen Sie mithilfe von**:`PreferHostingUrls`

Dieses Feature ist neu in ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Diese Funktion steht in ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Zu verhindern, dass Hosting starten

Verhindert, dass das automatische Laden von hosting Startassemblys, einschließlich der app-Assembly.

**Schlüssel**: PreventHostingStartup  
**Typ**: *Bool* (`true` oder `1`)  
**Standard**: "false"  
**Legen Sie mithilfe von**:`UseSetting`

Dieses Feature ist neu in ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Diese Funktion steht in ASP.NET Core 1.x.

---

### <a name="server-urls"></a>Server-URLs

Gibt die IP-Adressen oder Adressen mit Ports und Protokolle, die der Server auf Anforderungen Lauschen soll.

**Schlüssel**: Urls  
**Typ**: *Zeichenfolge*  
**Standard**: http://localhost: 5000  
**Legen Sie mithilfe von**:`UseUrls`

Legen Sie auf eine durch Semikolon getrennt (;) Liste von URL-Präfixen, die auf der Server reagieren soll. Beispielsweise `http://localhost:123`. Mit "\*", um anzugeben, dass der Server nach Anforderungen für alle IP-Adresse oder Hostname mit dem angegebenen Port und Protokoll abgehört werden soll (z. B. `http://*:5000`). Das Protokoll (`http://` oder `https://`) für jede URL eingeschlossen werden müssen. Zu den unterstützten Bildformaten variieren zwischen Servern.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel verfügt über einen eigenen Endpunkt-Konfigurations-API. Weitere Informationen finden Sie unter [Kestrel web server implementation in ASP.NET Core (Kestrel-Webserverimplementierung in ASP.NET Core)](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Timeout beim Herunterfahren

Gibt den Umfang der Wartezeit für den Webhost zum Herunterfahren.

**Schlüssel**: ShutdownTimeoutSeconds  
**Typ**: *Int*  
**Standard**: 5  
**Legen Sie mithilfe von**:`UseShutdownTimeout`

Obwohl der Schlüssel akzeptiert ein *Int* mit `UseSetting` (z. B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), die `UseShutdownTimeout` Erweiterungsmethode akzeptiert eine `TimeSpan`. Dieses Feature ist neu in ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Diese Funktion steht in ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Startassembly

Die Assembly zu suchende bestimmt die `Startup` Klasse.

**Schlüssel**: StartupAssembly  
**Typ**: *Zeichenfolge*  
**Standard**: die app-Assembly  
**Legen Sie mithilfe von**:`UseStartup`

Sie können auf die Assembly anhand des Namens verweisen (`string`) oder (`TStartup`). Wenn mehrere `UseStartup` Methoden aufgerufen werden, das letzte Lesezeichen hat Vorrang vor.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Webstamm

Legt den relativen Pfad auf die app statische Objekte fest.

**Schlüssel**: Webroot-Verzeichnis.  
**Typ**: *Zeichenfolge*  
**Standardmäßige**: Wenn nicht angegeben, wird der Standardwert ist ""(Content Root)/wwwroot "", wenn der Pfad vorhanden ist. Wenn der Pfad nicht vorhanden ist, wird ein ohne-Op-dateisystemanbieter verwendet.  
**Legen Sie mithilfe von**:`UseWebRoot`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Überschreiben die Konfiguration

Verwendung [Konfiguration](configuration.md) auf den Host konfigurieren. Im folgenden Beispiel wird optional Hostkonfiguration angegeben ein *hosting.json* Datei. Eine Konfiguration geladen wird, aus der *hosting.json* Datei überschrieben werden kann, durch die Befehlszeilenargumente. Die Konfiguration erstellt (in `config`) wird verwendet, um den Host mit konfigurieren `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> Die `UseConfiguration` Erweiterungsmethode ist zurzeit der Analyse eines zurückgegebenen Konfigurationsabschnitts kann nicht `GetSection` (z. B. `.UseConfiguration(Configuration.GetSection("section"))`. Die `GetSection` Methode filtert die Schlüssel der Konfiguration im Abschnitt angefordert, sondern bleibt der Name des Abschnitts auf die Schlüssel (z. B. `section:urls`, `section:environment`). Die `UseConfiguration` Methode erwartet die Schlüssel entsprechend dem `WebHostBuilder` Schlüssel (z. B. `urls`, `environment`). Das Vorhandensein der Abschnittsname den Schlüsseln wird verhindert, dass Werte im Abschnitt konfigurieren den Host. Dieses Problem wird in einer bevorstehenden Version behoben. Weitere Informationen und problemumgehungen finden Sie unter [vollständige Schlüssel übergeben Konfigurationsabschnitt in WebHostBuilder.UseConfiguration verwendet](https://github.com/aspnet/Hosting/issues/839).

Um den Host aus, führen Sie auf eine bestimmte URL angeben, Sie könnten übergeben in den gewünschten Wert an einer Eingabeaufforderung Ausführung `dotnet run`. Das Befehlszeilenargument überschreibt die `urls` Wert aus der *hosting.json* Datei und der Server lauscht an Port 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>Reihenfolge der Wichtigkeit

Einige der `WebHostBuilder` Einstellungen werden zuerst aus der Umgebungsvariablen, gelesen, wenn festgelegt. Diese Umgebungsvariablen verwenden Sie das Format `ASPNETCORE_{configurationKey}`. Legen Sie zum Festlegen der URLs, die der Server standardmäßig überwacht `ASPNETCORE_URLS`.

Sie können eine der folgenden Werte für Umgebungsvariablen überschreiben, indem Konfiguration angeben (mit `UseConfiguration`) oder indem Sie den Wert explizit festlegen (mit `UseSetting` oder eine der expliziten Erweiterungsmethoden, wie z. B. `UseUrls`). Vom Host verwendet wird, unabhängig davon, welche Option zuletzt legt den Wert fest. Wenn Sie programmgesteuert die Standard-URL auf einen Wert festgelegt, aber lassen sie bei der Konfiguration außer Kraft gesetzt werden möchten, können Sie eine Befehlszeilenkonfiguration nach dem Festlegen der URL. Finden Sie unter [überschreiben Konfiguration](#overriding-configuration).

## <a name="starting-the-host"></a>Der Host wird gestartet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Run**

Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:

```csharp
host.Run();
```

**Start**

Sie können den Host in eine nicht blockierende Weise ausführen, durch Aufrufen seiner `Start` Methode:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Wenn Sie eine Liste mit URLs zum Übergeben der `Start` -Methode, auf die angegebenen URLs überwacht:

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

Sie initialisieren und starten Sie einen neuen Host mit den Standardwerten vorkonfiguriert, dass `CreateDefaultBuilder` mithilfe einer statischen Hilfsmethode. Diese Methoden starten Sie den Server ohne Konsolenausgabe und [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) warten Sie, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM):

**Start (RequestDelegate app)**

Beginnen Sie mit einem `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!" `WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird. Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.

**Start (String Url, RequestDelegate app)**

Beginnen Sie mit einer URL und `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Führt zum gleiche Ergebnis wie **starten (RequestDelegate app)**, außer die Anwendung reagiert auf `http://localhost:8080`.

**Starten (Aktion<IRouteBuilder> RouteBuilder)**

Verwenden Sie eine Instanz des `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) routing Middleware verwenden:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Verwenden Sie die folgenden Browseranforderungen mit dem Beispiel:

| Anforderung                                    | Antwort                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos Massenzahlungsverkehrssysteme, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Löst eine Ausnahme mit der Zeichenfolge "Ooops!" |
| `http://localhost:5000/throw`              | Löst eine Ausnahme mit der Zeichenfolge "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird. Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.

**Starten (string Url, Aktion<IRouteBuilder> RouteBuilder)**

Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Führt zum gleiche Ergebnis wie **starten (Aktion<IRouteBuilder> RouteBuilder)**, außer die Anwendung reagiert auf `http://localhost:8080`.

**StartWith (Aktion<IApplicationBuilder> app)**

Geben Sie ein Delegat zum Konfigurieren einer `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!" `WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird. Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.

**StartWith (string Url, Aktion<IApplicationBuilder> app)**

Geben Sie eine URL und ein Delegat zum Konfigurieren einer `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Führt zum gleiche Ergebnis wie **StartWith (Aktion<IApplicationBuilder> app)**, außer die Anwendung reagiert auf `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Run**

Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:

```csharp
host.Run();
```

**Start**

Sie können den Host in eine nicht blockierende Weise ausführen, durch Aufrufen seiner `Start` Methode:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Wenn Sie eine Liste mit URLs zum Übergeben der `Start` -Methode, auf die angegebenen URLs überwacht:


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

---

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment-Schnittstelle

Die [IHostingEnvironment Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) enthält Informationen zu den app-Web-hosting-Umgebung. Können Sie [Konstruktoreinfügung](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment` um die Eigenschaften und die Erweiterungsmethoden verwenden:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

Können Sie eine [konventionsbasierte Ansatz](xref:fundamentals/environments#startup-conventions) zum Konfigurieren Ihrer Anwendung beim Start auf Grundlage der Umgebung. Alternativ können Sie Einfügen der `IHostingEnvironment` in der `Startup` Konstruktor für die Verwendung in `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Zusätzlich zu den `IsDevelopment` Erweiterungsmethode, `IHostingEnvironment` bietet `IsStaging`, `IsProduction`, und `IsEnvironment(string environmentName)` Methoden. Finden Sie unter [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) Details.

Die `IHostingEnvironment` Service kann auch direkt in injiziert die `Configure` Methode zum Einrichten der Verarbeitungspipeline:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

Sie können einfügen `IHostingEnvironment` in der `Invoke` Methode, wenn benutzerdefinierte [Middleware](xref:fundamentals/middleware#writing-middleware):

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime-Schnittstelle

Die [IApplicationLifetime Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) können Sie nach dem Starten und Herunterfahren Aktivitäten ausführen. Drei Eigenschaften auf der Schnittstelle sind Abbruchtoken, das Sie beim registrieren können `Action` Methoden zum Starten und Herunterfahren Ereignisse zu definieren. Es gibt auch eine `StopApplication` Methode.

| Abbruchtoken    | Wird ausgelöst, wenn &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Der Host wurde vollständig gestartet. |
| `ApplicationStopping` | Der Host führt ein ordnungsgemäßes Herunterfahren. Anforderungen möglicherweise noch verarbeitet werden. Shutdown ist blockiert, bis diese abgeschlossen wird. |
| `ApplicationStopped`  | Der Host wird ein ordnungsgemäßes Herunterfahren abgeschlossen. Alle Anforderungen sollten vollständig verarbeitet werden. Shutdown ist blockiert, bis diese abgeschlossen wird. |

| Methode            | Aktion                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Anforderungen Beendigung der aktuellen Anwendung. |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a>Problembehandlung bei System.ArgumentException

**Gilt für nur ASP.NET Core 2.0**

Wenn Sie den Host von Räumen erstellen `IStartup` direkt in die abhängigkeitseinschleusungscontainer anstelle von Aufrufen `UseStartup` oder `Configure`, treten möglicherweise die folgenden Fehler: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.

Dies geschieht, weil die [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (die aktuelle Assembly) ist erforderlich, um die Überprüfung `HostingStartupAttributes`. Wenn Sie manuell einfügen `IStartup` in der abhängigkeitseinschleusungscontainer hinzufügen den folgenden Aufruf Ihrer `WebHostBuilder` mit dem Assemblynamen angegeben:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Fügen Sie alternativ eine Dummy `Configure` auf Ihre `WebHostBuilder`, welche die `applicationName`(`ApplicationKey`) automatisch:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Hinweis**: Dies ist nur erforderlich, mit der Version 2.0 von ASP.NET Core und nur beim Aufrufen nicht `UseStartup` oder `Configure`.

Weitere Informationen finden Sie unter [Ankündigungen: Microsoft.Extensions.PlatformAbstractions wurde entfernt / / (Kommentar)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und [StartupInjection Beispiel](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Veröffentlichen Sie in Windows unter Verwendung von IIS](../publishing/iis.md)
* [Veröffentlichen Sie in Linux mit Nginx](../publishing/linuxproduction.md)
* [Veröffentlichen Sie in Linux mit Apache](../publishing/apache-proxy.md)
* [Host in einem Windowsdienst](xref:hosting/windows-service)
