---
title: Webserverimplementierungen in ASP.NET Core
author: tdykstra
description: "Enthält eine einführende Beschreibung der Webserver Kestrel und WebListener für ASP.NET Core. Enthält Hinweise zur Auswahl eines Webservers und zu dessen Verwendung mit einem Reverseproxyserver."
keywords: ASP.NET Core, IServer, Webserver, Kestrel, WebListener, Reverseproxy
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 04dee100dff91f7868175ff4be01156787e13e81
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a>Webserverimplementierungen in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) und [Chris Ross](https://github.com/Tratcher)

Eine ASP.NET Core-Anwendung wird über eine In-Process-Implementierung eines HTTP-Servers ausgeführt. Die Serverimplementierung lauscht auf HTTP-Anforderungen und leitet diese als [Anforderungsfunktionen](https://docs.microsoft.com/aspnet/core/fundamentals/request-features), die in einem `HttpContext` zusammengefasst werden, an die Anwendung weiter.

ASP.NET Core stellt zwei Serverimplementierungen zur Verfügung:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) ist ein plattformübergreifender HTTP-Server, der auf der plattformübergreifenden asynchronen E/A-Bibliothek [libuv](https://github.com/libuv/libuv) basiert.

* [HTTP.sys](httpsys.md) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber ](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) ist ein plattformübergreifender HTTP-Server, der auf der plattformübergreifenden asynchronen E/A-Bibliothek [libuv](https://github.com/libuv/libuv) basiert.

* [WebListener](weblistener.md) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber ](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.

---

## <a name="kestrel"></a>Kestrel

Kestrel ist der Webserver, der standardmäßig in ASP.NET Core in Vorlagen für neue Projekte enthalten ist. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Sie können Kestrel als eigenständigen Webserver oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Jede der beiden Konfigurationen &mdash;, egal ob mit oder ohne Reverseproxyserver &mdash;, kann auch dann verwendet werden, wenn Kestrel nur in einem internen Netzwerk verfügbar gemacht wird.

Informationen zur sinnvollen Verwendung von Kestrel mit einem Reverseproxy finden Sie unter [Introduction to Kestrel (Einführung in Kestrel)](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn Ihre Anwendung nur Anforderungen aus einem internen Netzwerk akzeptiert, können Sie Kestrel als eigenständigen Webserver verwenden.

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

Wenn Sie Ihre Anwendung im Internet verfügbar machen, müssen Sie IIS, Nginx oder Apache als *Reverseproxyserver* verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese wie im folgenden Diagramm dargestellt nach einer vorbereitenden Verarbeitung an Kestrel weiter.

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

Der wichtigste Grund für die Verwendung eines Reverseproxys für Edge-Bereitstellungen, die im Internet verfügbar gemacht werden, ist die IT-Sicherheit. Die Kestrel-Versionen 1.x besitzen keine umfassenden Schutzmaßnahmen gegenüber Angriffen. Geeignete Timeouts und Größenlimits sowie eine Beschränkung der Anzahl gleichzeitiger Verbindungen sind beispielsweise nicht vorhanden.

Informationen zur sinnvollen Verwendung von Kestrel mit einem Reverseproxy finden Sie unter [Introduction to Kestrel (Einführung in Kestrel)](kestrel.md).

---

Sie können IIS, Nginx oder Apache nicht ohne Kestrel oder eine [benutzerdefinierte Serverimplementierung](#custom-servers) verwenden. ASP.NET Core wurde entwickelt, um in einem eigenen Prozess ausgeführt werden zu können, sodass plattformübergreifend konsistentes Verhalten gewährleistet wird. IIS, Nginx und Apache legen ihren eigenen Startprozess und ihre eigene Umgebung fest. Zur direkten Verwendung dieser Webserver müsste ASP.NET Core an die jeweilige Webserversoftware angepasst werden. Durch eine Webserverimplementierung wie Kestrel kann ASP.NET Core den Startprozess und die Umgebung steuern. Sie müssen daher nicht ASP.NET Core an IIS, Nginx oder Apache anpassen, sondern die Webserver nur so einrichten, dass sie als Proxy Anforderungen an Kestrel weiterleiten. Mit dieser Konfiguration können fast identische `Program.Main`- und `Startup`-Klassen unabhängig vom Bereitstellungsszenario verwendet werden.

### <a name="iis-with-kestrel"></a>IIS und Kestrel

Wenn Sie IIS oder IIS Express als Reverseproxy für ASP.NET Core verwenden, wird die ASP.NET Core-Anwendung in einem Prozess ausgeführt, der vom IIS-Workerprozess getrennt ist. Im IIS-Prozess wird ein spezielles IIS-Modul ausgeführt, das die Beziehung zum Reverseproxy steuert.  Hierbei handelt es sich um das *ASP.NET Core-Modul*. Die Hauptfunktionen des ASP.NET Core-Moduls bestehen darin, die ASP.NET Core-Anwendung zu starten, bei einem Absturz neu zu starten und HTTP-Datenverkehr an sie weiterzuleiten. Weitere Informationen finden Sie unter [ASP.NET Core Module (ASP.NET Core-Modul)](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx und Kestrel

Informationen zur Verwendung von Nginx unter Linux als Reverseproxyserver für Kestrel finden Sie unter [Publish to a Linux Production Environment (Veröffentlichen in einer Linux-Produktionsumgebung)](../../publishing/linuxproduction.md).

### <a name="apache-with-kestrel"></a>Apache und Kestrel

Informationen zur Verwendung von Apache unter Linux als Reverseproxyserver für Kestrel finden Sie unter [Using Apache Web Server as a reverse proxy (Verwenden von Apache-Webserver als Reverseproxy)](../../publishing/apache-proxy.md).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wenn Sie eine ASP.NET Core-Anwendung unter Windows ausführen, ist HTTP.sys eine Alternative zu Kestrel. Sie können HTTP.sys in Szenarios einsetzen, in denen Sie Ihre Anwendung über das Internet verfügbar machen und HTTP.sys-Funktionen benötigen, die Kestrel nicht unterstützt. 

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

Außerdem kann HTTP.sys auch bei Anwendungen zum Einsatz kommen, die nur in einem internen Netzwerk verfügbar gemacht werden. 

![HTTP.sys kommuniziert direkt mit Ihrem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

Für Szenarios mit einem internen Netzwerk wird im Allgemeinen Kestrel für eine optimale Leistung empfohlen. In einigen Szenarios sollten Sie jedoch eine Funktion verwenden, die nur HTTP.sys zur Verfügung stellt. Informationen zu HTTP.sys-Funktionen finden Sie unter [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys wird in ASP.NET Core 1.x WebListener genannt. Wenn Sie eine ASP.NET Core-Anwendung unter Windows ausführen, können Sie alternativ WebListener in Szenarios verwenden, in denen Sie Ihre Anwendung über das Internet verfügbar machen möchten, jedoch nicht IIS verwenden können.

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

WebListener kann auch anstelle von Kestrel für Anwendungen verwendet werden, die nur in einem internen Netzwerk verfügbar gemacht werden, wenn Sie WebListener-Funktionen benötigen, die Kestrel nicht unterstützt. 

![WebListener kommuniziert direkt mit Ihrem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

Für Szenarios mit einem interne Netzwerk wird im Allgemeinen Kestrel für eine optimale Leistung empfohlen. In einigen Szenarios sollten Sie jedoch eine Funktion verwenden, die nur WebListener zur Verfügung stellt. Informationen zu WebListener-Funktionen finden Sie unter [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Hinweise zur ASP.NET Core-Serverinfrastruktur

Die Schnittstelle [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder), die in der `Startup`-Klasse in der `Configure`-Methode vorhanden ist, macht die Eigenschaft `ServerFeatures` vom Typ [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection) verfügbar. Kestrel und WebListener machen nur die Funktion [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) verfügbar. Unterschiedliche Serverimplementierungen können jedoch möglicherweise zusätzliche Funktionalitäten verfügbar machen.

Mit `IServerAddressesFeature` kann ermittelt werden, an welchen Port die Serverimplementierung zur Laufzeit gebunden ist.

## <a name="custom-servers"></a>Benutzerdefinierte Server

Wenn die integrierten Server Ihren Anforderungen nicht gerecht werden, können Sie eine benutzerdefinierte Serverimplementierung erstellen. In der [Anleitung zu Open Web Interface for .NET (OWIN)](../owin.md) wird erläutert, wie eine auf [Nowin](https://github.com/Bobris/Nowin) basierende [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver)-Implementierung erstellt wird. Sie haben selbstverständlich die Möglichkeit, nur die Funktionsschnittstellen zu implementieren, die für Ihre Anwendung erforderlich sind. [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) und [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) müssen jedoch auf jeden Fall unterstützt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel with IIS (Kestrel und IIS)](aspnet-core-module.md)
- [Kestrel with Nginx (Kestrel und Nginx)](../../publishing/linuxproduction.md)
- [Kestrel with Apache (Kestrel und Apache)](../../publishing/apache-proxy.md)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel with IIS (Kestrel und IIS)](aspnet-core-module.md)
- [Kestrel with Nginx (Kestrel und Nginx)](../../publishing/linuxproduction.md)
- [Kestrel with Apache (Kestrel und Apache)](../../publishing/apache-proxy.md)
- [WebListener](weblistener.md)

---
