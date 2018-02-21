---
title: Verwenden IIS-Module mit ASP.NET Core
author: guardrex
description: "Aktive und inaktive IIS-Module für ASP.NET Core-apps und zum Verwalten von IIS-Module zu ermitteln."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 5032c9f07af4f9291b44538cecbc310bfabc8e02
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>Verwenden IIS-Module mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

ASP.NET Core-apps werden in einer reverse-Proxy-Konfiguration von IIS gehostet. Einige der systemeigenen IIS-Module und allen Modulen verwaltet IIS nicht zur Verarbeitung von Anforderungen für ASP.NET Core-apps verfügbar. In vielen Fällen bietet ASP.NET Core Alternative zu den Funktionen von IIS-Module für systemeigenen und verwalteten.

## <a name="native-modules"></a>Systemeigene Module

| Modul | .NET Core Active | ASP.NET Core Option |
| ------ | :--------------: | ------------------- |
| **Anonyme Authentifizierung**<br>`AnonymousAuthenticationModule` | Ja | |
| **Standardauthentifizierung**<br>`BasicAuthenticationModule` | Ja | |
| **Client-Zertifizierung Clientzertifikatzuordnung-Authentifizierung**<br>`CertificateMappingAuthenticationModule` | Ja | |
| **CGI**<br>`CgiModule` | Nein | |
| **Überprüfung der Konfiguration**<br>`ConfigurationValidationModule` | Ja | |
| HTTP-Fehler<br>`CustomErrorModule` | Nein | [Status Code Seiten Middleware](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Benutzerdefinierte Protokollierung**<br>`CustomLoggingModule` | Ja | |
| Standarddokument<br>`DefaultDocumentModule` | Nein | [Standardmäßig Dateien Middleware](xref:fundamentals/static-files#serve-a-default-document) |
| **Die Digest-Authentifizierung**<br>`DigestAuthenticationModule` | Ja | |
| Verzeichnissuche<br>`DirectoryListingModule` | Nein | [Verzeichnis durchsuchen Middleware](xref:fundamentals/static-files#enable-directory-browsing) |
| **Die dynamische Komprimierung**<br>`DynamicCompressionModule` | Ja | [Antworten komprimierende Middleware](xref:performance/response-compression) |
| **Ablaufverfolgung**<br>`FailedRequestsTracingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Zwischenspeicherung von Dateien**<br>`FileCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| **HTTP-Caching**<br>`HttpCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| HTTP-Protokollierung<br>`HttpLoggingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index)<br>Implementierungen: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP-Umleitung**<br>`HttpRedirectionModule` | Ja | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| **IIS-Clientzertifikatzuordnung-Authentifizierung**<br>`IISCertificateMappingAuthenticationModule` | Ja | |
| **IP- und Domäneneinschränkungen**<br>`IpRestrictionModule` | Ja | |
| **ISAPI-Filter**<br>`IsapiFilterModule` | Ja | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Ja | [Middleware](xref:fundamentals/middleware/index) |
| **Protokollunterstützung**<br>`ProtocolSupportModule` | Ja | |
| Anforderungsfilterung<br>`RequestFilteringModule` | Ja | [URL umschreiben Middleware `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| Anforderungsüberwachung<br>`RequestMonitorModule` | Ja | |
| **URLs**<br>`RewriteModule` | Ja &#8224; | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| **Serverseitige Include-Dateien**<br>`ServerSideIncludeModule` | Nein | |
| **Komprimierung statischer**<br>`StaticCompressionModule` | Nein | [Antworten komprimierende Middleware](xref:performance/response-compression) |
| Statischer Inhalt<br>`StaticFileModule` | Nein | [Middleware für statische Dateien](xref:fundamentals/static-files) |
| **Das Zwischenspeichern von Token**<br>`TokenCacheModule` | Ja | |
| **URI-Cache**<br>`UriCacheModule` | Ja | |
| **URL-Autorisierung**<br>`UrlAuthorizationModule` | Ja | [ASP.NET Core Identität](xref:security/authentication/identity) |
| **Windows-Authentifizierung**<br>`WindowsAuthenticationModule` | Ja | |

&#8224; Das URL Rewrite-Modul des `isFile` und `isDirectory` Übereinstimmungstypen nicht unterstützt, mit ASP.NET Core apps aufgrund der Änderungen in [Verzeichnisstruktur](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Verwaltete Module

| Modul                  | .NET Core Active | ASP.NET Core Option |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | Nein               | |
| DefaultAuthentication   | Nein               | |
| FileAuthorization       | Nein               | |
| FormsAuthentication     | Nein               | [Cookie-authentifizierungsmiddleware beziehen.](xref:security/authentication/cookie) |
| OutputCache             | Nein               | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| Profile                 | Nein               | |
| RoleManager             | Nein               | |
| ScriptModule-4.0        | Nein               | |
| Sitzung                 | Nein               | [Sitzung Middleware](xref:fundamentals/app-state) |
| UrlAuthorization        | Nein               | |
| UrlMappingsModule       | Nein               | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | Nein               | [ASP.NET Core Identität](xref:security/authentication/identity) |
| WindowsAuthentication   | Nein               | |

## <a name="iis-manager-application-changes"></a>IIS-Manager-anwendungsänderungen

Verwendung von IIS-Manager zum Konfigurieren von Einstellungen, die *"Web.config"* -Datei der app geändert wird. Wenn eine app bereitstellen und einschließlich *"Web.config"*, alle mit IIS-Manager vorgenommenen Änderungen überschrieben werden durch die bereitgestellte *"Web.config"* Datei. Wenn Änderungen an des Servers vorgenommen werden *"Web.config"* Datei, kopieren Sie die aktualisierte *"Web.config"* Datei auf dem Server sofort auf dem lokalen Projekt.

## <a name="disabling-iis-modules"></a>Deaktivieren die IIS-Module

Wenn eine IIS-Modul auf Serverebene konfiguriert ist, die für eine app, die eine Ergänzung der app muss deaktiviert werden *"Web.config"* Datei kann das Modul deaktivieren. Belassen Sie das Modul vorhanden und deaktivieren Sie es über eine Konfigurationseinstellung (falls verfügbar) oder entfernen Sie das Modul aus der app.

### <a name="module-deactivation"></a>Deaktivierung des Moduls

Viele Module bieten eine Konfigurationseinstellung, die deaktiviert werden, ohne dass das Modul aus der app entfernt werden können. Dies ist die einfachste und schnellste Möglichkeit, ein Modul zu deaktivieren. Beispielsweise kann das IIS URL Rewrite-Modul deaktiviert werden, mit der  **\<HttpRedirect >** Element im *"Web.config"*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Weitere Informationen zu Modulen mit Konfigurationseinstellungen zu deaktivieren, führen Sie die Links in der *Unterelemente* Abschnitt [IIS \<"System.Webserver" >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Entfernen des Moduls

Wenn die Entscheidung zum Entfernen eines Moduls mit einer Einstellung in *"Web.config"*, entsperren Sie das Modul und Entsperren der  **\<Module >** Abschnitt *"Web.config"* erste:

1. Entsperren Sie das Modul auf Serverebene. Wählen Sie den IIS-Server im IIS-Manager **Verbindungen** Randleiste. Öffnen der **Module** in der **IIS** Bereich. Wählen Sie das Modul in der Liste aus. In der **Aktionen** wählen Sie auf der rechten Seite Randleiste **Unlock**. Entsperren Sie so viele Module, wie Sie aufheben möchten *"Web.config"* später erneut.

1. Bereitstellen die app ohne einen  **\<Module >** im Abschnitt *"Web.config"*. Wenn eine app bereitgestellt wurde, mit einer *"Web.config"* , enthält der  **\<Module >** Abschnitt, ohne dass entsperrt werden im Abschnitt zuerst im IIS-Manager, den Konfigurations-Manager löst eine Ausnahme aus. Beim Versuch, den Abschnitt zu entsperren. Aus diesem Grund bereitstellen die app ohne einen  **\<Module >** Abschnitt.

1. Entsperren der  **\<Module >** Abschnitt *"Web.config"*. In der **Verbindungen** Randleiste, wählen Sie die Website im **Sites**. In der **Management** öffnen Sie im Bereich der **Dienstkonfigurations-Editor**. Verwenden Sie die Navigationssteuerelemente Auswählen der `system.webServer/modules` Abschnitt. In der **Aktionen** Randleiste auf der rechten Seite auswählen, um **Unlock** Abschnitt.

1. An diesem Punkt ein  **\<Module >** Abschnitt hinzugefügt werden kann die *"Web.config"* -Datei mit einem  **\<entfernen >** Element So entfernen Sie das Modul aus die app. Mehrere  **\<entfernen >** -Elemente hinzugefügt werden können, um mehrere Module zu entfernen. Wenn *"Web.config"* Änderungen auf dem Server vorgenommen werden, nehmen Sie die gleichen Änderungen sofort auf des Projekts *"Web.config"* lokal. Entfernen eines Moduls, das auf diese Weise wird nicht die Verwendung des Moduls mit anderen apps auf dem Server betreffen.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Für eine IIS-Installation mit den Standard-Modulen, die installiert werden soll, verwenden Sie die folgenden  **\<Modul >** Abschnitt aus, um die Standardmodule zu entfernen.

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

Ein IIS-Modul kann auch entfernt werden, mit *Appcmd.exe*. Geben Sie die `MODULE_NAME` und `APPLICATION_NAME` im Befehl:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Entfernen Sie z. B. die `DynamicCompressionModule` aus Default Web Site:

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
