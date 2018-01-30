---
title: Verwenden IIS-Module mit ASP.NET Core
author: guardrex
description: "Aktive und inaktive IIS-Module für ASP.NET Core-Anwendungen beschreiben Referenzdokumente."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/08/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 1b5391c113ca0b980eb3c47bcce0717d4a4739ed
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>Verwenden IIS-Module mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

ASP.NET Core-Anwendungen werden in einer reverse-Proxy-Konfiguration von IIS gehostet. Einige der systemeigenen IIS-Module und allen Modulen verwaltet IIS nicht zur Verarbeitung von Anforderungen für ASP.NET Core-apps verfügbar. In vielen Fällen bietet ASP.NET Core Alternative zu den Funktionen von IIS-Module für systemeigenen und verwalteten.

## <a name="native-modules"></a>Systemeigene Module

Modul | .NET Core Active | ASP.NET Core Option
--- | :---: | ---
**Anonyme Authentifizierung**<br>`AnonymousAuthenticationModule` | Ja | 
**Standardauthentifizierung**<br>`BasicAuthenticationModule` | Ja | 
**Client-Zertifizierung Clientzertifikatzuordnung-Authentifizierung**<br>`CertificateMappingAuthenticationModule` | Ja | 
**CGI**<br>`CgiModule` | Nein | 
**Überprüfung der Konfiguration**<br>`ConfigurationValidationModule` | Ja | 
**HTTP-Fehler**<br>`CustomErrorModule` | Nein | [Status Code Seiten Middleware](xref:fundamentals/error-handling#configuring-status-code-pages)
**Benutzerdefinierte Protokollierung**<br>`CustomLoggingModule` | Ja | 
**Standarddokument**<br>`DefaultDocumentModule` | Nein | [Standardmäßig Dateien Middleware](xref:fundamentals/static-files#serve-a-default-document)
**Die Digest-Authentifizierung**<br>`DigestAuthenticationModule` | Ja | 
**Verzeichnissuche**<br>`DirectoryListingModule` | Nein | [Verzeichnis durchsuchen Middleware](xref:fundamentals/static-files#enable-directory-browsing)
**Die dynamische Komprimierung**<br>`DynamicCompressionModule` | Ja | [Antworten komprimierende Middleware](xref:performance/response-compression)
**Ablaufverfolgung**<br>`FailedRequestsTracingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index#the-tracesource-provider)
**Zwischenspeicherung von Dateien**<br>`FileCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware)
**HTTP-Caching**<br>`HttpCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware)
**HTTP-Protokollierung**<br>`HttpLoggingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index)<br>Implementierungen: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**HTTP-Umleitung**<br>`HttpRedirectionModule` | Ja | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting)
**IIS-Clientzertifikatzuordnung-Authentifizierung**<br>`IISCertificateMappingAuthenticationModule` | Ja | 
**IP- und Domäneneinschränkungen**<br>`IpRestrictionModule` | Ja | 
**ISAPI-Filter**<br>`IsapiFilterModule` | Ja | [Middleware](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Ja | [Middleware](xref:fundamentals/middleware)
**Protokollunterstützung**<br>`ProtocolSupportModule` | Ja | 
**Anforderungsfilterung**<br>`RequestFilteringModule` | Ja | [URL umschreiben Middleware`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Anforderungsüberwachung**<br>`RequestMonitorModule` | Ja | 
**URLs**<br>`RewriteModule` | Yes† | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting)
**Serverseitige Include-Dateien**<br>`ServerSideIncludeModule` | Nein | 
**Komprimierung statischer**<br>`StaticCompressionModule` | Nein | [Antworten komprimierende Middleware](xref:performance/response-compression)
**Statischer Inhalt**<br>`StaticFileModule` | Nein | [Middleware für statische Dateien](xref:fundamentals/static-files)
**Das Zwischenspeichern von Token**<br>`TokenCacheModule` | Ja | 
**URI-Cache**<br>`UriCacheModule` | Ja | 
**URL-Autorisierung**<br>`UrlAuthorizationModule` | Ja | [ASP.NET Core Identität](xref:security/authentication/identity)
**Windows-Authentifizierung**<br>`WindowsAuthenticationModule` | Ja | 

†The-URL-Rewrite-Modul `isFile` und `isDirectory` nicht arbeiten mit ASP.NET Core apps aufgrund der Änderungen im [Verzeichnisstruktur](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Verwaltete Module

Modul | .NET Core Active | ASP.NET Core Option
--- | :---: | ---
AnonymousIdentification | Nein | 
DefaultAuthentication | Nein | 
FileAuthorization | Nein | 
FormsAuthentication | Nein | [Cookie-authentifizierungsmiddleware beziehen.](xref:security/authentication/cookie)
OutputCache | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware)
Profile | Nein | 
RoleManager | Nein | 
ScriptModule-4.0 | Nein | 
Sitzung | Nein | [Sitzung Middleware](xref:fundamentals/app-state)
UrlAuthorization | Nein | 
UrlMappingsModule | Nein | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting)
UrlRoutingModule-4.0 | Nein | [ASP.NET Core Identität](xref:security/authentication/identity)
WindowsAuthentication | Nein | 

## <a name="iis-manager-application-changes"></a>IIS-Manager-anwendungsänderungen

Mit IIS-Manager zum Konfigurieren von Einstellungen, die *"Web.config"* -Datei der app geändert wird. Wenn eine app bereitstellen und einschließlich *"Web.config"*, alle mit IIS-Manager vorgenommenen Änderungen überschrieben werden durch die bereitgestellte *"Web.config"* Datei. Wenn Änderungen an des Servers vorgenommen werden *"Web.config"* Datei, kopieren Sie die aktualisierte *"Web.config"* Datei zum lokalen Projekt sofort.

## <a name="disabling-iis-modules"></a>Deaktivieren die IIS-Module

Wenn eine IIS-Modul auf Serverebene konfiguriert ist, die für eine app, die eine Ergänzung der app muss deaktiviert werden *"Web.config"* Datei kann das Modul deaktivieren. Belassen Sie das Modul vorhanden und deaktivieren Sie es über eine Konfigurationseinstellung (falls verfügbar) oder entfernen Sie das Modul aus der app.

### <a name="module-deactivation"></a>Deaktivierung des Moduls

Viele Module bieten eine Konfigurationseinstellung, die möglich ist, Sie werden sie deaktiviert, ohne sie aus der app entfernt werden. Dies ist die einfachste und schnellste Möglichkeit, ein Modul zu deaktivieren. Verwenden Sie z. B. möchten, deaktivieren Sie das IIS URL Rewrite-Modul, das `<httpRedirect>` -Element wie unten dargestellt. Weitere Informationen zu Modulen mit Konfigurationseinstellungen zu deaktivieren, führen Sie die Links in der *Unterelemente* Abschnitt [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Entfernen des Moduls

Wenn die Entscheidung zum Entfernen eines Moduls mit einer Einstellung in *"Web.config"*, entsperren Sie das Modul und Entsperren der `<modules>` Abschnitt *"Web.config"* erste. Im folgenden werden die Schritte erläutert:

1. Entsperren Sie das Modul auf Serverebene. Klicken Sie auf dem IIS-Server im IIS-Manager **Verbindungen** Randleiste. Öffnen der **Module** in der **IIS** Bereich. Klicken Sie auf das Modul in der Liste. In der **Aktionen** Randleiste auf der rechten Seite klicken Sie auf **Unlock**. Entsperren Sie so viele Module wie geplant sind, aufheben *"Web.config"* später erneut.

2. Bereitstellen die app ohne einen `<modules>` im Abschnitt *"Web.config"*. Wenn eine app bereitgestellt wurde, mit einer *"Web.config"* , enthält die `<modules>` Abschnitt, ohne dass entsperrt werden im Abschnitt zuerst im IIS-Manager, den Konfigurations-Manager löst eine Ausnahme aus, bei dem Versuch, die im Abschnitt zu entsperren. Aus diesem Grund bereitstellen die app ohne einen `<modules>` Abschnitt.

3. Entsperren der `<modules>` Abschnitt *"Web.config"*. In der **Verbindungen** Randleiste, klicken Sie auf der Website im **Sites**. In der **Management** öffnen Sie im Bereich der **Dienstkonfigurations-Editor**. Verwenden Sie die Navigationssteuerelemente Auswählen der `system.webServer/modules` Abschnitt. In der **Aktionen** Randleiste auf der rechten Seite klicken Sie auf, um **Unlock** Abschnitt.

4. An diesem Punkt ein `<modules>` Abschnitt hinzugefügt werden kann die *"Web.config"* -Datei mit einem `<remove>` Element zum Entfernen des Moduls aus der app. Mehrere `<remove>` -Elemente hinzugefügt werden können, um mehrere Module zu entfernen. Vergessen Sie nicht, auch wenn *"Web.config"* werden Änderungen auf dem Server sofort im Projekt lokal machen. Entfernen eines Moduls, das auf diese Weise wird nicht die Verwendung des Moduls mit anderen apps auf dem Server betreffen.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Für eine IIS-Installation mit den Standard-Modulen, die installiert werden soll, verwenden Sie die folgenden `<module>` Abschnitt aus, um die Standardmodule zu entfernen.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

Ein IIS-Modul kann auch entfernt werden, mit *Appcmd.exe*. Geben Sie die `MODULE_NAME` und `APPLICATION_NAME` in dem unten gezeigten Befehl:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

So entfernen Sie die `DynamicCompressionModule` aus Default Web Site:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Minimalen Modulkonfiguration

Nur Module zum Ausführen einer app ASP.NET Core erforderlich sind, die anonyme Authentifizierung-Modul und ASP.NET Core-Modul.

![IIS-Manager öffnen, um Module mit der minimalen Modulkonfiguration angezeigt](modules/_static/modules.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosten unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Übersicht über die IIS-Module](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Anpassen von IIS 7.0-Rollen und Module](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
