---
title: Webserverimplementierungen in ASP.NET Core
author: rick-anderson
description: Ermitteln Sie die Webserver Kestrel und HTTP.sys für ASP.NET Core. Erfahren Sie mehr über das Auswählen eines Servers und darüber, wann ein Reverseproxyserver zu verwenden ist.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: bb0331d7201d4e979e6c6524cbf630280c4eaeb6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274442"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Webserverimplementierungen in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) und [Chris Ross](https://github.com/Tratcher)

Eine ASP.NET Core-App wird über eine In-Process-Implementierung eines HTTP-Servers ausgeführt. Die Serverimplementierung lauscht auf HTTP-Anforderungen und leitet diese als [Anforderungsfunktionen](xref:fundamentals/request-features), die in einem [HttpContext](/dotnet/api/system.web.httpcontext) zusammengefasst werden, an die App weiter.

ASP.NET Core stellt zwei Serverimplementierungen zur Verfügung:

* [Kestrel](xref:fundamentals/servers/kestrel) ist der plattformübergreifende HTTP-Standardserver für ASP.NET Core.
* [HTTP.sys](xref:fundamentals/servers/httpsys) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber und der HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert. (HTTP.sys wird in ASP.NET Core 1.x als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.)

## <a name="kestrel"></a>Kestrel

Kestrel ist der Standardwebserver, der in ASP.NET Core-Projektvorlagen enthalten ist.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel kann eigenständig oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwendet werden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps. Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn die App nur Anforderungen aus einem internen Netzwerk akzeptiert, kann Kestrel eigenständig verwendet werden.

![Kestrel kommuniziert direkt mit dem internen Netzwerk.](kestrel/_static/kestrel-to-internal.png)

Wenn die App für das Internet verfügbar gemacht ist, muss Kestrel IIS, Nginx oder Apache als *Reverseproxyserver* verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese wie im folgenden Diagramm dargestellt nach einer vorbereitenden Verarbeitung an Kestrel weiter:

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Der wichtigste Grund für die Verwendung eines Reverseproxys für Edge-Bereitstellungen, die im Internet verfügbar gemacht werden, ist die IT-Sicherheit. Die 1.x-Versionen von Kestrel haben keine wesentlichen Sicherheitsfeatures zum Schutz vor Angriffen aus dem Internet. So sind z. B. keine geeigneten Timeouts, Größenlimits für Anforderungen sowie Beschränkungen der Anzahl gleichzeitiger Verbindungen vorhanden.

Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

---

IIS, Nginx oder Apache können nicht ohne Kestrel oder eine [benutzerdefinierte Serverimplementierung](#custom-servers) verwendet werden. ASP.NET Core wurde entwickelt, um in einem eigenen Prozess ausgeführt werden zu können, sodass plattformübergreifend konsistentes Verhalten gewährleistet wird. IIS, Nginx und Apache geben ihre eigene Startprozedur und Umgebung vor. Um diese Servertechnologien direkt nutzen zu können, muss ASP.NET Core an die Anforderungen jedes Servers angepasst werden. Durch eine Webserverimplementierung wie Kestrel kann ASP.NET Core den Startprozess und die Umgebung steuern, wenn es auf anderen Servertechnologien gehostet wird.

### <a name="iis-with-kestrel"></a>IIS und Kestrel

Wenn [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) als Reverseproxy für ASP.NET Core verwendet wird, wird die ASP.NET Core-App in einem Prozess ausgeführt, der vom IIS-Workerprozess getrennt ist. Im IIS-Prozess steuert das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) die Beziehung zum Reverseproxy. Die Hauptfunktionen des ASP.NET Core-Moduls bestehen darin, die ASP.NET Core-App zu starten, die App nach einem Absturz neu zu starten und HTTP-Datenverkehr an die App weiterzuleiten. Weitere Informationen finden Sie unter [ASP.NET Core Module (ASP.NET Core-Modul)](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx und Kestrel

Informationen dazu, wie Nginx unter Linux als Reverseproxyserver für Kestrel verwendet wird, finden Sie unter [Hosten unter Linux mit Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache und Kestrel

Informationen dazu, wie Apache unter Linux als Reverseproxyserver für Kestrel verwendet wird, finden Sie unter [Hosten unter Linux mit Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wenn ASP.NET Core-Apps unter Windows ausgeführt werden, ist HTTP.sys eine Alternative zu Kestrel. Grundsätzlich empfiehlt sich Kestrel, um die beste Leistung zu erzielen. HTTP.sys kann in Szenarien verwendet werden, in denen die App dem Internet verfügbar gemacht ist und Funktionen erfordert, die von HTTP.sys, aber nicht von Kestrel unterstützt werden. Informationen zu HTTP.sys-Funktionen finden Sie unter [HTTP.sys](xref:fundamentals/servers/httpsys).

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

Außerdem kann HTTP.sys auch bei Apss zum Einsatz kommen, die nur in einem internen Netzwerk verfügbar gemacht werden. 

![HTTP.sys kommuniziert direkt mit dem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys wird in ASP.NET Core 1.x [WebListener](xref:fundamentals/servers/weblistener) genannt. Wenn ASP.NET Core-Apps unter Windows ausgeführt werden, ist WebListener eine Alternative für Szenarien, in denen Apps nicht von IIS gehostet werden können.

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

WebListener kann auch anstelle von Kestrel für Apps verwendet werden, die nur in einem internen Netzwerk verfügbar gemacht werden, wenn erforderliche Funktionen durch WebListener, aber nicht durch Kestrel unterstützt werden. Informationen zu WebListener-Funktionen finden Sie unter [WebListener](xref:fundamentals/servers/weblistener).

![WebListener kommuniziert direkt mit dem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core-Serverinfrastruktur

Die [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)-Schnittstelle, sie in der `Startup.Configure`-Methode verfügbar ist, macht die [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures)-Eigenschaft des Typs [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) verfügbar. Kestrel und HTTP.sys (WebListener in ASP.NET Core 1.x) machen nur jeweils eine einzelne Funktion, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), verfügbar, andere Serverimplementierungen machen jedoch möglicherweise weitere Funktionalität verfügbar.

Mit `IServerAddressesFeature` kann ermittelt werden, an welchen Port die Serverimplementierung zur Laufzeit gebunden ist.

## <a name="custom-servers"></a>Benutzerdefinierte Server

Wenn die integrierten Server nicht den Anforderungen der App entsprechen, kann eine benutzerdefinierte Serverimplementierung erstellt werden. In der [Anleitung zu Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) wird erläutert, wie eine auf [Nowin](https://github.com/Bobris/Nowin) basierende [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver)-Implementierung erstellt wird. Nur die Feature-Schnittstellen, die von der App verwendet werden, erfordern Implementierung , wobei mindestens [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) und [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) unterstützt werden müssen.

## <a name="server-startup"></a>Serverstart

Wenn [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio für Mac](https://www.visualstudio.com/vs/mac/) oder [Visual Studio Code](https://code.visualstudio.com/) verwendet wird, wird der Server gestartet, wenn die App durch die integrierte Entwicklungsumgebung gestartet wird. In Visual Studio unter Windows können Startprofile verwendet werden, um die App und den Server mit [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) oder mit der Konsole zu starten. In Visual Studio Code werden die App und der Server durch [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) gestartet, das den CoreCLR-Debugger aktiviert. Wird Visual Studio für Mac verwendet, werden die App und der Server durch den [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) gestartet.

Wird eine App über eine Eingabeaufforderung im Ordner des Projekts gestartet, startet [dotnet run](/dotnet/core/tools/dotnet-run) die App und den Server (nur Kestrel und HTTP.sys). Die Konfiguration wird durch die Option `-c|--configuration` angegeben, die entweder auf `Debug` (Standardwert) oder `Release` festgelegt ist. Sind in der Datei *launchSettings.json* Startprofile vorhanden, verwenden Sie die Option `--launch-profile <NAME>`, um das Startprofil festzulegen (z. B. `Development` oder `Production`). Weitere Informationen hierzu finden Sie in den Themen [dotnet run](/dotnet/core/tools/dotnet-run) und [Verpacken einer Verteilung von .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Kestrel with IIS (Kestrel und IIS)](xref:fundamentals/servers/aspnet-core-module)
* [Hosten unter Linux mit Nginx](xref:host-and-deploy/linux-nginx)
* [Hosten unter Linux mit Apache](xref:host-and-deploy/linux-apache)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (für ASP.NET Core 1.x siehe [WebListener](xref:fundamentals/servers/weblistener))
