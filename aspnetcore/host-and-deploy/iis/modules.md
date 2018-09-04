---
title: IIS-Module mit ASP.NET Core
author: guardrex
description: Lernen Sie aktive und inaktive IIS-Module für ASP.NET Core-Apps kennen, und erfahren Sie, wie Sie diese verwalten können.
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 40af94f9cbb83f27f22d90b6b0f2854090687d34
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312345"
---
# <a name="iis-modules-with-aspnet-core"></a>IIS-Module mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

ASP.NET Core-Apps werden von IIS in einer Reverseproxykonfiguration gehostet. Einige der nativen IIS-Module und sämtliche der IIS-verwalteten Module stehen für die Verarbeitung von Anforderungen für ASP.NET Core-Apps nicht zur Verfügung. In vielen Fällen bietet ASP.NET Core eine Alternative zu den Features der nativen IIS-Module und der IIS-verwalteten Module.

## <a name="native-modules"></a>Native Module

Die Tabelle enthält native IIS-Module, die bei Reverseproxyanforderungen an ASP.NET Core-Apps funktionsfähig sind.

| Modul | Funktionsfähig mit ASP.NET Core-Apps | ASP.NET Core-Option |
| ------ | :-------------------------------: | ------------------- |
| **Anonyme Authentifizierung**<br>`AnonymousAuthenticationModule` | Ja | |
| **Standardauthentifizierung**<br>`BasicAuthenticationModule` | Ja | |
| **Authentifizierung durch Clientzertifikatszuordnung**<br>`CertificateMappingAuthenticationModule` | Ja | |
| **CGI**<br>`CgiModule` | Nein | |
| **Konfigurationsvalidierung**<br>`ConfigurationValidationModule` | Ja | |
| **HTTP-Fehler**<br>`CustomErrorModule` | Nein | [Middleware für Statuscodeseiten](xref:fundamentals/error-handling#configure-status-code-pages) |
| **Benutzerdefinierte Protokollierung**<br>`CustomLoggingModule` | Ja | |
| **Standarddokument**<br>`DefaultDocumentModule` | Nein | [Middleware für Standarddateien](xref:fundamentals/static-files#serve-a-default-document) |
| **Hashwertauthentifizierung**<br>`DigestAuthenticationModule` | Ja | |
| **Verzeichnissuche**<br>`DirectoryListingModule` | Nein | [Middleware für Verzeichnissuche](xref:fundamentals/static-files#enable-directory-browsing) |
| **Dynamische Komprimierung**<br>`DynamicCompressionModule` | Ja | [Antworten komprimierende Middleware](xref:performance/response-compression) |
| **Ablaufverfolgung**<br>`FailedRequestsTracingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index#tracesource-provider) |
| **Dateizwischenspeicherung**<br>`FileCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| **HTTP-Zwischenspeicherung**<br>`HttpCacheModule` | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| **HTTP-Protokollierung**<br>`HttpLoggingModule` | Ja | [ASP.NET Core-Protokollierung](xref:fundamentals/logging/index)<br>Implementierungen: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP-Umleitung**<br>`HttpRedirectionModule` | Ja | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| **Authentifizierung durch IIS-Clientzertifikatszuordnung**<br>`IISCertificateMappingAuthenticationModule` | Ja | |
| **IP- und Domäneneinschränkungen**<br>`IpRestrictionModule` | Ja | |
| **ISAPI-Filter**<br>`IsapiFilterModule` | Ja | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Ja | [Middleware](xref:fundamentals/middleware/index) |
| **Protokollunterstützung**<br>`ProtocolSupportModule` | Ja | |
| **Anforderungsfilterung**<br>`RequestFilteringModule` | Ja | [URL-umschreibende Middleware`IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Anforderungsüberwachung**<br>`RequestMonitorModule` | Ja | |
| **URL-Umschreibung**<br>`RewriteModule` | Ja&#8224; | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| **Serverseitige Includedateien**<br>`ServerSideIncludeModule` | Nein | |
| **Statische Komprimierung**<br>`StaticCompressionModule` | Nein | [Antworten komprimierende Middleware](xref:performance/response-compression) |
| **Statischer Inhalt**<br>`StaticFileModule` | Nein | [Middleware für statische Dateien](xref:fundamentals/static-files) |
| **Token-Zwischenspeicherung**<br>`TokenCacheModule` | Ja | |
| **URI-Zwischenspeicherung**<br>`UriCacheModule` | Ja | |
| **URL-Autorisierung**<br>`UrlAuthorizationModule` | Ja | [ASP.NET Core-Identität](xref:security/authentication/identity) |
| **Windows-Authentifizierung**<br>`WindowsAuthenticationModule` | Ja | |

&#8224;Die Übereinstimmungstypen `isFile` und `isDirectory` des URL-Rewrite-Moduls können aufgrund der Änderungen in der [Verzeichnisstruktur](xref:host-and-deploy/directory-structure) nicht in ASP.NET Core-Apps ausgeführt werden.

## <a name="managed-modules"></a>Verwaltete Module

Verwaltete Module sind mit gehosteten ASP.NET Core-Apps *nicht* funktionsfähig, wenn die .NET CLR-Version des App-Pools auf **Kein verwalteter Code** festgelegt ist. ASP.NET Core bietet in verschiedenen Fällen Alternativen für Middleware an.

| Modul                  | ASP.NET Core-Option |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Middleware für Cookieauthentifizierung](xref:security/authentication/cookie) |
| OutputCache             | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware) |
| Profile                 | |
| RoleManager             | |
| ScriptModule-4.0        | |
| Sitzung                 | [Middleware für Sitzungen](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [ASP.NET Core-Identität](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS Manager-Anwendungsänderungen

Bei Verwendung von IIS Manager für die Konfiguration von Einstellungen wurde die Datei *web.config* der App geändert. Wenn eine App bereitgestellt und *web.config* einbezogen wird, werden sämtliche mit IIS Manager vorgenommene Änderungen von der bereitgestellten Datei *web.config* überschrieben. Wenn Änderungen an der Datei *web.config* des Servers vorgenommen werden, kopieren Sie die aktualisierte Datei *web.config* auf dem Server direkt in das lokale Projekt.

## <a name="disabling-iis-modules"></a>Deaktivieren von IIS-Modulen

Wenn ein IIS-Modul auf Serverebene konfiguriert wird und für eine App deaktiviert werden muss, ist dies durch eine Erweiterung der Datei *web.config* der App möglich. Lassen Sie das Modul an der Stelle und deaktivieren Sie es mit einer Konfigurationseinstellung (falls verfügbar), oder entfernen Sie das Modul aus der App.

### <a name="module-deactivation"></a>Deaktivierung von Modulen

Viele Module bieten eine Konfigurationseinstellung, mit der sie deaktiviert werden können, ohne aus der App entfernt werden zu müssen. Dies ist der einfachste und schnellste Weg zur Deaktivierung eines Moduls. Das HTTP-Umleitungsmodul kann beispielsweise mit dem Element **\<httpRedirect>** in *web.config* deaktiviert werden:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Für weitere Informationen zum Deaktivieren von Modulen mit Konfigurationseinstellungen folgen Sie den Links im Abschnitt *Untergeordnete Elemente* von [IIS \<system.webServer>](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Entfernen von Modulen

Wenn Sie ein Modul mit einer Einstellung in der Datei *web.config* entfernen können, entsperren Sie das Modul, und entsperren Sie zunächst den Abschnitt **\<modules>** in der Datei *web.config*:

1. Entsperren Sie das Modul auf Serverebene. Wählen Sie den IIS-Server in der IIS-Manager-Sidebar **Verbindungen** aus. Öffnen Sie **Module** im Bereich **IIS**. Wählen Sie das Modul aus der Liste aus. Wählen Sie in der Sidebar **Aktionen** auf der rechten Seite die Option **Entsperren** aus. Entsperren Sie alle Module, die Sie später aus *web.config* entfernen möchten.

2. Stellen Sie die App ohne den Abschnitt **\<modules>** in der Datei *web.config* bereit. Wenn eine App bereitgestellt wird, in der die Datei *web.config* den Abschnitt **\<modules>** enthält, ohne dass der Abschnitt zuvor im IIS-Manager entsperrt wurde, löst der Configuration Manager bei dem Versuch, den Abschnitt zu entsperren, eine Ausnahme aus. Daher sollten Sie die App ohne den Abschnitt **\<modules>** bereitstellen.

3. Entsperren Sie den Abschnitt **\<modules>** in der Datei *web.config*. Wählen Sie in der Sidebar **Verbindungen** unter **Sites** die Website aus. Öffnen Sie im Bereich **Verwaltung** den **Konfigurations-Editor**. Wählen Sie mithilfe der Navigationssteuerelemente den Abschnitt `system.webServer/modules` aus. Wählen Sie in der Seitenleiste **Aktionen** auf der rechten Seite die Option **Entsperren** für den Abschnitt aus.

4. An diesem Punkt kann der Abschnitt **\<modules>** zur Datei *web.config* mit dem Element **\<remove>** hinzugefügt werden, um das Modul aus der App zu entfernen. Es können mehrere Elemente vom Typ **\<remove>** hinzugefügt werden, um mehrere Module zu entfernen. Wenn auf dem Server Änderungen an der Datei *web.config* vorgenommen werden, nehmen Sie sofort lokal die gleichen Änderungen an der Datei *web.config* des Projekts vor. Wenn ein Modul auf diese Weise entfernt wird, hat dies keine Auswirkungen auf die Verwendung des Moduls mit anderen Apps auf dem Server.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Ein IIS-Modul kann auch mit *Appcmd.exe* entfernt werden. Geben Sie `MODULE_NAME` und `APPLICATION_NAME` im Befehl an:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Entfernen Sie beispielsweise `DynamicCompressionModule` aus „Default Web Site“ (Standardwebsite):

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Modulmindestkonfiguration

Die einzigen Module, die für die Ausführung einer ASP.NET Core-App erforderlich sind, sind das anonyme Authentifizierungsmodul und das ASP.NET Core-Modul.

![Bereich „Modules“ (Module) im IIS-Manager mit der angezeigten Modulmindestkonfiguration](modules/_static/modules.png)

Mit dem URI-Zwischenspeicherungsmodul (`UriCacheModule`) kann IIS die Websitekonfiguration auf URL-Ebene zwischenspeichern. Ohne dieses Modul muss IIS die Konfiguration bei der Anforderung lesen und analysieren, selbst dann, wenn wiederholt die gleiche URL angefordert wird. Das Analysieren der Konfiguration bei jeder Anforderung führt zu erheblichen Leistungseinbußen. *Auch wenn das URI-Zwischenspeicherungsmodul grundsätzlich nicht für die Ausführung einer gehosteten ASP.NET Core-App erforderlich ist, wird empfohlen, das URI-Zwischenspeicherungsmodul für sämtliche ASP.NET Core-Bereitstellungen zu aktivieren.*

Das HTTP-Zwischenspeicherungsmodul (`HttpCacheModule`) implementiert den IIS-Ausgabecache und die Logik für das Zwischenspeichern von Elementen im HTTP.sys-Cache. Ohne dieses Modul werden Inhalte nicht mehr im Kernelmodus zwischengespeichert, und Cacheprofile werden ignoriert. Das Entfernen des HTTP-Zwischenspeicherungsmoduls hat in der Regel negative Auswirkungen auf Leistung und Ressourcennutzung. *Auch wenn das HTTP-Zwischenspeicherungsmodul grundsätzlich nicht für die Ausführung einer gehosteten ASP.NET Core-App erforderlich ist, wird empfohlen, das HTTP-Zwischenspeicherungsmodul für sämtliche ASP.NET Core-Bereitstellungen zu aktivieren.*

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosten unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Einführung in IIS-Architekturen: Module in IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Übersicht über IIS-Module](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Anpassen von IIS 7.0-Rollen und -Modulen](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](/iis/configuration/system.webServer/)
