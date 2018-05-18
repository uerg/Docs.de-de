---
title: IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core
author: shirhatti
description: Ermitteln Sie die Unterstützung für das Debuggen von ASP.NET Core-apps, wenn IIS unter Windows Server zurückliegt.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core

Durch [Sourabh Shirhatti](https://twitter.com/sshirhatti) und [Luke Latham](https://github.com/guardrex)

Dieser Artikel beschreibt [Visual Studio](https://www.visualstudio.com/vs/) Unterstützung für das Debuggen von ASP.NET Core-apps, die hinter IIS unter Windows Server ausgeführt. Dieses Thema führt Sie durch die Aktivierung dieser Funktion und Einrichten eines Projekts.

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Aktivieren von IIS

1. Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).
1. Wählen Sie die **Internetinformationsdienste (IIS)** Kontrollkästchen.

![Windows-Features mit Internetinformationsdienste (IIS) das Kontrollkästchen aktiviert als schwarze Quadrat (nicht mit einem Häkchen), der angibt, dass einige der IIS-Funktionen aktiviert sind](development-time-iis-support/_static/enable_iis.png)

Die IIS-Installation möglicherweise einen Neustart des Systems erforderlich.

## <a name="configure-iis"></a>Konfigurieren von IIS

IIS muss es sich um eine Website wie folgt konfiguriert sein:

* Hostname für ein Hostnamen an, der die app-Start-Profil-URL entspricht.
* Die Bindung für Port 443 mit einem zugeordneten Zertifikat.

Z. B. die **Hostnamen** für eine zusätzliche Website auf "Localhost" (das Start-Profil verwenden ebenfalls "Localhost" weiter unten in diesem Thema) festgelegt ist. Der Port ist auf "443" (HTTPS) festgelegt. Die **IIS Express Entwicklungszertifikat** funktioniert auf die Website, aber ein gültiges Zertifikat zugewiesen:

![Fügen Sie Fenster in IIS mit den Satz der Bindung für "localhost" über Port 443 mit einem Zertifikat zugeordnet.](development-time-iis-support/_static/add-website-window.png)

Verfügt die IIS-Installation bereits eine **Default Web Site** mit einem Hostnamen, die Hostnamen für die app starten Profil-URL entspricht:

* Fügen Sie eine portbindung für Port 443 (HTTPS) hinzu.
* Weisen Sie ein gültiges Zertifikat der Website an.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Aktivieren Sie die Entwicklungszeit IIS-Unterstützung in Visual Studio

1. Starten Sie Visual Studio-Installer.
1. Wählen Sie die **Entwicklungszeit IIS unterstützen** Komponente. Die Komponente aufgelistet, als optional in der **Zusammenfassung** Bereich für die **ASP.NET und zur Webentwicklung** arbeitsauslastung. Die Komponente installiert den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), dies ist ein systemeigenes IIS-Modul, das zum Ausführen von ASP.NET Core apps hinter IIS in einer reverse-Proxy-Konfiguration erforderlich.

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt. Im Bereich „Web und Cloud“ ist der Bereich „ASP.NET und Webentwicklung“ ausgewählt. Auf der rechten Seite im optionalen Bereich der Bereiche "Zusammenfassung" vorliegt ein Kontrollkästchen für Entwicklungszeit, IIS zu unterstützen.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurieren des Projekts

### <a name="https-redirection"></a>HTTPS-Umleitung

Für ein neues Projekt, wählen Sie dieses Kontrollkästchen, um **für HTTPS konfigurieren** in der **neue ASP.NET-Webanwendung für Core** Fenster:

![Neue ASP.NET Core Web Application-Fenster mit der für HTTPS-Kontrollkästchen konfigurieren.](development-time-iis-support/_static/new-app.png)

Verwenden Sie in einem vorhandenen Projekt HTTPS-Umleitung Middleware in `Startup.Configure` durch Aufrufen der [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) Erweiterungsmethode:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Starten Sie IIS Profil

Erstellen Sie ein neues Launch-Profil Entwicklungszeit IIS-Unterstützung hinzuzufügen:

1. Für **Profil**, wählen die **neu** Schaltfläche. Nennen Sie das Profil "IIS" im Popup-Fenster. Wählen Sie **OK** zum Erstellen des Profils.
1. Für die **starten** wählen Sie die Einstellung **IIS** aus der Liste.
1. Wählen Sie das Kontrollkästchen für **starten Browser** , und geben Sie die Endpunkt-URL. Verwenden Sie das HTTPS-Protokoll. In diesem Beispiel wird `https://localhost/WebApplication1` verwendet.
1. In der **Umgebungsvariablen** Abschnitt der **hinzufügen** Schaltfläche. Geben Sie eine Umgebungsvariable mit dem Schlüssel `ASPNETCORE_ENVIRONMENT` und den Wert `Development`.
1. In der **Webservereinstellungen** legen Sie im Bereich der **App-URL**. In diesem Beispiel wird `https://localhost/WebApplication1` verwendet.
1. Speichern Sie das Profil ein.

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist Die Profil- und Starteinstellungen werden auf IIS festgelegt. Die Start-Browser-Funktion aktiviert ist, mit der Adresse https://localhost/WebApplication1. Die gleiche Adresse wird auch im Feld "App-URL" des Web-servereinstellungen Bereichs bereitgestellt.](development-time-iis-support/_static/project_properties.png)

Auch manuell hinzufügen ein Profils starten, das die [launchSettings.json](http://json.schemastore.org/launchsettings) Datei in der app:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Führen Sie das Projekt

Legen Sie die Schaltfläche "ausführen" in der Visual Studio-Benutzeroberfläche, auf die **IIS** Profil, und wählen Sie die Schaltfläche, um die app zu starten:

![Führen Sie Schaltfläche aus "" in der Visual Studio-Symbolleiste auf das Profil "IIS" festgelegt.](development-time-iis-support/_static/toolbar.png)

Visual Studio möglicherweise einen Neustart aufgefordert, wenn Sie nicht als Administrator ausführen. Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.

Wenn eine nicht vertrauenswürdige Entwicklungszertifikat verwendet wird, der Browser benötigen Sie möglicherweise zum Erstellen einer Ausnahme für das Zertifikat nicht vertrauenswürdig.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
* [Erzwingen von HTTPS](xref:security/enforcing-ssl)
