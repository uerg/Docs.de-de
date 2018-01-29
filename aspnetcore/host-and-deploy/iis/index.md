---
title: Hosten von ASP.NET Core unter Windows mit IIS
author: guardrex
description: Erfahren Sie, wie ASP.NET Core-Apps in Windows Server Internet Information Services (IIS) gehostet werden.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hosten von ASP.NET Core unter Windows mit IIS

Von [Luke Latham](https://github.com/guardrex) und [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgenden Betriebssysteme werden unterstützt:

* Windows 7 und höher
* Windows Server 2008 R2 und höher&#8224;

&#8224;Vom Konzept her gilt die in diesem Dokument beschriebene IIS-Konfiguration auch für das Hosten von ASP.NET Core-Apps mit IIS unter Nano Server, die genauen Anweisungen finden Sie jedoch unter [ASP.NET Core mit IIS unter Nano Server](xref:tutorials/nano-server).

Der [HTTP.SYS-Server](xref:fundamentals/servers/httpsys) (zuvor [WebListener](xref:fundamentals/servers/weblistener) genannt) funktioniert nicht in einer Reverseproxykonfiguration mit IIS. Verwenden Sie den [Kestrel-Server](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>IIS-Konfiguration

Aktivieren Sie die Rolle **Webserver (IIS)**, und richten Sie Rollendienste ein.

### <a name="windows-desktop-operating-systems"></a>Windows-Desktopbetriebssysteme

Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm). Öffnen Sie die Gruppe für **Internetinformationsdienste** und **Webverwaltungstools**. Aktivieren Sie das Kontrollkästchen für **IIS-Verwaltungskonsole**. Aktivieren Sie das Kontrollkästchen für **WWW-Dienste**. Akzeptieren Sie die Standardfeatures für **WWW-Dienste**, oder passen Sie die IIS-Features an.

![Die IIS-Verwaltungskonsole und WWW-Dienste werden in Windows-Features ausgewählt.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server-Betriebssysteme

Verwenden Sie für Serverbetriebssysteme den Assistenten **Rollen und Features hinzufügen** im Menü **Verwalten** oder den Link in **Server-Manager**. Aktivieren Sie im Schritt **Serverrollen** das Kontrollkästchen für **Webserver (IIS)**.

![Die Rolle „Webserver (IIS)“ wird im Schritt „Serverrollen auswählen“ ausgewählt.](index/_static/server-roles-ws2016.png)

Wählen Sie im Schritt **Rollendienste** die gewünschten IIS-Rollendienste aus, oder übernehmen Sie die bereitgestellten Standardrollendienste.

![Die Standardrollendienste werden im Schritt „Rollendienste auswählen“ ausgewählt.](index/_static/role-services-ws2016.png)

Fahren Sie mit dem Schritt **Bestätigung** fort, um die Webserverrolle und die Dienste zu installieren. Ein Server-/IIS-Neustart ist nach der Installation der Rolle „Webserver (IIS)“ nicht erforderlich.

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Installieren des Pakets „.NET Core Windows Server Hosting“

1. Installieren Sie das [Paket „.NET Core Windows Server Hosting“](https://aka.ms/dotnetcore-2-windowshosting) im Hostsystem. Das Paket installiert die .NET Core-Runtime, die .NET Core-Bibliothek und das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module). Das Modul erstellt den Reverseproxy zwischen IIS und dem Kestrel-Server. Wenn das System nicht über eine Internetverbindung verfügt, beziehen und installieren Sie [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840), bevor Sie das Paket „.NET Core Windows Server Hosting“ installieren.

2. Starten Sie das System neu, oder führen Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus, um eine Änderung am Systempfad zu übernehmen.

> [!NOTE]
> Informationen zur Verwendung einer IIS-Freigabekonfiguration finden Sie unter [ASP.NET Core-Modul mit IIS-Freigabekonfiguration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Installieren von Web Deploy beim Veröffentlichen mit Visual Studio

Wenn Sie Apps auf Servern mit [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) bereitstellen, installieren Sie die neueste Version von Web Deploy auf dem Server. Um Web Deploy zu installieren, verwenden Sie den [Webplattform-Installer (Web PI)](https://www.microsoft.com/web/downloads/platform.aspx) oder rufen einen Installer direkt aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717) ab. Die bevorzugte Methode ist die Verwendung von WebPI. WebPI bietet ein eigenständiges Setup und eine Konfiguration für Hostinganbieter.

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

Nehmen Sie eine Abhängigkeit vom Paket [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) in die App-Abhängigkeiten auf. Binden Sie Middleware für die Integration von IIS in die App ein, indem Sie die Erweiterungsmethode *UseIISIntegration* zu *WebHostBuilder* hinzufügen:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Es sind jeweils `UseKestrel` und `UseIISIntegration` erforderlich. Ein Code, der `UseIISIntegration` aufruft, hat keinen Einfluss auf die Codeportabilität. Wenn Die App nicht hinter IIS ausgeführt wird (die App wird z.B. direkt auf Kestrel ausgeführt), hat `UseIISIntegration` keine Funktion.

---

Weitere Informationen zum Hosten finden Sie unter [Hosting in ASP.NET Core (Hosten in ASP.NET Core)](xref:fundamentals/hosting).

### <a name="iis-options"></a>IIS-Optionen

Schließen Sie zur Konfiguration von IIS-Optionen eine Dienstkonfiguration für `IISOptions` in `ConfigureServices` ein:

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

Die Datei *Web.config* dient in erster Linie der Konfiguration des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module). Sie kann optional zusätzliche IIS-Konfigurationseinstellungen bereitstellen. Das Erstellen, das Transformieren und die Veröffentlichung von *web.config* erfolgt durch das .NET Core Web SDK (`Microsoft.NET.Sdk.Web`). Das SDK wird am Anfang der Projektdatei (`<Project Sdk="Microsoft.NET.Sdk.Web">`) festgelegt. Um zu verhindern, dass das SDK die Datei *web.config* transformiert, fügen Sie der Projektdatei die Eigenschaft **\<IsTransformWebConfigDisabled** mit der Einstellung `true` hinzu:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Wenn eine Datei namens *Web.config* im Projekt vorhanden ist, wird sie zur Konfiguration des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) mit dem richtigen *processPath*- und *arguments*-Attribut transformiert und in die [veröffentlichte Ausgabe](xref:host-and-deploy/directory-structure) verschoben. Die Transformation ändert die IIS-Konfigurationseinstellungen in der Datei nicht.

### <a name="webconfig-location"></a>Speicherort der Datei „Web.config“

.NET Core-Apps werden über einen Reverseproxy zwischen IIS und dem Kestrel-Server gehostet. Zum Erstellen des Reverseproxys muss die Datei *Web.config* im Stammpfad des Inhalts (normalerweise dem App-Basispfad) der bereitgestellten App vorhanden sein. Dies ist der physische Pfad der Website, der in IIS bereitgestellt wurde. Die Datei *Web.config* wird im Stammverzeichnis der App benötigt, um die Veröffentlichung mehrerer Apps mit Web Deploy zu ermöglichen.

Im physischen Pfad und den Unterordnern der App sind vertrauliche Dateien vorhanden, wie z.B. *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML-Dokumentationskommentare) und *\<assembly_name>.deps.json*. Wenn die Datei *Web.config* vorhanden ist und die Website konfiguriert, verhindert IIS, dass diese vertraulichen Dateien verarbeitet werden. **Daher ist es wichtig, dass die Datei *web.config* nicht versehentlich umbenannt oder aus der Bereitstellung entfernt wird.**

## <a name="create-the-iis-website"></a>Erstellen der IIS-Website

1. Erstellen Sie im IIS-Zielsystem einen Ordner für die veröffentlichten Ordner und Dateien der App, die in [Verzeichnisstruktur](xref:host-and-deploy/directory-structure) beschrieben sind.

2. Erstellen Sie im Ordner einen Ordner *Protokolle*, um darin StdOut-Protokolle zu speichern, wenn die StdOut-Protokollierung aktiviert ist. Wenn die App mit einem Ordner *Protokolle* in der Payload bereitgestellt wird, können Sie diesen Schritt überspringen. Im Artikel zur *Verzeichnisstruktur* wird erläutert, wie Sie MSBuild dazu veranlassen, den Ordner [Protokolle](xref:host-and-deploy/directory-structure) zu erstellen.

3. Erstellen Sie im **IIS-Manager** eine neue Website. Geben Sie einen **Websitenamen** an, und legen Sie den **physischen Pfad** auf den Bereitstellungsordner der App fest. Geben Sie die Konfiguration unter **Bindung** an, und erstellen Sie die Website.

4. Legen Sie den Anwendungspool auf **Kein verwalteter Code** fest. ASP.NET Core wird in einem separaten Prozess ausgeführt und verwaltet die Runtime.

5. Öffnen Sie das Fenster **Website hinzufügen**.

   ![Klicken Sie im Kontextmenü „Websites“ auf „Website hinzufügen“.](index/_static/add-website-context-menu-ws2016.png)

6. Konfigurieren Sie die Website.

   ![Geben Sie den Websitenamen, den physischen Pfad und den Hostnamen im Schritt „Website hinzufügen“ an.](index/_static/add-website-ws2016.png)

7. Öffnen Sie im Bereich **Anwendungspools** das Fenster **Anwendungspool bearbeiten**, indem Sie mit der rechten Maustaste auf den Anwendungspool der Website klicken und im Popupmenü **Grundeinstellungen** auswählen.

   ![Wählen Sie „Grundeinstellungen“ aus dem Kontextmenü des Anwendungspools aus.](index/_static/apppools-basic-settings-ws2016.png)

8. Legen Sie die **.NET CLR-Version** auf **Kein verwalteter Code** fest.

   ![Legen Sie „Kein verwalteter Code“ für die .NET CLR-Version fest.](index/_static/edit-apppool-ws2016.png)
     
    Hinweis: Das Festlegen der **.NET CLR-Version** auf **Kein verwalteter Code** ist optional. Für ASP.NET Core ist das Laden der Desktop-CLR nicht erforderlich.

9. Vergewissern Sie sich, dass die Prozessmodellidentität über die richtigen Berechtigungen verfügt.

   Wenn die Standardidentität des App-Pools (**Prozessmodell** > **Identität**) von **ApplicationPoolIdentity** in eine andere Identität geändert wird, stellen Sie sicher, dass die neue Identität über die erforderlichen Berechtigungen zum Zugriff auf den Ordner, die Datenbank und andere erforderliche Ressourcen der App verfügt.
   
## <a name="deploy-the-app"></a>Bereitstellen der App

Stellen Sie die App in dem Ordner bereit, den Sie im IIS-Zielsystem erstellt haben. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ist die empfohlene Methode zur Bereitstellung.

Stellen Sie sicher, dass die zur Bereitstellung veröffentlichte App nicht ausgeführt wird. Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird. Gesperrte Dateien können nicht überschrieben werden. Um gesperrte Dateien in einer Bereitstellung freizugeben, beenden Sie den App-Pool mit einer der folgenden Methoden:

* Durch manuelles Beenden im IIS-Manager auf dem Server.
* Durch Verwenden von Web Deploy und Verweisen auf `Microsoft.NET.Sdk.Web` in der Projektdatei. Eine Datei namens *app_offline.htm* befindet sich im Stammverzeichnis des Web-App-Verzeichnisses. Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung. Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
* Verwenden Sie PowerShell, um den App-Pool zu beenden und neu zu starten (erfordert PowerShell 5 oder höher):
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>Web Deploy mit Visual Studio

Informationen zum Erstellen eines Veröffentlichungsprofils zur Verwendung mit Web Deploy finden Sie im Thema [Visual Studio publish profiles for ASP.NET Core app deployment (Visual Studio-Veröffentlichungsprofile zum Bereitstellen von ASP.NET Core-Apps)](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles). Wenn der Hostinganbieter ein Veröffentlichungsprofil oder Unterstützung für das Erstellen eines solchen Profils bereitstellt, laden Sie dessen Profil herunter, und importieren Sie es mithilfe des Visual Studio-Dialogfelds **Veröffentlichen**.

![Dialogfeld „Veröffentlichen“](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web Deploy außerhalb von Visual Studio

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) kann über die Befehlszeile auch außerhalb von Visual Studio verwendet werden. Weitere Informationen finden Sie unter [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool) (Webbereitstellungstool).

### <a name="alternatives-to-web-deploy"></a>Alternativen zu Web Deploy

Verwenden Sie eine der folgenden Methoden, um die App zum Hostingsystem zu migrieren, z.B. XCopy, Robocopy oder PowerShell. Visual Studio-Benutzer können die [Veröffentlichungsbeispiele](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md) verwenden.

## <a name="browse-the-website"></a>Navigieren auf der Website

![Der Microsoft Edge-Browser hat die IIS-Startseite geladen.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Schutz von Daten

Der Schutz von Daten wird von mehreren ASP.NET-Middlewarefunktionen genutzt, einschließlich der Middleware, die bei der Authentifizierung verwendet wird. Selbst wenn die Datenschutz-APIs nicht aus dem Code des Benutzers aufgerufen werden, sollte der Schutz von Daten für die Erstellung eines beständigen Schlüsselspeichers mit einem Bereitstellungsskript oder im Benutzercode konfiguriert werden. Wenn der Schutz von Daten nicht konfiguriert ist, werden die Schlüssel beim Neustarten der App im Arbeitsspeicher gespeichert und verworfen.

Falls der Schlüsselbund im Arbeitsspeicher gespeichert wird, wenn die App neu gestartet wird:

* Werden alle Formularauthentifizierungstoken als ungültig ausgewiesen. 
* Benutzer müssen sich bei ihrer nächsten Anforderung erneut anmelden. 
* Alle mit dem Schlüsselbund geschützte Daten können nicht mehr entschlüsselt werden.

Zum Konfigurieren des Schutzes von Daten unter IIS verwenden Sie **einen** der folgenden Ansätze:

### <a name="create-a-data-protection-registry-hive"></a>Erstellen einer Registrierungsstruktur für den Schutz von Daten

Schlüssel für den Schutz von Daten, die von ASP.NET-Apps verwendet werden, werden in Registrierungsstrukturen außerhalb der Apps gespeichert. Um die Schlüssel für eine bestimmte App beizubehalten, müssen Sie eine Registrierungsstruktur für den App-Pool erstellen.

Bei eigenständigen IIS-Installationen, die ohne Webfarm vorgesehen sind, kann das [PowerShell-Skript „Provision-AutoGenKeys.ps1“ für den Schutz von Daten](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) für jeden App-Pool genutzt werden, das mit einer ASP.NET Core-App verwendet wird. Dieses Skript erstellt einen besonderen Registrierungsschlüssel in der HKLM-Registrierung, der nur in der ACL des Workerprozesskontos bereitgestellt wird. Schlüssel werden in ruhendem Zustand mit DPAPI mit einem computerweiten Schlüssel verschlüsselt.

In Webfarmszenarios kann eine App so konfiguriert werden, dass sie einen UNC-Pfad verwendet, um den Schlüsselbund für den Schutz von Daten zu speichern. Standardmäßig werden die Schlüssel für den Schutz von Daten nicht verschlüsselt. Stellen Sie sicher, dass die Dateiberechtigungen für eine solche Freigabe auf das Windows-Konto beschränkt sind, mit dem die App ausgeführt wird. Darüber hinaus kann ein X.509-Zertifikat zum Schützen von Schlüsseln im ruhenden Zustand verwendet werden. Ziehen Sie einen Mechanismus in Erwägung, um es Benutzern zu ermöglichen, Zertifikate hochzuladen: Platzieren Sie Zertifikate im Zertifikatspeicher des Benutzers für vertrauenswürdige Anbieter, und stellen Sie sicher, dass sie auf allen Computern verfügbar sind, auf denen die App des Benutzers ausgeführt wird. Details finden Sie unter [Konfigurieren des Schutzes von Daten](xref:security/data-protection/configuration/overview).

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Konfigurieren des IIS-Anwendungspools zum Laden des Benutzerprofils

Diese Einstellung befindet sich im Abschnitt **Prozessmodell** unter **Erweiterte Einstellungen** für den App-Pool. Legen Sie „Benutzerprofil laden“ auf `True` fest. Dadurch werden Schlüssel im Benutzerprofilverzeichnis gespeichert und mit DPAPI mit einem Schlüssel geschützt, der nur für das Benutzerkonto gilt, das für den App-Pool verwendet wird.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Verwenden des Dateisystems als Schlüsselbundspeicher

Passen Sie den App-Code so an, dass er [das Dateisystem als Schlüsselbundspeicher verwendet](xref:security/data-protection/configuration/overview). Verwenden Sie ein X.509-Zertifikat, um den Schlüsselbund zu schützen, und stellen Sie sicher, dass es sich bei dem Zertifikat um ein vertrauenswürdiges Zertifikat handelt. Ein selbstsigniertes Zertifikat müssen Sie im vertrauenswürdigen Stammspeicher platzieren.

Wenn IIS in einer Webfarm verwendet wird:

* Verwenden Sie eine Dateifreigabe, auf die alle Computer zugreifen können.
* Stellen Sie ein X509-Zertifikat auf jedem Computer bereit. Konfigurieren Sie den [Schutz von Daten im Code](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Festlegen einer computerweiten Richtlinie für den Schutz von Daten

Das System zum Schutz von Daten verfügt über eine eingeschränkte Unterstützung zum Festlegen einer [computerweiten Standardrichtlinie](xref:security/data-protection/configuration/machine-wide-policy) für alle Apps, die die Datenschutz-APIs nutzen. Weitere Details finden Sie in der Dokumentation zum [Schutz von Daten](xref:security/data-protection/index).

## <a name="configuration-of-sub-applications"></a>Konfiguration von untergeordneten Anwendungen

Untergeordnete Apps, die unter der Stamm-App hinzugefügt werden, sollten kein ASP.NET Core-Modul als Handler enthalten. Wenn das Modul als Handler in einer Datei namens *Web.config* einer untergeordneten App hinzugefügt wird, erhalten Sie bei dem Versuch zum Navigieren in der untergeordneten App den Code 500.19 (interner Serverfehler), der auf die fehlerhafte Konfigurationsdatei verweist. Das folgende Beispiel zeigt den Inhalt einer veröffentlichten *web.config*-Datei für eine untergeordnete ASP.NET Core-App:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Wenn Sie eine untergeordnete App ohne ASP.NET Core unterhalb einer ASP.NET Core-App hosten, entfernen Sie den geerbten Handler ausdrücklich aus der Datei *Web.config* der untergeordneten App:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Weitere Informationen zum Konfigurieren des ASP.NET Core-Moduls finden Sie im Thema [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und in der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguration von IIS mit der Datei „web.config“

Die IIS-Konfiguration wird im Hinblick auf IIS-Features, die für eine Reverseproxykonfiguration gelten, vom Abschnitt **\<system.webServer>** der Datei *Web.config* beeinflusst. Wenn IIS auf Systemebene für die Verwendung der dynamischen Komprimierung konfiguriert ist, kann diese Einstellung für eine App mit dem Element **\<urlCompression>** in der Datei *Web.config* der App deaktiviert werden. Weitere Informationen finden Sie in der [Konfigurationsreferenz für \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module) und unter [Verwenden von IIS-Modulen mit ASP.NET Core](xref:host-and-deploy/iis/modules). Wenn Umgebungsvariablen für einzelne Apps festgelegt werden müssen, die in isolierten App-Pools ausgeführt werden (unterstützt für IIS 10.0 und höher), lesen Sie den Abschnitt zum *Befehl „AppCmd.exe“* im Thema [Umgebungsvariablen \<EnvironmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) in der IIS-Referenzdokumentation.

## <a name="configuration-sections-of-webconfig"></a>Konfigurationsabschnitte von „web.config“

Konfigurationsabschnitte von ASP.NET Framework-Apps in der Datei *Web.config* werden nicht von ASP.NET Core-Apps für die Konfiguration verwendet:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

ASP.NET Core-Apps werden mit anderen Konfigurationsanbietern konfiguriert. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Anwendungspools

Wenn Sie mehrere Websites in einem einzigen System hosten, isolieren Sie die Apps voneinander, indem Sie jede App in ihrem eigenen App-Pool ausführen. Im IIS-Dialogfeld **Website hinzufügen** wird standardmäßig diese Konfiguration eingesetzt. Wenn ein **Websitename** angegeben ist, wird der Text automatisch in das Textfeld **Anwendungspool** übertragen. Ein neuer App-Pool wird mit dem Namen der Website erstellt, wenn die Website hinzugefügt wird.

## <a name="application-pool-identity"></a>Identität des Anwendungspools

Mit einem Konto für die Identität des App-Pools können Sie eine App unter einem eindeutigen Konto ausführen, ohne Domänen oder lokale Konten erstellen und verwalten zu müssen. Mit IIS 8.0 und höher erstellt der IIS-Administratorworkerprozess (WAS) ein virtuelles Konto mit dem Namen des neuen App-Pools und führt die Workerprozesse des App-Pools standardmäßig unter diesem Konto aus. Stellen Sie sicher, dass in der IIS-Verwaltungskonsole unter **Erweiterte Einstellungen** für den App-Pool die **Identität** auf **ApplicationPoolIdentity** festgelegt ist:

![Dialogfeld „Erweiterte Einstellungen“ für den Anwendungspool](index/_static/apppool-identity.png)

Der IIS-Verwaltungsprozess erstellt im Windows-Sicherheitssystem einen sicheren Bezeichner mit dem Namen des App-Pools. Ressourcen können unter Verwendung dieser Identität gesichert werden. Allerdings ist diese Identität kein echtes Benutzerkonto und wird in der Windows-Benutzerverwaltungskonsole nicht angezeigt.

Wenn der IIS-Workerprozess erhöhte Rechte für den Zugriff auf Ihre Anwendung erfordert, ändern Sie die Zugriffssteuerungsliste (ACL) für das Verzeichnis mit der App:

1. Öffnen Sie Windows Explorer, und navigieren Sie zum Verzeichnis.

2. Klicken Sie mit der rechten Maustaste auf das Verzeichnis, und klicken Sie auf **Eigenschaften**.

3. Klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Bearbeiten** und dann auf die Schaltfläche **Hinzufügen**.

4. Wählen Sie die Schaltfläche **Speicherorte** aus, und stellen Sie sicher, dass das System ausgewählt ist.

5. Geben Sie in das Textfeld **Geben Sie die zu verwendenden Objektnamen ein** den Text **IIS AppPool\DefaultAppPool** ein.

   ![Dialogfeld „Benutzer oder Gruppen auswählen“ für den App-Ordner](index/_static/select-users-or-groups-1.png)

6. Klicken Sie auf die Schaltfläche **Namen überprüfen**. Klicken Sie auf **OK**.

   ![Dialogfeld „Benutzer oder Gruppen auswählen“ für den App-Ordner](index/_static/select-users-or-groups-2.png)

Dies kann auch über eine Eingabeaufforderung mit dem Tool **ICACLS** erreicht werden:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Problembehandlung bei ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
* [Verwenden von IIS-Modulen mit ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Einführung in ASP.NET Core](../index.md)
* [Die offizielle Microsoft IIS-Website](https://www.iis.net/)
* [Microsoft TechNet-Bibliothek: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
