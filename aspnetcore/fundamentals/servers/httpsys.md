---
title: Implementierung des Http.sys-Webservers in ASP.NET Core
author: rick-anderson
description: "Einführung in den Http.sys-Webserver für ASP.NET Core unter Windows Http.sys basiert auf dem Http.Sys-Kernelmodustreiber, stellt eine Alternative zu Kestrel dar und kann zum Herstellen einer direkten Verbindung mit dem Internet ohne Internetinformationsdienste (IIS) verwendet werden."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementierung des Http.sys-Webservers in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Dieser Artikel bezieht sich nur auf ASP.NET Core 2.0 und höher. In früheren Versionen von ASP.NET Core wird HTTP.sys als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.

HTTP.sys ist ein [Webserver für ASP.NET Core](index.md), der nur unter Windows ausgeführt werden kann. Er basiert auf dem [Http.Sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys stellt eine Alternative zu [Kestrel](kestrel.md) dar und bietet sogar noch mehr Features. **HTTP.sys kann nicht mit IIS oder IIS Express verwendet werden, da keine Kompatibilität mit dem [ASP.NET Core-Modul](aspnet-core-module.md) vorliegt.**

HTTP.sys unterstützt die folgenden Features:

- [Windows-Authentifizierung](xref:security/authentication/windowsauth)
- Portfreigabe
- HTTPS mit SNI
- HTTP/2 über TLS (Windows 10)
- Direkte Dateiübertragung
- Zwischenspeichern von Antworten
- WebSockets (Windows 8)

Unterstützte Windows-Versionen:

- Windows 7 und Windows Server 2008 R2 und höher

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Empfohlene Verwendung von HTTP.sys

HTTP.sys unterstützt Sie bei Bereitstellungen, bei denen Sie den Server ohne IIS direkt mit dem Internet verbinden müssen.

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

Da HTTP.sys auf HTTP.Sys basiert, ist kein Reverseproxyserver für den Schutz gegen Angriffe erforderlich. Bei Http.Sys handelt es sich um eine ausgereifte Technologie, die einen Schutz gegen viele Arten von Angriffen darstellt, und die Stabilität, Sicherheit und Skalierbarkeit eines vollständigen Webservers bereitstellt. IIS wird neben Http.Sys auch als HTTP-Listener ausgeführt. 

HTTP.sys eignet sich gut für interne Bereitstellungen, wenn Sie ein Feature benötigen, das in Kestrel nicht verfügbar ist, z.B. die Windows-Authentifizierung.

![HTTP.sys kommuniziert direkt mit Ihrem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Empfohlene Verwendung von HTTP.sys

Im Folgenden erhalten Sie eine Übersicht zum Einrichten von Aufgaben für das Hostbetriebssystem und Ihre ASP.NET Core-Anwendung.

### <a name="configure-windows-server"></a>Konfigurieren von Windows Server

* Installieren Sie die für Ihre Anwendung erforderliche .NET-Version, z.B. [.NET Core](https://www.microsoft.com/net/download/core) oder [.NET Framework](https://www.microsoft.com/net/download/framework).

* Registrieren Sie URL-Präfixe vorab, um sie an HTTP.sys zu binden, und richten Sie SSL-Zertifikate ein.

   Wenn Sie unter Windows vorab keine URL-Präfixe registrieren, müssen Sie Ihre Anwendung mit Administratorberechtigungen ausführen. Die einzige Ausnahme besteht, wenn Sie mithilfe von HTTP (nicht HTTPS) eine Verbindung mit dem Localhost herstellen und die Portnummer größer als 1024 ist. In diesem Fall sind keine Administratorberechtigungen erforderlich.

   Weitere Informationen finden Sie im Abschnitt [Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl).

* Öffnen Sie Firewallports, damit der Datenverkehr HTTP.sys erreicht.

   Sie können dafür *netsh.exe* oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906) verwenden.

Außerdem gibt es [Einstellungen für die Http.Sys-Registrierung](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Konfigurieren Ihrer ASP.NET Core-Anwendung zur Verwendung von HTTP.sys

* Wenn Sie das [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)-Metapaket verwenden, ist keine Paketinstallation notwendig. Das [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)-Paket ist im Metapaket enthalten.

* Rufen Sie die Erweiterungsmethode `UseHttpSys` unter `WebHostBuilder` in Ihrer `Main`-Methode auf, und geben Sie dabei wie im folgenden Beispiel dargestellt alle erforderlichen [HTTP.sys-Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) an:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Konfigurieren von HTTP.sys-Optionen

Im Folgenden werden einige Einstellungen und Einschränkungen für HTTP.sys aufgeführt, die Sie konfigurieren können.

**Maximale Anzahl der Clientverbindungen**

Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann mithilfe von folgendem Code in *Program.cs* für die gesamte Anwendung festgelegt werden:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Die maximale Anzahl von Verbindungen ist standardmäßig nicht begrenzt (NULL).

**Maximale Größe des Anforderungstexts**

Die maximale Größe des Anforderungstexts beträgt standardmäßig 30.000.000 Byte, also ungefähr 28,6 MB.

Die empfohlene Methode zur Außerkraftsetzung des Grenzwerts in einer ASP.NET Core-MVC-App besteht im Verwenden des [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)-Attributs in einer Aktionsmethode:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Im folgenden Beispiel wird veranschaulicht, wie die Einschränkung für die gesamte Anwendung und alle Anfragen konfiguriert wird:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Sie können diese Einstellung für eine bestimmte Anforderung in *Startup.cs* außer Kraft setzen:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert einer Anforderung zu konfigurieren, nachdem die Anwendung bereits mit dem Lesen der Anforderung begonnen hat. Es gibt eine `IsReadOnly`-Eigenschaft, die Ihnen mitteilt, wenn sich die `MaxRequestBodySize`-Eigenschaft im schreibgeschützten Zustand befindet, also wenn der Grenzwert nicht mehr konfiguriert werden kann.

Informationen zu anderen HTTP.sys-Optionen finden Site unter [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Konfigurieren von URLs und Ports, die überwacht werden sollen 

Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden. Wenn Sie URL-Präfixe und Ports konfigurieren möchten, können Sie die Erweiterungsmethode `UseUrls`, das Befehlszeilenargument `urls`, die Umgebungsvariable „ASPNETCORE_URLS“ oder die Eigenschaft `UrlPrefixes` unter [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) verwenden. Im folgenden Codebeispiel wird `UrlPrefixes` verwendet.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Der Vorteil von `UrlPrefixes` ist, dass Sie umgehend eine Fehlermeldung erhalten, wenn Sie versuchen, ein Präfix hinzuzufügen, das nicht richtig formatiert ist. Der Vorteil von `UseUrls` (für `urls` und „ASPNETCORE_URLS“ freigegeben) ist, dass Sie einfacher zwischen Kestrel und HTTP.sys wechseln können.

Wenn Sie sowohl `UseUrls` (bzw. `urls` oder „ASPNETCORE_URLS“) als auch `UrlPrefixes` verwenden, überschreiben die Einstellungen in `UrlPrefixes` die Einstellungen in `UseUrls`. Weitere Informationen finden Sie unter [Hosting](xref:fundamentals/hosting).

HTTP.sys verwendet die [HTTP Server-API-UrlPrefix-Zeichenfolgenformate](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Vergewissern Sie sich, dass Sie dieselben Präfixzeichenfolgen in `UseUrls` oder `UrlPrefixes` verwenden, die Sie auf dem Server vorab registriert haben. 

### <a name="dont-use-iis"></a>Verwenden Sie keine Internetinformationsdienste (IIS).

Vergewissern Sie sich außerdem, dass Ihre Anwendung nicht für IIS oder IIS Express konfiguriert ist.

In Visual Studio ist das Standardstartprofil auf IIS Express ausgerichtet. Wenn das Projekt als Konsolenanwendung ausgeführt werden soll, ändern Sie wie im folgenden Screenshot dargestellt das ausgewählte Profil manuell.

![Auswählen des Profils der App-Konsole](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL

Sowohl IIS als auch HTTP.sys sind abhängig von dem zugrunde liegenden Http.Sys-Kernelmodustreiber, der Anforderungen überwacht und Vorverarbeitungen durchführt. In IIS stellt die Benutzeroberfläche für die Verwaltung eine relativ einfache Möglichkeit zum Konfigurieren dar. Sie müssen HTTP.Sys hingegen manuell konfigurieren. Dafür ist das Tool *netsh.exe* in WebListener integriert. 

Mithilfe von *netsh.exe* können Sie URL-Präfixe reservieren und SSL-Zertifikate zuweisen. Für das Tool sind Administratorrechte erforderlich.

Im folgenden Beispiel wird dargestellt, welche Elemente mindestens benötigt werden, um URL-Präfixe für die Ports 80 und 443 zu reservieren:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Im folgenden Beispiel wird dargestellt, wie Sie ein SSL-Zertifikat zuweisen:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Die Referenzdokumentation für *netsh.exe* finden Sie hier:

* [Netsh Commands for Hypertext Transfer Protocol (HTTP) (Netsh-Befehle für Hypertext Transfer-Protokolle (HTTP))](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix Strings (UrlPrefix-Zeichenfolgen)](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

In den folgenden Ressourcen finden Sie detaillierte Anweisungen zu verschiedenen Szenarios. Artikel, die sich auf HttpListener beziehen, beziehen sich auch auf HTTP.sys, da beide Tools auf Http.Sys basieren.

* [Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication – HttpListener based Hosting and Client Certification (HTTPS-Kommunikation: HttpListener basierend auf Hosting- und Clientzertifizierung)](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Hierbei handelt es sich um einen Blog eines Drittanbieters, der zwar schon recht alt ist, aber trotzdem nützliche Informationen enthält.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server (Vorgehensweise: Exemplarische Vorgehensweise zum Verwenden von nicht verwaltetem Code (C++) für HttpListener oder Http-Server als einfacher SSL-Server)](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/): Hierbei handelt es sich ebenfalls um einen älteren Blog mit nützlichen Informationen.

Die Verwendung der folgenden Tools von Drittanbietern ist möglicherweise einfacher als die Verwendung der *netsh.exe*-Befehlszeile. Diese Tools werden allerdings von Microsoft weder bereitgestellt noch unterstützt. Die Tools werden standardmäßig als Administratoren ausgeführt, da *netsh.exe* Administratorberechtigungen erfordert.

* Der [Http.sys-Manager](http://httpsysmanager.codeplex.com/) stellt eine Benutzeroberfläche bereit, die zum Auflisten und Konfigurieren von SSL-Zertifikaten und -Optionen, Präfixreservierungen und Zertifikatvertrauenslisten verwendet werden kann. 
* Mithilfe von [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie SSL-Zertifikate und URL-Präfixe auflisten und konfigurieren. Die Benutzeroberfläche ist besser optimiert als beim Http.sys-Manager und beinhaltet einige zusätzliche Optionen für die Konfiguration. Ansonsten sind die Funktionen allerdings ähnlich. Es können zwar keine neuen Zertifikatsvertrauenslisten erstellt werden, jedoch können bereits vorhandene Listen hinzugefügt werden.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Beispiel-App zu diesem Artikel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys-Quellcode](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/hosting)
