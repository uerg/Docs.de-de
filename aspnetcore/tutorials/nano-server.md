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
ms.openlocfilehash: 337cc69ef522452c17cdd6ea4a5e71cd122035dc
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="a1dfc-104">ASP.NET Core mit IIS auf Nano Server</span><span class="sxs-lookup"><span data-stu-id="a1dfc-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="a1dfc-105">Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="a1dfc-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="a1dfc-106">In diesem Tutorial stellen Sie eine bestehende ASP.NET Core-App auf einer Nano Server-Instanz bereit, auf der IIS ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="a1dfc-107">Einführung</span><span class="sxs-lookup"><span data-stu-id="a1dfc-107">Introduction</span></span>

<span data-ttu-id="a1dfc-108">Nano Server ist eine Installationsoption in Windows Server 2016. Sie zeichnet sich gegenüber den Installationsoptionen „Server Core“ und „vollständiger“ Server durch geringeren Speicherplatzbedarf, mehr Sicherheit und bessere Wartbarkeit aus.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="a1dfc-109">Weitere Informationen und Downloadlinks für eine 180 Tage gültige Testversion finden Sie in der offiziellen [Dokumentation zu Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server).</span><span class="sxs-lookup"><span data-stu-id="a1dfc-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="a1dfc-110">Sie verfügen über drei einfache Möglichkeiten, Nano Server zu testen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="a1dfc-111">Wenn Sie sich mit Ihrem MS-Konto anmelden, können Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="a1dfc-112">Sie können die ISO-Datei für Windows Server 2016 herunterladen und ein Nano Server-Image erstellen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="a1dfc-113">Sie können die Nano Server-VHD herunterladen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="a1dfc-114">Sie können mit dem Nano Server-Image aus dem Azure-Katalog eine VM in Azure erstellen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="a1dfc-115">Wenn Sie noch über kein Azure-Konto verfügen, können Sie sich für eine kostenlose 30-Tage-Testversion registrieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="a1dfc-116">In diesem Tutorial verwenden wir die zweite Option, die vorgefertigte Nano Server-VHD von Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="a1dfc-117">Um mit diesem Tutorial fortfahren zu können, benötigen Sie die [veröffentlichte Ausgabe](xref:hosting/directory-structure) einer vorhandenen ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-117">Before proceeding with this tutorial, you will need the [published output](xref:hosting/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="a1dfc-118">Vergewissern Sie sich, dass ihre Anwendung so erstellt wurde, dass sie in einem **64-Bit**-Prozess ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="a1dfc-119">Einrichten der Nano Server-Instanz</span><span class="sxs-lookup"><span data-stu-id="a1dfc-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="a1dfc-120">[Erstellen Sie auf Ihrem Entwicklungscomputer einen neuen virtuellen Computer mit Hyper-V](https://technet.microsoft.com/library/hh846766.aspx). Verwenden Sie dazu die vorher heruntergeladene VHD.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="a1dfc-121">Vor der ersten Anmeldung an dieser VM müssen Sie ein Administratorkennwort festlegen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="a1dfc-122">Drücken Sie in der VM-Konsole auf F11, um das Kennwort vor der ersten Anmeldung festzulegen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="a1dfc-123">Sie müssen auch die IP-Adresse Ihrer neuen VM überprüfen. Dies können Sie tun, indem Sie die feste IP-Adresse Ihres DHCP-Servers überprüfen, die während der Bereitstellung der VM vergeben wurde. Alternativ können Sie dazu aber auch die Netzwerkeinstellungen in der Nano Server-Wiederherstellungskonsole aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="a1dfc-124">Angenommen, Ihre neue VM weist die lokale IPv4-Adresse 192.168.1.10 auf.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="a1dfc-125">Jetzt können Sie sie mit PowerShell-Remoting verwalten. Nur auf diese Weise verfügen Sie über vollen Administratorzugriff auf Ihren Nano Server.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="a1dfc-126">Herstellen der Verbindung mit Ihrer Nano Server-Instanz mithilfe von PowerShell-Remoting</span><span class="sxs-lookup"><span data-stu-id="a1dfc-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="a1dfc-127">Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, um Ihre Nano Server-Remoteinstanz zu Ihrer `TrustedHosts`-Liste hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="a1dfc-128">Ersetzen Sie die Variable `$nanoServerIpAddress` durch Ihre tatsächliche IP-Adresse.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="a1dfc-129">Nachdem Sie Ihre Nano Server-Instanz zu Ihrer `TrustedHosts`-Liste hinzugefügt haben, können sie über PowerShell-Remoting eine Verbindung mit ihr herstellen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="a1dfc-130">Bei erfolgreich hergestellter Verbindung erhalten Sie ein Ergebnis ähnlich diesem: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="a1dfc-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="a1dfc-131">Erstellen einer Dateifreigabe</span><span class="sxs-lookup"><span data-stu-id="a1dfc-131">Creating a file share</span></span>

<span data-ttu-id="a1dfc-132">Erstellen Sie eine Dateifreigabe auf dem Nano Server, damit Sie die veröffentlichte Anwendung in diesen kopieren können.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="a1dfc-133">Führen Sie in der Remotesitzung die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="a1dfc-134">Danach sollten Sie auf diese Freigabe zugreifen können, indem Sie im Windows Explorer des Hostcomputers zu `\\192.168.1.10\AspNetCoreSampleForNano` navigieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="a1dfc-135">Öffnen des Ports in der Firewall</span><span class="sxs-lookup"><span data-stu-id="a1dfc-135">Open port in the firewall</span></span>

<span data-ttu-id="a1dfc-136">Führen Sie in der Remotesitzung die folgenden Befehle aus, um einen Port in der Firewall zu öffnen, damit IIS über den Port TCP/8000 auf TCP-Datenverkehr lauschen kann.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="a1dfc-137">Installieren der IIS</span><span class="sxs-lookup"><span data-stu-id="a1dfc-137">Installing IIS</span></span>

<span data-ttu-id="a1dfc-138">Fügen Sie den Anbieter `NanoServerPackage` aus dem PowerShell-Katalog hinzu.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="a1dfc-139">Nach der Installation und dem Import des Anbieters können Sie Windows-Pakete installieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="a1dfc-140">Führen Sie in der zuvor erstellten PowerShell-Sitzung die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="a1dfc-141">Um schnell zu überprüfen, ob IIS korrekt eingerichtet wurde, rufen Sie die URL `http://192.168.1.10/` auf. Ihnen sollte eine Startseite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="a1dfc-142">Wenn IIS installiert wird, wird standardmäßig eine Startseite namens `Default Web Site` erstellt, die an Port 80 lauscht.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="a1dfc-143">Installieren des ASP.NET Core-Moduls (ANCM)</span><span class="sxs-lookup"><span data-stu-id="a1dfc-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="a1dfc-144">Das ASP.NET Core-Modul ist ein Modul für IIS 7.5 und höher. Es ist verantwortlich für die Prozessverwaltung für ASP.NET Core-HTTP-Listener, und es dient als Proxy für Anforderungen an Prozesse, die von ihm verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="a1dfc-145">Gegenwärtig müssen Sie das ASP.NET Core-Modul für IIS manuell installieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="a1dfc-146">Sie müssen das [Paket .NET Core Windows Server Hosting](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) auf einem regulären Computer (keinem Nano-Computer) installieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="a1dfc-147">Nach der Installation des Pakets auf einem regulären Computer müssen Sie die folgenden Dateien in die zuvor erstellte Dateifreigabe kopieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="a1dfc-148">Führen sie auf einem regulären Server (kein Nano-Server) mit IIS die folgenden Kopierbefehle aus:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="a1dfc-149">Ersetzen sie auf einem Windows 10-Computer `C:\windows\system32\inetsrv` durch `C:\Program Files\IIS Express`.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="a1dfc-150">Auf dem Nano-Server müssen Sie die folgenden Befehle aus der zuvor erstellten Dateifreigabe in die gültigen Speicherorte kopieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="a1dfc-151">Führen Sie also die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="a1dfc-152">Führen Sie in der Remotesitzung das folgende Skript aus:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-152">Run the following script in the remote session:</span></span>

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
> <span data-ttu-id="a1dfc-153">Löschen Sie nach diesem Schritt die Dateien *aspnetcore.dll* und *aspnetcore_schema.xml* aus der Freigabe.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="a1dfc-154">Installieren von .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="a1dfc-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="a1dfc-155">Wenn Ihre App als [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) veröffentlicht wird, muss .NET Core auf dem Server installiert sein.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-155">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="a1dfc-156">Verwenden Sie das [dotnet-install.ps1-PowerShell-Skript](https://dot.net/v1/dotnet-install.ps1) in einer Remotesitzung von PowerShell aus, um .NET Core auf Ihrem Nano Server zu installieren.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-156">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="a1dfc-157">Übergeben Sie die CLI-Version mit dem `-Version`-Switch:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-157">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="a1dfc-158">Veröffentlichen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="a1dfc-158">Publishing the application</span></span>

<span data-ttu-id="a1dfc-159">Kopieren Sie die veröffentlichte Ausgabe der vorhandenen Anwendung in das Stammverzeichnis der Dateifreigabe.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-159">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="a1dfc-160">Damit auf den Ort verwiesen wird, an dem Sie *dotnet.exe* extrahiert haben, müssen Sie möglicherweise Änderungen an *web.config* vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-160">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="a1dfc-161">Alternativ können Sie auch *dotnet.exe* zu Ihrem Pfad hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-161">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="a1dfc-162">Beispiel dafür, wie *web.config* aussehen kann, wenn *dotnet.exe* **nicht** zum Pfad hinzugefügt wird:</span><span class="sxs-lookup"><span data-stu-id="a1dfc-162">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

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

<span data-ttu-id="a1dfc-163">Führen Sie die folgenden Befehle in der Remotesitzung aus, um in IIS auf einem anderen Port als dem, der für die Standardwebsite vorgesehen ist, einen neuen Standort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-163">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="a1dfc-164">Außerdem müssen Sie diesen Port öffnen, um Zugriff auf das Internet zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-164">You also need to open that port to access the web.</span></span> <span data-ttu-id="a1dfc-165">Dieses Skript verwendet den `DefaultAppPool` aus Gründen der Einfachheit.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-165">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="a1dfc-166">Weitere Überlegungen zur Ausführung unter einem Anwendungspool finden Sie unter [Application Pools (Anwendungspools)](xref:publishing/iis#application-pools).</span><span class="sxs-lookup"><span data-stu-id="a1dfc-166">For more considerations on running under an application pool, see [Application Pools](xref:publishing/iis#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="a1dfc-167">Bekanntes Problem beim Ausführen der .NET Core CLI auf Nano Server und Umgehung des Problems</span><span class="sxs-lookup"><span data-stu-id="a1dfc-167">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="a1dfc-168">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="a1dfc-168">Running the application</span></span>

<span data-ttu-id="a1dfc-169">Auf die veröffentlichte Web-App können Sie in einem Browser über `http://192.168.1.10:8000` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-169">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="a1dfc-170">Wenn Sie die Protokollierung wie unter [Log creation and redirection (Erstellen und Umleiten von Protokollen)](xref:hosting/aspnet-core-module#log-creation-and-redirection) beschrieben eingerichtet haben, können Sie sich die Protokolle unter *C:\PublishedApps\AspNetCoreSampleForNano\logs* ansehen.</span><span class="sxs-lookup"><span data-stu-id="a1dfc-170">If you've set up logging as described in [Log creation and redirection](xref:hosting/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
