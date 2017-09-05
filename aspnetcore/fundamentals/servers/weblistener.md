---
title: WebListener webserverimplementierung in ASP.NET Core
author: rick-anderson
description: "Führt ein WebListener, einen Webserver für ASP.NET Core unter Windows. Basiert auf Http.Sys-Kernelmodustreiber und ist WebListener eine Alternative zum Kestrel, die für die direkte Verbindung mit dem Internet ohne IIS verwendet werden kann."
keywords: "ASP.NET Core, WebListener, HttpListener, Url-Präfixe, SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: bcd225875cfe2a544581c331231c704094780ea3
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>WebListener webserverimplementierung in ASP.NET Core

Durch [Tom Dykstra](http://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Dieses Thema gilt nur für ASP.NET Core 1.x. In ASP.NET Core 2.0 WebListener heißt [HTTP.sys](httpsys.md).

WebListener ist ein [Webserver für ASP.NET Core](index.md) , die nur unter Windows ausgeführt wird. Es basiert auf der [Http.Sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener ist eine Alternative zum [Kestrel](kestrel.md) , für die direkte Verbindung mit dem Internet ohne Rückgriff auf IIS als reverse-Proxy-Server verwendet werden kann. In der Tat **WebListener kann nicht mit IIS oder IIS Express verwendet werden, als nicht kompatibel mit ist der [ASP.NET Core-Modul](aspnet-core-module.md).**

Obwohl WebListener für ASP.NET Core entwickelt wurde, kann es direkt in jeder beliebigen .NET Core oder .NET Framework-Anwendung über verwendet die [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet-Paket.

WebListener unterstützt die folgenden Funktionen:

- [Windows-Authentifizierung](xref:security/authentication/windowsauth)
- Anschlussfreigabe
- HTTPS mit SNI
- HTTP/2 über TLS (Windows 10)
- Direkte Übertragung
- Zwischenspeichern von Antworten
- WebSockets (Windows 8)

Unterstützte Windows-Versionen:

- Windows 7 und Windows Server 2008 R2 und höher

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)

## <a name="when-to-use-weblistener"></a>WebListener verwenden

WebListener eignet sich für Bereitstellungen, in dem Sie der Server direkt mit dem Internet verfügbar zu machen, ohne mit IIS müssen.

![Weblistener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

Da er auf Http.Sys basiert, erfordern nicht WebListener einen reverse-Proxy-Server für den Schutz vor Angriffen durch. Http.Sys ist ausgereifte Technologie, die für viele Arten von Angriffen geschützt und die Stabilität, Sicherheit und Skalierbarkeit von einem Webserver mit vollem Funktionsumfang. IIS selbst wird als eine HTTP-Listener auf Http.Sys ausgeführt. 

WebListener ist auch eine gute Wahl für interne Bereitstellungen, wenn Sie eine der Funktionen benötigen, bietet, dass Sie mithilfe von Kestrel abrufen können.

![Weblistener kommuniziert direkt mit Ihrem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Gewusst wie: Verwenden von WebListener

Hier ist eine Übersicht der Setupaufgaben für das Hostbetriebssystem und ASP.NET Core-Anwendung.

### <a name="configure-windows-server"></a>Konfigurieren von WindowsServer

* Installieren Sie die Version von .NET, die Ihre Anwendung benötigt werden, z. B. [.NET Core](https://go.microsoft.com/fwlink/?LinkID=827524) oder .NET Framework 4.5.1.

* Zu registrieren Sie URL-Präfixe zum Binden an WebListener und Einrichten von SSL-Zertifikaten

   Wenn Sie URL-Präfixe in Windows nicht vorab registrieren, müssen Sie die Anwendung mit Administratorrechten ausgeführt werden. Die einzige Ausnahme ist, wenn Sie auf "localhost", die über HTTP (nicht "HTTPS") mit einer Portnummer, die größer als 1024 binden. In diesem Fall sind keine Administratorrechte erforderlich.

   Weitere Informationen finden Sie unter [zu Präfixe registrieren und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl) weiter unten in diesem Artikel.

* Öffnen Sie die Firewall-Ports zum Zulassen des Datenverkehrs WebListener zu erreichen.

   Sie können netsh.exe verwenden oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906).

Es gibt auch [Http.Sys-registrierungseinstellungen](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Konfigurieren Sie die ASP.NET Core-Anwendung

* Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Dies installiert außerdem [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) als Abhängigkeit.

* Rufen Sie die [ `UseWebListener` ](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Hosting/WebHostBuilderKestrelExtensions/index.html#Microsoft.AspNetCore.Hosting.WebHostBuilderWebListenerExtensions.UseWebListener.md) Erweiterungsmethode auf [WebHostBuilder](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Hosting/WebHostBuilder/index.html#Microsoft.AspNetCore.Hosting.WebHostBuilder.md) in Ihrer `Main` Methode, dabei werden alle WebListener [Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) und [ Einstellungen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) , die Sie benötigen, wie im folgenden Beispiel gezeigt:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Konfigurieren von URLs und Ports Lauschen an 

  Standardmäßig ASP.NET Core bindet an `http://localhost:5000`. Konfigurieren Sie URL-Präfixe und Ports können Sie die `UseURLs` Erweiterungsmethode, die `urls` Befehlszeilenargument oder das Konfigurationssystem ASP.NET Core. Weitere Informationen finden Sie unter [Hosting](../../fundamentals/hosting.md).

  Web-Listener verwendet die [Http.Sys Präfix Zeichenfolgenformate](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Es sind keine formatanforderungen Präfix Zeichenfolge, die für WebListener spezifisch sind.

  > [!NOTE]
  > Stellen Sie sicher, dass Sie angeben, dass die gleichen Präfixzeichenfolgen in `UseUrls` , die Sie auf dem Server zu registrieren. 

* Stellen Sie sicher, dass Ihre Anwendung nicht für die Ausführung der IIS- oder IIS Express konfiguriert ist.

  Ist in Visual Studio das Standardprofil für den Start für IIS Express.  Zum Ausführen des Projekts als Konsolenanwendung müssen Sie manuell das ausgewählte Profil zu ändern, wie im folgenden Screenshot gezeigt.

  ![Wählen Sie die Konsole app-Profil](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Gewusst wie: Verwenden Sie WebListener außerhalb von ASP.NET Core

* Installieren der [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet-Paket.

* [Zu URL-Präfixe zum Binden an WebListener und Einrichten von SSL-Zertifikaten registrieren](#preregister-url-prefixes-and-configure-ssl) wie bei Verwendung in ASP.NET Core.

Es gibt auch [Http.Sys-registrierungseinstellungen](https://support.microsoft.com/kb/820129).


Hier ist ein Codebeispiel, die WebListener Verwendung außerhalb von ASP.NET Core veranschaulicht:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Zu URL-Präfixe registrieren und Konfigurieren von SSL

Sowohl IIS als auch WebListener basieren auf den zugrunde liegenden Http.Sys-Kernelmodustreiber zum Abhören von Anforderungen und Verarbeitung ursprüngliche. In IIS bietet die Verwaltungsbenutzeroberfläche eine relativ einfache Möglichkeit, alles zu konfigurieren. Allerdings müssen Sie bei Verwendung von WebListener Http.Sys selbst konfigurieren. Die integrierten Tool für die auf diese Weise ist netsh.exe. 

Am häufigsten auszuführenden Aufgaben, denen Sie für netsh.exe verwenden müssen, sind Reservieren von URL-Präfixe und Zuweisen von SSL-Zertifikate.

NetSh.exe ist ein benutzerfreundliches Tool für Anfänger verwenden. Das folgende Beispiel zeigt die absolute Mindestanforderungen zum Reservieren von URL-Präfixe für die Ports 80 und 443 erforderlich sind:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Im folgende Beispiel wird gezeigt, wie ein SSL-Zertifikat zugewiesen werden:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

So sieht die offizielle Dokumentation:

* [Netsh-Befehle für Hypertext Transfer-Protokoll (HTTP)](http://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix Zeichenfolgen](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Die folgenden Ressourcen bieten detaillierte Anweisungen für verschiedene Szenarien. Artikel, die auf verweisen `HttpListener` gelten gleichermaßen für `WebListener`, wie sowohl auf Http.Sys basieren.

* [Vorgehensweise: Konfigurieren eines Anschlusses mit einem SSL-Zertifikat](http://msdn.microsoft.com/library/ms733791.aspx)
* [HTTPS-Kommunikation - HttpListener basierend Hosting und Clientzertifizierung](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) dies ein Drittanbieter-Blog und ist ziemlich ALT, aber weiterhin enthält nützliche Informationen.
* [Gewusst wie: Exemplarische Vorgehensweise mithilfe von HttpListener oder HTTP-Server (C++) Code, wie eine einfache SSL-Server nicht verwaltete](http://blogs.msdn.com/b/jpsanders/archive/2009/09/29/walkthrough-using-httplistener-as-an-ssl-simple-server.aspx) Dies ist eine ältere Blog mit nützlichen Informationen.
* [Wie richte ich ein .NET Core WebListener mit SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Hier sind einige Drittanbieter-Tools, die einfacher zu verwenden als das netsh.exe-Befehlszeile werden können. Diese werden nicht von bereitgestellt oder Unterstützung von Microsoft. Die Tools Ausführen als Administrator standardmäßig, da netsh.exe selbst Administratorrechte erforderlich.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/) bietet eine Benutzeroberfläche Liste und Konfigurieren von SSL-Zertifikate und Optionen, Präfix Reservierungen und Zertifikatsvertrauenslisten. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie aus, oder konfigurieren Sie SSL-Zertifikate und URL-Präfixen. Die Benutzeroberfläche als http.sys Manager verfeinerten und stellt einige weitere Konfigurationsoptionen zur Verfügung, jedoch andernfalls er verfügt über ähnliche Funktionen. Eine neue Zertifikatsvertrauensliste (CTL) kann nicht erstellt werden, aber vorhandene zuweisen können.

Zum Generieren von selbstsignierten SSL-Zertifikate, bietet Microsoft Befehlszeilenprogramme: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) und das PowerShell-Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633). Es gibt auch Drittanbieter-UI-Tools, die Sie zum Generieren von selbstsignierten SSL-Zertifikate erleichtern:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [MakeCert-Benutzeroberfläche](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Beispiel-app für diesen Artikel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener-Quellcode](https://github.com/aspnet/HttpSysServer/)
* [Hosting](../hosting.md)
