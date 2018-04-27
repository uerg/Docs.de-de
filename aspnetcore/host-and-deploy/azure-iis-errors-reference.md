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
ms.openlocfilehash: 1500f026c245f80de4120d6db4901cb117552966
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Allgemeine Referenz zu Fehlern für Azure App Service und IIS mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Die folgende Liste ist keine vollständige Fehlerliste. Wenn Sie hier nicht aufgeführten Fehler auftreten [öffnen Sie ein neues Problem](https://github.com/aspnet/Docs/issues/new) detaillierte Anweisungen zum Reproduzieren des Fehlers.

Sammeln Sie die folgende Informationen an:

* Browserverhalten
* Anwendungsereignis-Protokolleinträge
* ASP.NET Core-Modul "stdout" Protokolleinträge

Vergleichen Sie die Informationen zu den folgenden allgemeinen Fehlermeldungen an. Wenn eine Übereinstimmung gefunden wird, befolgen Sie die Hinweise zur Fehlerbehebung.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Installationsprogramm konnte VC++ Redistributable nicht abrufen

* **Ausnahme des Installationsprogramms:** 0x80072efd oder 0x80072f76: Unbekannter Fehler

* **Protokollausnahme des Installationsprogramms&#8224;:** Fehler 0x80072efd oder 0x80072f76: Fehler beim Ausführen des EXE-Pakets

  &#8224;Das Protokoll befindet sich unter „C:\Benutzer\\{BENUTZER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log“.

Problembehandlung:

* Wenn das System Zugriff auf das Internet nicht während der Installation von Paket hosten, diese Ausnahme tritt beim Abrufen der Installer verhindert wird. die *Microsoft Visual C++ 2015 Redistributable*. Abrufen ein Installationsprogramms aus der [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Wenn das Installationsprogramm schlägt fehl, kann der Server nicht die .NET Core-Laufzeit, die zum Hosten einer Bereitstellung Framework abhängiges (Diskettenlaufwerk) erforderlich empfängt. Ein Diskettenlaufwerk hosten zu können, stellen Sie sicher, dass die Common Language Runtime, in Programmen installiert ist &amp; Funktionen. Bei Bedarf erhalten Sie einen Runtime-Installer aus [.NET alle Downloads](https://www.microsoft.com/net/download/all). Starten Sie nach dem Installieren der Runtime das System neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Durch ein Upgrade des Betriebssystems wird das ASP.NET Core-Modul (32-Bit) entfernt

* **Anwendungsprotokoll:** Die Modul-DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** konnte nicht geladen werden. Die Daten sind der Fehler.

Problembehandlung:

* Nicht zum Betriebssystem gehörende Dateien im Verzeichnis **C:\Windows\SysWOW64\inetsrv** werden während eines Betriebssystemupgrades nicht beibehalten. Wenn ASP.NET Core-Modul installiert ist, vor einem Betriebssystemupgrade, und klicken Sie dann alle AppPool wird nach einem Betriebssystemupgrade in 32-Bit-Modus ausgeführt werden, dieser Fehler tritt. Reparieren Sie nach einem Betriebssystemupgrade das ASP.NET Core-Modul. Finden Sie unter [installieren Sie das Paket für das Hosten von .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Wählen Sie **Reparatur** Wenn das Installationsprogramm ausgeführt wird.

## <a name="platform-conflicts-with-rid"></a>Plattformkonflikte mit RID

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm "" c: "\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline" "" c: "\\{Pfad} {Assembly}. {} EXE-Datei | Dll} "", ErrorCode = "0 x 80004005: ff.

* **ASP.NET Core-Modul-Log:** Ausnahmefehler: System.BadImageFormatException: konnten nicht geladen werden Datei- oder Assemblyname "{}-Assembly-DLL". Es wurde versucht, ein Programm mit einem falschen Format zu laden.

Problembehandlung:

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Überprüfen Sie, ob die `<PlatformTarget>` in der *csproj* ist mit der RID-Speicherplatz vereinbar. Geben Sie z. B. eine `<PlatformTarget>` von `x86` und eine RID aus der Veröffentlichung `win10-x64`, entweder mithilfe *Dotnet - C Version der R - win10-X64 veröffentlichen* oder durch Festlegen der `<RuntimeIdentifiers>` in die *csproj*  auf `win10-x64`. Das Projekt wird ohne Warnung oder Fehler veröffentlicht, schlägt jedoch mit den oben genannten protokollierten Ausnahmen im System fehl.

* Wenn diese Ausnahme für ein Azure-App-Bereitstellung tritt auf, wenn Sie eine app aktualisieren und Bereitstellen von neueren Assemblys manuell löscht alle Dateien aus der vorherigen Bereitstellung. Veraltete inkompatible Assemblys können zu einer `System.BadImageFormatException`-Ausnahme führen, wenn Sie eine aktualisierte App bereitstellen.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI-Endpunkt falsch oder beendete Website

* **Browser:** ERR_CONNECTION_REFUSED

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Vergewissern Sie sich, dass die richtige URI-Endpunkt für die app verwendet wird. Überprüfen Sie die Bindungen an.

* Vergewissern Sie sich, dass die IIS-Website befindet sich nicht in der *beendet* Zustand.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine oder W3SVC-Serverfeatures deaktiviert

* **Ausnahme des Betriebssystems:** Die Features CoreWebEngine und W3SVC von IIS 7.0 müssen installiert sein, um das ASP.NET Core-Modul zu verwenden.

Problembehandlung:

* Vergewissern Sie sich, dass die richtige Rolle und die Funktionen aktiviert sind. Siehe [IIS-Konfiguration](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Falsche Website physischen Pfad oder die app fehlt

* **Browser:** 403 Verboten: Zugriff wird verweigert **– oder –** 403.14 Verboten: Der Webserver ist so konfiguriert, dass der Inhalt dieses Verzeichnisses nicht aufgelistet wird.

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Überprüfen Sie die IIS-Website **Grundeinstellungen** und physischen Anwendungsordners. Vergewissern Sie sich, dass die app in den Ordner auf der IIS-Website ist **physischen Pfad**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Falsche Rolle, Modul nicht installiert oder falsche Berechtigungen

* **Browser:** 500.19 Interner Serverfehler: Auf die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Vergewissern Sie sich, dass die richtige Rolle aktiviert ist. Siehe [IIS-Konfiguration](xref:host-and-deploy/iis/index#iis-configuration).

* Überprüfen Sie unter **Programme und Features**, ob das **Microsoft ASP.NET Core-Modul** installiert wurde. Wenn das **Microsoft ASP.NET Core-Modul** in der Liste der installierten Programme nicht vorhanden ist, installieren Sie das Modul. Finden Sie unter [das Kernfeature .NET Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Stellen Sie sicher, dass die **Anwendungspool** > **Prozessmodell** > **Identität** festgelegt ist, um **ApplicationPoolIdentity** oder die benutzerdefinierte Identität verfügt über die erforderlichen Berechtigungen zum Bereitstellungsordner für die app zugreifen.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Enthaltenem "falsch", fehlende Pfadvariable, Hosting-Paket nicht installiert, System/IIS nicht neu gestartet, VC++-Redistributable nicht installiert oder zugriffsverletzung dotnet.exe

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline "".\{ Assembly} .exe"', Fehlercode =" 0 x 80070002: 0.

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer

Problembehandlung:

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Überprüfen Sie die *ProcessPath* -Attribut auf die `<aspNetCore>` Element in *"Web.config"* zu bestätigen, dass es *Dotnet* für eine abhängige Framework-Bereitstellung (Diskettenlaufwerk) oder *. \{Assembly} .exe* für eine eigenständige Bereitstellung (SCD).

* Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise über die PATH-Einstellungen nicht verfügbar. Überprüfen Sie, ob *C:\Programme\dotnet\* in den PATH-Einstellungen des Systems vorhanden ist.

* Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise für die Benutzeridentität des Anwendungspools nicht verfügbar. Vergewissern Sie sich, dass die AppPool-Benutzeridentität Zugriff auf das Verzeichnis *C:\Programme\dotnet* hat. Vergewissern Sie sich, dass es keine Deny-Regeln für die Identität des AppPool-Benutzers auf konfiguriert sind die *c:\Programme\Microsoft Files\dotnet* und app-Verzeichnisse.

* Ein Diskettenlaufwerk wurden bereitgestellt und .NET Core ohne Neustarten von IIS installiert. Starten Sie den Server neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

* Ein Diskettenlaufwerk ist möglicherweise bereitgestellt wurde ohne die .NET Core-Laufzeit installieren, auf dem Hostsystem. Wenn die .NET Core-Laufzeit wurde nicht installiert wurde, führen Sie die **.NET Core-Hosting-Paket-Installer** auf dem System. Finden Sie unter [das Kernfeature .NET Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Wenn es sich bei dem Versuch, die .NET Core-Laufzeit auf einem System ohne Internetverbindung installieren, erhalten Sie die Laufzeit von [.NET alle Downloads](https://www.microsoft.com/net/download/all) und führen Sie den Hosting-Paket-Installer zum Installieren des ASP.NET Core-Moduls. Schließen Sie die Installation ab, indem Sie das System oder IIS neu starten. Führen Sie dazu **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus.

* Ein Diskettenlaufwerk wurden bereitgestellt und die *Microsoft Visual C++ 2015 Redistributable (x64)* ist nicht auf dem System installiert. Abrufen ein Installationsprogramms aus der [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Falsche Argumente des Elements \<aspNetCore\>

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline ""Dotnet".\{ DLL-Assembly}', Fehlercode = "0 x 80004005: 80008081.

* **ASP.NET Core-Modul-Protokoll:** führen Sie die Anwendung ist nicht vorhanden: "Pfad\{Assembly} .dll"

Problembehandlung:

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Überprüfen der *Argumente* -Attribut auf die `<aspNetCore>` Element im *"Web.config"* zu bestätigen, dass es sich handelt: (a) *.\{ DLL-Assembly}* für eine Bereitstellung Framework abhängiges (Diskettenlaufwerk); oder (b) nicht vorhanden, wird eine leere Zeichenfolge (*Argumente = ""*), oder eine Liste von Argumenten für die app (*Argumente = "arg1, arg2,..."*) für eine eigenständige Bereitstellung (SCD).

## <a name="missing-net-framework-version"></a>Fehlende .NET Framework-Version

* **Browser:** 502.3 Ungültiges Gateway: Bei der Weiterleitung der Anforderung ist ein Verbindungsfehler aufgetreten.

* **Anwendungsprotokoll:** ErrorCode Anwendung = "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Fehler beim Starten des Prozesses mit Commandline ""Dotnet".\{ DLL-Assembly}', Fehlercode = "0 x 80004005: 80008081.

* **Protokoll des ASP.NET Core-Moduls:** Ausnahme wegen fehlender Methode, Datei oder Assembly. Die in der Ausnahme angegebene Methode, Datei oder Assembly ist eine .NET Framework-Methode, -Datei oder -Assembly.

Problembehandlung:

* Installieren Sie die .NET Framework-Version, die im System fehlt.

* Für eine Bereitstellung Framework abhängiges (Diskettenlaufwerk) Vergewissern Sie sich, dass die richtige Common Language Runtime auf dem System installiert. Wenn das Projekt ein von 1.1, 2.0 Upgrade ist, auf dem Hostsystem bereitgestellt und diese Ausnahme führt, stellen Sie sicher, dass das Framework 2.0 auf dem Hostsystem ist.

## <a name="stopped-application-pool"></a>Beendeter Anwendungspool

* **Browser:** 503 Dienst nicht verfügbar

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung

* Vergewissern Sie sich, dass der Anwendungspool befindet sich nicht in der *beendet* Zustand.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware für die Integration von IIS nicht implementiert

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Prozess mit Commandline erstellt "" "c:"\\{Pfad}\{Assembly}. {} EXE-Datei | Dll} "" jedoch entweder abgestürzt hat keine Antwort oder lauscht nicht auf den angegebenen Port {PORT}, ErrorCode = "0x800705b4"

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei wurde erstellt und gibt Normalbetrieb an.

Problembehandlung

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Bestätigen Sie, dass entweder ein:
  * Die Integration von IIS-Middleware ist Referencedby Aufrufen der `UseIISIntegration` Methode für der app `WebHostBuilder` (ASP.NET Core 1.x)
  * Die apps verwendet die `CreateDefaultBuilder` Methode (ASP.NET Core 2.x).
  
  Weitere Informationen finden Sie unter [Hosten in ASP.NET Core](xref:fundamentals/hosting).

## <a name="sub-application-includes-a-handlers-section"></a>Untergeordnete Anwendung enthält einen \<handlers\>-Abschnitt

* **Browser:** HTTP-Fehler 500.19: Interner Serverfehler

* **Anwendungsprotokoll:** Kein Eintrag

* **ASP.NET Core-Modul-Log:** Protokolldatei erstellt, und zeigt normale Vorgang für die Stamm-app. Die Protokolldatei für die Sub-app nicht erstellt.

Problembehandlung

* Überprüfen Sie, ob die Datei *web.config* der untergeordneten App einen `<handlers>`-Abschnitt enthält.

## <a name="stdout-log-path-incorrect"></a>"stdout" Protokollpfad ist falsch

* **Browser:** die app normal reagiert.

* **Anwendungsprotokoll:** Warnung: "stdoutlogfile" konnte nicht erstellt \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log ErrorCode = - 2147024893.

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung

* Die `stdoutLogFile` im angegebenen Pfad die `<aspNetCore>` Element des *"Web.config"* ist nicht vorhanden. Weitere Informationen finden Sie unter der [protokollieren erstellen und die Umleitung](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) Abschnitt des Referenzthemas zur Konfiguration ASP.NET Core-Modul.

## <a name="application-configuration-general-issue"></a>Allgemeines Problem mit der Anwendungskonfiguration

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung "MACHINE/WEBROOT/APPHOST / {-ASSEMBLY}" mit physischen Stamm ' "c:"\\{Pfad}\' Prozess mit Commandline erstellt "" "c:"\\{Pfad}\{Assembly}. {} EXE-Datei | Dll} "" jedoch entweder abgestürzt hat keine Antwort oder lauscht nicht auf den angegebenen Port {PORT}, ErrorCode = "0x800705b4"

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer

Problembehandlung

* Diese allgemeine Ausnahme gibt an, dass der Prozess konnte nicht gestartet, wahrscheinlich aufgrund eines Problems der app-Konfiguration. Verweisen auf [Verzeichnisstruktur](xref:host-and-deploy/directory-structure), vergewissern Sie sich, dass der app, Dateien bereitgestellt und Ordner geeignet sind und die app-Konfigurationsdateien vorhanden sind und die richtigen Einstellungen für die app und die Umgebung enthalten. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).
