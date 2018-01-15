---
title: ASP.NET Core auf Nano Server
author: shirhatti
description: "Hier erfahren Sie, wie Sie eine bestehende ASP.NET Core-App auf einer Nano Server-Instanz bereitstellen, auf der Internetinformationsdienste (Internet Information Services – IIS) ausgeführt werden."
keywords: ASP.NET Core, Nano Server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: f30e911703d5c36d076872f91d4b2fafeefb91f5
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>ASP.NET Core mit IIS auf Nano Server

Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)

In diesem Tutorial stellen Sie eine bestehende ASP.NET Core-App auf einer Nano Server-Instanz bereit, auf der IIS ausgeführt werden.

## <a name="introduction"></a>Einführung

Nano Server ist eine Installationsoption in Windows Server 2016. Sie zeichnet sich gegenüber den Installationsoptionen „Server Core“ und „vollständiger“ Server durch geringeren Speicherplatzbedarf, mehr Sicherheit und bessere Wartbarkeit aus. Weitere Informationen und Downloadlinks für eine 180 Tage gültige Testversion finden Sie in der offiziellen [Dokumentation zu Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server). 

Sie verfügen über drei einfache Möglichkeiten, Nano Server zu testen. Wenn Sie sich mit Ihrem MS-Konto anmelden, können Sie Folgendes tun:

1. Sie können die ISO-Datei für Windows Server 2016 herunterladen und ein Nano Server-Image erstellen.

2. Sie können die Nano Server-VHD herunterladen.

3. Sie können mit dem Nano Server-Image aus dem Azure-Katalog eine VM in Azure erstellen. Wenn Sie noch über kein Azure-Konto verfügen, können Sie sich für eine kostenlose 30-Tage-Testversion registrieren.

In diesem Tutorial verwenden wir die zweite Option, die vorgefertigte Nano Server-VHD von Windows Server 2016.

Um mit diesem Tutorial fortfahren zu können, benötigen Sie die [veröffentlichte Ausgabe](xref:host-and-deploy/directory-structure) einer vorhandenen ASP.NET Core-Anwendung. Vergewissern Sie sich, dass ihre Anwendung so erstellt wurde, dass sie in einem **64-Bit**-Prozess ausgeführt werden kann.

## <a name="setting-up-the-nano-server-instance"></a>Einrichten der Nano Server-Instanz

[Erstellen Sie auf Ihrem Entwicklungscomputer einen neuen virtuellen Computer mit Hyper-V](https://technet.microsoft.com/library/hh846766.aspx). Verwenden Sie dazu die vorher heruntergeladene VHD. Vor der ersten Anmeldung an dieser VM müssen Sie ein Administratorkennwort festlegen. Drücken Sie in der VM-Konsole auf F11, um das Kennwort vor der ersten Anmeldung festzulegen. Sie müssen auch die IP-Adresse Ihrer neuen VM überprüfen. Dies können Sie tun, indem Sie die feste IP-Adresse Ihres DHCP-Servers überprüfen, die während der Bereitstellung der VM vergeben wurde. Alternativ können Sie dazu aber auch die Netzwerkeinstellungen in der Nano Server-Wiederherstellungskonsole aufrufen.

> [!NOTE]
> Angenommen, Ihre neue VM weist die lokale IPv4-Adresse 192.168.1.10 auf.

Jetzt können Sie sie mit PowerShell-Remoting verwalten. Nur auf diese Weise verfügen Sie über vollen Administratorzugriff auf Ihren Nano Server.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Herstellen der Verbindung mit Ihrer Nano Server-Instanz mithilfe von PowerShell-Remoting

Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, um Ihre Nano Server-Remoteinstanz zu Ihrer `TrustedHosts`-Liste hinzuzufügen.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Ersetzen Sie die Variable `$nanoServerIpAddress` durch Ihre tatsächliche IP-Adresse.

Nachdem Sie Ihre Nano Server-Instanz zu Ihrer `TrustedHosts`-Liste hinzugefügt haben, können sie über PowerShell-Remoting eine Verbindung mit ihr herstellen.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Bei erfolgreich hergestellter Verbindung erhalten Sie ein Ergebnis ähnlich diesem: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Erstellen einer Dateifreigabe

Erstellen Sie eine Dateifreigabe auf dem Nano Server, damit Sie die veröffentlichte Anwendung in diesen kopieren können. Führen Sie in der Remotesitzung die folgenden Befehle aus:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Danach sollten Sie auf diese Freigabe zugreifen können, indem Sie im Windows Explorer des Hostcomputers zu `\\192.168.1.10\AspNetCoreSampleForNano` navigieren.

## <a name="open-port-in-the-firewall"></a>Öffnen des Ports in der Firewall

Führen Sie in der Remotesitzung die folgenden Befehle aus, um einen Port in der Firewall zu öffnen, damit IIS über den Port TCP/8000 auf TCP-Datenverkehr lauschen kann.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Installieren der IIS

Fügen Sie den Anbieter `NanoServerPackage` aus dem PowerShell-Katalog hinzu. Nach der Installation und dem Import des Anbieters können Sie Windows-Pakete installieren.

Führen Sie in der zuvor erstellten PowerShell-Sitzung die folgenden Befehle aus:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Um schnell zu überprüfen, ob IIS korrekt eingerichtet wurde, rufen Sie die URL `http://192.168.1.10/` auf. Ihnen sollte eine Startseite angezeigt werden. Wenn IIS installiert wird, wird standardmäßig eine Startseite namens `Default Web Site` erstellt, die an Port 80 lauscht.

## <a name="installing-the-aspnet-core-module-ancm"></a>Installieren des ASP.NET Core-Moduls (ANCM)

Das ASP.NET Core-Modul ist ein Modul für IIS 7.5 und höher. Es ist verantwortlich für die Prozessverwaltung für ASP.NET Core-HTTP-Listener, und es dient als Proxy für Anforderungen an Prozesse, die von ihm verwaltet werden. Gegenwärtig müssen Sie das ASP.NET Core-Modul für IIS manuell installieren. Sie müssen das [Paket .NET Core Windows Server Hosting](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) auf einem regulären Computer (keinem Nano-Computer) installieren. Nach der Installation des Pakets auf einem regulären Computer müssen Sie die folgenden Dateien in die zuvor erstellte Dateifreigabe kopieren.

Führen sie auf einem regulären Server (kein Nano-Server) mit IIS die folgenden Kopierbefehle aus:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Ersetzen sie auf einem Windows 10-Computer `C:\windows\system32\inetsrv` durch `C:\Program Files\IIS Express`.

Auf dem Nano-Server müssen Sie die folgenden Befehle aus der zuvor erstellten Dateifreigabe in die gültigen Speicherorte kopieren. Führen Sie also die folgenden Befehle aus:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

Führen Sie in der Remotesitzung das folgende Skript aus:

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> Löschen Sie nach diesem Schritt die Dateien *aspnetcore.dll* und *aspnetcore_schema.xml* aus der Freigabe.

## <a name="installing-net-core-framework"></a>Installieren von .NET Core Framework

Wenn Ihre App als [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) veröffentlicht wird, muss .NET Core auf dem Server installiert sein. Verwenden Sie das [dotnet-install.ps1-PowerShell-Skript](https://dot.net/v1/dotnet-install.ps1) in einer Remotesitzung von PowerShell aus, um .NET Core auf Ihrem Nano Server zu installieren. Übergeben Sie die CLI-Version mit dem `-Version`-Switch:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Veröffentlichen der Anwendung

Kopieren Sie die veröffentlichte Ausgabe der vorhandenen Anwendung in das Stammverzeichnis der Dateifreigabe.

Damit auf den Ort verwiesen wird, an dem Sie *dotnet.exe* extrahiert haben, müssen Sie möglicherweise Änderungen an *web.config* vornehmen. Alternativ können Sie auch *dotnet.exe* zu Ihrem Pfad hinzufügen.

Beispiel dafür, wie *web.config* aussehen kann, wenn *dotnet.exe* **nicht** zum Pfad hinzugefügt wird:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

Führen Sie die folgenden Befehle in der Remotesitzung aus, um in IIS auf einem anderen Port als dem, der für die Standardwebsite vorgesehen ist, einen neuen Standort zu erstellen. Außerdem müssen Sie diesen Port öffnen, um Zugriff auf das Internet zu erhalten. Dieses Skript verwendet den `DefaultAppPool` aus Gründen der Einfachheit. Weitere Überlegungen zur Ausführung unter einem Anwendungspool finden Sie unter [Application Pools (Anwendungspools)](xref:host-and-deploy/iis/index#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Bekanntes Problem beim Ausführen der .NET Core CLI auf Nano Server und Umgehung des Problems
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Ausführen der Anwendung

Auf die veröffentlichte Web-App können Sie in einem Browser über `http://192.168.1.10:8000` zugreifen. Wenn Sie die Protokollierung wie unter [Log creation and redirection (Erstellen und Umleiten von Protokollen)](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) beschrieben eingerichtet haben, können Sie sich die Protokolle unter *C:\PublishedApps\AspNetCoreSampleForNano\logs* ansehen.
