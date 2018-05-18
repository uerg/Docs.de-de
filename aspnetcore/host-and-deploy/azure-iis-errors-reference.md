---
title: Allgemeine Referenz zu Fehlern für Azure App Service und IIS mit ASP.NET Core
author: guardrex
description: Unterscheiden Sie häufige Fehler, wenn ASP.NET Core-apps auf Azure-Apps-Dienst und IIS-hosting.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 995890a5e6b0cc1d9cebc21486917a7a39587076
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="f5ad9-103">Allgemeine Referenz zu Fehlern für Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f5ad9-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="f5ad9-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f5ad9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f5ad9-105">Die folgende Liste ist keine vollständige Fehlerliste.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="f5ad9-106">Wenn Sie hier nicht aufgeführten Fehler auftreten [öffnen Sie ein neues Problem](https://github.com/aspnet/Docs/issues/new) detaillierte Anweisungen zum Reproduzieren des Fehlers.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="f5ad9-107">Sammeln Sie die folgende Informationen an:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-107">Collect the following information:</span></span>

* <span data-ttu-id="f5ad9-108">Browserverhalten</span><span class="sxs-lookup"><span data-stu-id="f5ad9-108">Browser behavior</span></span>
* <span data-ttu-id="f5ad9-109">Anwendungsereignis-Protokolleinträge</span><span class="sxs-lookup"><span data-stu-id="f5ad9-109">Application Event Log entries</span></span>
* <span data-ttu-id="f5ad9-110">ASP.NET Core-Modul "stdout" Protokolleinträge</span><span class="sxs-lookup"><span data-stu-id="f5ad9-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="f5ad9-111">Vergleichen Sie die Informationen zu den folgenden allgemeinen Fehlermeldungen an.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="f5ad9-112">Wenn eine Übereinstimmung gefunden wird, befolgen Sie die Hinweise zur Fehlerbehebung.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="f5ad9-113">Installationsprogramm konnte VC++ Redistributable nicht abrufen</span><span class="sxs-lookup"><span data-stu-id="f5ad9-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="f5ad9-114">**Ausnahme des Installationsprogramms:** 0x80072efd oder 0x80072f76: Unbekannter Fehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="f5ad9-115">**Protokollausnahme des Installationsprogramms&#8224;:** Fehler 0x80072efd oder 0x80072f76: Fehler beim Ausführen des EXE-Pakets</span><span class="sxs-lookup"><span data-stu-id="f5ad9-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="f5ad9-116">&#8224;Das Protokoll befindet sich unter „C:\Benutzer\\{BENUTZER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log“.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="f5ad9-117">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-117">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-118">Wenn das System Zugriff auf das Internet nicht während der Installation von Paket hosten, diese Ausnahme tritt beim Abrufen der Installer verhindert wird. die *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-118">If the system doesn't have Internet access while installing the Hosting Bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="f5ad9-119">Abrufen ein Installationsprogramms aus der [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="f5ad9-120">Wenn das Installationsprogramm schlägt fehl, kann der Server nicht die .NET Core-Laufzeit, die zum Hosten einer Bereitstellung Framework abhängiges (Diskettenlaufwerk) erforderlich empfängt.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="f5ad9-121">Ein Diskettenlaufwerk hosten zu können, stellen Sie sicher, dass die Common Language Runtime, in Programmen installiert ist &amp; Funktionen.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="f5ad9-122">Bei Bedarf erhalten Sie einen Runtime-Installer aus [.NET alle Downloads](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="f5ad9-123">Starten Sie nach dem Installieren der Runtime das System neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="f5ad9-124">Durch ein Upgrade des Betriebssystems wird das ASP.NET Core-Modul (32-Bit) entfernt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="f5ad9-125">**Anwendungsprotokoll:** Die Modul-DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** konnte nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="f5ad9-126">Die Daten sind der Fehler.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-126">The data is the error.</span></span>

<span data-ttu-id="f5ad9-127">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-127">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-128">Nicht zum Betriebssystem gehörende Dateien im Verzeichnis **C:\Windows\SysWOW64\inetsrv** werden während eines Betriebssystemupgrades nicht beibehalten.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="f5ad9-129">Wenn ASP.NET Core-Modul installiert ist, vor einem Betriebssystemupgrade, und klicken Sie dann alle AppPool wird nach einem Betriebssystemupgrade in 32-Bit-Modus ausgeführt werden, dieser Fehler tritt.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="f5ad9-130">Reparieren Sie nach einem Betriebssystemupgrade das ASP.NET Core-Modul.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="f5ad9-131">Finden Sie unter [installieren Sie das Paket für das Hosten von .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="f5ad9-132">Wählen Sie **Reparatur** Wenn das Installationsprogramm ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="f5ad9-133">Plattformkonflikte mit RID</span><span class="sxs-lookup"><span data-stu-id="f5ad9-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="f5ad9-134">**Browser:** HTTP-Fehler 502.5: Prozessfehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f5ad9-135">**Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm "" c: "\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline" "" c: "\\{Pfad} {Assembly}. {} EXE-Datei | Dll} "", ErrorCode = "0 x 80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="f5ad9-136">**ASP.NET Core-Modul-Log:** Ausnahmefehler: System.BadImageFormatException: konnten nicht geladen werden Datei- oder Assemblyname "{}-Assembly-DLL".</span><span class="sxs-lookup"><span data-stu-id="f5ad9-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="f5ad9-137">Es wurde versucht, ein Programm mit einem falschen Format zu laden.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="f5ad9-138">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-138">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-139">Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f5ad9-140">Ein Prozessfehler könnte die Folge eines Problems in der App sein.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f5ad9-141">Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="f5ad9-142">Überprüfen Sie, ob die `<PlatformTarget>` in der *csproj* ist mit der RID-Speicherplatz vereinbar.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="f5ad9-143">Geben Sie z. B. eine `<PlatformTarget>` von `x86` und eine RID aus der Veröffentlichung `win10-x64`, entweder mithilfe *Dotnet - C Version der R - win10-X64 veröffentlichen* oder durch Festlegen der `<RuntimeIdentifiers>` in die *csproj*  auf `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="f5ad9-144">Das Projekt wird ohne Warnung oder Fehler veröffentlicht, schlägt jedoch mit den oben genannten protokollierten Ausnahmen im System fehl.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="f5ad9-145">Wenn diese Ausnahme für ein Azure-App-Bereitstellung tritt auf, wenn Sie eine app aktualisieren und Bereitstellen von neueren Assemblys manuell löscht alle Dateien aus der vorherigen Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="f5ad9-146">Veraltete inkompatible Assemblys können zu einer `System.BadImageFormatException`-Ausnahme führen, wenn Sie eine aktualisierte App bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="f5ad9-147">URI-Endpunkt falsch oder beendete Website</span><span class="sxs-lookup"><span data-stu-id="f5ad9-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="f5ad9-148">**Browser:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="f5ad9-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="f5ad9-149">**Anwendungsprotokoll:** Kein Eintrag</span><span class="sxs-lookup"><span data-stu-id="f5ad9-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="f5ad9-150">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="f5ad9-151">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-151">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-152">Vergewissern Sie sich, dass die richtige URI-Endpunkt für die app verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="f5ad9-153">Überprüfen Sie die Bindungen an.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-153">Check the bindings.</span></span>

* <span data-ttu-id="f5ad9-154">Vergewissern Sie sich, dass die IIS-Website befindet sich nicht in der *beendet* Zustand.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="f5ad9-155">CoreWebEngine oder W3SVC-Serverfeatures deaktiviert</span><span class="sxs-lookup"><span data-stu-id="f5ad9-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="f5ad9-156">**Ausnahme des Betriebssystems:** Die Features CoreWebEngine und W3SVC von IIS 7.0 müssen installiert sein, um das ASP.NET Core-Modul zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="f5ad9-157">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-157">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-158">Vergewissern Sie sich, dass die richtige Rolle und die Funktionen aktiviert sind.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="f5ad9-159">Siehe [IIS-Konfiguration](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="f5ad9-160">Falsche Website physischen Pfad oder die app fehlt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="f5ad9-161">**Browser:** 403 Verboten: Zugriff wird verweigert **– oder –** 403.14 Verboten: Der Webserver ist so konfiguriert, dass der Inhalt dieses Verzeichnisses nicht aufgelistet wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="f5ad9-162">**Anwendungsprotokoll:** Kein Eintrag</span><span class="sxs-lookup"><span data-stu-id="f5ad9-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="f5ad9-163">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="f5ad9-164">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-164">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-165">Überprüfen Sie die IIS-Website **Grundeinstellungen** und physischen Anwendungsordners.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="f5ad9-166">Vergewissern Sie sich, dass die app in den Ordner auf der IIS-Website ist **physischen Pfad**.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="f5ad9-167">Falsche Rolle, Modul nicht installiert oder falsche Berechtigungen</span><span class="sxs-lookup"><span data-stu-id="f5ad9-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="f5ad9-168">**Browser:** 500.19 Interner Serverfehler: Auf die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="f5ad9-169">**Anwendungsprotokoll:** Kein Eintrag</span><span class="sxs-lookup"><span data-stu-id="f5ad9-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="f5ad9-170">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="f5ad9-171">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-171">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-172">Vergewissern Sie sich, dass die richtige Rolle aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="f5ad9-173">Siehe [IIS-Konfiguration](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="f5ad9-174">Überprüfen Sie unter **Programme und Features**, ob das **Microsoft ASP.NET Core-Modul** installiert wurde.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="f5ad9-175">Wenn das **Microsoft ASP.NET Core-Modul** in der Liste der installierten Programme nicht vorhanden ist, installieren Sie das Modul.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="f5ad9-176">Finden Sie unter [das Kernfeature .NET Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-176">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="f5ad9-177">Stellen Sie sicher, dass die **Anwendungspool** > **Prozessmodell** > **Identität** festgelegt ist, um **ApplicationPoolIdentity** oder die benutzerdefinierte Identität verfügt über die erforderlichen Berechtigungen zum Bereitstellungsordner für die app zugreifen.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="f5ad9-178">Enthaltenem "falsch", fehlende Pfadvariable, Hosting-Paket nicht installiert, System/IIS nicht neu gestartet, VC++-Redistributable nicht installiert oder zugriffsverletzung dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="f5ad9-178">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="f5ad9-179">**Browser:** HTTP-Fehler 502.5: Prozessfehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f5ad9-180">**Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline "".\{ Assembly} .exe"', Fehlercode =" 0 x 80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="f5ad9-181">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer</span><span class="sxs-lookup"><span data-stu-id="f5ad9-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="f5ad9-182">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-182">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-183">Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f5ad9-184">Ein Prozessfehler könnte die Folge eines Problems in der App sein.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f5ad9-185">Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="f5ad9-186">Überprüfen Sie die *ProcessPath* -Attribut auf die `<aspNetCore>` Element in *"Web.config"* zu bestätigen, dass es *Dotnet* für eine abhängige Framework-Bereitstellung (Diskettenlaufwerk) oder *. \{Assembly} .exe* für eine eigenständige Bereitstellung (SCD).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="f5ad9-187">Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise über die PATH-Einstellungen nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="f5ad9-188">Überprüfen Sie, ob *C:\Programme\dotnet\* in den PATH-Einstellungen des Systems vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="f5ad9-189">Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise für die Benutzeridentität des Anwendungspools nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="f5ad9-190">Vergewissern Sie sich, dass die AppPool-Benutzeridentität Zugriff auf das Verzeichnis *C:\Programme\dotnet* hat.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="f5ad9-191">Vergewissern Sie sich, dass es keine Deny-Regeln für die Identität des AppPool-Benutzers auf konfiguriert sind die *c:\Programme\Microsoft Files\dotnet* und app-Verzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="f5ad9-192">Ein Diskettenlaufwerk wurden bereitgestellt und .NET Core ohne Neustarten von IIS installiert.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="f5ad9-193">Starten Sie den Server neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="f5ad9-194">Ein Diskettenlaufwerk ist möglicherweise bereitgestellt wurde ohne die .NET Core-Laufzeit installieren, auf dem Hostsystem.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="f5ad9-195">Wenn die .NET Core-Laufzeit wurde nicht installiert wurde, führen Sie die **.NET Core-Hosting-Paket-Installer** auf dem System.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span> <span data-ttu-id="f5ad9-196">Finden Sie unter [das Kernfeature .NET Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-196">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="f5ad9-197">Wenn es sich bei dem Versuch, die .NET Core-Laufzeit auf einem System ohne Internetverbindung installieren, erhalten Sie die Laufzeit von [.NET alle Downloads](https://www.microsoft.com/net/download/all) und führen Sie den Hosting-Paket-Installer zum Installieren des ASP.NET Core-Moduls.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the Hosting Bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="f5ad9-198">Schließen Sie die Installation ab, indem Sie das System oder IIS neu starten. Führen Sie dazu **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="f5ad9-199">Ein Diskettenlaufwerk wurden bereitgestellt und die *Microsoft Visual C++ 2015 Redistributable (x64)* ist nicht auf dem System installiert.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="f5ad9-200">Abrufen ein Installationsprogramms aus der [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="f5ad9-201">Falsche Argumente des Elements \<aspNetCore\></span><span class="sxs-lookup"><span data-stu-id="f5ad9-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="f5ad9-202">**Browser:** HTTP-Fehler 502.5: Prozessfehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f5ad9-203">**Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline ""Dotnet".\{ DLL-Assembly}', Fehlercode = "0 x 80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="f5ad9-204">**ASP.NET Core-Modul-Protokoll:** führen Sie die Anwendung ist nicht vorhanden: "Pfad\{Assembly} .dll"</span><span class="sxs-lookup"><span data-stu-id="f5ad9-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="f5ad9-205">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-205">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-206">Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f5ad9-207">Ein Prozessfehler könnte die Folge eines Problems in der App sein.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f5ad9-208">Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="f5ad9-209">Überprüfen der *Argumente* -Attribut auf die `<aspNetCore>` Element im *"Web.config"* zu bestätigen, dass es sich handelt: (a) *.\{ DLL-Assembly}* für eine Bereitstellung Framework abhängiges (Diskettenlaufwerk); oder (b) nicht vorhanden, wird eine leere Zeichenfolge (*Argumente = ""*), oder eine Liste von Argumenten für die app (*Argumente = "arg1, arg2,..."*) für eine eigenständige Bereitstellung (SCD).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="f5ad9-210">Fehlende .NET Framework-Version</span><span class="sxs-lookup"><span data-stu-id="f5ad9-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="f5ad9-211">**Browser:** 502.3 Ungültiges Gateway: Bei der Weiterleitung der Anforderung ist ein Verbindungsfehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="f5ad9-212">**Anwendungsprotokoll:** ErrorCode Anwendung = "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline ""Dotnet".\{ DLL-Assembly}', Fehlercode = "0 x 80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="f5ad9-213">**Protokoll des ASP.NET Core-Moduls:** Ausnahme wegen fehlender Methode, Datei oder Assembly.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="f5ad9-214">Die in der Ausnahme angegebene Methode, Datei oder Assembly ist eine .NET Framework-Methode, -Datei oder -Assembly.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="f5ad9-215">Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-215">Troubleshooting:</span></span>

* <span data-ttu-id="f5ad9-216">Installieren Sie die .NET Framework-Version, die im System fehlt.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="f5ad9-217">Für eine Bereitstellung Framework abhängiges (Diskettenlaufwerk) Vergewissern Sie sich, dass die richtige Common Language Runtime auf dem System installiert.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="f5ad9-218">Wenn das Projekt ein von 1.1, 2.0 Upgrade ist, auf dem Hostsystem bereitgestellt und diese Ausnahme führt, stellen Sie sicher, dass das Framework 2.0 auf dem Hostsystem ist.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="f5ad9-219">Beendeter Anwendungspool</span><span class="sxs-lookup"><span data-stu-id="f5ad9-219">Stopped Application Pool</span></span>

* <span data-ttu-id="f5ad9-220">**Browser:** 503 Dienst nicht verfügbar</span><span class="sxs-lookup"><span data-stu-id="f5ad9-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="f5ad9-221">**Anwendungsprotokoll:** Kein Eintrag</span><span class="sxs-lookup"><span data-stu-id="f5ad9-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="f5ad9-222">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="f5ad9-223">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f5ad9-223">Troubleshooting</span></span>

* <span data-ttu-id="f5ad9-224">Vergewissern Sie sich, dass der Anwendungspool befindet sich nicht in der *beendet* Zustand.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="f5ad9-225">Middleware für die Integration von IIS nicht implementiert</span><span class="sxs-lookup"><span data-stu-id="f5ad9-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="f5ad9-226">**Browser:** HTTP-Fehler 502.5: Prozessfehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f5ad9-227">**Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Prozess mit Commandline erstellt "" "c:"\\{Pfad}\{Assembly}. {} EXE-Datei | Dll} "" jedoch entweder abgestürzt hat keine Antwort oder lauscht nicht auf den angegebenen Port {PORT}, ErrorCode = "0x800705b4"</span><span class="sxs-lookup"><span data-stu-id="f5ad9-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="f5ad9-228">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei wurde erstellt und gibt Normalbetrieb an.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="f5ad9-229">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f5ad9-229">Troubleshooting</span></span>

* <span data-ttu-id="f5ad9-230">Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f5ad9-231">Ein Prozessfehler könnte die Folge eines Problems in der App sein.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f5ad9-232">Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="f5ad9-233">Bestätigen Sie, dass entweder ein:</span><span class="sxs-lookup"><span data-stu-id="f5ad9-233">Confirm that either:</span></span>
  * <span data-ttu-id="f5ad9-234">Die Integration von IIS-Middleware ist Referencedby Aufrufen der `UseIISIntegration` Methode für der app `WebHostBuilder` (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="f5ad9-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="f5ad9-235">Die apps verwendet die `CreateDefaultBuilder` Methode (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="f5ad9-236">Finden Sie unter [Host in ASP.NET Core](xref:fundamentals/host/index) Details.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-236">See [Host in ASP.NET Core](xref:fundamentals/host/index) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="f5ad9-237">Untergeordnete Anwendung enthält einen \<handlers\>-Abschnitt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="f5ad9-238">**Browser:** HTTP-Fehler 500.19: Interner Serverfehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="f5ad9-239">**Anwendungsprotokoll:** Kein Eintrag</span><span class="sxs-lookup"><span data-stu-id="f5ad9-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="f5ad9-240">**ASP.NET Core-Modul-Log:** Protokolldatei erstellt, und zeigt normale Vorgang für die Stamm-app.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="f5ad9-241">Die Protokolldatei für die Sub-app nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="f5ad9-242">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f5ad9-242">Troubleshooting</span></span>

* <span data-ttu-id="f5ad9-243">Überprüfen Sie, ob die Datei *web.config* der untergeordneten App einen `<handlers>`-Abschnitt enthält.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="f5ad9-244">"stdout" Protokollpfad ist falsch</span><span class="sxs-lookup"><span data-stu-id="f5ad9-244">stdout log path incorrect</span></span>

* <span data-ttu-id="f5ad9-245">**Browser:** die app normal reagiert.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="f5ad9-246">**Anwendungsprotokoll:** Warnung: "stdoutlogfile" konnte nicht erstellt \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log ErrorCode = - 2147024893.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="f5ad9-247">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt</span><span class="sxs-lookup"><span data-stu-id="f5ad9-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="f5ad9-248">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f5ad9-248">Troubleshooting</span></span>

* <span data-ttu-id="f5ad9-249">Die `stdoutLogFile` im angegebenen Pfad die `<aspNetCore>` Element des *"Web.config"* ist nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="f5ad9-250">Weitere Informationen finden Sie unter der [protokollieren erstellen und die Umleitung](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) Abschnitt des Referenzthemas zur Konfiguration ASP.NET Core-Modul.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="f5ad9-251">Allgemeines Problem mit der Anwendungskonfiguration</span><span class="sxs-lookup"><span data-stu-id="f5ad9-251">Application configuration general issue</span></span>

* <span data-ttu-id="f5ad9-252">**Browser:** HTTP-Fehler 502.5: Prozessfehler</span><span class="sxs-lookup"><span data-stu-id="f5ad9-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f5ad9-253">**Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Prozess mit Commandline erstellt "" "c:"\\{Pfad}\{Assembly}. {} EXE-Datei | Dll} "" jedoch entweder abgestürzt hat keine Antwort oder lauscht nicht auf den angegebenen Port {PORT}, ErrorCode = "0x800705b4"</span><span class="sxs-lookup"><span data-stu-id="f5ad9-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="f5ad9-254">**Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer</span><span class="sxs-lookup"><span data-stu-id="f5ad9-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="f5ad9-255">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f5ad9-255">Troubleshooting</span></span>

* <span data-ttu-id="f5ad9-256">Diese allgemeine Ausnahme gibt an, dass der Prozess konnte nicht gestartet, wahrscheinlich aufgrund eines Problems der app-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="f5ad9-257">Verweisen auf [Verzeichnisstruktur](xref:host-and-deploy/directory-structure), vergewissern Sie sich, dass der app, Dateien bereitgestellt und Ordner geeignet sind und die app-Konfigurationsdateien vorhanden sind und die richtigen Einstellungen für die app und die Umgebung enthalten.</span><span class="sxs-lookup"><span data-stu-id="f5ad9-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="f5ad9-258">Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="f5ad9-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
