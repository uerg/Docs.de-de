---
title: ASP.NET Core-Modul
author: tdykstra
description: "Dieser Artikel führt das ASP.NET Core-Modul (ANCM) ein. Dabei handelt es sich um ein IIS-Modul, mit dem der Kestrel-Webserver IIS oder IIS Express als Reverseproxyserver verwenden kann."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Einführung in das ASP.NET Core-Modul

Von [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) und [Chris Ross](https://github.com/Tratcher) 

Mit dem ASP.NET Core-Modul (ANCM) können Sie ASP.NET Core-Anwendungen hinter IIS ausführen und IIS und [Kestrel](kestrel.md) für das nutzen, wofür sie jeweils gut sind (IIS: Sicherheit, Verwaltbarkeit; Kestrel: Schnelligkeit). Sie erhalten somit die Vorteile von beiden Technologien gleichzeitig. **Das ANCM funktioniert nur mit Kestrel; es ist nicht mit WebListener (in ASP.NET Core 1.x) oder HTTP.sys (in 2.x) kompatibel.** 

Unterstützte Windows-Versionen:

* Windows 7 und Windows Server 2008 R2 und höher

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Funktionen vom ASP.NET Core-Modul

Das ANCM ist ein natives IIS-Modul, das sich in die IIS-Pipeline einklinkt und den Datenverkehr an die Back-End-Anwendung ASP.NET Core leitet. Die meisten anderen Module, beispielsweise die Windows-Authentifizierung, können immer noch ausgeführt werden. Das ANCM übernimmt nur die Kontrolle, wenn ein Handler für die Anforderung aktiviert und die Handlerzuordnung in der *Web.config*-Datei der Anwendung definiert ist.

Da ASP.NET Core-Anwendungen in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das ANCM auch Prozessmanagement durch. Das ANCM startet den Prozess für die ASP.NET Core-Anwendung, wenn die erste Anforderung eingeht und startet ihn neu, wenn er abstürzt. Dies ist im Prinzip das gleiche Verhalten wie in klassischen ASP.NET-Anwendungen, die prozessintern in IIS ausgeführt und durch WAS (Windows Activation Service) verwaltet werden.

Hier sehen Sie ein Diagramm, das die Beziehung zwischen IIS, ANCM und ASP.NET Core-Anwendungen veranschaulicht.

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm.png)

Anforderungen gehen aus dem Internet auf den Kernelmodustreiber Http.Sys ein, der sie in IIS auf dem primären Port (80) oder dem SSL-Port (443) weiterleitet. Das ANCM leitet die Anforderungen an die ASP.NET Core-Anwendung auf dem HTTP-Port weiter, der für die Anwendung konfiguriert ist. Es handelt sich dabei nicht um Port 80/443.

Kestrel hört den Datenverkehr aus dem ANCM ab.  Das ANCM gibt den Port über die Umgebungsvariable beim Start an. Die [UseIISIntegration](#call-useiisintegration)-Methode konfiguriert den Server zum Lauschen auf `http://localhost:{port}`. Es gibt zusätzliche Überprüfungen, um Anfragen abzulehnen, die nicht vom ANCM kommen. (Das ANCM unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.)

Kestrel übernimmt ANCM-Anforderungen und überträgt sie per Push in die Middlewarepipeline von ASP.NET Core. Dort werden sie dann verarbeitet und als `HttpContext`-Instanzen an die Anwendungslogik übergeben. Die Antworten der Anwendung werden dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben werden, der die Anforderungen initiiert hat.

Das ANCM verfügt auch über einige andere Funktionen:

* Legt Umgebungsvariablen fest.
* Protokolliert die `stdout`-Ausgabe im Dateispeicher.
* Leitet Windows-Authentifizierungstoken weiter.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Vorgehensweise: Verwenden von ANCM in ASP.NET Core-Anwendungen

Dieser Abschnitt enthält eine Übersicht über den Prozess zum Einrichten eines IIS-Servers und einer ASP.NET Core-Anwendung. Ausführliche Anweisungen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Installieren von ANCM


Das ASP.NET Core-Modul muss in IIS auf Ihren Servern und in IIS Express auf Ihrem Entwicklungscomputer installiert werden. Bei Servern ist ANCM im [.NET Core Windows Server Hosting-Paket](https://aka.ms/dotnetcore-2-windowshosting) enthalten. Bei Entwicklungscomputern installiert Visual Studio ANCM automatisch in IIS Express und IIS, wenn dies bereits auf dem Computer installiert ist.

### <a name="install-the-iisintegration-nuget-package"></a>Installieren des IISIntegration NuGet-Pakets

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Das [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)-Paket ist in den ASP.NET Core-Metapaketen ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) und [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)) enthalten. Wenn Sie eines der Metapakete nicht verwenden, installieren Sie `Microsoft.AspNetCore.Server.IISIntegration` getrennt. Das `IISIntegration`-Paket ist ein interoperables Paket, das die Umgebungsvariablen liest, die von ANCM übertragen werden, um Ihre Anwendung einzurichten. Die Umgebungsvariablen bieten Konfigurationsinformationen, z.B. den Port, den es abzuhören gilt. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installieren Sie [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) in Ihrer Anwendung. Das `IISIntegration`-Paket ist ein interoperables Paket, das die Umgebungsvariablen liest, die von ANCM übertragen werden, um Ihre Anwendung einzurichten. Die Umgebungsvariablen bieten Konfigurationsinformationen, z.B. den Port, den es abzuhören gilt. 

---

### <a name="call-useiisintegration"></a>Aufrufen von UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Die `UseIISIntegration`-Erweiterungsmethode auf [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) wird automatisch aufgerufen, wenn Sie sie mit IIS ausführen.

Wenn Sie eines der ASP.NET Core-Metapakete nicht verwenden und das `Microsoft.AspNetCore.Server.IISIntegration`-Paket nicht installiert haben, dann erhalten Sie einen Laufzeitfehler. Beim expliziten Aufrufen von `UseIISIntegration` erhalten Sie einen Kompilierzeitfehler, wenn das Paket nicht installiert ist.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Rufen Sie die `UseIISIntegration`-Erweiterungsmethode unter [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in der `Main`-Methode Ihrer Anwendung auf. 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

Die `UseIISIntegration`-Methode sucht nach Umgebungsvariablen, die das ANCM festlegt. Es gibt keinen Vorgang, wenn sie nicht gefunden werden. Dieses Verhalten erleichtert Szenarios wie die Entwicklung und Tests auf MacOS oder Linux und die Bereitstellung auf einem Server mit IIS. Wenn Kestrel auf MacOS oder Linux ausgeführt wird, verhält es sich wie der Webserver. Wenn die Anwendung aber für die IIS-Umgebung bereitgestellt wird, verwendet sie automatisch ANCM und IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Die ANCM-Portbindung überschreibt andere Portbindungen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM generiert einen dynamischen Port, der dem Back-End-Prozess zugewiesen wird. Die `UseIISIntegration`-Methode übernimmt diesen dynamischen Port und konfiguriert Kestrel zum Lauschen von `http://locahost:{dynamicPort}/`. Dies überschreibt andere URL-Konfigurationen, z.B. Aufrufe von `UseUrls` oder [Kestrels Listen-API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Aus diesem Grund müssen Sie `UseUrls` oder Kestrels `Listen`-API bei der Verwendung von ANCM nicht aufrufen. Wenn Sie `UseUrls` oder `Listen` aufrufen, hört Kestrel den Port ab, den Sie angeben, wenn Sie die Anwendung ohne IIS ausführen.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM generiert einen dynamischen Port, der dem Back-End-Prozess zugewiesen wird. Die `UseIISIntegration`-Methode übernimmt diesen dynamischen Port und konfiguriert Kestrel zum Lauschen von `http://locahost:{dynamicPort}/`. Dies überschreibt andere URL-Konfigurationen, z.B. Aufrufe von `UseUrls`. Aus diesem Grund müssen Sie `UseUrls` bei der Verwendung von ANCM nicht aufrufen. Wenn Sie `UseUrls` aufrufen, hört Kestrel den Port ab, den Sie angeben, wenn Sie die App ohne IIS ausführen.

Wenn Sie in ASP.NET Core 1.0 `UseUrls` aufrufen, dann rufen Sie es auf, **bevor** Sie `UseIISIntegration` aufrufen, damit der von ANCM konfigurierte Port nicht überschrieben wird. Diese Aufrufreihenfolge ist in ASP.NET Core 1.1 nicht erforderlich, weil die ANCM-Einstellung `UseUrls` überschreibt.

---

### <a name="configure-ancm-options-in-webconfig"></a>Konfigurieren von ANCM-Optionen in „Web.config“

Die Konfiguration für das ASP.NET Core-Modul ist in der *Web.config*-Datei gespeichert, die sich im Stammordner der Anwendung befindet. Die Einstellungen in dieser Datei zeigen auf den Startbefehl und die Argumente, die Ihre ASP.NET Core-Anwendung starten. Beispiele für *Web.config*-Code und eine Anleitung für Konfigurationsoptionen finden Sie in der [Referenz zur ASP.NET Core-Modulkonfiguration](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Ausführen mit IIS Express in der Entwicklung

IIS Express kann unter Verwendung des durch die ASP.NET Core-Vorlagen definierten Standardprofils von Visual Studio gestartet werden.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Die Proxykonfiguration verwendet das HTTP-Protokoll und ein Paarbildungstoken

Der Proxy, der zwischen dem ANCM und Kestrel erstellt wurde, verwendet das HTTP-Protokoll. Die HTTP-Nutzung ist eine Leistungsoptimierung, bei der der Datenverkehr zwischen dem ANCM und Kestrel in einer Loopbackadresse außerhalb der Netzwerkschnittstelle stattfindet. Es gibt kein Risiko für Lauschangriffe auf den Datenverkehr zwischen dem ANCM und Kestrel von einem Speicherort außerhalb des Servers.

Ein Paarbildungstoken wird verwendet, um sicherzustellen, dass die von Kestrel empfangenen Anfragen von IIS über einen Proxy gesendet wurden und nicht von einer anderen Quelle stammen. Das Paarbildungstoken wurde durch das ANCM erstellt und in einer Umgebungsvariablen (`ASPNETCORE_TOKEN`) festgelegt. Das Paarbildungstoken ist auch bei jeder Proxyanforderung in einem Header (`MSAspNetCoreToken`) festgelegt. IIS-Middleware überprüft jede erhaltene Anforderung, um sicherzustellen, dass der Headerwert des Paarbildungstokens dem Wert der Umgebungsvariablen entspricht. Wenn die Tokenwerte nicht übereinstimmen, wird die Anforderung protokolliert und abgelehnt. Es kann nicht von einem Speicherort außerhalb des Servers auf die Umgebungsvariablen des Paarbildungstokens und den Datenverkehr zwischen dem ANCM und Kestrel zugegriffen werden. Wenn ein Angreifer den Wert des Paarbildungstokens nicht kennt, kann er keine Anforderungen einreichen, die die IIS-Middleware-Prüfung umgehen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Beispiel-App zu diesem Artikel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core-Modulquellcode](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core-Modulkonfigurationsverweis](xref:host-and-deploy/aspnet-core-module)
* [Hosten unter Windows mit IIS](xref:host-and-deploy/iis/index)
