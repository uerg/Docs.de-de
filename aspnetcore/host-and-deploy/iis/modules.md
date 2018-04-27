---
title: IIS-Module mit ASP.NET Core
author: guardrex
description: Aktive und inaktive IIS-Module für ASP.NET Core-apps und zum Verwalten von IIS-Module zu ermitteln.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: e88526d997618658f58488adb37ae1e519ea3f59
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="iis-modules-with-aspnet-core"></a>IIS-Module mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

ASP.NET Core-apps werden in einer reverse-Proxy-Konfiguration von IIS gehostet. Einige der systemeigenen IIS-Module und allen Modulen verwaltet IIS nicht zur Verarbeitung von Anforderungen für ASP.NET Core-apps verfügbar. In vielen Fällen bietet ASP.NET Core Alternative zu den Funktionen von IIS-Module für systemeigenen und verwalteten.

## <a name="native-modules"></a>Systemeigene Module

Die Tabelle zeigt die systemeigene IIS-Module, die für reverse-Proxy-Anforderungen an ASP.NET Core apps funktionsfähig sind.

| Modul | Mit ASP.NET Core apps funktionsfähig | ASP.NET Core-Option |
| ------ | :-------------------------------: | ------------------- |
| **Anonyme Authentifizierung**<br>`AnonymousAuthenticationModule` | Ja | |
| **Standardauthentifizierung**<br>`BasicAuthenticationModule` | Ja | |
| **Client-Zertifizierung Clientzertifikatzuordnung-Authentifizierung**<br>`CertificateMappingAuthenticationModule` | Ja | |
| **CGI**<br>`CgiModule` | Nein | |
| **Überprüfung der Konfiguration**<br>`ConfigurationValidationModule` | Ja | |
| **HTTP-Fehler**<br>`CustomErrorModule` | Nein | [Status Code Seiten Middleware](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Benutzerdefinierte Protokollierung**<br>`CustomLoggingModule` | Ja | |
| **Standarddokument**<br>`DefaultDocumentModule` | Nein | [Standardmäßig Dateien Middleware](xref:fundamentals/static-files#serve-a-default-document) |
| **Die Digest-Authentifizierung**<br>`DigestAuthenticationModule` | Ja | |
| **Verzeichnissuche**<br>`DirectoryListingModule` | Nein | [Verzeichnis durchsuchen Middleware](xref:fundamentals/static-files#enable-directory-browsing) |
| **Die dynamische Komprimierung**<br>`DynamicCompressionModule` | Ja | [Antworten komprimierende Middleware](xref:performance/response-compression) |
| **Ablaufverfolgung**<br>`FailedRequestsTracingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Zwischenspeicherung von Dateien**<br>`FileCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| **HTTP-Caching**<br>`HttpCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| **HTTP-Protokollierung**<br>`HttpLoggingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index)<br>Implementierungen: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP-Umleitung**<br>`HttpRedirectionModule` | Ja | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| **IIS-Clientzertifikatzuordnung-Authentifizierung**<br>`IISCertificateMappingAuthenticationModule` | Ja | |
| **IP- und Domäneneinschränkungen**<br>`IpRestrictionModule` | Ja | |
| **ISAPI-Filter**<br>`IsapiFilterModule` | Ja | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Ja | [Middleware](xref:fundamentals/middleware/index) |
| **Protokollunterstützung**<br>`ProtocolSupportModule` | Ja | |
| **Anforderungsfilterung**<br>`RequestFilteringModule` | Ja | [URL umschreiben Middleware `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Anforderungsüberwachung**<br>`RequestMonitorModule` | Ja | |
| **URLs**<br>`RewriteModule` | Ja&#8224; | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| **Serverseitige Include-Dateien**<br>`ServerSideIncludeModule` | Nein | |
| **Komprimierung statischer**<br>`StaticCompressionModule` | Nein | [Antworten komprimierende Middleware](xref:performance/response-compression) |
| **Statischer Inhalt**<br>`StaticFileModule` | Nein | [Middleware für statische Dateien](xref:fundamentals/static-files) |
| **Das Zwischenspeichern von Token**<br>`TokenCacheModule` | Ja | |
| **URI-Cache**<br>`UriCacheModule` | Ja | |
| **URL-Autorisierung**<br>`UrlAuthorizationModule` | Ja | [ASP.NET Core Identität](xref:security/authentication/identity) |
| **Windows-Authentifizierung**<br>`WindowsAuthenticationModule` | Ja | |

&#8224;Das URL Rewrite-Modul des `isFile` und `isDirectory` Übereinstimmungstypen nicht unterstützt, mit ASP.NET Core apps aufgrund der Änderungen in [Verzeichnisstruktur](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Verwaltete Module

Verwaltete Module sind *nicht* mit gehosteten ASP.NET Core-apps, wenn die app-Pool .NET CLR-Version festgelegt ist funktionsfähig **kein verwalteter Code**. ASP.NET Core bietet Middleware Alternativen für mehrere Fälle.

| Modul                  | ASP.NET Core-Option |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Cookie-authentifizierungsmiddleware beziehen.](xref:security/authentication/cookie) |
| OutputCache             | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| Profile                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Sitzung                 | [Sitzung Middleware](xref:fundamentals/app-state) |
| Dateiautorisierung        | |
| UrlMappingsModule       | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| 4.0-UrlRoutingModule fest    | [ASP.NET Core Identität](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS-Manager-anwendungsänderungen

Verwendung von IIS-Manager zum Konfigurieren von Einstellungen, die *"Web.config"* -Datei der app geändert wird. Wenn eine app bereitstellen und einschließlich *"Web.config"*, alle mit IIS-Manager vorgenommenen Änderungen überschrieben werden durch die bereitgestellte *"Web.config"* Datei. Wenn Änderungen an des Servers vorgenommen werden *"Web.config"* Datei, kopieren Sie die aktualisierte *"Web.config"* Datei auf dem Server sofort auf dem lokalen Projekt.

## <a name="disabling-iis-modules"></a>Deaktivieren die IIS-Module

Wenn eine IIS-Modul auf Serverebene konfiguriert ist, die für eine app, die eine Ergänzung der app muss deaktiviert werden *"Web.config"* Datei kann das Modul deaktivieren. Belassen Sie das Modul vorhanden und deaktivieren Sie es über eine Konfigurationseinstellung (falls verfügbar) oder entfernen Sie das Modul aus der app.

### <a name="module-deactivation"></a>Deaktivierung des Moduls

Viele Module bieten eine Konfigurationseinstellung, die deaktiviert werden, ohne dass das Modul aus der app entfernt werden können. Dies ist die einfachste und schnellste Möglichkeit, ein Modul zu deaktivieren. Beispielsweise kann der HTTP-Umleitung Modul deaktiviert werden, mit der  **\<HttpRedirect >** Element im *"Web.config"*:

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

2. Bereitstellen die app ohne einen  **\<Module >** im Abschnitt *"Web.config"*. Wenn eine app bereitgestellt wurde, mit einer *"Web.config"* , enthält der  **\<Module >** Abschnitt, ohne dass entsperrt werden im Abschnitt zuerst im IIS-Manager, den Konfigurations-Manager löst eine Ausnahme aus. Beim Versuch, den Abschnitt zu entsperren. Aus diesem Grund bereitstellen die app ohne einen  **\<Module >** Abschnitt.

3. Entsperren der  **\<Module >** Abschnitt *"Web.config"*. In der **Verbindungen** Randleiste, wählen Sie die Website im **Sites**. In der **Management** öffnen Sie im Bereich der **Dienstkonfigurations-Editor**. Verwenden Sie die Navigationssteuerelemente Auswählen der `system.webServer/modules` Abschnitt. In der **Aktionen** Randleiste auf der rechten Seite auswählen, um **Unlock** Abschnitt.

4. An diesem Punkt ein  **\<Module >** Abschnitt hinzugefügt werden kann die *"Web.config"* -Datei mit einem  **\<entfernen >** Element So entfernen Sie das Modul aus die app. Mehrere  **\<entfernen >** -Elemente hinzugefügt werden können, um mehrere Module zu entfernen. Wenn *"Web.config"* Änderungen auf dem Server vorgenommen werden, nehmen Sie die gleichen Änderungen sofort auf des Projekts *"Web.config"* lokal. Entfernen eines Moduls, das auf diese Weise wird nicht die Verwendung des Moduls mit anderen apps auf dem Server betreffen.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
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

Das URI-Caching-Modul (`UriCacheModule`) kann IIS-Website Cachekonfiguration auf den URL-Ebene. Ohne dieses Modul muss IIS lesen und Analysieren der Konfiguration bei jeder Anforderung auch, wenn die gleiche URL wiederholt angefordert wird. Analyse der Konfiguration führt jede Anforderung eine erhebliche Leistungseinbußen. *Obwohl das URI-Caching-Modul nicht zwingend erforderlich, damit eine gehostete ASP.NET Core app ausgeführt wird, wird empfohlen, dass das Modul für die URI-Zwischenspeichern aktiviert sein, damit alle ASP.NET Core-Bereitstellungen.*

Das HTTP-Caching-Modul (`HttpCacheModule`) implementiert, die IIS-Ausgabecache sowie die Logik zum Zwischenspeichern von Elementen in der HTTP.sys-Caches. Ohne dieses Modul Inhalte nicht mehr im Kernelmodus zwischengespeichert und Cacheprofile werden ignoriert. Entfernen das HTTP-Caching-Modul in der Regel hat nachteilige Auswirkungen auf Leistung und ressourcennutzung. *Obwohl vom HTTP-Caching-Modul nicht zwingend erforderlich, damit eine gehostete ASP.NET Core app ausgeführt wird, wird empfohlen, dass vom HTTP-Modul Zwischenspeichern aktiviert sein, damit alle ASP.NET Core-Bereitstellungen.*

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosten unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Einführung in IIS-Architekturen: Module in IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Übersicht über die IIS-Module](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Anpassen von IIS 7.0-Rollen und Module](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
