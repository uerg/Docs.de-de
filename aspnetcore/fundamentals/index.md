---
title: ASP.NET Core – Grundlagen
author: rick-anderson
description: Lernen Sie die grundlegenden Konzepte zum Erstellen von ASP.NET Core-Apps kennen.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: 56344315acc59003248ffaf1e61455b94a93a545
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090718"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core – Grundlagen

Eine ASP.NET Core-App ist eine Konsolen-App, in deren `Program.Main`-Methode ein Webserver erstellt wird. Die `Main`-Methode ist der *verwaltete Einstiegspunkt* der App:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

Der .NET Core-Host:

* Lädt die [.NET Core-Runtime](https://github.com/dotnet/coreclr).
* Verwendet das erste Argument in der Befehlszeile als Pfad zur verwalteten Binärdatei, die den Einstiegspunkt (`Main`) enthält, und beginnt mit der Ausführung des Codes.

Die `Main`-Methode ruft die [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*)[-Methode auf, die nach dem ](https://wikipedia.org/wiki/Builder_pattern)Builder-Muster einen Webhost erstellt. Der Builder verfügt über Methoden, die den Webserver (z.B. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) und die Startklasse (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) definieren. Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver automatisch zugeordnet. Wenn IIS verfügbar ist, wird versucht, den Webhost von ASP.NET Core auf IIS auszuführen. Andere Webserver wie [HTTP.sys](xref:fundamentals/servers/httpsys) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden. `UseStartup` wird im nächsten Abschnitt näher erläutert.

Der Rückgabetyp <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> des `WebHost.CreateDefaultBuilder`-Aufrufs stellt viele optionale Methoden zur Verfügung. Zu diesen Methoden gehören `UseHttpSys` für das Hosten der Anwendung in HTTP.sys und <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> für das Festlegen des Stamminhaltsverzeichnisses. Mit den Methoden <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> und <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> wird das <xref:Microsoft.AspNetCore.Hosting.IWebHost>-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

Der .NET Core-Host:

* Lädt die [.NET Core-Runtime](https://github.com/dotnet/coreclr).
* Verwendet das erste Argument in der Befehlszeile als Pfad zur verwalteten Binärdatei, die den Einstiegspunkt (`Main`) enthält, und beginnt mit der Ausführung des Codes.

Die `Main`-Methode verwendet eine Instanz von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, die nach dem [Builder-Muster](https://wikipedia.org/wiki/Builder_pattern) einen Web-App-Host erstellt. Der Builder verfügt über Methoden, die den Webserver (z.B. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) und die Startklasse (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) definieren. Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet. Andere Webserver wie [WebListener](xref:fundamentals/servers/weblistener) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden. `UseStartup` wird im nächsten Abschnitt näher erläutert.

`WebHostBuilder` stellt viele optionale Methoden wie z.B. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> für das Hosten in IIS und IIS Express und <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> für das Festlegen des Inhaltsstammverzeichnisses zur Verfügung. Mit den Methoden <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> und <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> wird das <xref:Microsoft.AspNetCore.Hosting.IWebHost>-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.

::: moniker-end

## <a name="startup"></a>Start

Die `UseStartup`-Methode in `WebHostBuilder` gibt die `Startup`-Klasse für Ihre Anwendung an:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

In der `Startup`-Klasse definieren Sie die Pipeline zur Anforderungsverarbeitung. Außerdem werden in dieser Klasse alle von der Anwendung benötigten Dienste konfiguriert. Die `Startup`-Klasse muss öffentlich sein und die folgenden Methoden enthalten:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definiert die [Dienste](#dependency-injection-services) Ihrer Anwendung (z.B. ASP.NET Core MVC, Entity Framework Core, Identity). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definiert die [Middleware](xref:fundamentals/middleware/index), die in der Anforderungspipeline aufgerufen wird.

Weitere Informationen finden Sie unter <xref:fundamentals/startup>.

## <a name="content-root"></a>Inhaltsstammverzeichnis

Das Inhaltsstammverzeichnis ist der Basispfad zu allen von der Anwendung verwendeten Inhalten wie, [Razor Pages](xref:razor-pages/index), MVC-Ansichten und statischen Objekten. Standardmäßig entspricht das Inhaltsstammverzeichnis dem App-Basispfad der ausführbaren Datei, mit der die App gehostet wird.

## <a name="web-root-webroot"></a>Webstamm (webroot)

Das Webstammverzeichnis einer Anwendung ist das Projektverzeichnis, in dem sich öffentliche statische Ressourcen wie etwa CSS-, JavaScript- und Imagedateien befinden. *wwwroot* ist standardmäßig der Webstamm.

Für Razor-Dateien (*.cshtml*) zeigen die Tilde und der Schrägstrich `~/` auf den Webstamm. Pfade, die mit `~/` beginnen, werden als virtuelle Pfade bezeichnet.

## <a name="dependency-injection-services"></a>Abhängigkeitsinjektion (Dienste)

Ein *Dienst* ist eine Komponente, die für die gemeinsame Nutzung in einer App vorgesehen ist. Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung gestellt. ASP.NET Core enthält einen nativen IoC-Container (Inversion of Control, Steuerungsumkehr), der die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) standardmäßig unterstützt. Bei Bedarf können Sie den Standardcontainer durch einen anderen ersetzen. Neben der [losen Kopplung](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) besteht ein weiterer Vorteil darin, dass von der DI Dienste in der gesamten App zur Verfügung gestellt werden (z.B. die [Protokollierung](xref:fundamentals/logging/index)).

Weitere Informationen finden Sie unter <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Middleware

In ASP.NET Core erstellen Sie Ihre Anforderungspipeline mithilfe von [Middleware](xref:fundamentals/middleware/index). ASP.NET Core-Middleware führt asynchrone Operationen im `HttpContext` aus und ruft anschließend die nächste Middlewareanwendung in der Pipeline auf oder beendet die Anforderung.

Eine Middlewarekomponente mit dem Namen „XYZ“ wird gemäß der Konvention durch den Aufruf einer `UseXYZ`-Erweiterungsmethode in der `Configure`-Methode der Pipeline hinzugefügt.

ASP.NET Core enthält eine Reihe umfangreicher Middleware, und außerdem können Sie benutzerdefinierte Middleware erstellen. [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) ermöglicht Web-Apps das Entkoppeln von Webservern und wird in ASP.NET Core unterstützt.

Weitere Informationen finden Sie unter <xref:fundamentals/middleware/index> und <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Initiieren von HTTP-Anforderungen

<xref:System.Net.Http.IHttpClientFactory> ist für den Zugriff auf <xref:System.Net.Http.HttpClient>-Instanzen verfügbar, um HTTP-Anforderung durchzuführen.

Weitere Informationen finden Sie unter <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Umgebungen

Umgebungen wie *Entwicklung* und *Produktion* sind in ASP.NET Core von besonderer Bedeutung und können über Umgebungsvariablen, die Einstellungsdatei sowie das Befehlszeilenargument festgelegt werden.

Weitere Informationen finden Sie unter <xref:fundamentals/environments>.

## <a name="hosting"></a>Hosting

ASP.NET Core-Apps konfigurieren und starten einen *Host*, der für das Starten der App und das Verwalten deren Lebensdauer verantwortlich ist.

Weitere Informationen finden Sie unter <xref:fundamentals/host/index>.

## <a name="servers"></a>Server

Das Hostingmodell von ASP.NET Core lauscht nicht direkt auf Anforderungen. Das Hostingmodell ist darauf angewiesen, dass eine HTTP-Serverimplementierung die Anforderungen an die App weiterleitet. Die weitergeleitete Anforderung wird von Funktionsobjekten umschlossen, auf die Sie über Schnittstellen zugreifen können. ASP.NET Core enthält den verwalteten, plattformübergreifenden Webserver [Kestrel](xref:fundamentals/servers/kestrel). Kestrel wird in der Regel hinter einem Produktionswebserver ausgeführt, z.B. [IIS](https://www.iis.net/) oder [Nginx](http://nginx.org) in einer Reverseproxykonfiguration. Kestrel kann ebenfalls als öffentlich zugänglicher Edge-Server ausgeführt werden, der direkt mit dem Internet in ASP.NET Core 2.0 oder höher verknüpft ist.

Weitere Informationen finden Sie unter <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Konfiguration

ASP.NET Core verwendet ein Konfigurationsmodell, das auf Name/Wert-Paaren basiert. Das Konfigurationsmodell basiert weder auf <xref:System.Configuration> noch auf *web.config*. Die Konfiguration erhält Einstellungen von einer geordneten Menge an Konfigurationsanbietern. Die integrierten Konfigurationsanbieter unterstützen zahlreiche Dateiformate (XML, JSON, INI), Umgebungsvariablen und Befehlszeilenargumente. Sie können auch benutzerdefinierte Konfigurationsanbieter erstellen.

Weitere Informationen finden Sie unter <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Protokollierung

ASP.NET Core unterstützt eine Protokollierungs-API, die mit mehreren verschiedenen Protokollanbietern funktioniert. Integrierte Anbieter unterstützen das Senden von Protokollen an mindestens einen Zielanbieter. Protokollierungsframeworks von Drittanbietern sind ebenso zulässig.

Weitere Informationen finden Sie unter <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Fehlerbehandlung

ASP.NET Core verfügt über integrierte Szenarios zur Fehlerbehandlung in Apps. Dazu zählen die Ausnahmenseite für Entwickler, Seiten für benutzerdefinierte Fehler, Seiten für statischen Statuscode sowie die Startausnahmebehandlung.

Weitere Informationen finden Sie unter <xref:fundamentals/error-handling>.

## <a name="routing"></a>Routing

ASP.NET Core verfügt über Szenarios zum Routing von App-Anforderung an Routenhandler.

Weitere Informationen finden Sie unter <xref:fundamentals/routing>.

## <a name="file-providers"></a>Dateianbieter

ASP.NET Core abstrahiert den Dateisystemzugriff mithilfe von Dateianbietern, wodurch eine einfache Schnittstelle zum plattformübergreifenden Arbeiten mit Dateien zur Verfügung gestellt wird.

Weitere Informationen finden Sie unter <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Statische Dateien

Middleware für statische Dateien kümmert sich um statische Dateien wie etwa HTML-, CSS-, Image- und JavaScript-Dateien.

Weitere Informationen finden Sie unter <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Sitzungs- und Anwendungszustand

ASP.NET Core bietet verschiedene Ansätze zum Beibehalten des Sitzungs- und Anwendungszustand, während der Benutzer eine Web-App durchsucht.

Weitere Informationen finden Sie unter <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Globalisierung und Lokalisierung

Wenn Sie eine mehrsprachige Website mit ASP.NET Core erstellen, können Sie ein breiteres Publikum erreichen. ASP.NET Core stellt Dienste und Middleware für das Lokalisieren von Inhalten in verschiedene Sprachen und Kulturen bereit.

Weitere Informationen finden Sie unter <xref:fundamentals/localization>.

## <a name="request-features"></a>Anforderungsfeatures

Ausführliche Informationen zur Webserverimplementierung, die mit HTTP-Anforderungen und -antworten in Verbindung stehen, werden in Schnittstellen definiert. Diese Schnittstellen werden von Serverimplementierungen und Middleware verwendet, um die Hostingpipeline der App zu erstellen und anzupassen.

Weitere Informationen finden Sie unter <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Hintergrundaufgaben

Hintergrundaufgaben werden als *gehostete Dienste* implementiert. Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle <xref:Microsoft.Extensions.Hosting.IHostedService> implementiert.

Weitere Informationen finden Sie unter <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Zugriff auf HttpContext

`HttpContext` ist automatisch verfügbar, wenn Anforderungen mit Razor Pages und MVC verarbeitet werden. In Fällen, in denen `HttpContext` nicht unmittelbar verfügbar ist, können Sie auf `HttpContext` über die <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>-Schnittstelle und deren Standardimplementierung, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>, zugreifen.

Weitere Informationen finden Sie unter <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>WebSockets

Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht. Es wird in Chat-, Börsenticker- und Spiele-Apps sowie überall dort verwendet, wo Echtzeitfunktionen in einer Web-App benötigt werden. ASP.NET Core unterstützt WebSockets-Szenarios.

Weitere Informationen finden Sie unter <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Metapaket „Microsoft.AspNetCore.App“

Mit dem Metapaket [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) wird die Paketverwaltung vereinfacht.

Weitere Informationen finden Sie unter <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Metapaket „Microsoft.AspNetCore.All“

Das Metapaket [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) für ASP.NET Core enthält:

* alle unterstützten Pakete des ASP.NET Core-Teams
* alle unterstützten Pakete von Entity Framework Core
* interne und Drittanbieterabhängigkeiten, die von ASP.NET Core und Entity Framework Core verwendet werden

Weitere Informationen finden Sie unter <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core-Runtime im Vergleich zur .NET Framework-Laufzeit

Eine ASP.NET Core-Anwendung kann für die .NET Core- oder .NET Framework-Laufzeit entwickelt werden.

Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Wählen zwischen ASP.NET und ASP.NET Core

Weitere Informationen, die Ihnen die Wahl zwischen ASP.NET Core und ASP.NET erleichtern, finden Sie unter <xref:fundamentals/choose-between-aspnet-and-aspnetcore>
