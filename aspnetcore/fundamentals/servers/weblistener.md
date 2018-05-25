---
title: Implementierung des Webservers WebListener in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr über WebListener, einen Webserver für ASP.NET Core unter Windows, der für die direkte Verbindung mit dem Internet ohne IIS verwendet werden kann.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 46871edb744ad152df8eb958b344068b7408dd1e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementierung des Webservers WebListener in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Dieser Artikel bezieht sich nur auf ASP.NET Core 1.x. In ASP.NET Core 2.0 wird WebListener [HTTP.sys](httpsys.md) genannt.

Der WebListener ist ein [Webserver für ASP.NET Core](index.md), der nur unter Windows ausgeführt werden kann. Er basiert auf dem [Http.sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). Er stellt eine Alternative zu [Kestrel](kestrel.md) dar und kann zum Herstellen einer direkten Verbindung mit dem Internet ohne Internetinformationsdienste (IIS) als Reverseproxyserver verwendet werden. **Der WebListener kann nicht mit IIS oder IIS Express verwendet werden, da er nicht mit dem [ASP.NET Core-Modul kompatibel](aspnet-core-module.md) ist.**

Obwohl der WebListener für ASP.NET Core entwickelt wurde, kann er in allen .NET Core oder .NET Framework-Anwendungen über das NuGet-Paket [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) verwendet werden.

Der WebListener unterstützt die folgenden Features:

- [Windows-Authentifizierung](xref:security/authentication/windowsauth)
- Portfreigabe
- HTTPS mit SNI
- HTTP/2 über TLS (Windows 10)
- Direkte Dateiübertragung
- Zwischenspeichern von Antworten
- WebSockets (Windows 8)

Unterstützte Windows-Versionen:

- Windows 7 und Windows Server 2008 R2 und höher

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Empfohlene Verwendung des WebListeners

Der WebListener unterstützt Sie bei Bereitstellungen in Rahmen derer Sie ohne IIS den Server direkt mit dem Internet verbinden müssen.

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

Da der WebListener auf HTTP.sys basiert, ist kein Reverseproxyserver für den Schutz gegen Angriffe erforderlich. Bei Http.Sys handelt es sich um eine ausgereifte Technologie, die einen Schutz gegen viele Arten von Angriffen darstellt, und die Stabilität, Sicherheit und Skalierbarkeit eines vollständigen Webservers bereitstellt. IIS wird neben Http.Sys auch als HTTP-Listener ausgeführt. 

Der WebListener bietet außerdem eine gute Möglichkeit zur internen Bereitstellung, wenn Sie eins der im Lieferumfang enthaltenen Features benötigen, die Kestrel nicht anbietet.

![WebListener kommuniziert direkt mit Ihrem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Verwendung des WebListeners

Im Folgenden erhalten Sie eine Übersicht zum Einrichten von Aufgaben für das Hostbetriebssystem und Ihre ASP.NET Core-Anwendung.

### <a name="configure-windows-server"></a>Konfigurieren von Windows Server

* Installieren Sie die für Ihre Anwendung erforderliche .NET-Version – z.B. [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) oder .NET Framework 4.5.1.

* Registrieren Sie URL-Präfixe vorab, um sie an den WebListener zu binden, und richten Sie SSL-Zertifikate ein.

   Wenn Sie unter Windows vorab keine URL-Präfixe registrieren, müssen Sie Ihre Anwendung mit Administratorberechtigungen ausführen. Die einzige Ausnahme besteht, wenn Sie mithilfe von HTTP (nicht HTTPS) eine Verbindung mit dem Localhost herstellen und die Portnummer größer als 1024 ist. In diesem Fall sind keine Administratorberechtigungen erforderlich.

   Weitere Informationen finden Sie im Abschnitt [Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl).

* Öffnen Sie Firewallports, damit der Datenverkehr den WebListener erreicht.

   Sie können dafür „netsh.exe“ oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906) verwenden.

Außerdem gibt es [Einstellungen für die Http.Sys-Registrierung](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Konfigurieren Ihrer ASP.NET Core-Anwendung

* Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Dadurch wird auch [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) als Abhängigkeit installiert.

* Rufen Sie die Erweiterungsmethode `UseWebListener` auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in der Methode `Main` ab, und legen Sie dabei wie im folgenden Beispiel dargestellt alle benötigten WebListener-[Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) und -[Einstellungen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) fest:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Konfigurieren von URLs und Ports, die überwacht werden sollen 

  Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden. Wenn Sie URL-Präfixe und Ports konfigurieren möchten, können Sie die Erweiterungsmethode `UseURLs`, das Befehlszeilenargument `urls` oder das ASP.NET Core-Konfigurationssystem verwenden. Weitere Informationen finden Sie unter [Host in ASP.NET Core (Hosten in ASP.NET Core)](xref:fundamentals/host/index).

  Der WebListener verwendet die [Präfixzeichenfolgenformate von Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Es gibt keine spezifischen Anforderungen an Präfixzeichenfolgenformate für Präfixe für den WebListener.

  > [!WARNING]
  > Allgemeine Platzhalterbindungen (`http://*:80/` und `http://+:80`) dürfen **nicht** verwendet werden. Platzhalterbindungen auf oberster Ebene gefährden die Sicherheit Ihrer App. Dies gilt für starke und schwache Platzhalter. Verwenden Sie statt Platzhaltern explizite Hostnamen. Platzhalterbindungen in untergeordneten Domänen (z.B. `*.mysub.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist). Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

  > [!NOTE]
  > Vergewissern Sie sich, dass Sie dieselben Präfixzeichenfolgen in `UseUrls` angeben, die Sie auf dem Server vorab registriert haben. 

* Vergewissern Sie sich außerdem, dass Ihre Anwendung nicht für IIS oder IIS Express konfiguriert ist.

  In Visual Studio ist das Standardstartprofil auf IIS Express ausgerichtet.  Wenn das Projekt als Konsolenanwendung ausgeführt werden soll, müssen Sie wie im folgenden Screenshot dargestellt das ausgewählte Profil manuell ändern.

  ![Auswählen des Profils der App-Konsole](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Verwendung des WebListeners außerhalb von ASP.NET Core

* Installieren Sie das NuGet-Paket [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

* [Registrieren Sie URL-Präfixe vorab, um sie an den WebListener zu binden, und richten Sie SSL-Zertifikate ein](#preregister-url-prefixes-and-configure-ssl), wie Sie es auch für die Verwendung in ASP.NET Core tun würden.

Außerdem gibt es [Einstellungen für die Http.Sys-Registrierung](https://support.microsoft.com/kb/820129).


Im Folgenden ist ein Codebeispiel dargestellt, in dem Sie die Verwendung des WebListeners außerhalb von ASP.NET Core sehen können:

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL

Sowohl IIS als auch der WebListener sind abhängig von dem zugrunde liegenden Http.Sys-Kernelmodustreiber, der Anforderungen überwacht und Vorverarbeitungen durchführt. In IIS stellt die Benutzeroberfläche für die Verwaltung eine relativ einfache Möglichkeit zum Konfigurieren dar. Wenn Sie hingegen WebListener verwenden, müssen Sie Http.Sys manuell konfigurieren. Dafür ist das Tool „netsh.exe“ in WebListener integriert. 

„netsh.exe“ wird vor allem dazu verwendet, URL-Präfixe zu reservieren und SSL-Zertifikate zuzuweisen.

„Netsh.exe“ ist ein benutzerfreundliches Tool, das auch von Anfängern verwendet werden kann. Im folgenden Beispiel wird dargestellt, welche Elemente mindestens benötigt werden, um URL-Präfixe für die Ports 80 und 443 zu reservieren:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Im folgenden Beispiel wird dargestellt, wie Sie ein SSL-Zertifikat zuweisen:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Die offizielle Referenzdokumentation finden Sie unter den folgenden Links:

* [Netsh Commands for Hypertext Transfer Protocol (HTTP) (Netsh-Befehle für Hypertext Transfer-Protokolle (HTTP))](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix Strings (UrlPrefix-Zeichenfolgen)](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

In den folgenden Ressourcen finden Sie detaillierte Anweisungen zu verschiedenen Szenarios. Artikel, die sich auf `HttpListener` beziehen, beziehen sich auch auf `WebListener`, da beide Tools auf Http.Sys basieren.

* [Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication – HttpListener based Hosting and Client Certification (HTTPS-Kommunikation: HttpListener basierend auf Hosting- und Clientzertifizierung)](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Hierbei handelt es sich um einen Blog eines Drittanbieters, der zwar schon recht alt ist, aber trotzdem nützliche Informationen enthält.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server (Vorgehensweise: Exemplarische Vorgehensweise zum Verwenden von nicht verwaltetem Code (C++) für HttpListener oder Http-Server als einfacher SSL-Server)](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Hierbei handelt es sich ebenfalls um einen älteren Blog mit nützlichen Informationen.
* [How Do I Set Up A .NET Core WebListener With SSL? (Einrichten eines .NET Core-WebListeners mit SSL)](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Die Verwendung der folgenden Tools von Drittanbietern ist möglicherweise einfacher als die Verwendung der netsh.exe-Befehlszeile. Diese Tools werden allerdings von Microsoft weder bereitgestellt noch unterstützt. Die Tools werden standardmäßig als Administratoren ausgeführt, da „netsh.exe“ Administratorberechtigungen erfordert.

* Der [Http.sys-Manager](http://httpsysmanager.codeplex.com/) stellt eine Benutzeroberfläche bereit, die zum Auflisten und Konfigurieren von SSL-Zertifikaten und -Optionen, Präfixreservierungen und Zertifikatsvertrauenslisten verwendet werden kann. 
* Mithilfe von [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie SSL-Zertifikate und URL-Präfixe auflisten und konfigurieren. Die Benutzeroberfläche ist besser optimiert als beim Http.sys-Manager und beinhaltet einige zusätzliche Optionen für die Konfiguration. Ansonsten sind die Funktionen allerdings ähnlich. Es können zwar keine neuen Zertifikatsvertrauenslisten erstellt werden, jedoch können bereits vorhandene Listen hinzugefügt werden.

Wenn Sie selbstsignierte SSL-Zertifikate generieren möchten, können Sie die Befehlszeilenprogramme [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) und das PowerShell-Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate) von Microsoft verwenden. Es gibt außerdem Benutzeroberflächentools, die Ihnen das Generieren selbstsignierter SSL-Zertifikate erleichtern:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Beispiel-App zu diesem Artikel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener source code (Quellcode für den WebListener)](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/host/index)
