---
title: HTTP.sys webserverimplementierung in ASP.NET Core
author: rick-anderson
description: "Führt ein HTTP.sys, einen Webserver für ASP.NET Core unter Windows. HTTP.sys ist basiert auf Http.Sys-Kernelmodustreiber und ist eine Alternative zum Kestrel, die für die direkte Verbindung mit dem Internet ohne IIS verwendet werden kann."
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url Präfixe, SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 8d46862af44379d8592efdf214a80214dce2d69d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>HTTP.sys webserverimplementierung in ASP.NET Core

Durch [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Dieses Thema gilt nur für ASP.NET Core 2.0 und höher. In früheren Versionen von ASP.NET Core HTTP.sys heißt [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys ist eine [Webserver für ASP.NET Core](index.md) , die nur unter Windows ausgeführt wird. Es basiert auf der [Http.Sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys ist eine Alternative zum [Kestrel](kestrel.md) , einige Funktionen, die nicht von Kestel bietet. **"Http.sys" kann nicht mit IIS oder IIS Express verwendet werden, da es inkompatibel mit ist der [ASP.NET Core-Modul](aspnet-core-module.md).**

HTTP.sys unterstützt die folgenden Funktionen:

- [Windows-Authentifizierung](xref:security/authentication/windowsauth)
- Anschlussfreigabe
- HTTPS mit SNI
- HTTP/2 über TLS (Windows 10)
- Direkte Übertragung
- Zwischenspeichern von Antworten
- WebSockets (Windows 8)

Unterstützte Windows-Versionen:

- Windows 7 und Windows Server 2008 R2 und höher

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Verwendung von "http.sys"

HTTP.sys ist nützlich für Bereitstellungen, in dem Sie der Server direkt mit dem Internet verfügbar zu machen, ohne mit IIS müssen.

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

Da er auf Http.Sys basiert, erfordern nicht HTTP.sys einen reverse-Proxy-Server für den Schutz vor Angriffen durch. Http.Sys ist ausgereifte Technologie, die für viele Arten von Angriffen geschützt und die Stabilität, Sicherheit und Skalierbarkeit von einem Webserver mit vollem Funktionsumfang. IIS selbst wird als eine HTTP-Listener auf Http.Sys ausgeführt. 

HTTP.sys ist eine gute Wahl für interne Bereitstellungen an, wenn Sie eine Funktion nicht verfügbar in Kestrel, z. B. Windows-Authentifizierung benötigen.

![HTTP.sys kommuniziert direkt mit Ihrem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Gewusst wie: Verwenden von "http.sys"

Hier ist eine Übersicht der Setupaufgaben für das Hostbetriebssystem und ASP.NET Core-Anwendung.

### <a name="configure-windows-server"></a>Konfigurieren von WindowsServer

* Installieren Sie die Version von .NET, die Ihre Anwendung benötigt werden, z. B. [.NET Core](https://www.microsoft.com/net/download/core) oder [.NET Framework](https://www.microsoft.com/net/download/framework).

* Zu registrieren Sie URL-Präfixe zum Binden an HTTP.sys und Einrichten von SSL-Zertifikaten

   Wenn Sie URL-Präfixe in Windows nicht vorab registrieren, müssen Sie die Anwendung mit Administratorrechten ausgeführt werden. Die einzige Ausnahme ist, wenn Sie auf "localhost", die über HTTP (nicht "HTTPS") mit einer Portnummer, die größer als 1024 binden. In diesem Fall sind keine Administratorrechte erforderlich.

   Weitere Informationen finden Sie unter [zu Präfixe registrieren und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl) weiter unten in diesem Artikel.

* Öffnen Sie die Firewall-Ports zum Zulassen des Datenverkehrs HTTP.sys zu erreichen.

   Sie können *netsh.exe* oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906).

Es gibt auch [Http.Sys-registrierungseinstellungen](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Konfigurieren Sie die ASP.NET Core-Anwendung mithilfe von HTTP.sys

* Keine Paketinstallation ist erforderlich, wenn Sie mithilfe der [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) Metapackage. Die [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) Paket ist in der Metapackage enthalten.

* Rufen Sie die `UseHttpSys` Erweiterungsmethode auf `WebHostBuilder` in Ihrer `Main` Methode, dabei werden alle [HTTP.sys Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) , die Sie benötigen, wie im folgenden Beispiel gezeigt:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>HTTP.sys-Optionen konfigurieren

Hier sind einige der HTTP.sys-Einstellungen und Grenzwerte, die Sie konfigurieren können.

**Maximaler Client-Verbindungen**

Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann festgelegt werden, für die gesamte Anwendung mithilfe des folgenden Codes in *"Program.cs"*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Die maximale Anzahl von Verbindungen ist standardmäßig unbegrenzt (Null).

**Maximale Anforderungsgröße-Text**

Die Standardgröße der Höchstwert für die Anforderung Text ist 30,000,000 Bytes, also ungefähr 28.6 MB.

Die empfohlene Methode zum Überschreiben Sie des Grenzwert von in einer ASP.NET-MVC-Anwendung Core ist die Verwendung der [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) Attribut auf eine Aktionsmethode:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Hier ist ein Beispiel, das veranschaulicht, wie die Einschränkung für die gesamte Anwendung, jede Anforderung zu konfigurieren:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Sie können die Einstellung für eine bestimmte Anforderung in überschreiben *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert für eine Anforderung zu konfigurieren, nach dem Start der Anwendung Lesen der Anforderung. Ist ein `IsReadOnly` -Eigenschaft, die Aufschluss darüber gibt Wenn die `MaxRequestBodySize` Eigenschaft befindet sich im schreibgeschützten Zustand, d. h., es ist zu spät so konfigurieren Sie den Grenzwert.

Informationen zu den anderen HTTP.sys-Optionen finden Sie unter [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Konfigurieren von URLs und Ports Lauschen an 

Standardmäßig ASP.NET Core bindet an `http://localhost:5000`. Um URL-Präfixe und Ports konfigurieren, können Sie die `UseUrls` Erweiterungsmethode, die `urls` Befehlszeilenargument, das die Umgebungsvariable ASPNETCORE_URLS oder die `UrlPrefixes` Eigenschaft [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Im folgenden Codebeispiel wird mit `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Ein Vorteil der `UrlPrefixes` ist, dass Sie eine Fehlermeldung sofort abrufen, wenn Sie versuchen, ein Präfix hinzuzufügen, die falsch formatiert ist. Ein Vorteil der `UseUrls` (mit freigegebenen `urls` und ASPNETCORE_URLS) ist, dass Sie leichter zwischen Kestrel und HTTP.sys wechseln können.

Wenn Sie beides verwenden `UseUrls` (oder `urls` oder ASPNETCORE_URLS) und `UrlPrefixes`, die Einstellungen in `UrlPrefixes` außer Kraft setzen in `UseUrls`. Weitere Informationen finden Sie unter [Hosting](xref:fundamentals/hosting).

"Http.sys" verwendet die [HTTP Server API UrlPrefix Zeichenfolgenformate](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Stellen Sie sicher, dass Sie angeben, dass die gleichen Präfixzeichenfolgen in `UseUrls` oder `UrlPrefixes` , die Sie auf dem Server zu registrieren. 

### <a name="dont-use-iis"></a>Verwenden Sie keine IIS

Stellen Sie sicher, dass Ihre Anwendung zum Ausführen von IIS oder IIS Express konfiguriert ist.

Ist in Visual Studio das Standardprofil für den Start für IIS Express. Um das Projekt als Konsolenanwendung ausführen, ändern Sie manuell das ausgewählte Profil, wie im folgenden Screenshot gezeigt.

![Wählen Sie die Konsole app-Profil](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Zu URL-Präfixe registrieren und Konfigurieren von SSL

Sowohl IIS als auch HTTP.sys basieren auf den zugrunde liegenden Http.Sys-Kernelmodustreiber zum Abhören von Anforderungen, und der ersten Verarbeitung. In IIS bietet die Verwaltungsbenutzeroberfläche eine relativ einfache Möglichkeit, alles zu konfigurieren. Allerdings müssen Sie Http.Sys selbst konfigurieren. Die integriertes Tool, d. h. dafür *netsh.exe*. 

Mit *netsh.exe* können Sie reservieren von URL-Präfixe und Zuweisen von SSL-Zertifikate. Die Tools sind Administratorrechte erforderlich.

Das folgende Beispiel zeigt die mindestens erforderlich, um die URL-Präfixe für die Ports 80 und 443 zu reservieren:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Im folgende Beispiel wird gezeigt, wie ein SSL-Zertifikat zugewiesen werden:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Hier ist die Referenzdokumentation für *netsh.exe*:

* [Netsh-Befehle für Hypertext Transfer-Protokoll (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix Zeichenfolgen](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Die folgenden Ressourcen bieten detaillierte Anweisungen für verschiedene Szenarien. Artikel, die auf HttpListener verweisen gelten gleichermaßen für HTTP.sys, wie sowohl auf Http.Sys basieren.

* [Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS-Kommunikation - HttpListener basierend Hosting und Clientzertifizierung](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) dies ein Drittanbieter-Blog und ist ziemlich ALT, aber weiterhin enthält nützliche Informationen.
* [Gewusst wie: Exemplarische Vorgehensweise mithilfe von HttpListener oder HTTP-Server (C++) Code, wie eine einfache SSL-Server nicht verwaltete](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Dies ist eine ältere Blog mit nützlichen Informationen.

Hier sind einige Drittanbieter-Tools, die einfacher zu verwenden als die *netsh.exe* über die Befehlszeile. Diese werden nicht von bereitgestellt oder Unterstützung von Microsoft. Die Tools als Administrator ausführen standardmäßig, da *netsh.exe* selbst sind Administratorrechte erforderlich.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/) bietet eine Benutzeroberfläche Liste und Konfigurieren von SSL-Zertifikate und Optionen, Präfix Reservierungen und Zertifikatsvertrauenslisten. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie aus, oder konfigurieren Sie SSL-Zertifikate und URL-Präfixen. Die Benutzeroberfläche als http.sys Manager verfeinerten und stellt einige weitere Konfigurationsoptionen zur Verfügung, jedoch andernfalls er verfügt über ähnliche Funktionen. Eine neue Zertifikatsvertrauensliste (CTL) kann nicht erstellt werden, aber vorhandene zuweisen können.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Beispiel-app für diesen Artikel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys-Quellcode](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/hosting)
