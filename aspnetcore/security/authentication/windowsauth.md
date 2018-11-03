---
title: Konfigurieren der Windows-Authentifizierung in ASP.NET Core
author: scottaddie
description: Informationen Sie zum Konfigurieren der Windows-Authentifizierung in ASP.NET Core mit IIS Express, IIS, HTTP.sys und WebListener.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 87fcab75555c1dae0b2815c30d79fd4615df9660
ms.sourcegitcommit: 85f2939af7a167b9694e1d2093277ffc9a741b23
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968292"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurieren der Windows-Authentifizierung in ASP.NET Core

Von [Steve Smith](https://ardalis.com) und [Scott Addie](https://twitter.com/Scott_Addie)

Windows-Authentifizierung können konfiguriert werden, für ASP.NET Core-apps mit IIS gehosteten [HTTP.sys](xref:fundamentals/servers/httpsys), oder [WebListener](xref:fundamentals/servers/weblistener).

## <a name="windows-authentication"></a>Windows-Authentifizierung

Windows-Authentifizierung basiert auf dem Betriebssystem zur Authentifizierung von Benutzern von ASP.NET Core-apps. Sie können Windows-Authentifizierung verwenden, wenn Ihre Server in einem Unternehmensnetzwerk, die mithilfe von Active Directory-domänenidentitäten oder andere Windows-Konten zur Identifizierung von Benutzern ausgeführt wird. Windows-Authentifizierung ist am besten für Intranetumgebungen, in denen Benutzer, Client-Anwendungen und Webserver mit der gleichen Windows-Domäne gehören.

[Weitere Informationen zur Windows-Authentifizierung und der Installation für IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Aktivieren der Windows-Authentifizierung in ASP.NET Core-Apps

Die Visual Studio Web Application-Vorlage kann für die Unterstützung der Windows-Authentifizierung konfiguriert werden.

### <a name="use-the-windows-authentication-app-template"></a>Verwenden Sie die Windows-Authentifizierung-app-Vorlage

In Visual Studio:

1. Erstellen Sie eine neue ASP.NET Core-Webanwendung.
1. Wählen Sie aus der Liste der Vorlagen-Webanwendung.
1. Wählen Sie die **Authentifizierung ändern** Schaltfläche, und wählen **Windows-Authentifizierung**.

Führen Sie die App aus. Der Benutzername wird angezeigt, in der oberen rechten der app.

![Screenshot des Windows-Authentifizierung-Browsers](windowsauth/_static/browser-screenshot.png)

Für die Entwicklung verwenden, die mit IIS Express bietet die Vorlage für alle Konfigurationen, die Windows-Authentifizierung verwendet. Der folgende Abschnitt zeigt, wie Sie eine ASP.NET Core-app für Windows-Authentifizierung manuell konfigurieren.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Visual Studio-Einstellungen für Windows und die anonyme Authentifizierung

Visual Studio-Projekt **Eigenschaften** Seite **Debuggen** Registerkarte enthält das Kontrollkästchen für die Windows-Authentifizierung und die anonyme Authentifizierung.

![Screenshot des Windows-Authentifizierung-Browsers](windowsauth/_static/vs-auth-property-menu.png)

Alternativ können diese beiden Eigenschaften konfiguriert werden, der *"launchsettings.JSON"* Datei:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Aktivieren der Windows-Authentifizierung mit IIS

IIS verwendet den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zum Hosten von ASP.NET Core-apps. Windows-Authentifizierung ist in IIS nicht auf die app konfiguriert. Die folgenden Abschnitte zeigen, wie Sie die IIS-Manager verwenden, um eine ASP.NET Core-app zur Verwendung von Windows-Authentifizierung zu konfigurieren.

### <a name="iis-configuration"></a>IIS-Konfiguration

Den Rollendienst "IIS" für Windows-Authentifizierung zu aktivieren. Weitere Informationen finden Sie unter [Aktivieren der Windows-Authentifizierung in IIS-Rollendienste (Siehe Schritt2)](xref:host-and-deploy/iis/index#iis-configuration).

Middleware für die Integration von IIS ist standardmäßig konfiguriert, um Anforderungen zu authentifizieren. Weitere Informationen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS: IIS-Optionen (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

ASP.NET Core-Modul ist zum Weiterleiten von der Windows-Authentifizierung-Token für die app wird standardmäßig konfiguriert. Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul: Attribute des-Elements AspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Erstellen einer neuen IIS-Website

Geben Sie einen Namen und den Ordner, und erlauben Sie, dass Sie einen neuen Anwendungspool zu erstellen.

### <a name="customize-authentication"></a>Anpassen der Authentifizierung

Öffnen Sie die Authentifizierungsfunktionen für den Standort an.

![Menü "IIS-Authentifizierung"](windowsauth/_static/iis-authentication-menu.png)

Deaktivieren Sie anonyme Authentifizierung und aktivieren Sie der Windows-Authentifizierung.

![IIS-Authentifizierungseinstellungen](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Veröffentlichen Sie Ihr Projekt in der IIS-Site-Ordner

Veröffentlichen Sie die app mithilfe von Visual Studio oder .NET Core-CLI, in den Zielordner aus.

![Visual Studio, Dialogfeld "Veröffentlichen"](windowsauth/_static/vs-publish-app.png)

Erfahren Sie mehr über [in IIS veröffentlichen](xref:host-and-deploy/iis/index).

Starten Sie die app aus, um sicherzustellen, dass die Windows-Authentifizierung funktioniert.

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a>Aktivieren der Windows-Authentifizierung mit HTTP.sys

Obwohl Windows-Authentifizierung von Kestrel nicht unterstützt wird, können Sie [HTTP.sys](xref:fundamentals/servers/httpsys) selbst gehostete Szenarios für Windows zu unterstützen. Im folgenden Beispiel wird der app Web-Host, um die "http.sys" mit der Windows-Authentifizierung zu verwenden:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll. Die Benutzermodusauthentifizierung wird nicht von Kerberos und HTTP.sys unterstützt. Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens/-Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird. Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.

> [!NOTE]
> HTTP.sys wird nicht auf Nano Server-Version 1709 oder höher unterstützt. Um Windows-Authentifizierung und HTTP.sys mit Nano Server verwenden möchten, verwenden eine [Server Core (Microsoft/Windowsservercore) Container](https://hub.docker.com/r/microsoft/windowsservercore/). Weitere Informationen zur Server Core finden Sie unter [Was ist, dass der Server Core-Installationsoption in Windows Server?](/windows-server/administration/server-core/what-is-server-core).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a>Aktivieren der Windows-Authentifizierung mit WebListener

Obwohl Windows-Authentifizierung von Kestrel nicht unterstützt wird, können Sie [WebListener](xref:fundamentals/servers/weblistener) selbst gehostete Szenarios für Windows zu unterstützen. Im folgenden Beispiel wird die app Web-Host zum Verwenden von WebListener mit Windows-Authentifizierung:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> WebListener delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll. Die Benutzermodusauthentifizierung wird nicht von Kerberos und WebListener unterstützt. Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens bzw. -Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird. Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.

::: moniker-end

## <a name="work-with-windows-authentication"></a>Arbeiten mit Windows-Authentifizierung

Der Konfigurationsstatus des anonymen Zugriffs bestimmt die Art, wie die `[Authorize]` und `[AllowAnonymous]` Attribute werden in der app verwendet. In den folgenden zwei Abschnitten wird erläutert, wie die nicht zulässigen und die zulässigen Zustände des anonymen Zugriffs zu behandeln.

### <a name="disallow-anonymous-access"></a>Anonyme Zugriffe nicht zulassen

Wenn Windows-Authentifizierung aktiviert ist und der anonyme Zugriff deaktiviert ist, die `[Authorize]` und `[AllowAnonymous]` Attribute haben keine Auswirkung. Wenn der IIS-Site (oder HTTP.sys oder WebListener-Server) konfiguriert ist, um den anonymen Zugriff verweigern, erreicht die Anforderung nie Ihrer app. Aus diesem Grund die `[AllowAnonymous]` Attribut ist nicht anwendbar.

### <a name="allow-anonymous-access"></a>Anonymen Zugriff zulassen

Wenn sowohl für Windows-Authentifizierung als auch für anonymen Zugriff aktiviert sind, verwenden die `[Authorize]` und `[AllowAnonymous]` Attribute. Die `[Authorize]` Attribut können Sie Teile der app schützen, das wirklich Windows-Authentifizierung erforderlich ist. Die `[AllowAnonymous]` -Attribut überschreibt `[Authorize]` Attribut Nutzung in apps, die anonymen Zugriff zulassen. Finden Sie unter [einfache Autorisierung](xref:security/authorization/simple) Nutzungsdetails Attribut.

In ASP.NET Core 2.x, die `[Authorize]` Attribut erfordert zusätzliche Konfigurationsschritte in *"Startup.cs"* sollen anonyme Anforderungen für die Windows-Authentifizierung. Die empfohlene Konfiguration variiert leicht basierend auf dem Webserver verwendet wird.

> [!NOTE]
> Standardmäßig werden Benutzer, die Autorisierung zum Zugriff auf eine Seite nicht über eine leere HTTP 403-Antwort angezeigt. Die [StatusCodePages Middleware](xref:fundamentals/error-handling#configure-status-code-pages) kann konfiguriert werden, um Benutzern mit "Zugriff verweigert" Benutzerfreundlicher zu ermöglichen.

#### <a name="iis"></a>IIS

Wenn Sie IIS verwenden, fügen Sie den folgenden der `ConfigureServices` Methode:

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Wenn Sie "http.sys" verwenden, fügen Sie den folgenden der `ConfigureServices` Methode:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Identitätswechsel

ASP.NET Core implementiert keine Identitätswechsel. Apps, die mit der Anwendungsidentität für alle Anforderungen, die mithilfe von app-Pool oder Prozess Identität ausgeführt werden. Wenn Sie explizit eine Aktion im Auftrag eines Benutzers ausführen müssen, verwenden Sie `WindowsIdentity.RunImpersonated`. Eine einzelne Aktion in diesem Kontext ausgeführt, und schließen Sie den Kontext.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Beachten Sie, dass `RunImpersonated` nicht asynchrone Vorgänge unterstützt und sollte nicht für komplexe Szenarien verwendet werden. Z. B. wird nicht wrapping gesamte Anforderungen oder Middleware verkettet unterstützt oder empfohlen.
