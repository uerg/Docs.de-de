---
title: Hosten von ASP.NET Core unter Windows mit IIS
author: guardrex
description: "Konfiguration und Bereitstellung von Windows Server-Internetinformationsdienste (Internet Information Services, IIS) für ASP.NET Core-Anwendungen."
keywords: ASP.NET Core, Internetinformationsdienste, IIS, Windows Server, Hostingpaket, ASP.NET Core-Modul, Web Deploy
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: e9e9019d5b879498e8800bb579c177dd3ad64061
ms.sourcegitcommit: 96af03c9f44f7c206e68ae3ef8596068e6b4e5fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hosten von ASP.NET Core unter Windows mit IIS

Von [Luke Latham](https://github.com/guardrex) und [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgenden Betriebssysteme werden unterstützt:

* Windows 7 und höher
* Windows Server 2008 R2 und höher&#8224;

&#8224;Vom Konzept her gilt die in diesem Dokument beschriebene IIS-Konfiguration auch für das Hosten von ASP.NET Core-Anwendungen mit IIS unter Nano Server, die genauen Anweisungen finden Sie jedoch unter [ASP.NET Core mit IIS unter Nano Server](xref:tutorials/nano-server).

[HTTP.sys-Server](xref:fundamentals/servers/httpsys) (zuvor [WebListener](xref:fundamentals/servers/weblistener) genannt) funktioniert nicht in einer Reverseproxykonfiguration mit IIS. Verwenden Sie den [Kestrel-Server](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>IIS-Konfiguration

Aktivieren Sie die Rolle **Webserver (IIS)**, und richten Sie Rollendienste ein.

### <a name="windows-desktop-operating-systems"></a>Windows-Desktopbetriebssysteme

Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm). Öffnen Sie die Gruppe für **Internetinformationsdienste** und **Webverwaltungstools**. Aktivieren Sie das Kontrollkästchen für **IIS-Verwaltungskonsole**. Aktivieren Sie das Kontrollkästchen für **WWW-Dienste**. Akzeptieren Sie die Standardfeatures für **WWW-Dienste**, oder passen Sie die IIS-Features an Ihre speziellen Anforderungen an.

![Die IIS-Verwaltungskonsole und WWW-Dienste werden in Windows-Features ausgewählt.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server-Betriebssysteme

Verwenden Sie für Serverbetriebssysteme den Assistenten **Rollen und Features hinzufügen** im Menü **Verwalten** oder den Link in **Server-Manager**. Aktivieren Sie im Schritt **Serverrollen** das Kontrollkästchen für **Webserver (IIS)**.

![Die Rolle „Webserver (IIS)“ wird im Schritt „Serverrollen auswählen“ ausgewählt.](iis/_static/server-roles-ws2016.png)

Wählen Sie im Schritt **Rollendienste** die gewünschten IIS-Rollendienste aus, oder übernehmen Sie die angegebenen Standardrollendienste.

![Die Standardrollendienste werden im Schritt „Rollendienste auswählen“ ausgewählt.](iis/_static/role-services-ws2016.png)

Fahren Sie mit dem Schritt **Bestätigung** fort, um die Webserverrolle und die Dienste zu installieren. Ein Server-/IIS-Neustart ist nach der Installation der Rolle „Webserver (IIS)“ nicht erforderlich.

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Installieren des Pakets „.NET Core Windows Server Hosting“

1. Installieren Sie das [Paket „.NET Core Windows Server Hosting“](https://download.microsoft.com/download/5/C/1/5C190037-632B-443D-842D-39085F02E1E8/DotNetCore.2.0.3-WindowsHosting.exe) im Hostsystem. Das Paket installiert die .NET Core-Runtime, die .NET Core-Bibliothek und das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module). Das Modul erstellt den Reverseproxy zwischen IIS und dem Kestrel-Server. Wenn das System nicht über eine Internetverbindung verfügt, beziehen und installieren Sie [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840), bevor Sie das Paket „.NET Core Windows Server Hosting“ installieren.

2. Starten Sie das System neu, oder führen Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus, um eine Änderung am Systempfad zu übernehmen.

> [!NOTE]
> Weitere Informationen zur Verwendung einer IIS-Freigabekonfiguration finden Sie unter [ASP.NET Core-Modul mit IIS-Freigabekonfiguration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Installieren von Web Deploy beim Veröffentlichen mit Visual Studio

Wenn Sie Ihre Anwendungen mit [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) in [Visual Studio](https://www.visualstudio.com/vs/) bereitstellen möchten, installieren Sie die neueste Version von Web Deploy auf dem Hostsystem. Um Web Deploy zu installieren, können Sie den [Webplattform-Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) verwenden oder ein Installationsprogramm direkt aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717) abrufen. Die bevorzugte Methode ist die Verwendung von WebPI. WebPI bietet ein eigenständiges Setup und eine Konfiguration für Hostinganbieter.

## <a name="application-configuration"></a>Anwendungskonfiguration

### <a name="enabling-the-iisintegration-components"></a>Aktivieren der IISIntegration-Komponenten

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Eine typische *Program.cs*-Datei ruft [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) auf, um die Einrichtung eines Hosts zu starten. `CreateDefaultBuilder` konfiguriert [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und aktiviert die IIS-Integration durch Konfigurierung des Basispfads und Ports für das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Nehmen Sie eine Abhängigkeit vom Paket [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) in Ihre App-Abhängigkeiten auf. Binden Sie Middleware für die Integration von IIS in die App ein, indem Sie die Erweiterungsmethode *UseIISIntegration* zu *WebHostBuilder* hinzufügen:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Es sind jeweils `UseKestrel` und `UseIISIntegration` erforderlich. Ein Code, der *.UseIISIntegration* aufruft, hat keinen Einfluss auf die Codeportabilität. Wenn Die App nicht hinter IIS ausgeführt wird (die App wird z.B. direkt auf Kestrel ausgeführt), hat `UseIISIntegration` keine Funktion.

---

Weitere Informationen zum Hosten finden Sie unter [Hosting in ASP.NET Core (Hosten in ASP.NET Core)](xref:fundamentals/hosting).

### <a name="iis-options"></a>IIS-Optionen

Um *IISIntegration*-Dienstoptionen zu konfigurieren, beziehen Sie eine Dienstkonfiguration für *IISOptions* in *ConfigureServices* mit ein:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Option                         | Standard | Einstellung |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Wenn `true`, dann legt die Authentifizierungsmiddleware `HttpContext.User` fest und reagiert auf generische Herausforderungen. Wenn `false`, dann stellt die Authentifizierungsmiddleware nur eine Identität bereit (`HttpContext.User`) und reagiert nur dann auf Herausforderungen, wenn sie dazu explizit von `AuthenticationScheme` angewiesen wird. Die Windows-Authentifizierung muss in IIS aktiviert sein, damit `AutomaticAuthentication` funktioniert. |
| `AuthenticationDisplayName`    | `null`  | Legt den Anzeigename fest, der Benutzern auf Anmeldungsseiten angezeigt wird |
| `ForwardClientCertificate`     | `true`  | Wenn diese Option `true` ist und der Anforderungsheader `MS-ASPNETCORE-CLIENTCERT` vorhanden ist, wird das `HttpContext.Connection.ClientCertificate` aufgefüllt. |

### <a name="webconfig"></a>web.config

Die Datei *web.config* konfiguriert in erster Linie das ASP.NET Core-Modul. Sie kann optional zusätzliche IIS-Konfigurationseinstellungen bereitstellen. Das Erstellen, das Transformieren und die Veröffentlichung von *web.config* erfolgt durch das .NET Core Web SDK (`Microsoft.NET.Sdk.Web`). Das SDK wird am Anfang der Projektdatei (*CSPROJ-Datei*) festgelegt: `<Project Sdk="Microsoft.NET.Sdk.Web">`. Um zu verhindern, dass das SDK die Datei *web.config* transformiert, fügen Sie der Projektdatei die Eigenschaft **\<IsTransformWebConfigDisabled** mit der Einstellung `true` hinzu:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Wenn im Projekt keine Datei *web.config* vorhanden ist, wird die Datei bei der Veröffentlichung mit *dotnet publish* oder durch Veröffentlichen in Visual Studio in der [veröffentlichten Ausgabe](xref:hosting/directory-structure) für Sie erstellt. Wenn eine *web.config*-Datei in Ihrem Projekt vorhanden ist, wird sie mit dem richtigen *processPath* und den richtigen *Argumenten* transformiert, um das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zu konfigurieren, und in die veröffentlichte Ausgabe verschoben. Die Transformation ändert die IIS-Konfigurationseinstellungen nicht, die Sie in die Datei aufgenommen haben.

## <a name="create-the-iis-website"></a>Erstellen der IIS-Website

1. Erstellen Sie im IIS-Zielsystem einen Ordner für die veröffentlichten Ordner und Dateien der App, die in [Verzeichnisstruktur](xref:hosting/directory-structure) beschrieben sind.

2. Erstellen Sie in diesem Ordner einen Ordner *Protokolle* zum Speichern der stdout-Protokolle (wenn Sie die Protokollierung aktivieren möchten, um Probleme beim Start zu behandeln). Wenn Sie planen, die Anwendung mit einem Ordner *Protokolle* in der Nutzlast bereitzustellen, können Sie diesen Schritt überspringen. Es besteht eine [offene Frage zum automatischen Erstellen des Ordners](https://github.com/aspnet/AspNetCoreModule/issues/30). Wenn Sie möchten, dass MSBuild den *Protokoll*-Ordner für Sie erstellt, fügen Sie folgendes `Target` zu Ihrer Projektdatei hinzu:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. Erstellen Sie im **IIS-Manager** eine neue Website. Geben Sie einen **Websitenamen** an, und legen Sie den **physischen Pfad** auf den erstellten Bereitstellungsordner der App fest. Geben Sie die Konfiguration unter **Bindung** an, und erstellen Sie die Website.

4. Legen Sie den Anwendungspool auf **Kein verwalteter Code** fest. ASP.NET Core wird in einem separaten Prozess ausgeführt und verwaltet die Runtime.

5. Öffnen Sie das Fenster **Website hinzufügen**.

   ![Klicken Sie im Kontextmenü „Websites“ auf „Website hinzufügen“.](iis/_static/add-website-context-menu-ws2016.png)

6. Konfigurieren Sie die Website.

   ![Geben Sie den Websitenamen, den physischen Pfad und den Hostnamen im Schritt „Website hinzufügen“ an.](iis/_static/add-website-ws2016.png)

7. Öffnen Sie im Bereich **Anwendungspools** das Fenster **Anwendungspool bearbeiten**, indem Sie mit der rechten Maustaste auf den Anwendungspool der Website klicken und im Popupmenü **Grundeinstellungen** auswählen.

   ![Wählen Sie „Grundeinstellungen“ aus dem Kontextmenü des Anwendungspools aus.](iis/_static/apppools-basic-settings-ws2016.png)

8. Legen Sie die **.NET CLR-Version** auf **Kein verwalteter Code** fest.

   ![Legen Sie „Kein verwalteter Code“ für die .NET CLR-Version fest.](iis/_static/edit-apppool-ws2016.png)
     
    Hinweis: Das Festlegen der **.NET CLR-Version** auf **Kein verwalteter Code** ist optional. Für ASP.NET Core ist das Laden der Desktop-CLR nicht erforderlich.

9. Vergewissern Sie sich, dass die Prozessmodellidentität über die richtigen Berechtigungen verfügt.

   Wenn Sie die Standardidentität des App-Pools (**Prozessmodell** > **Identität**) von **ApplicationPoolIdentity** in eine andere Identität ändern, stellen Sie sicher, dass die neue Identität über die erforderlichen Berechtigungen zum Zugriff auf den Ordner, die Datenbank und andere erforderliche Ressourcen der App verfügt.
   
## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Stellen Sie die Anwendung in dem Ordner bereit, den Sie im IIS-Zielsystem erstellt haben. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ist die empfohlene Methode zur Bereitstellung. Alternativen zu Web Deploy sind unten aufgeführt.

Stellen Sie sicher, dass die zur Bereitstellung veröffentlichte App nicht ausgeführt wird. Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird. Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.

### <a name="web-deploy-with-visual-studio"></a>Web Deploy mit Visual Studio
Informationen zum Erstellen eines Veröffentlichungsprofils zur Verwendung mit Web Deploy finden Sie im Thema [Erstellen von Veröffentlichungsprofilen für Visual Studio und MSBuild zum Bereitstellen von ASP.NET Core-Apps](xref:publishing/web-publishing-vs#publish-profiles). Wenn Ihr Hostinganbieter ein Veröffentlichungsprofil oder Unterstützung für das Erstellen eines solchen Profils bereitstellt, laden Sie dessen Profil herunter, und importieren Sie es mithilfe des Visual Studio-Dialogfelds **Veröffentlichen**.

![Dialogfeld „Veröffentlichen“](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web Deploy außerhalb von Visual Studio
Sie können [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) über die Befehlszeile auch außerhalb von Visual Studio verwenden. Weitere Informationen finden Sie unter [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool) (Webbereitstellungstool).

### <a name="alternatives-to-web-deploy"></a>Alternativen zu Web Deploy
Wenn Sie Web Deploy nicht verwenden möchten oder Visual Studio nicht nutzen, stehen Ihnen verschiedene Methoden zur Verfügung, um die Anwendung in das Hostsystem zu verschieben, z.B. Xcopy, Robocopy oder PowerShell. Visual Studio-Benutzer können die [Veröffentlichungsbeispiele](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md) verwenden.

## <a name="browse-the-website"></a>Navigieren auf der Website
![Der Microsoft Edge-Browser hat die IIS-Startseite geladen.](iis/_static/browsewebsite.png)
   
> [!WARNING]
> .NET Core-Apps werden über einen Reverseproxy zwischen IIS und dem Kestrel-Server gehostet. Zum Erstellen des Reverseproxys muss die Datei *web.config* im Stammpfad des Inhalts (normalerweise der App-Basispfad) der bereitgestellten Anwendung vorhanden sein. Dies ist der physische Pfad der Website, der in IIS bereitgestellt wurde. Im physischen Pfad und den Unterordnern der App sind vertrauliche Dateien vorhanden, wie z.B. *my_application.runtimeconfig.json*, *my_application.xml* (XML-Dokumentationskommentare) und *my_application.deps.json*. Die Datei *web.config* ist erforderlich, um den Reverseproxy zu Kestrel zu erstellen. Dadurch wird verhindert, dass IIS diese und andere vertrauliche Dateien bereitstellt. **Daher ist es wichtig, dass die Datei *web.config* nicht versehentlich umbenannt oder aus der Bereitstellung entfernt wird.**

## <a name="data-protection"></a>Schutz von Daten

Eine ASP.NET Core-Anwendung speichert unter den folgenden Bedingungen den Schlüsselbund im Arbeitsspeicher:

* Eine Website wird hinter IIS gehostet.
* Der Stapel für den Schutz von Daten wurde nicht so konfiguriert, dass der Schlüsselbund in einem permanenten Speicher speichert wird.

Falls der Schlüsselbund im Arbeitsspeicher gespeichert wird, wenn die App neu gestartet wird:

* Werden alle Formularauthentifizierungstoken als ungültig ausgewiesen. 
* Müssen Benutzer sich bei ihrer nächsten Anforderung erneut anmelden. 
* Sind alle Daten, die Sie mit dem Schlüsselbund geschützt haben, nicht mehr geschützt.

> [!WARNING]
> Der Schutz von Daten wird von mehreren ASP.NET-Middlewarefunktionen genutzt, einschließlich der Middleware, die bei der Authentifizierung verwendet wird. Auch wenn Sie keine Datenschutz-APIs über Ihren eigenen Code aufrufen, sollten Sie den Schutz von Daten mit einem Bereitstellungsskript oder in Ihrem Code konfigurieren. Wenn Sie den Schutz von Daten nicht konfigurieren, werden die Schlüssel standardmäßig im Arbeitsspeicher gespeichert und verworfen, wenn Ihre App neu gestartet wird. Durch den Neustart werden alle von der Cookieauthentifizierungs-Middleware geschriebenen Cookies ungültig, und Benutzer müssen sich erneut anmelden.

Zum Konfigurieren des Schutzes von Daten unter IIS müssen Sie einen der folgenden Ansätze verwenden:

* Führen Sie ein [PowerShell-Skript](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) aus, um geeignete Registrierungseinträge zu erstellen (z.B. `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). Dadurch werden Schlüssel in der Registrierung gespeichert und mit DPAPI mit einem für den gesamten Computer geltenden Schlüssel geschützt.
* Konfigurieren Sie den IIS-Anwendungspool zum Laden des Benutzerprofils. Diese Einstellung befindet sich im Abschnitt **Prozessmodell** unter **Erweiterte Einstellungen** für den Anwendungspool. Legen Sie **Benutzerprofil laden** auf `True` fest. Dadurch werden Schlüssel im Benutzerprofilverzeichnis gespeichert und mit DPAPI mit einem Schlüssel geschützt, der nur für das Benutzerkonto gilt, das für den App-Pool verwendet wird.
* Passen Sie den App-Code so an, dass er [das Dateisystem als Schlüsselringspeicher verwendet](xref:security/data-protection/configuration/overview). Verwenden Sie ein X509-Zertifikat, um den Schlüsselring zu schützen, und stellen Sie sicher, dass es sich um ein vertrauenswürdiges Zertifikat handelt. Ein selbst signiertes Zertifikat müssen Sie im vertrauenswürdigen Stammspeicher platzieren.

Wenn IIS in einer Webfarm verwendet wird:

* Verwenden Sie eine Dateifreigabe, auf die alle Computer zugreifen können.
* Stellen Sie ein X509-Zertifikat auf jedem Computer bereit. Konfigurieren Sie den [Schutz von Daten im Code](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Erstellen einer Registrierungsstruktur für den Schutz von Daten

Schlüssel für den Schutz von Daten, die von ASP.NET-Anwendungen verwendet werden, werden in Registrierungsstrukturen außerhalb der Anwendungen gespeichert. Um die Schlüssel für eine bestimmte Anwendung beizubehalten, müssen Sie eine Registrierungsstruktur für den Anwendungspool der App erstellen.

Bei eigenständigen IIS-Installationen können Sie das [PowerShell-Skript „Provision-AutoGenKeys.ps1“ für den Schutz von Daten](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) für jeden App-Pool nutzen, der mit einer ASP.NET Core-App verwendet wird. Dieses Skript erstellt einen besonderen Registrierungsschlüssel in der HKLM-Registrierung, der nur in der ACL des Workerprozesskontos bereitgestellt wird. Schlüssel werden im Ruhezustand mit DPAPI verschlüsselt.

In Webfarmszenarios kann eine App so konfiguriert werden, dass sie einen UNC-Pfad verwendet, um den Schlüsselbund für den Schutz von Daten zu speichern. Standardmäßig werden die Schlüssel für den Schutz von Daten nicht verschlüsselt. Sie sollten sicherstellen, dass die Dateiberechtigungen für eine solche Freigabe auf das Windows-Konto beschränkt sind, mit dem die App ausgeführt wird. Darüber hinaus können Sie Schlüssel im Ruhezustand mit einem X509-Zertifikat schützen. Sie sollten sich einen Mechanismus überlegen, um es Benutzern zu ermöglichen, Zertifikate hochzuladen: Platzieren Sie Zertifikate im Speicher für vertrauenswürdige Zertifikate des Benutzers, und stellen Sie sicher, dass sie auf allen Computern verfügbar sind, auf denen die App des Benutzers ausgeführt wird. Details finden Sie unter [Konfigurieren des Schutzes von Daten](xref:security/data-protection/configuration/overview).

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Konfigurieren des IIS-Anwendungspools zum Laden des Benutzerprofils

Diese Einstellung befindet sich im Abschnitt **Prozessmodell** unter **Erweiterte Einstellungen** für den App-Pool. Legen Sie „Benutzerprofil laden“ auf `True` fest. Dadurch werden Schlüssel im Benutzerprofilverzeichnis gespeichert und mit DPAPI mit einem Schlüssel geschützt, der nur für das Benutzerkonto gilt, das für den App-Pool verwendet wird.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Computerweite Richtlinie für den Schutz von Daten

Das System zum Schutz von Daten verfügt über eine eingeschränkte Unterstützung zum Festlegen einer [computerweiten Standardrichtlinie](xref:security/data-protection/configuration/machine-wide-policy) für alle Apps, die die Datenschutz-APIs nutzen. Weitere Details finden Sie in der Dokumentation zum [Schutz von Daten](xref:security/data-protection/index).

## <a name="configuration-of-sub-applications"></a>Konfiguration von untergeordneten Anwendungen

Untergeordnete Anwendungen, die unter der Stammanwendung hinzugefügt werden, sollten kein ASP.NET Core-Modul als Handler enthalten. Wenn Sie das Modul als Handler in einer *web.config*-Datei einer untergeordneten Anwendung hinzufügen, erhalten Sie den Code 500.19 (interner Serverfehler), der auf die fehlerhafte Konfigurationsdatei verweist, wenn Sie versuchen, in der untergeordneten App zu navigieren. Das folgende Beispiel zeigt den Inhalt einer veröffentlichten *web.config*-Datei für eine untergeordnete ASP.NET Core-App:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Wenn Sie beabsichtigen, eine untergeordnete App ohne ASP.NET Core unterhalb einer ASP.NET Core-App zu hosten, müssen Sie die geerbten Handler explizit aus der *web.config*-Datei der untergeordneten App entfernen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Weitere Informationen zum Konfigurieren des ASP.NET Core-Moduls mit der Datei *web.config* finden Sie im Thema [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und in der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguration von IIS mit der Datei „web.config“

Die IIS-Konfiguration wird weiterhin im Hinblick auf IIS-Features, die für eine Reverseproxykonfiguration gelten, vom Abschnitt `<system.webServer>` der Datei *web.config* beeinflusst. Sie könnten z.B. IIS auf Systemebene so konfiguriert haben, dass die dynamische Komprimierung verwendet wird. Sie könnten diese Einstellung jedoch für eine App mit dem Element `<urlCompression>` in der Datei *web.config* der App deaktivieren. Weitere Informationen finden Sie in der [Konfigurationsreferenz für `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), in der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:hosting/aspnet-core-module) und unter [Verwenden von IIS-Modulen mit ASP.NET Core](xref:hosting/iis-modules). Wenn Sie Umgebungsvariablen für einzelne Apps festlegen müssen, die in isolierten Anwendungspools ausgeführt werden (unterstützt für IIS 10.0 und höher), finden Sie weitere Informationen im Abschnitt zum *Befehl „AppCmd.exe“* im Thema [Umgebungsvariablen \< EnvironmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) in der IIS-Referenzdokumentation.

## <a name="configuration-sections-of-webconfig"></a>Konfigurationsabschnitte von „web.config“

Im Gegensatz zu .NET Framework-Anwendungen, die mit den Elementen `<system.web>`, `<appSettings>`, `<connectionStrings>` und `<location>` in *web.config* konfiguriert werden, werden ASP.NET Core-Apps über andere Konfigurationsanbieter konfiguriert. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration).

## <a name="application-pools"></a>Anwendungspools

Wenn Sie mehrere Websites in einem System hosten, sollten Sie die Apps voneinander isolieren, indem Sie jede App in ihrem eigenen Anwendungspool ausführen. Im IIS-Dialogfeld **Website hinzufügen** wird standardmäßig dieses Verhalten eingesetzt. Wenn Sie einen **Websitenamen** angeben, wird der Text automatisch in das Textfeld **Anwendungspool** übertragen. Ein neuer Anwendungspool wird mit dem Namen der Website erstellt, wenn Sie die Website hinzufügen.

## <a name="application-pool-identity"></a>Identität des Anwendungspools

Mit einem Konto für die Identität des Anwendungspools können Sie eine Anwendung unter einem eindeutigen Konto ausführen, ohne Domänen oder lokale Konten erstellen und verwalten zu müssen. Mit IIS 8.0 und höher erstellt der IIS-Administratorworkerprozess (WAS) ein virtuelles Konto mit dem Namen des neuen Anwendungspools und führt die Workerprozesse des App-Pools standardmäßig unter diesem Konto aus. Stellen Sie in der IIS-Verwaltungskonsole unter **Erweiterte Einstellungen** für den App-Pool sicher, dass die **Identität** wie in der folgenden Abbildung dargestellt auf **ApplicationPoolIdentity** festgelegt ist.

![Dialogfeld „Erweiterte Einstellungen“ für den Anwendungspool](iis/_static/apppool-identity.png)

Der IIS-Verwaltungsprozess erstellt im Windows-Sicherheitssystem einen sicheren Bezeichner mit dem Namen des App-Pools. Ressourcen können unter Verwendung dieser Identität gesichert werden. Allerdings ist diese Identität kein echtes Benutzerkonto und wird in der Windows-Benutzerverwaltungskonsole nicht angezeigt.

Wenn Sie dem IIS-Workerprozess Zugriff auf Ihre Anwendung mit erhöhten Rechten gewähren müssen, müssen Sie die Zugriffssteuerungsliste (ACL) für das Verzeichnis mit der App ändern.

1. Öffnen Sie Windows Explorer, und navigieren Sie zum Verzeichnis.

2. Klicken Sie mit der rechten Maustaste auf das Verzeichnis, und klicken Sie auf **Eigenschaften**.

3. Klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Bearbeiten** und dann auf die Schaltfläche **Hinzufügen**.

4. Klicken Sie auf die Schaltfläche **Speicherorte**, und stellen Sie sicher, dass Sie Ihr System auswählen.

5. Geben Sie in das Textfeld **Geben Sie die zu verwendenden Objektnamen ein** den Text **IIS AppPool\DefaultAppPool** ein.

  ![Dialogfeld zum Auswählen von Benutzern oder Gruppen für den Anwendungsordner](iis/_static/select-users-or-groups-1.png)

6. Klicken Sie auf die Schaltfläche **Namen überprüfen** und dann auf **OK**.

  ![Dialogfeld zum Auswählen von Benutzern oder Gruppen für den Anwendungsordner](iis/_static/select-users-or-groups-2.png)

Hierzu können Sie auch eine Eingabeaufforderung mit dem Tool **ICACLS** verwenden:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

So diagnostizieren Sie Probleme mit IIS-Bereitstellungen:

* Untersuchen Sie die Browserausgabe.
* Überprüfen Sie das **Anwendungsprotokoll** des Systems über die **Ereignisanzeige**.
* Aktivieren Sie die `stdout`-Protokollierung. Das Protokoll des **ASP.NET Core-Moduls** befindet sich im vom Attribut *stdoutLogFile* des Elements `<aspNetCore>` in *web.config* angegebenen Pfad. Alle Ordner im Pfad, der im Attributwert angegeben ist, müssen in der Bereitstellung vorhanden sein. Sie müssen außerdem *stdoutLogEnabled="true"* festlegen. Bei Apps, die das `Microsoft.NET.Sdk.Web` SDK zum Erstellen der Datei *web.config* verwenden, ist die Einstellung *stdoutLogEnabled* standardmäßig auf *FALSE* festgelegt. Daher müssen Sie die Datei *web.config* manuell angeben oder die Datei ändern, um die `stdout`-Protokollierung zu ermöglichen.

Einige der häufigen Fehler werden erst im Browser, Anwendungsprotokoll und ASP.NET Core-Modulprotokoll angezeigt, wenn die Werte für *startupTimeLimit* (Standardwert: 120 Sekunden) und *startupRetryCount* (Standardwert: 2) für das Modul überschritten wurden. Warten Sie daher sechs Minuten, bevor Sie davon ausgehen, dass das Modul keinen Prozess für die App gestartet hat.

Eine schnelle Möglichkeit, um festzustellen, ob die App ordnungsgemäß funktioniert, ist die direkte Ausführung der App auf Kestrel. Wenn die App als Framework-abhängige Bereitstellung veröffentlicht wurde, führen Sie `dotnet my_application.dll` im Bereitstellungsordner aus, also dem physischen IIS-Pfad für die App. Wenn die App als eigenständige Bereitstellung veröffentlicht wurde, führen Sie die ausführbare Datei der App direkt über eine Eingabeaufforderung, `my_application.exe`, im Bereitstellungsordner aus. Wenn Kestrel am Standardport 5000 lauscht, sollten Sie die App über `http://localhost:5000/` aufrufen können. Wenn die App normalerweise an der Kestrel-Endpunktadresse antwortet, hängt das Problem wahrscheinlich mit der Konfiguration von IIS bzw. dem ASP.NET Core-Modul bzw. Kestrel und vermutlich nicht mit der App selbst zusammen.

Eine Möglichkeit zu bestimmen, ob der IIS-Reverseproxy zum Kestrel-Server ordnungsgemäß funktioniert, ist das Ausführen einer einfachen statischen Dateianforderung für ein Stylesheet, Skript oder Image mit den statischen Dateien der App in *wwwroot* mithilfe der [Middleware für statische Dateien](xref:fundamentals/static-files). Wenn die App die statischen Dateien verarbeiten kann, jedoch MVC-Ansichten und andere Endpunkte fehlschlagen, wird das Problem wahrscheinlich nicht durch die Konfiguration von IIS bzw. dem ASP.NET Core-Modul bzw. Kestrel verursacht, sondern eher durch die App selbst (z.B. MVC-Routing oder 500 (interner Serverfehler)).

Wenn Kestrel normalerweise hinter IIS gestartet wird, aber die App nicht im System ausgeführt wird, nachdem sie erfolgreich lokal ausgeführt wurde, können Sie vorübergehend eine Umgebungsvariable zu *web.config* hinzufügen und `ASPNETCORE_ENVIRONMENT` auf `Development` festlegen. Solange Sie die Umgebung beim App-Start nicht außer Kraft setzen, kann dadurch die [Ausnahmeseite für Entwickler](xref:fundamentals/error-handling) angezeigt werden, wenn die App im System ausgeführt wird. Das Festlegen der Umgebungsvariablen für `ASPNETCORE_ENVIRONMENT` auf diese Weise wird nur für Staging-/Testsysteme empfohlen, die nicht für das Internet verfügbar gemacht werden. Wenn Sie fertig sind, müssen Sie die Umgebungsvariable aus *web.config* entfernen. Informationen zum Festlegen von Umgebungsvariablen über *web.config* für den Reverseproxy finden Sie unter [Untergeordnetes environmentVariables-Element von aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).

In den meisten Fällen hilft das Aktivieren der Anwendungsprotokollierung bei der Behandlung von Problemen mit der App oder dem Reverseproxy. Weitere Informationen finden Sie unter [Protokollierung](xref:fundamentals/logging/index).

Unsere letzter Tipp zur Problembehandlung bezieht sich auf Apps, die nach einem Upgrade des .NET Core SDK auf dem Entwicklungsomputer oder einer Paketversion innerhalb der App nicht ausgeführt werden. In einigen Fällen können inkohärente Pakete eine App beschädigen, wenn größere Upgrades durchgeführt werden. Sie können die meisten dieser Probleme beheben, indem Sie die Ordner `bin` und `obj` im Projekt löschen, die Paketcaches unter `%UserProfile%\.nuget\packages\` und `%LocalAppData%\Nuget\v3-cache` löschen, das Projekt wiederherstellen und überprüfen, ob die vorherige Bereitstellung im System vollständig gelöscht wurde, bevor Sie die App erneut bereitstellen.

> [!TIP]
> Eine einfache Möglichkeit zum Löschen des Paketcaches ist, das Tool *NuGet.exe* von [NuGet.org](https://www.nuget.org/) abzurufen, es Ihrem Systempfad hinzuzufügen, und `nuget locals all -clear` über eine Eingabeaufforderung auszuführen. Sie können auch den Befehl `dotnet nuget locals all --clear` über eine Eingabeaufforderung ausführen, ohne *NuGet.exe* abzurufen.

## <a name="common-errors"></a>Häufige Fehler

Die folgende Liste ist keine vollständige Fehlerliste. Wenn bei Ihnen ein Fehler auftreten sollte, der hier nicht aufgeführt ist, geben Sie bitte eine detaillierte Fehlermeldung unten im Abschnitt „Kommentare“ ein.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>Installationsprogramm konnte VC++ Redistributable nicht abrufen

* **Ausnahme des Installationsprogramms:** 0x80072efd oder 0x80072f76: Unbekannter Fehler

* **Protokollausnahme des Installationsprogramms&#8224;:** Fehler 0x80072efd oder 0x80072f76: Fehler beim Ausführen des EXE-Pakets

  &#8224;Das Protokoll befindet sich unter „C:\Benutzer\\{BENUTZER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log“.

Problembehandlung:

* Verfügt das System während der Installation des Server Hosting-Pakets nicht über Zugriff auf das Internet, tritt diese Ausnahme auf, wenn das Installationsprogramm *Microsoft Visual C++ 2015 Redistributable* nicht abrufen kann. Sie können ein Installationsprogramm aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840) abrufen. Wenn das Installationsprogramm fehlschlägt, erhalten Sie möglicherweise die .NET Core-Runtime nicht, die zum Hosten einer Framework-abhängigen Bereitstellung erforderlich ist. Wenn Sie beabsichtigen, eine Framework-abhängige Bereitstellung zu hosten, vergewissern Sie sich, dass die Runtime unter „Programme &amp; Features“ installiert ist. Ein Runtime-Installationsprogramm können Sie über die [.NET-Downloads](https://www.microsoft.com/net/download/core) herunterladen. Starten Sie nach dem Installieren der Runtime das System neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Durch ein Upgrade des Betriebssystems wird das ASP.NET Core-Modul (32-Bit) entfernt

* **Anwendungsprotokoll:** Die Modul-DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** konnte nicht geladen werden. Die Daten sind der Fehler.

Problembehandlung:

* Nicht zum Betriebssystem gehörende Dateien im Verzeichnis **C:\Windows\SysWOW64\inetsrv** werden während eines Betriebssystemupgrades nicht beibehalten. Wenn Sie das ASP.NET Core-Modul installiert haben, bevor Sie ein Betriebssystemupgrade durchführen, und dann nach einem Betriebssystemupgrade versuchen, einen App-Pool im 32-Bit-Modus auszuführen, tritt dieses Problem auf. Reparieren Sie nach einem Betriebssystemupgrade das ASP.NET Core-Modul. Siehe [Installieren des Pakets „.NET Core Windows Server Hosting“](#install-the-net-core-windows-server-hosting-bundle). Wählen Sie **Reparatur** aus, wenn Sie das Installationsprogramm ausführen.

### <a name="platform-conflicts-with-rid"></a>Plattformkonflikte mit RID

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' mit dem physischen Stamm 'C:\{PFAD}\'' konnte den Prozess mit der Befehlszeile '"C:\\{PFAD}\my_application.{exe|dll}"' nicht starten. Fehlercode = 0 x 80004005: ff.

* **Protokoll des ASP.NET Core-Moduls:** Ausnahmefehler: System.BadImageFormatException: Datei oder Assembly „my_application.dll“ konnte nicht geladen werden. Es wurde versucht, ein Programm mit einem falschen Format zu laden.

Problembehandlung:

* Vergewissern Sie sich, dass die Anwendung lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der Anwendung sein. Weitere Informationen finden Sie unter [Tipps zur Problembehandlung](#troubleshooting-tips).

* Vergewissern Sie sich, dass Sie kein `<PlatformTarget>` in Ihrer *CSPROJ* festgelegt haben, das im Konflikt mit der RID steht. Geben Sie z.B. als `<PlatformTarget>` nicht `x86` an, und führen Sie die Veröffentlichung nicht mit der RID `win10-x64` durch, entweder mit *dotnet publish -c Release -r win10-x64* oder durch Festlegen von `<RuntimeIdentifiers>` in Ihrer *CSPROJ*  auf `win10-x64`. Das Projekt wird ohne Warnung oder Fehler veröffentlicht, schlägt jedoch mit den oben genannten protokollierten Ausnahmen im System fehl.

* Wenn diese Ausnahme für eine Azure-App-Bereitstellung beim Aktualisieren einer Anwendung und beim Bereitstellen neuerer Assemblys auftritt, löschen Sie alle Dateien aus der vorherigen Bereitstellung manuell. Veraltete inkompatible Assemblys können zu einer `System.BadImageFormatException`-Ausnahme führen, wenn Sie eine aktualisierte App bereitstellen.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>URI-Endpunkt falsch oder beendete Website

* **Browser:** ERR_CONNECTION_REFUSED

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Vergewissern Sie sich, dass Sie den richtigen URI-Endpunkt für die Anwendung verwenden. Überprüfen Sie Ihre Bindungen.

* Vergewissern Sie sich, dass die IIS-Website nicht den Status *Beendet* aufweist.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine oder W3SVC-Serverfeatures deaktiviert

* **Ausnahme des Betriebssystems:** Die Features CoreWebEngine und W3SVC von IIS 7.0 müssen installiert sein, um das ASP.NET Core-Modul zu verwenden.

Problembehandlung:

* Vergewissern Sie sich, dass Sie die richtigen Rollen und Features aktiviert haben. Siehe [IIS-Konfiguration](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Falscher physischer Pfad der Website oder Anwendung fehlt

* **Browser:** 403 Verboten: Zugriff wird verweigert  **– oder –**  403.14 Verboten: Der Webserver ist so konfiguriert, dass der Inhalt dieses Verzeichnisses nicht aufgelistet wird.

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Überprüfen Sie die **Grundeinstellungen** der IIS-Website und den physischen Anwendungsordner. Vergewissern Sie sich, dass sich die Anwendung im Ordner im **physischen Pfad** der IIS-Website befindet.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Falsche Rolle, Modul nicht installiert oder falsche Berechtigungen

* **Browser:** 500.19 Interner Serverfehler: Auf die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Vergewissern Sie sich, dass Sie die richtige Rolle aktiviert haben. Siehe [IIS-Konfiguration](#iis-configuration).

* Überprüfen Sie unter **Programme und Features**, ob das **Microsoft ASP.NET Core-Modul** installiert wurde. Wenn das **Microsoft ASP.NET Core-Modul** in der Liste der installierten Programme nicht vorhanden ist, installieren Sie das Modul. Siehe [Installieren des Pakets „.NET Core Windows Server Hosting“](#install-the-net-core-windows-server-hosting-bundle).

* Stellen Sie sicher, dass **Anwendungspool** > **Prozessmodell** > **Identität** auf **ApplicationPoolIdentity** festgelegt ist oder dass Ihre benutzerdefinierte Identität über die erforderlichen Berechtigungen zum Zugriff auf den Bereitstellungsordner der App verfügt.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Falscher processPath-Wert, fehlende PATH-Variable, Hostingpaket nicht installiert, System/IIS wird nicht neu gestartet, VC++ Redistributable nicht installiert oder dotnet.exe-Zugriffsverletzung

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' mit dem physischen Stamm 'C:\\{PFAD}\'' konnte den Prozess mit der Befehlszeile '".\my_application.exe"' nicht starten. Fehlercode = 0x80070002 : 0.

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer

Problembehandlung:

* Vergewissern Sie sich, dass die Anwendung lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der Anwendung sein. Weitere Informationen finden Sie unter [Tipps zur Problembehandlung](#troubleshooting-tips).

* Überprüfen Sie das Attribut *processPath* im Element `<aspNetCore>` in der Datei *web.config*, um sicherzustellen, dass es *dotnet* für eine Framework-abhängige Bereitstellung oder *.\my_application.exe* für eine eigenständige Bereitstellung ist.

* Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise über die PATH-Einstellungen nicht verfügbar. Überprüfen Sie, ob *C:\Programme\dotnet\* in den PATH-Einstellungen des Systems vorhanden ist.

* Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise für die Benutzeridentität des Anwendungspools nicht verfügbar. Vergewissern Sie sich, dass die AppPool-Benutzeridentität Zugriff auf das Verzeichnis *C:\Programme\dotnet* hat. Vergewissern Sie sich, dass keine Verweigerungsregeln für die AppPool-Benutzeridentität für *C:\Programme\dotnet* und Anwendungsverzeichnisse konfiguriert sind.

* Möglicherweise haben Sie eine Framework-abhängige Bereitstellung bereitgestellt und .NET Core installiert, ohne IIS neu zu starten. Starten Sie den Server neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

* Möglicherweise haben Sie eine Framework-abhängige Bereitstellung bereitgestellt, ohne die .NET Core-Runtime auf dem Hostsystem zu installieren. Wenn Sie versuchen, eine Framework-abhängige Bereitstellung bereitzustellen, und die .NET Core-Runtime nicht installiert haben, führen Sie das **Installationsprogramm für das Paket „.NET Core Windows Server Hosting“** in dem System aus. Siehe [Installieren des Pakets „.NET Core Windows Server Hosting“](#install-the-net-core-windows-server-hosting-bundle). Wenn Sie versuchen, die .NET Core-Runtime in einem System ohne Internetverbindung zu installieren, laden Sie die Runtime aus den [.NET-Downloads](https://www.microsoft.com/net/download/core) herunter, und führen Sie das Installationsprogramm für das Hostingpaket aus, um das ASP.NET Core-Modul zu installieren. Schließen Sie die Installation ab, indem Sie das System oder IIS neu starten. Führen Sie dazu **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus.

* Möglicherweise haben Sie eine Framework-abhängige Bereitstellung bereitgestellt und .NET Core installiert, ohne system/IIS neu zu starten. Starten Sie das System neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

* Möglicherweise haben Sie eine Framework-abhängige Bereitstellung bereitgestellt, und *Microsoft Visual C++ 2015 Redistributable (x64)* ist im System nicht installiert. Sie können ein Installationsprogramm aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840) abrufen.

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Falsche Argumente des Elements \<aspNetCore\>

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' mit dem physischen Stamm 'C:\\{PFAD}\'' konnte den Prozess mit der Befehlszeile '"dotnet" .\my_application.dll' nicht starten. Fehlercode = 0x80004005 : 80008081.

* **Protokoll des ASP.NET Core-Moduls:** Die auszuführende Anwendung ist nicht vorhanden: 'PFAD\my_application.dll'

Problembehandlung:

* Vergewissern Sie sich, dass die Anwendung lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der Anwendung sein. Weitere Informationen finden Sie unter [Tipps zur Problembehandlung](#troubleshooting-tips).

* Überprüfen Sie das Attribut *arguments* im Element `<aspNetCore>` in *web.config*, um sicherzustellen, dass es (a) *.\my_applciation.dll* für eine Framework-abhängige Bereitstellung oder (b) nicht vorhanden, eine leere Zeichenfolge (*arguments=""*) oder eine Liste von Argumenten der App (*arguments="arg1, arg2,..."*) für eine eigenständige Bereitstellung ist.

### <a name="missing-net-framework-version"></a>Fehlende .NET Framework-Version

* **Browser:** 502.3 Ungültiges Gateway: Bei der Weiterleitung der Anforderung ist ein Verbindungsfehler aufgetreten.

* **Anwendungsprotokoll:** Fehlercode: Anwendung 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' mit dem physischen Stamm 'C:\\{PFAD}\'' konnte den Prozess mit der Befehlszeile '"dotnet" .\my_application.dll' nicht starten. Fehlercode = 0x80004005 : 80008081.

* **Protokoll des ASP.NET Core-Moduls:** Ausnahme wegen fehlender Methode, Datei oder Assembly. Die in der Ausnahme angegebene Methode, Datei oder Assembly ist eine .NET Framework-Methode, -Datei oder -Assembly.

Problembehandlung:

* Installieren Sie die .NET Framework-Version, die im System fehlt.

* Stellen Sie für eine Framework-abhängige Bereitstellung sicher, dass die richtige Runtime im System installiert ist. Wenn Sie ein Projekt von 1.1 auf 2.0 aktualisieren, auf dem Hostsystem bereitstellen und diese Ausnahme erhalten, müssen Sie Framework 2.0 auf dem Hostsystem installieren.

### <a name="stopped-application-pool"></a>Beendeter Anwendungspool

* **Browser:** 503 Dienst nicht verfügbar

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung

* Vergewissern Sie sich, dass der Anwendungspool nicht den Status *Beendet* aufweist.

### <a name="iis-integration-middleware-not-implemented"></a>Middleware für die Integration von IIS nicht implementiert

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' mit dem physischen Stamm 'C:\\{PFAD}\' hat den Prozess mit der Befehlszeile '"C:\\{PFAD}\my_application.{exe|dll}"' erstellt, ist jedoch entweder abgestürzt, hat nicht geantwortet oder lauscht nicht am angegebenen Port '{PORT}', Fehlercode = '0x800705b4'

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei wurde erstellt und gibt Normalbetrieb an.

Problembehandlung

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Tipps zur Problembehandlung](#troubleshooting-tips).

* Stellen Sie sicher, dass Sie ordnungsgemäß auf die Middleware für die Integration von IIS verwiesen haben, indem Sie die Methode *.UseIISIntegration()* für *WebHostBuilder()* (ASP.NET Core 1.x) der Anwendung aufrufen, oder dass Sie die Methode `CreateDefaultBuilder` (ASP.NET Core 2.x) verwendet haben. Weitere Informationen finden Sie unter [Hosten in ASP.NET Core](xref:fundamentals/hosting).

### <a name="sub-application-includes-a-handlers-section"></a>Untergeordnete Anwendung enthält einen \<handlers\>-Abschnitt

* **Browser:** HTTP-Fehler 500.19: Interner Serverfehler

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei wurde erstellt und gibt für die Stammanwendung Normalbetrieb an. Die Protokolldatei wurde für die untergeordnete Anwendung nicht erstellt.

Problembehandlung

* Überprüfen Sie, ob die Datei *web.config* der untergeordneten App einen `<handlers>`-Abschnitt enthält.

### <a name="application-configuration-general-issue"></a>Allgemeines Problem mit der Anwendungskonfiguration

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' mit dem physischen Stamm 'C:\\{PFAD}\' hat den Prozess mit der Befehlszeile '"C:\\{PFAD}\my_application.{exe|dll}"' erstellt, ist jedoch entweder abgestürzt, hat nicht geantwortet oder lauscht nicht am angegebenen Port '{PORT}', Fehlercode = '0x800705b4'

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer

Problembehandlung

* Diese allgemeine Ausnahme gibt an, dass ein Prozess nicht gestartet wurde, wahrscheinlich aufgrund eines Problems mit der Anwendungskonfiguration. Überprüfen Sie in der [Verzeichnisstruktur](xref:hosting/directory-structure), ob die bereitgestellten Dateien und Ordner der App in Ordnung sind und ob die Konfigurationsdateien der App vorhanden sind und die richtigen Einstellungen für Ihre App und die Umgebung enthalten. Weitere Informationen finden Sie unter [Tipps zur Problembehandlung](#troubleshooting-tips).

## <a name="resources"></a>Ressourcen

* [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:hosting/aspnet-core-module)
* [Verwenden von IIS-Modulen mit ASP.NET Core](xref:hosting/iis-modules)
* [Einführung in ASP.NET Core](../index.md)
* [Die offizielle Microsoft IIS-Website](https://www.iis.net/)
* [Microsoft TechNet-Bibliothek: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
