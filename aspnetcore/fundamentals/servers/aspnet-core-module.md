---
title: ASP.NET Core-Modul
author: tdykstra
description: "Führt ASP.NET Core Modul (ANCM), ein IIS-Modul, in dem die Kestrel Webserver IIS- oder IIS Express als reverse-Proxy-Server verwenden kann."
keywords: ASP.NET Core, IIS, IIS Express, ASP.NET Core-Modul UseIISIntegration
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4124f71f30b758d82a6bf641328a8d5abf779f2
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-aspnet-core-module"></a>Einführung in ASP.NET Core-Modul

Durch [Tom Dykstra](http://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), und [Chris Ross](https://github.com/Tratcher) 

ASP.NET Core Modul (ANCM) können Sie ASP.NET Core Anwendungen hinter IIS ausführen mit IIS für was gut (Sicherheit, Verwaltbarkeit und viele weitere) ist und mit [Kestrel](kestrel.md) für was gut (schnellen), sondern ist und das Abrufen der Vorteile von beide Technologien gleichzeitig. **ANCM funktioniert nur mit Kestrel; Es ist nicht kompatibel mit WebListener (in ASP.NET Core 1.x) oder HTTP.sys (in 2.x).** 

Unterstützte Windows-Versionen:

* Windows 7 und Windows Server 2008 R2 und höher

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)

## <a name="what-aspnet-core-module-does"></a>Was bewirkt, dass ASP.NET Core-Modul

ANCM ist ein systemeigenes IIS-Modul, das in der IIS-Pipeline hooks und leitet den Datenverkehr an den Back-End-Anwendung ASP.NET Core. Die meisten anderen Modulen wie z. B. Windows-Authentifizierung, erhalten immer noch ausgeführt. ANCM übernimmt nur die Kontrolle, wenn ein Handler für die Anforderung aktiviert ist, und Handlerzuordnung ist in der Anwendung definierten *"Web.config"* Datei.

Da ASP.NET Core-Anwendungen, die in einem Prozess Ausführen von IIS-Arbeitsprozess trennen, werden ANCM auch Management verarbeitet. ANCM startet den Prozess für die ASP.NET Core-Anwendung aus, wenn die erste Anforderung eingeht und neu gestartet, wenn er abstürzt. Dies ist im Prinzip das gleiche Verhalten wie klassischen ASP.NET-Anwendungen ausgeführten in-Process in IIS, und durch WAS (Windows Activation Service) verwaltet werden.

Hier wird ein Diagramm, das die Beziehung zwischen ANCM, IIS und ASP.NET Core Anwendungen veranschaulicht.

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm.png)

Anforderungen aus dem Internet stammen, und drücken Kernelmodustreiber Http.Sys, der Sie in IIS auf dem primären Ports (80) oder SSL-Port (Port 443) weiterleitet. ANCM leitet die Anforderungen an die ASP.NET Core-Anwendung über die HTTP-Port für die Anwendung, die nicht 80/443 port ist konfiguriert.

Kestrel überwacht Datenverkehr aus ANCM.  ANCM gibt den Port, über die Umgebungsvariable beim Start und die [UseIISIntegration](#call-useiisintegration) Methode konfiguriert den Server zum Lauschen an `http://localhost:{port}`. Es gibt zusätzliche Überprüfungen, um Anfragen nicht ANCM abgelehnt werden. (ANCM unterstützt keine HTTPS weiterleiten, damit Anforderungen über HTTP weitergeleitet werden, selbst wenn von IIS über HTTPS erhalten hat.)

Kestrel ANCM ein Anforderungen übernommen und stellt diese in die Middleware-Pipeline von ASP.NET Core, dann verarbeitet sie und übergibt diese als `HttpContext` -Instanzen, die Anwendungslogik. Die Anwendung Antworten werden dann zurück an IIS, welche Push-Vorgänge übergeben, die sie an den HTTP-Client zurück, die die Anforderungen initiiert.

ANCM hat auch einigen andere Funktionen:

* Legt die Umgebungsvariablen fest.
* Protokolle `stdout` auf Dateispeicher ausgegeben.
* Werden Windows-Authentifizierungstoken weitergeleitet.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Gewusst wie: ANCM in ASP.NET Core-apps verwendet wird.

Dieser Abschnitt enthält eine Übersicht über den Prozess zum Einrichten einer IIS-Server und ASP.NET Core-Anwendung. Ausführliche Anweisungen finden Sie unter [in IIS veröffentlichen](../../publishing/iis.md).

### <a name="install-ancm"></a>Installieren von ANCM

ASP.NET Core-Modul muss in IIS auf Ihren Servern und in IIS Express auf Ihrem Entwicklungscomputer installiert werden. Bei Servern, ANCM enthalten ist, der [.NET Core Windows Server-Hosting-Bundle](https://aka.ms/dotnetcore.2.0.0-windowshosting). Für Entwicklungscomputern installiert Visual Studio automatisch ANCM in IIS Express und IIS, wenn sie bereits auf dem Computer installiert ist.

### <a name="install-the-iisintegration-nuget-package"></a>Installieren Sie das IISIntegration NuGet-Paket

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) Paket ist in der ASP.NET Core Metapackages enthalten ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) und [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Wenn Sie eines der Metapackages nicht verwenden, installieren Sie `Microsoft.AspNetCore.Server.IISIntegration` getrennt. Die `IISIntegration` Paket ist ein Interoperabilität Pack, das liest Umgebungsvariablen übertragen, indem Sie ANCM So richten Sie Ihre app ein. Die Umgebungsvariablen bieten Konfigurationsinformationen, z. B. den Port zu lauschen. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installieren Sie in der Anwendung [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). Die `IISIntegration` Paket ist ein Interoperabilität Pack, das liest Umgebungsvariablen übertragen, indem Sie ANCM So richten Sie Ihre app ein. Die Umgebungsvariablen bieten Konfigurationsinformationen, z. B. den Port zu lauschen. 

---

### <a name="call-useiisintegration"></a>Rufen Sie UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die `UseIISIntegration` Erweiterungsmethode auf [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) wird automatisch aufgerufen, wenn Sie mit IIS ausführen.

Wenn Sie noch nicht installiert und sind nicht eines der ASP.NET Core Metapackages verwenden die `Microsoft.AspNetCore.Server.IISIntegration` Paket, erhalten Sie einen Laufzeitfehler. Beim Aufrufen `UseIISIntegration` explizit, Sie einen Fehler zur Kompilierzeit abzurufen, wenn das Paket nicht installiert ist.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

In der Anwendungsverzeichnis `Main` -Methode, rufen die `UseIISIntegration` Erweiterungsmethode auf [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

Die `UseIISIntegration` Methode sucht nach den Umgebungsvariablen, die ANCM festlegt, und es keine-Ops, wenn sie nicht gefunden werden. Dieses Verhalten ermöglicht Szenarien wie Entwicklung und Tests auf MacOS oder Linux und die Bereitstellung auf einem Server mit IIS. Beim auf MacOS oder Linux ausgeführt wird, verhält sich wie der Webserver Kestrel; aber wenn die app für die IIS-Umgebung bereitgestellt wird, verwendet es automatisch ANCM und IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Portbindung ANCM überschreibt andere portbindungen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM generiert einen dynamischen Port zuweisen an den Back-End-Prozess. Die `UseIISIntegration` -Methode übernimmt diese dynamischen Port und Kestrel für Lauschen konfiguriert `http://locahost:{dynamicPort}/`. Dies überschreibt anderen URL-Konfigurationen, z. B. Aufrufe der `UseUrls` oder [des Kestrel Lauschen API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Aus diesem Grund Sie aufrufen müssen `UseUrls` oder des Kestrel `Listen` API, die bei der Verwendung von ANCM. Wenn Sie aufrufen `UseUrls` oder `Listen`, Kestrel überwacht den Port, die Sie angeben, wenn Sie die app ohne IIS ausführen.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM generiert einen dynamischen Port zuweisen an den Back-End-Prozess. Die `UseIISIntegration` -Methode übernimmt diese dynamischen Port und Kestrel für Lauschen konfiguriert `http://locahost:{dynamicPort}/`. Dies überschreibt anderen URL-Konfigurationen, z. B. Aufrufe der `UseUrls`. Aus diesem Grund Sie aufrufen müssen `UseUrls` bei Verwendung von ANCM. Wenn Sie aufrufen `UseUrls`, Kestrel überwacht den Port, die Sie angeben, wenn Sie die app ohne IIS ausführen.

In ASP.NET Core 1.0, wenn Sie aufrufen `UseUrls`, rufen sie **vor** rufen Sie `UseIISIntegration` , damit die ANCM konfigurierte Port nicht überschrieben. Diese Reihenfolge des Aufrufs angezeigt wird nicht in ASP.NET Core 1.1 erforderlich, weil die ANCM Einstellung überschreibt `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>ANCM-Optionen in der Datei "Web.config" Konfigurieren

Konfiguration für ASP.NET Core-Modul befindet sich in der *"Web.config"* -Datei, die im Stammordner der Anwendung befindet. Einstellungen in dieser Datei zeigen Sie auf der Startbefehl und die Argumente, die die ASP.NET Core-app zu starten. Beispielcode für die Datei "Web.config" und unterstützende Informationen zu Konfigurationsoptionen finden Sie unter [ASP.NET Core Konfiguration Modulverweis](../../hosting/aspnet-core-module.md).

### <a name="run-with-iis-express-in-development"></a>Führen Sie in der Entwicklung mit IIS Express

IIS Express kann von Visual Studio unter Verwendung der durch die ASP.NET Core definierten Standardprofil gestartet werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Beispiel-app für diesen Artikel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core-Modul-Quellcode](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core Modulverweis-Konfiguration](../../hosting/aspnet-core-module.md)
* [Veröffentlichen in IIS](../../publishing/iis.md)
