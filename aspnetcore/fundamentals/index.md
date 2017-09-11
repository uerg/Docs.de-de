---
title: "ASP.NET Core – Grundlagen"
author: rick-anderson
description: "Dieser Artikel enthält einen allgemeinen Überblick über grundlegende Konzepte, die bei der Erstellung von ASP.NET Core-Anwendungen relevant sind."
keywords: "ASP.NET Core, Grundlagen, Übersicht"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99fbe0e02be27a0fbbb7ff65bc15713aab58c003
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>Übersicht über Grundlagen von ASP.NET Core

Eine ASP.NET Core-Anwendung ist eine Konsolenanwendung, in deren `Main`-Methode ein Webserver erstellt wird:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

Die `Main`-Methode ruft die `WebHost.CreateDefaultBuilder`-Methode auf, die nach dem Builder-Muster einen Webanwendungshost erstellt. Der Builder verfügt über Methoden, die den Webserver (z.B. `UseKestrel`) und die Startklasse (`UseStartup`) definieren. Im obigen Beispiel wird ein [Kestrel](xref:fundamentals/servers/kestrel)-Webserver automatisch zugeordnet. Wenn IIS verfügbar ist, wird versucht, den Webhost von ASP.NET Core auf IIS auszuführen. Andere Webserver wie [HTTP.sys](xref:fundamentals/servers/httpsys) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden. `UseStartup` wird im nächsten Abschnitt näher erläutert.

Der Rückgabetyp `IWebHostBuilder` des `WebHost.CreateDefaultBuilder`-Aufrufs stellt viele optionale Methoden zur Verfügung. Zu diesen Methoden gehören `UseHttpSys` für das Hosten der Anwendung in HTTP.sys und `UseContentRoot` für das Festlegen des Stamminhaltsverzeichnisses. Mit den Methoden `Build` und `Run` wird das `IWebHost`-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

Die `Main`-Methode verwendet eine Instanz von `WebHostBuilder`, die nach dem Builder-Muster einen Webanwendungshost erstellt. Der Builder verfügt über Methoden, die den Webserver (z.B. `UseKestrel`) und die Startklasse (`UseStartup`) definieren. Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet. Andere Webserver wie [WebListener](xref:fundamentals/servers/weblistener) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden. `UseStartup` wird im nächsten Abschnitt näher erläutert.

`WebHostBuilder` stellt viele optionale Methoden wie z.B. `UseIISIntegration` für das Hosten in IIS und IIS Express und `UseContentRoot` für das Festlegen des Inhaltsstammverzeichnisses zur Verfügung. Mit den Methoden `Build` und `Run` wird das `IWebHost`-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.

---

## <a name="startup"></a>Start

Die `UseStartup`-Methode in `WebHostBuilder` gibt die `Startup`-Klasse für Ihre Anwendung an:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

In der `Startup`-Klasse definieren Sie die Pipeline zur Anforderungsverarbeitung. Außerdem werden in dieser Klasse alle von der Anwendung benötigten Dienste konfiguriert. Die `Startup`-Klasse muss öffentlich sein und die folgenden Methoden enthalten:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` definiert die [Dienste](#services) Ihrer Anwendung (z.B. ASP.NET Core MVC, Entity Framework Core, Identity usw.).

* `Configure` definiert die [Middleware](xref:fundamentals/middleware) in der Anforderungspipeline.

Weitere Informationen finden Sie unter [Application startup (Starten von Anwendungen)](xref:fundamentals/startup).

## <a name="services"></a>Dienste

Ein Dienst ist eine Komponente, die für die gemeinsame Nutzung in einer Anwendung vorgesehen ist. Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung gestellt. ASP.NET Core enthält einen nativen IoC-Container (Inversion of Control, Steuerungsumkehr), der die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) standardmäßig unterstützt. Der native Container kann mit einem Container Ihrer Wahl ersetzt werden. Neben der losen Kopplung besteht ein weiterer Vorteil darin, dass durch die Abhängigkeitsinjektion Dienste in der gesamten Anwendung zur Verfügung gestellt werden. So kann beispielsweise die [Protokollierung](xref:fundamentals/logging) in der gesamten Anwendung genutzt werden.

Weitere Informationen finden Sie unter [Dependency injection (Abhängigkeitsinjektion)](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Middleware

In ASP.NET Core erstellen Sie Ihre Anforderungspipeline mithilfe von [Middleware](xref:fundamentals/middleware). ASP.NET Core-Middleware führt asynchrone Logikoperationen im `HttpContext` aus und ruft anschließend die nächste Middlewareanwendung in der Sequenz auf oder beendet die Anforderung direkt. Eine Middlewarekomponente mit dem Namen „XYZ“ wird durch den Aufruf einer `UseXYZ`-Erweiterungsmethode in der `Configure`-Methode hinzugefügt.

ASP.NET Core enthält standardmäßig zahlreiche Middlewareanwendungen:

* [Statische Dateien](xref:fundamentals/static-files)

* [Routing](xref:fundamentals/routing)

* [Authentifizierung](xref:security/authentication/index)

Sie können jede auf [OWIN](http://owin.org) basierende Middleware mit ASP.NET Core verwenden und außerdem benutzerdefinierte Middleware erstellen.

Weitere Informationen finden Sie unter [Middleware](xref:fundamentals/middleware) und [Introduction to Open Web Interface for .NET (OWIN) (Einführung in Open Web Interface for .NET (OWIN))](xref:fundamentals/owin).

## <a name="servers"></a>Server

Das ASP.NET Core-Hostmodell lauscht nicht direkt auf Anforderungen. Vielmehr verwendet es eine HTTP-Serverimplementierung, mit der die Anforderung an die Anwendung weitergeleitet wird. Die weitergeleitete Anforderung wird von Funktionsobjekten umschlossen, auf die Sie über Schnittstellen zugreifen können. Die Anwendung kombiniert diese Objekte zu einem `HttpContext`. ASP.NET Core enthält den verwalteten, plattformübergreifenden Webserver [Kestrel](xref:fundamentals/servers/kestrel). Kestrel wird in der Regel hinter einem Produktionswebserver wie [IIS](https://iis.net) oder [nginx](http://nginx.org) ausgeführt.

Weitere Informationen finden Sie unter [Server](xref:fundamentals/servers/index) und [Hosting](xref:fundamentals/hosting).

## <a name="content-root"></a>Inhaltsstammverzeichnis

Das Inhaltsstammverzeichnis ist der Basispfad zu allen von der Anwendung verwendeten Inhalten wie Ansichten, [Razor-Seiten](xref:mvc/razor-pages/index) und statischen Objekten. Standardmäßig entspricht das Inhaltsstammverzeichnis dem Anwendungsbasispfad der ausführbaren Datei, mit der die Anwendung gehostet wird. Ein alternativer Speicherort für das Inhaltsstammverzeichnis kann mit `WebHostBuilder` angegeben werden.

## <a name="web-root"></a>Webstammverzeichnis

Das Webstammverzeichnis einer Anwendung ist das Projektverzeichnis, in dem sich öffentliche, statische Ressourcen wie CSS-, JavaScript- und Bilddateien befinden. Die Middleware für statische Dateien stellt standardmäßig nur Dateien aus dem Webstammverzeichnis und dessen Unterverzeichnissen bereit. Weitere Informationen finden Sie unter [working with static files (Arbeiten mit statischen Dateien)](xref:fundamentals/static-files). Das Webstammverzeichnis ist standardmäßig */wwwroot*. Sie können allerdings mit dem `WebHostBuilder` einen anderen Speicherort angeben.

## <a name="configuration"></a>Konfiguration

ASP.NET Core verwendet ein neues Konfigurationsmodell für die Verarbeitung einfacher Name/Wert-Paare. Das neue Konfigurationsmodell basiert nicht auf `System.Configuration` oder *web.config*, sondern ruft Daten aus einer geordneten Menge von Konfigurationsanbietern ab. Die integrierten Konfigurationsanbieter unterstützen zahlreiche Dateiformate (XML, JSON, INI) und Umgebungsvariablen, durch die eine umgebungsbasierte Konfiguration ermöglicht wird. Sie können auch benutzerdefinierte Konfigurationsanbieter erstellen.

Weitere Informationen finden Sie unter [Configuration (Konfiguration)](xref:fundamentals/configuration).

## <a name="environments"></a>Umgebungen

Umgebungen wie „Entwicklung“ und „Produktion“ sind in ASP.NET Core von besonderer Bedeutung und können über Umgebungsvariablen festgelegt werden.

Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core-Runtime im Vergleich zur .NET Framework-Laufzeit

Eine ASP.NET Core-Anwendung kann für die .NET Core- oder .NET Framework-Laufzeit entwickelt werden. Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="additional-information"></a>Zusätzliche Informationen

Weitere Informationen finden Sie auch in folgenden Themen:

- [Fehlerbehandlung](xref:fundamentals/error-handling)
- [File Provider (Dateianbieter)](xref:fundamentals/file-providers)
- [Globalization and localization (Globalisierung und Lokalisierung)](xref:fundamentals/localization)
- [Logging (Protokollierung)](xref:fundamentals/logging)
- [Managing Application State (Verwalten eines Anwendungszustands)](xref:fundamentals/app-state)