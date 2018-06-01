---
title: Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie häufige Fehler beim Hosten von ASP.NET Core-Apps in Azure Apps Service und IIS erkennen.
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
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233338"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Die folgende Liste ist keine vollständige Fehlerliste. Wenn Sie einen hier nicht aufgeführten Fehler finden, [öffnen Sie ein neues Problem](https://github.com/aspnet/Docs/issues/new) mit detaillierten Anweisungen zum Reproduzieren des Fehlers.

Sammeln Sie folgende Informationen:

* Browserverhalten
* Einträge im Anwendungsereignisprotokoll
* stdout-Protokolleinträge im ASP.NET Core-Modul

Vergleichen Sie die Informationen mit folgenden häufigen Fehlern. Befolgen Sie die Hinweise zur Fehlerbehebung, wenn eine Übereinstimmung gefunden wird.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Installationsprogramm konnte VC++ Redistributable nicht abrufen

* **Ausnahme des Installationsprogramms:** 0x80072efd oder 0x80072f76: Unbekannter Fehler

* **Protokollausnahme des Installationsprogramms&#8224;:** Fehler 0x80072efd oder 0x80072f76: Fehler beim Ausführen des EXE-Pakets

  &#8224;Das Protokoll befindet sich unter „C:\Benutzer\\{BENUTZER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log“.

Problembehandlung:

* Hat das System während der Installation des Hosting-Pakets keinen Zugriff auf das Internet, tritt diese Ausnahme auf, wenn der Installer *Microsoft Visual C++ 2015 Redistributable* nicht abrufen kann. Rufen Sie einen Installer aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840) ab. Wenn der Installer fehlschlägt, erhält der Server möglicherweise nicht die .NET Core-Runtime, die zum Hosten einer Framework-abhängigen Bereitstellung (Framework-Dependent Deployment, FDD) erforderlich ist. Wenn Sie eine FDD hosten, vergewissern Sie sich, dass die Runtime unter „Programme &amp; Features“ installiert ist. Rufen Sie ggf. über [Alle .NET-Downloads](https://www.microsoft.com/net/download/all) einen Runtime-Installer ab. Starten Sie nach dem Installieren der Runtime das System neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Durch ein Upgrade des Betriebssystems wird das ASP.NET Core-Modul (32-Bit) entfernt

* **Anwendungsprotokoll:** Die Modul-DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** konnte nicht geladen werden. Die Daten sind der Fehler.

Problembehandlung:

* Nicht zum Betriebssystem gehörende Dateien im Verzeichnis **C:\Windows\SysWOW64\inetsrv** werden während eines Betriebssystemupgrades nicht beibehalten. Wenn vor der Durchführung eines Betriebssystemupgrades das ASP.NET Core-Modul installiert wurde, und anschließend nach einem Betriebssystemupgrade ein App-Pool im 32-Bit-Modus ausgeführt wird, tritt dieses Problem auf. Reparieren Sie nach einem Betriebssystemupgrade das ASP.NET Core-Modul. Siehe [Installieren des .NET Core-Hostingpakets](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Wählen Sie **Reparatur** aus, wenn der Installer ausgeführt wird.

## <a name="platform-conflicts-with-rid"></a>Plattformkonflikte mit RID

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung „MACHINE/WEBROOT/APPHOST/{ASSEMBLY}“ mit dem physischen Stamm „C:\{PATH}\'“ konnte den Prozess mit der Befehlszeile „C:\\{PATH}{assembly}.{exe|dll}“ nicht starten. Fehlercode = 0x80004005 : ff.

* **Protokoll des ASP.NET Core-Moduls:** Nicht behandelte Ausnahme: System.BadImageFormatException: Datei oder Assembly „my_application.dll“ konnte nicht geladen werden. Es wurde versucht, ein Programm mit einem falschen Format zu laden.

Problembehandlung:

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Vergewissern Sie sich, dass `<PlatformTarget>` in der *CSPROJ*-Datei nicht mit der RID in Konflikt steht. Geben Sie z.B. als `<PlatformTarget>` nicht `x86` an, und führen Sie die Veröffentlichung nicht mit der RID `win10-x64` durch, entweder mit *dotnet publish -c Release -r win10-x64* oder durch Festlegen von `<RuntimeIdentifiers>` in *CSPROJ* auf `win10-x64`. Das Projekt wird ohne Warnung oder Fehler veröffentlicht, schlägt jedoch mit den oben genannten protokollierten Ausnahmen im System fehl.

* Wenn diese Ausnahme für eine Azure-App-Bereitstellung beim Aktualisieren einer App und beim Bereitstellen neuerer Assemblys auftritt, löschen Sie alle Dateien aus der vorherigen Bereitstellung manuell. Veraltete inkompatible Assemblys können zu einer `System.BadImageFormatException`-Ausnahme führen, wenn Sie eine aktualisierte App bereitstellen.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI-Endpunkt falsch oder beendete Website

* **Browser:** ERR_CONNECTION_REFUSED

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Vergewissern Sie sich, dass der richtige URI-Endpunkt für die App verwendet wird. Überprüfen Sie die Bindungen.

* Vergewissern Sie sich, dass die IIS-Website nicht den Status *Beendet* aufweist.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine oder W3SVC-Serverfeatures deaktiviert

* **Ausnahme des Betriebssystems:** Die Features CoreWebEngine und W3SVC von IIS 7.0 müssen installiert sein, um das ASP.NET Core-Modul zu verwenden.

Problembehandlung:

* Vergewissern Sie sich, dass die richtigen Rollen und Features aktiviert wurden. Siehe [IIS-Konfiguration](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Falscher physischer Pfad der Website oder App fehlt

* **Browser:** 403 Verboten: Zugriff wird verweigert **– oder –** 403.14 Verboten: Der Webserver ist so konfiguriert, dass der Inhalt dieses Verzeichnisses nicht aufgelistet wird.

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Überprüfen Sie die **Grundeinstellungen** der IIS-Website und den physischen App-Ordner. Vergewissern Sie sich, dass sich die App im Ordner im **physischen Pfad** der IIS-Website befindet.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Falsche Rolle, Modul nicht installiert oder falsche Berechtigungen

* **Browser:** 500.19 Interner Serverfehler: Auf die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung:

* Vergewissern Sie sich, dass die richtige Rolle aktiviert wurde. Siehe [IIS-Konfiguration](xref:host-and-deploy/iis/index#iis-configuration).

* Überprüfen Sie unter **Programme und Features**, ob das **Microsoft ASP.NET Core-Modul** installiert wurde. Wenn das **Microsoft ASP.NET Core-Modul** in der Liste der installierten Programme nicht vorhanden ist, installieren Sie das Modul. Weitere Informationen finden Sie unter [Installieren des .NET Core-Hostingpakets](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Stellen Sie sicher, dass **Anwendungspool** > **Prozessmodell** > **Identität** auf **ApplicationPoolIdentity** festgelegt ist, oder dass die benutzerdefinierte Identität über die erforderlichen Berechtigungen für den Zugriff auf den Bereitstellungsordner der App verfügt.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Falscher processPath-Wert, fehlende PATH-Variable, Hostingpaket nicht installiert, System/IIS wird nicht neu gestartet, VC++ Redistributable nicht installiert oder dotnet.exe-Zugriffsverletzung

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung „MACHINE/WEBROOT/APPHOST/{ASSEMBLY}“ mit dem physischen Stamm „C:\\{PATH}\'“ konnte den Prozess mit der Befehlszeile „\{assembly}.exe“ nicht starten. Fehlercode = 0x80070002 : 0.

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer

Problembehandlung:

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Überprüfen Sie das Attribut *processPath* im Element `<aspNetCore>` in der Datei *web.config*, um sicherzustellen, dass es *dotnet* für eine Framework-abhängige Bereitstellung (Framework-Dependent Deployment, FDD) oder *.\{assembly}.exe* für eine eigenständige Bereitstellung (Self-Contained Deployment, SCD) ist.

* Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise über die PATH-Einstellungen nicht verfügbar. Überprüfen Sie, ob *C:\Programme\dotnet\* in den PATH-Einstellungen des Systems vorhanden ist.

* Für eine Framework-abhängige Bereitstellung ist *dotnet.exe* möglicherweise für die Benutzeridentität des Anwendungspools nicht verfügbar. Vergewissern Sie sich, dass die AppPool-Benutzeridentität Zugriff auf das Verzeichnis *C:\Programme\dotnet* hat. Vergewissern Sie sich, dass keine Ablehnungsregeln für die AppPool-Benutzeridentität im Verzeichnis *C:\Programme\dotnet* und in App-Verzeichnissen konfiguriert sind.

* Möglicherweise wurde eine FDD bereitgestellt und .NET Core installiert, ohne IIS neu zu starten. Starten Sie den Server neu, oder starten Sie IIS neu, indem Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung ausführen.

* Möglicherweise wurde eine FDD bereitgestellt, ohne die .NET Core-Runtime auf dem Hostsystem zu installieren. Führen Sie den **Installer für das .NET Core-Hostingpaket** auf dem System aus, wenn die .NET Core-Runtime nicht installiert wurde. Weitere Informationen finden Sie unter [Installieren des .NET Core-Hostingpakets](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Wenn Sie versuchen, die .NET Core-Runtime auf einem System ohne Internetverbindung zu installieren, rufen Sie die Runtime über [Alle .NET-Downloads](https://www.microsoft.com/net/download/all) ab, und führen Sie den Installer für das Hostingpaket aus, um das ASP.NET Core-Modul zu installieren. Schließen Sie die Installation ab, indem Sie das System oder IIS neu starten. Führen Sie dazu **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus.

* Möglicherweise wurde eine FDD bereitgestellt, und *Microsoft Visual C++ 2015 Redistributable (x64)* ist auf dem System nicht installiert. Rufen Sie einen Installer aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840) ab.

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Falsche Argumente des Elements \<aspNetCore\>

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung „MACHINE/WEBROOT/APPHOST/{ASSEMBLY}“ mit dem physischen Stamm „C:\\{PATH}\'“ konnte den Prozess mit der Befehlszeile „dotnet .\{assembly}.dll“ nicht starten. Fehlercode = 0x80004005 : 80008081.

* **Protokoll des ASP.NET Core-Moduls:** Die auszuführende Anwendung ist nicht vorhanden: „PATH\{assembly}.dll“

Problembehandlung:

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Überprüfen Sie das Attribut *arguments* im Element `<aspNetCore>` in der Datei *web.config*, um sicherzustellen, dass es (a) *.\{assembly}.dll* für eine Framework-abhängige Bereitstellung (Framework-Dependent Deployment, FDD) oder (b) nicht vorhanden, eine leere Zeichenfolge (*arguments=""*) oder eine Liste von Argumenten der App (*arguments="arg1, arg2, ..."*) für eine eigenständige Bereitstellung (Self-Contained Deployment, SCD) ist.

## <a name="missing-net-framework-version"></a>Fehlende .NET Framework-Version

* **Browser:** 502.3 Ungültiges Gateway: Bei der Weiterleitung der Anforderung ist ein Verbindungsfehler aufgetreten.

* **Anwendungsprotokoll:** Fehlercode = Anwendung „MACHINE/WEBROOT/APPHOST/{ASSEMBLY}“ mit dem physischen Stamm „C:\\{PATH}\'“ konnte den Prozess mit der Befehlszeile „dotnet .\{assembly}.dll“ nicht starten. Fehlercode = 0x80004005 : 80008081.

* **Protokoll des ASP.NET Core-Moduls:** Ausnahme wegen fehlender Methode, Datei oder Assembly. Die in der Ausnahme angegebene Methode, Datei oder Assembly ist eine .NET Framework-Methode, -Datei oder -Assembly.

Problembehandlung:

* Installieren Sie die .NET Framework-Version, die im System fehlt.

* Stellen Sie für eine Framework-abhängige Bereitstellung (Framework-Dependent Deployment, FDD) sicher, dass die richtige Runtime im System installiert ist. Wenn für das Projekt ein Upgrade von 1.1 auf 2.0 durchgeführt wird, das Projekt auf dem Hostsystem bereitgestellt wird und diese Ausnahme auftritt, sollten Sie sicherstellen, dass sich das 2.0-Framework auf dem Hostsystem befindet.

## <a name="stopped-application-pool"></a>Beendeter Anwendungspool

* **Browser:** 503 Dienst nicht verfügbar

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung

* Vergewissern Sie sich, dass der Anwendungspool nicht den Status *Beendet* aufweist.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware für die Integration von IIS nicht implementiert

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung „MACHINE/WEBROOT/APPHOST/{ASSEMBLY}“ mit dem physischen Stamm „C:\\{PATH}\'“ hat den Prozess mit der Befehlszeile „C:\\{PATH}\{assembly}.{exe|dll}“ erstellt, ist jedoch entweder abgestürzt, hat nicht reagiert oder ist am angegebenen Port „{PORT}“ nicht empfangsbereit gewesen. Fehlercode = 0x800705b4

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei wurde erstellt und gibt Normalbetrieb an.

Problembehandlung

* Vergewissern Sie sich, dass die App lokal auf Kestrel ausgeführt wird. Ein Prozessfehler könnte die Folge eines Problems in der App sein. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).

* Bestätigen Sie, dass entweder:
  * Durch Aufrufen der Methode `UseIISIntegration` im `WebHostBuilder` der App (ASP.NET Core 1.x) auf die IIS-Integrationsmiddleware verwiesen wird
  * Die Apps die Methode `CreateDefaultBuilder` (ASP.NET Core 2.x) verwenden.
  
  Weitere Einzelheiten finden Sie unter [Hosten in ASP.NET Core](xref:fundamentals/host/index).

## <a name="sub-application-includes-a-handlers-section"></a>Untergeordnete Anwendung enthält einen \<handlers\>-Abschnitt

* **Browser:** HTTP-Fehler 500.19: Interner Serverfehler

* **Anwendungsprotokoll:** Kein Eintrag

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei wurde erstellt und zeigt für die Stamm-App Normalbetrieb an. Die Protokolldatei wurde für die untergeordnete App nicht erstellt.

Problembehandlung

* Überprüfen Sie, ob die Datei *web.config* der untergeordneten App einen `<handlers>`-Abschnitt enthält.

## <a name="stdout-log-path-incorrect"></a>Der stdout-Protokollpfad ist falsch

* **Browser:** die App reagiert normal.

* **Anwendungsprotokoll:** Warnung: „stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log“ konnte nicht erstellt werden. Fehlercode = -2147024893.

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei nicht erstellt

Problembehandlung

* Der im Element `<aspNetCore>` angegebene Pfad `stdoutLogFile` in der Datei *web.config* ist nicht vorhanden. Weitere Informationen finden Sie im Thema [Protokollerstellung und -umleitung](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) des Referenzthemas zur Konfiguration des ASP.NET Core-Moduls.

## <a name="application-configuration-general-issue"></a>Allgemeines Problem mit der Anwendungskonfiguration

* **Browser:** HTTP-Fehler 502.5: Prozessfehler

* **Anwendungsprotokoll:** Anwendung „MACHINE/WEBROOT/APPHOST/{ASSEMBLY}“ mit dem physischen Stamm „C:\\{PATH}\'“ hat den Prozess mit der Befehlszeile „C:\\{PATH}\{assembly}.{exe|dll}“ erstellt, ist jedoch entweder abgestürzt, hat nicht reagiert oder ist am angegebenen Port „{PORT}“ nicht empfangsbereit gewesen. Fehlercode = 0x800705b4

* **Protokoll des ASP.NET Core-Moduls:** Protokolldatei erstellt, sie ist aber leer

Problembehandlung

* Diese allgemeine Ausnahme gibt an, dass ein Prozess nicht gestartet wurde, wahrscheinlich aufgrund eines Problems mit der App-Konfiguration. Überprüfen Sie in der [Verzeichnisstruktur](xref:host-and-deploy/directory-structure), ob die bereitgestellten Dateien und Ordner der App in Ordnung sind und ob die Konfigurationsdateien der App vorhanden sind und die richtigen Einstellungen für Ihre App und die Umgebung enthalten. Weitere Informationen finden Sie unter [Problembehandlung](xref:host-and-deploy/iis/troubleshoot).
