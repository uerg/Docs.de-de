---
title: Problembehandlung bei ASP.NET Core unter IIS
author: guardrex
description: Informationen Sie zur diagnose von Problemen mit Internet Information Services (IIS)-Bereitstellungen von ASP.NET Core-apps.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Problembehandlung bei ASP.NET Core unter IIS

Von [Luke Latham](https://github.com/guardrex)

Dieser Artikel enthält Anweisungen für eine ASP.NET Core Diagnostizieren von app-Start-Problem beim Hosten von mit [(Internet Information Services, IIS)](/iis). Die Informationen in diesem Artikel gelten für hosting in IIS unter Windows Server und Windows-Desktop.

In Visual Studio ein Projekt ASP.NET Core standardmäßig [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) während des Debuggens hosten. Ein *502.5 Prozessfehler* auftritt, wenn Debuggen lokal eine Empfehlung in diesem Thema verwenden werden kann.

Zusätzliche Themen zur Problembehandlung:

[Problembehandlung bei ASP.NET Core in Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Obwohl App Service verwendet die [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und IIS auf Host-apps finden Sie im dedizierte Thema Anweisungen für App-Dienst spezifisch sind.

[Fehlerbehandlung](xref:fundamentals/error-handling)  
Gewusst wie: Behandeln von Fehlern in ASP.NET Core apps während der Entwicklung auf einem lokalen System zu ermitteln.

[Lernen Sie das Debuggen mit Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
In diesem Thema werden die Funktionen von Visual Studio-Debugger vorgestellt.

## <a name="app-startup-errors"></a>App-Start-Fehler

**502.5 Prozess Fehler**  
Der Arbeitsprozess schlägt fehl. Die app starten nicht.

ASP.NET Core-Modul versucht der Arbeitsprozess gestartet, jedoch nicht gestartet. Ein Prozess starten nach die Fehlerursache kann in der Regel von Einträgen in bestimmt werden die [Anwendungsereignisprotokoll](#application-event-log) und [ASP.NET Core-Modul "stdout" Log](#aspnet-core-module-stdout-log).

Die *502.5 Prozessfehler* Fehlerseite wird zurückgegeben, wenn eine Hosting- oder app-Fehlkonfiguration bewirkt, der Arbeitsprozess dass fehlschlägt:

![Browserfenster, die mit der Seite "502.5 Prozessfehler"](troubleshoot/_static/process-failure-page.png)

**500 Interner Serverfehler**  
Die app gestartet wurde, aber ein Fehler wird verhindert, dass es sich bei den Server die Anforderung.

Dieser Fehler tritt auf, in der app-Code während des Starts oder beim Erstellen einer Antwort. Enthält die Antwort möglicherweise keinen Inhalt, oder die Antwort möglicherweise angezeigt, wie eine *500 Internal Server Error* im Browser. Ereignisprotokoll der Anwendung gibt normalerweise an, dass die Anwendung normal gestartet. Aus Sicht des Servers ist richtig. Der Anwendungsstart, aber es kann keine gültige Antwort generiert. [Führen Sie die app an einer Eingabeaufforderung](#run-the-app-at-a-command-prompt) auf dem Server oder [aktivieren Sie das ASP.NET Core-Modul "stdout" Protokoll](#aspnet-core-module-stdout-log) um das Problem zu beheben.

**Zurücksetzen der Verbindung**

Wenn ein Fehler auftritt, nachdem die Header gesendet werden, ist es zu spät für den Server zum Senden einer **500 Internal Server Error** Wenn ein Fehler auftritt. Dies erfolgt häufig auf, wenn ein Fehler, während der Serialisierung von komplexen Objekten auf eine Antwort auftritt. Diese Art von Fehler angezeigt wird, als ein *Zurücksetzen der Verbindung* Fehler auf dem Client. [Anwendungsprotokollierung](xref:fundamentals/logging/index) hilft diese Fehlertypen zu beheben.

## <a name="default-startup-limits"></a>Starten von standardeinschränkungen

ASP.NET Core-Modul konfiguriert ist, hat den Standardwert *StartupTimeLimit* von 120 Sekunden. Wenn auf den Standardwert belassen, kann eine app dauern, bis zu zwei Minuten starten, bevor das Modul einen Prozessfehler protokolliert. Informationen zum Konfigurieren des Moduls finden Sie unter [Attribute des-Elements AspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Fehlerbehebung bei der app-Start

### <a name="application-event-log"></a>Anwendungsereignisprotokoll

Zugriff auf das Anwendungsereignisprotokoll:

1. Öffnen Sie im Menü Start, suchen Sie nach **Ereignisanzeige**, und wählen Sie dann die **Ereignisanzeige** app.
1. In **Ereignisanzeige**öffnen die **Windows-Protokolle** Knoten.
1. Wählen Sie **Anwendung** Ereignisprotokoll der Anwendung zu öffnen.
1. Suchen Sie nach Fehler im Zusammenhang mit der app ein. Fehler haben einen Wert von *AspNetCore-Modul für IIS* oder *Modul für IIS Express AspNetCore* in der *Quelle* Spalte.

### <a name="run-the-app-at-a-command-prompt"></a>Führen Sie die app an einer Eingabeaufforderung

Viele Startfehlern erzeugen keine nützlichen Informationen in das Anwendungsereignisprotokoll. Sie können die Ursache von Fehlern suchen, durch Ausführen der app an einer Eingabeaufforderung auf dem Hostsystem.

**Framework-abhängige-Bereitstellung**

Wenn die app ist eine [Framework abhängiges Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. An der Eingabeaufforderung, navigieren Sie in den Bereitstellungsordner, und führen Sie die app durch Ausführen der app-Assembly mit *dotnet.exe*. Ersetzen Sie in den folgenden Befehl ein, den Namen der app-Assembly für \<Assembly_name >: `dotnet .\<assembly_name>.dll`.
1. Die Konsolenausgabe aus der app, Anzeigen von Fehlern wird an das Konsolenfenster geschrieben.
1. Wenn der Fehler auftreten, wenn eine Anforderung an die app ausführen, führen Sie eine Anforderung an den Host und Port, auf dem Kestrel überwacht. Mit dem Standardhost und Post, führen Sie eine Anforderung zum `http://localhost:5000/`. Wenn die app normalerweise an die Endpunktadresse Kestrel antwortet, wird das Problem wahrscheinlich Zusammenhang mit der reverse-Proxy-Konfiguration und weniger wahrscheinlich innerhalb der app.

**Eigenständige Bereitstellung**

Wenn die app ist eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Navigieren Sie an der Eingabeaufforderung in den Bereitstellungsordner, und führen Sie die app ausführbare Datei. Ersetzen Sie in den folgenden Befehl ein, den Namen der app-Assembly für \<Assembly_name >: `<assembly_name>.exe`.
1. Die Konsolenausgabe aus der app, Anzeigen von Fehlern wird an das Konsolenfenster geschrieben.
1. Wenn der Fehler auftreten, wenn eine Anforderung an die app ausführen, führen Sie eine Anforderung an den Host und Port, auf dem Kestrel überwacht. Mit dem Standardhost und Post, führen Sie eine Anforderung zum `http://localhost:5000/`. Wenn die app normalerweise an die Endpunktadresse Kestrel antwortet, wird das Problem wahrscheinlich Zusammenhang mit der reverse-Proxy-Konfiguration und weniger wahrscheinlich innerhalb der app.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core-Modul "stdout" Protokoll

Zum Aktivieren und Anzeigen von "stdout" protokolliert:

1. Navigieren Sie zu der Website des Bereitstellungsordners auf dem Hostsystem.
1. Wenn die *Protokolle* Ordner nicht vorhanden ist, erstellen Sie den Ordner. Anweisungen zum Aktivieren von MSBuild zum Erstellen der *Protokolle* Ordner in der Bereitstellung automatisch, finden Sie unter der [Verzeichnisstruktur](xref:host-and-deploy/directory-structure) Thema.
1. Bearbeiten der *"Web.config"* Datei. Festlegen **StdoutLogEnabled** auf `true` , und ändern Sie die **"stdoutlogfile"** Pfad verweist auf die *Protokolle* Ordner (z. B. `.\logs\stdout`). `stdout`das Protokoll Dateinamenpräfix sich im Pfad befindet. Ein Zeitstempel, die Prozess-Id und die Dateierweiterung werden automatisch hinzugefügt, wenn das Protokoll erstellt wird. Mit `stdout` als Präfix des Dateinamens, eine typische Protokolldatei heißt *stdout_20180205184032_5412.log*. 
1. Speichern Sie die aktualisierte *"Web.config"* Datei.
1. Stellen Sie eine Anforderung an die app an.
1. Navigieren Sie zu der *Protokolle* Ordner. Suchen Sie und öffnen Sie das neueste "stdout"-Protokoll.
1. Untersuchen Sie das Anwendungsprotokoll auf Fehler.

**Wichtig!** Deaktivieren Sie "stdout" protokollieren, wenn die Problembehandlung abgeschlossen ist.

1. Bearbeiten der *"Web.config"* Datei.
1. Legen Sie **StdoutLogEnabled** auf `false`.
1. Speichern Sie die Datei.

> [!WARNING]
> Fehler beim Deaktivieren des Protokolls "stdout" kann zur app oder Serverausfall führen. Es ist, gilt keine Beschränkung für die Protokolldateigröße oder die Anzahl der erstellten Protokolldateien.
>
> Verwenden Sie für die routinemäßige Protokollierung in einer ASP.NET Core-app, eine Protokollierung-Bibliothek, die Protokolldateigröße beschränkt und die Protokolle dreht. Weitere Informationen finden Sie unter [eines Drittanbieters Protokollanbieter](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Aktivieren die Developer-Ausnahme-Seite

Die `ASPNETCORE_ENVIRONMENT` [Umgebungsvariable "Web.config" hinzugefügt werden kann](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) zum Ausführen der app in der Entwicklungsumgebung. Solange in app-Start von die Umgebung überschreiben ist nicht `UseEnvironment` auf den Host-Generator, ermöglicht das Festlegen der Umgebungsvariablen die [Developer Ausnahmeseite](xref:fundamentals/error-handling) angezeigt werden Wenn die app ausgeführt wird.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

Festlegen der Umgebungsvariablen für `ASPNETCORE_ENVIRONMENT` wird nur für die Verwendung in staging und Testen von Servern, die mit dem Internet verbunden werden nicht empfohlen. Entfernen Sie die Umgebungsvariable aus der *"Web.config"* Datei nach der Fehlerbehebung. Informationen zum Festlegen von Umgebungsvariablen *"Web.config"*, finden Sie unter [EnvironmentVariables untergeordnetes Element des AspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Gemeinsame Startfehlern 

Finden Sie unter der [ASP.NET Core allgemeine Fehler Begriff](xref:host-and-deploy/azure-iis-errors-reference). Die meisten der am häufigsten Probleme, die app-Start zu verhindern, werden in der Referenz im Abschnitt behandelt.

## <a name="slow-or-hanging-app"></a>Langsame oder hängenden-app

Wenn eine Anwendung langsam reagiert, oder auf eine Anforderung hängt, abrufen und Analysieren einer [Dumpdatei](/visualstudio/debugger/using-dump-files). Dumpdateien können mit einer der folgenden Tools abgerufen werden:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Debugging-Tools für Windows herunterladen](https://developer.microsoft.com/windows/hardware/download-windbg), [mithilfe WinDbg Debuggen](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Remotedebuggen

Finden Sie unter [Remote Debuggen ASP.NET Core auf einem IIS-Remotecomputer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in der Visual Studio-Dokumentation.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) stellt Telemetriedaten von apps, die von IIS, einschließlich der fehlerprotokollierung und den Berichterstattungsfunktionen gehostet wird. Application Insights können nur bei Fehlern melden, die auftreten, nachdem die app gestartet wird, wenn die app Protokollierungsfunktionen verfügbar sind. Weitere Informationen finden Sie unter [Application Insights für ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Weitere Hinweise zur Fehlerbehebung

In einigen Fällen schlägt fehl eine funktionierende app sofort nach der Aktualisierung von entweder der .NET Core SDK in den Development-Versionen für Computer oder ein Paket innerhalb der app ein. In einigen Fällen können inkohärente Pakete eine App beschädigen, wenn größere Upgrades durchgeführt werden. Die meisten dieser Probleme können folgendermaßen korrigiert werden:

1. Löschen der *"bin"* und *Obj* Ordner.
1. Löschen des Pakets werden zwischengespeichert, am *% USERPROFILE%\\.nuget\\Pakete* und *%LocalAppData%\\Nuget\\v3-Cache*.
1. Stellen Sie wieder her, und das Projekt neu.
1. Vergewissern Sie sich, dass die vorherige Bereitstellung auf dem Server vor der erneuten Bereitstellung der app vollständig gelöscht wurde.

> [!TIP]
> Eine einfache Möglichkeit zum leeren von Caches des Pakets wird auszuführende `dotnet nuget locals all --clear` über eine Eingabeaufforderung.
> 
> Löschen von Zwischenspeichern von Paket kann auch erfolgen mithilfe der [nuget.exe](https://www.nuget.org/downloads) Tool und die Ausführung des Befehls `nuget locals all -clear`. *NuGet.exe* , ist eine gebündelte Installation mit dem Windows-desktop-Betriebssystem und müssen separat abgerufen werden, aus der [NuGet-Website](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Einführung in die Fehlerbehandlung in ASP.NET Core](xref:fundamentals/error-handling)
* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
* [Problembehandlung bei ASP.NET Core in Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
