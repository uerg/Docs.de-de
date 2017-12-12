---
title: Verwenden IIS-Module mit ASP.NET Core
author: guardrex
description: "Aktive und inaktive IIS-Module für ASP.NET Core-Anwendungen beschreiben Referenzdokumente."
keywords: ASP.NET Core, Iis, Modul, Reverse-proxy
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: fee8e830ab43f731de9c90fad06b577662760f87
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>Verwenden IIS-Module mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

ASP.NET Core-Anwendungen werden in einer Reverse-Proxy-Konfiguration von IIS gehostet. Einige der systemeigenen IIS-Module und allen Modulen verwaltet IIS sind nicht zur Verarbeitung von Anforderungen für ASP.NET Core-apps verfügbar. In vielen Fällen bietet ASP.NET Core Alternative zu den Funktionen von IIS-Module für systemeigenen und verwalteten.

## <a name="native-modules"></a>Systemeigene Module
Modul | .NET Core aktiv | ASP.NET Core-Option
--- | :---: | ---
**Anonyme Authentifizierung**<br>`AnonymousAuthenticationModule` | Ja | 
**Standardauthentifizierung**<br>`BasicAuthenticationModule` | Ja | 
**Client-Zertifizierung Clientzertifikatzuordnung-Authentifizierung**<br>`CertificateMappingAuthenticationModule` | Ja | 
**CGI**<br>`CgiModule` | Nein | 
**Überprüfung der Konfiguration**<br>`ConfigurationValidationModule` | Ja | 
**HTTP-Fehler**<br>`CustomErrorModule` | Nein | [Status Code Seiten Middleware](xref:fundamentals/error-handling#configuring-status-code-pages)
**Benutzerdefinierte Protokollierung**<br>`CustomLoggingModule` | Ja | 
**Standarddokument**<br>`DefaultDocumentModule` | Nein | [Standardmäßig Dateien Middleware](xref:fundamentals/static-files#serving-a-default-document)
**Die Digest-Authentifizierung**<br>`DigestAuthenticationModule` | Ja | 
**Verzeichnissuche**<br>`DirectoryListingModule` | Nein | [Verzeichnis durchsuchen Middleware](xref:fundamentals/static-files#enabling-directory-browsing)
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

†The-URL-Rewrite-Modul `isFile` und `isDirectory` funktionieren nicht mit ASP.NET Core Anwendungen aufgrund der Änderungen im [Verzeichnisstruktur](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Verwaltete Module
Modul | .NET Core aktiv | ASP.NET Core-Option
--- | :---: | ---
AnonymousIdentification | Nein | 
DefaultAuthentication | Nein | 
FileAuthorization | Nein | 
FormsAuthentication | Nein | [Cookie-authentifizierungsmiddleware beziehen.](xref:security/authentication/cookie)
OutputCache | Nein | [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware)
Profile | Nein | 
RoleManager | Nein | 
ScriptModule 4.0 | Nein | 
Sitzung | Nein | [Sitzung Middleware](xref:fundamentals/app-state)
Dateiautorisierung | Nein | 
UrlMappingsModule | Nein | [URL-umschreibende Middleware](xref:fundamentals/url-rewriting)
4.0-UrlRoutingModule fest | Nein | [ASP.NET Core Identität](xref:security/authentication/identity)
WindowsAuthentication | Nein | 

## <a name="iis-manager-application-changes"></a>IIS-Manager-anwendungsänderungen
Wenn Sie IIS-Manager verwenden, um Einstellungen zu konfigurieren, ändern Sie direkt die *"Web.config"* -Datei der app. Wenn Sie Ihre app bereitstellen und umfassen *"Web.config"*, alle mit IIS-Manager vorgenommenen Änderungen überschrieben werden durch die bereitgestellte *Datei "Web.config"*. Aus diesem Grund, wenn Sie Änderungen an den Server senden *"Web.config"* Datei, kopieren Sie die aktualisierte *"Web.config"* Datei auf Ihrem lokalen Projekt sofort.

## <a name="disabling-iis-modules"></a>Deaktivieren die IIS-Module
Ein IIS-Modul, das auf Serverebene, die Sie, deaktivieren Sie für eine Anwendung möchten konfiguriert haben, können Sie dies tun, mit der eine Ergänzung der *"Web.config"* Datei. Belassen Sie das Modul vorhanden und deaktivieren Sie es über eine Konfigurationseinstellung (falls verfügbar) oder entfernen Sie das Modul aus der app.

### <a name="module-deactivation"></a>Deaktivierung des Moduls
Viele Module bieten eine Konfigurationseinstellung, die sie deaktivieren, ohne sie aus der Anwendung entfernt werden kann. Dies ist die einfachste und schnellste Möglichkeit, ein Modul zu deaktivieren. Angenommen, Sie verwenden möchten, deaktivieren das IIS URL Rewrite-Modul, verwenden die `<httpRedirect>` -Element wie unten dargestellt. Weitere Informationen zu Modulen mit Konfigurationseinstellungen zu deaktivieren, führen Sie die Links in der *Unterelemente* Abschnitt [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Entfernen des Moduls
Wenn Sie sich entscheiden, so entfernen Sie ein Modul mit einer Einstellung in *"Web.config"*, müssen Sie das Modul zu entsperren und Entsperren der `<modules>` Abschnitt *"Web.config"* erste. Im folgenden werden die Schritte erläutert:

1. Entsperren Sie das Modul auf Serverebene. Klicken Sie auf dem IIS-Server im IIS-Manager **Verbindungen** Randleiste. Öffnen der **Module** in der **IIS** Bereich. Klicken Sie auf das Modul in der Liste. In der **Aktionen** Randleiste auf der rechten Seite klicken Sie auf **Unlock**. Entsperren Sie so viele Module, die bei der Planung zum Entfernen der mit *"Web.config"* später erneut.

2. Bereitstellen der Anwendung ohne eine `<modules>` im Abschnitt *"Web.config"*. Wenn Sie beim Bereitstellen einer app mit einer *"Web.config"* , enthält die `<modules>` Abschnitt, ohne dass entsperrt werden im Abschnitt zuerst im IIS-Manager, den Konfigurations-Manager wird eine Ausnahme ausgelöst, wenn Sie versuchen, die im Abschnitt zu entsperren. Aus diesem Grund Bereitstellen Ihrer Anwendung ohne eine `<modules>` Abschnitt.

3. Entsperren der `<modules>` Abschnitt *"Web.config"*. In der **Verbindungen** Randleiste, klicken Sie auf der Website im **Sites**. In der **Management** öffnen Sie im Bereich der **Dienstkonfigurations-Editor**. Verwenden Sie die Navigationssteuerelemente Auswählen der `system.webServer/modules` Abschnitt. In der **Aktionen** Randleiste auf der rechten Seite klicken Sie auf, um **Unlock** Abschnitt.

4. Hinzufügen konnte zu diesem Zeitpunkt werden eine `<modules>` -Abschnitt hinzu Ihrer *"Web.config"* -Datei mit einer `<remove>` Element, das Modul aus der Anwendung zu entfernen. Sie können mehrere hinzufügen `<remove>` Elemente um mehrere Module zu entfernen. Vergessen Sie nicht, die bei *"Web.config"* Änderungen auf dem Server zu sofort im Projekt lokal machen. Entfernen eines Moduls, das auf diese Weise wird nicht die Verwendung des Moduls mit anderen apps auf dem Server betreffen.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Für eine IIS-Installation mit den Standard-Modulen, die installiert werden soll, verwenden Sie die folgende `<module>` Abschnitt aus, um die Standardmodule zu entfernen.

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

Sie können auch mit einem IIS-Modul entfernen *Appcmd.exe*. Geben Sie die `MODULE_NAME` und `APPLICATION_NAME` in dem unten gezeigten Befehl:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

So entfernen Sie die `DynamicCompressionModule` aus Default Web Site:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>Minimalen Modulkonfiguration
Nur Module zum Ausführen einer Anwendung ASP.NET Core erforderlich sind anonyme Authentifizierungsmodul und ASP.NET Core-Modul.

![IIS-Manager öffnen, um Module mit der minimalen Modulkonfiguration angezeigt](iis-modules/_static/modules.png)

## <a name="resources"></a>Ressourcen
* [Veröffentlichen in IIS](xref:publishing/iis)
* [Übersicht über die IIS-Module](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Anpassen von IIS 7.0-Rollen und Module](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
