---
title: Konfigurieren der Windows-Authentifizierung in ASP.NET Core
author: ardalis
description: Dieser Artikel beschreibt, wie die Windows-Authentifizierung in ASP.NET Core mit IIS Express, IIS, HTTP.sys und WebListener konfiguriert.
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: dbcef095561fe656bdd28c4fa6560c6b269a2db0
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2018
ms.locfileid: "34689008"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurieren der Windows-Authentifizierung in ASP.NET Core

Von [Steve Smith](https://ardalis.com) und [Scott Addie](https://twitter.com/Scott_Addie)

Windows-Authentifizierung konfiguriert werden kann, für ASP.NET Core-apps, die mit IIS gehostet [HTTP.sys](xref:fundamentals/servers/httpsys), oder [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>Was ist Windows-Authentifizierung?

Windows-Authentifizierung basiert auf dem Betriebssystem zur Authentifizierung von Benutzern von ASP.NET Core-apps. Sie können Windows-Authentifizierung verwenden, wenn Ihre Server in einem Unternehmensnetzwerk mithilfe von Active Directory-Domäne-Identitäten oder andere Windows-Konten zur Identifizierung von Benutzern ausgeführt wird. Windows-Authentifizierung ist für Intranetumgebungen, in denen Benutzer, Clientanwendungen und Webservern zu derselben Windows-Domäne gehören, geeignet.

[Erfahren Sie mehr über Windows-Authentifizierung und die Installation für IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Windows-Authentifizierung in einer ASP.NET Core app aktivieren

Der Visual Studio Web Application-Vorlage kann für die Unterstützung der Windows-Authentifizierung konfiguriert werden.

### <a name="use-the-windows-authentication-app-template"></a>Verwenden Sie die Windows-Authentifizierung app-Vorlage

In Visual Studio:
1. Erstellen Sie eine neue ASP.NET Core-Webanwendung. 
1. Wählen Sie aus der Liste der Vorlagen-Webanwendung.
1. Wählen Sie die **Authentifizierung ändern** Schaltfläche und wählen Sie **Windows-Authentifizierung**. 

Führen Sie die App aus. Der Benutzername wird angezeigt, in der oberen rechten der app.

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/browser-screenshot.png)

Die Vorlage bereitstellt für Entwicklungen mit IIS Express die zur Verwendung der Windows-Authentifizierung erforderlich Konfiguration. Der folgende Abschnitt zeigt, wie eine ASP.NET Core-app für Windows-Authentifizierung manuell konfiguriert wird.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Visual Studio-Einstellungen für Windows und anonyme Authentifizierung

Visual Studio-Projekt **Eigenschaften** Seite **Debuggen** Registerkarte enthält die Kontrollkästchen für die Windows-Authentifizierung und die anonyme Authentifizierung.

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/vs-auth-property-menu.png)

Alternativ können diese beiden Eigenschaften konfiguriert werden, der *launchSettings.json* Datei:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Windows-Authentifizierung in IIS aktivieren

IIS verwendet den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) Host ASP.NET Core-Apps. Das Modul ermöglicht Windows-Authentifizierung in IIS standardmäßig fließen. Windows-Authentifizierung ist in IIS nicht auf die app konfiguriert. Die folgenden Abschnitte zeigen, wie IIS-Manager verwenden, um eine ASP.NET Core-Anwendung, um die Windows-Authentifizierung zu konfigurieren.

### <a name="create-a-new-iis-site"></a>Erstellen einer neuen IIS-Website

Geben Sie einen Namen und den Ordner, und lassen sie einen neuen Anwendungspool zu erstellen.

### <a name="customize-authentication"></a>Anpassen der Authentifizierung

Öffnen Sie die Authentifizierung für den Standort.

![Menü für IIS-Authentifizierung](windowsauth/_static/iis-authentication-menu.png)

Deaktivieren Sie anonyme Authentifizierung und aktivieren Sie die Windows-Authentifizierung.

![IIS-Authentifizierungseinstellungen](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Veröffentlichen Sie Ihr Projekt in den IIS-Website-Ordner

Veröffentlichen Sie die app mithilfe von Visual Studio oder die .NET Core-CLI, in den Zielordner an.

![Visual Studio, Dialogfeld "Veröffentlichen"](windowsauth/_static/vs-publish-app.png)

Erfahren Sie mehr über [in IIS veröffentlichen](xref:host-and-deploy/iis/index).

Starten Sie die app aus, um sicherzustellen, dass die Windows-Authentifizierung funktionsfähig ist.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Windows-Authentifizierung mit HTTP.sys oder WebListener aktivieren

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Obwohl Kestrel Windows-Authentifizierung nicht unterstützt, können Sie [HTTP.sys](xref:fundamentals/servers/httpsys) selbst gehostete Szenarien unter Windows unterstützt. Das folgende Beispiel konfiguriert die app Webhost um HTTP.sys mit Windows-Authentifizierung zu verwenden:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Obwohl Kestrel Windows-Authentifizierung nicht unterstützt, können Sie [WebListener](xref:fundamentals/servers/weblistener) selbst gehostete Szenarien unter Windows unterstützt. Das folgende Beispiel konfiguriert die app Webhost um WebListener mit Windows-Authentifizierung zu verwenden:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Arbeiten mit Windows-Authentifizierung

Der Konfigurationsstatus des anonymen Zugriffs bestimmt die Art, wie die `[Authorize]` und `[AllowAnonymous]` Attribute werden in der app verwendet. Die folgenden beiden Abschnitten wird erläutert, wie die Zustände, zulässige und unzulässige Konfiguration des anonymen Zugriffs zu behandeln.

### <a name="disallow-anonymous-access"></a>Anonyme Zugriffe nicht zulassen

Wenn Windows-Authentifizierung aktiviert ist und der anonyme Zugriff deaktiviert ist, die `[Authorize]` und `[AllowAnonymous]` Attribute haben keine Auswirkungen. Wenn die IIS-Website (oder HTTP.sys oder WebListener Server) konfiguriert ist, um den anonymen Zugriff zu unterbinden, erreicht die Anforderung nie Ihrer app. Aus diesem Grund die `[AllowAnonymous]` Attribut nicht zutreffend ist.

### <a name="allow-anonymous-access"></a>Anonymen Zugriff zulassen

Wenn Windows-Authentifizierung und anonymem Zugriff aktiviert sind, verwenden die `[Authorize]` und `[AllowAnonymous]` Attribute. Die `[Authorize]` Attribut können Sie Teile der app zu sichern die Windows-Authentifizierung tatsächlich benötigen. Die `[AllowAnonymous]` -Attribut überschreibt `[Authorize]` -Attribut Auslastung in apps, die anonymen Zugriff zu ermöglichen. Finden Sie unter [einfache Autorisierung](xref:security/authorization/simple) für Nutzungsdetails Attribut.

In ASP.NET Core 2.x, die `[Authorize]` Attribut erfordert zusätzliche Konfigurationsschritte in *Startup.cs* , fordern Sie anonyme Anforderungen für die Windows-Authentifizierung. Die empfohlene Konfiguration variiert leicht basierend auf dem Webserver verwendet wird.

> [!NOTE]
> Standardmäßig werden Benutzer Autorisierung zum Zugriff auf einer Seite fehlender mit einer leeren HTTP 403-Antwort angezeigt. Die [StatusCodePages Middleware](xref:fundamentals/error-handling#configuring-status-code-pages) kann konfiguriert werden, um Benutzern eine Vereinfachung der Nutzung von "Zugriff verweigert" bereitstellen.

#### <a name="iis"></a>IIS

Wenn Sie IIS verwenden, fügen Sie Folgendes verwenden, um die `ConfigureServices` Methode: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Wenn HTTP.sys verwenden zu können, fügen Sie Folgendes verwenden, um die `ConfigureServices` Methode:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Identitätswechsel

ASP.NET Core implementiert keine Identitätswechsel. Apps, die mit der Anwendungsidentität für alle Anforderungen, die mit app-Pool oder Prozess Identität ausgeführt werden. Wenn Sie explizit eine Aktion im Auftrag eines Benutzers ausführen müssen, verwenden Sie `WindowsIdentity.RunImpersonated`. Eine einzelne Aktion in diesem Kontext ausgeführt, und schließen Sie dann den Kontext.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Beachten Sie, dass `RunImpersonated` keine asynchrone Vorgänge unterstützt und sollte nicht für komplexe Szenarios verwendet werden. Z. B. wird nicht wrapping gesamten Anfragen oder Middleware Ketten unterstützt oder empfohlen.

---
