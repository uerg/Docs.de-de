---
title: Problembehandlung bei ASP.NET Core in Azure App Service
author: guardrex
description: Erfahren Sie, wie Sie Probleme mit ASP.NET Core Azure App Service-Bereitstellungen diagnostizieren können.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 47056c80c7abf5dd5ad5ae96af7b821d31b21b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Problembehandlung bei ASP.NET Core in Azure App Service

Von [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Dieser Artikel enthält Anweisungen für eine ASP.NET Core Diagnostizieren von app-Start-Problem mit Azure App Service-Diagnosetools. Weitere Hinweise zur Fehlerbehebung, finden Sie unter [Übersicht über die Azure App Service](/azure/app-service/app-service-diagnostics) und [Vorgehensweise: Überwachen von Apps in Azure App Service](/azure/app-service/web-sites-monitor) in der Azure-Dokumentation.

## <a name="app-startup-errors"></a>App-Start-Fehler

**502.5 Prozess Fehler**  
Der Arbeitsprozess schlägt fehl. Die app starten nicht.

Die [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) beim Starten des Arbeitsprozesses aber nicht gestartet. Untersuchen das Ereignisprotokoll der Anwendung häufig können diese Art von Problem zu beheben. Zugreifen auf das Protokoll wird erläutert, der [Anwendungsereignisprotokoll](#application-event-log) Abschnitt.

Die *502.5 Prozessfehler* Fehlerseite wird zurückgegeben, wenn eine falsch konfigurierte app bewirkt, der Arbeitsprozess dass fehlschlägt:

![Browserfenster, die mit der Seite "502.5 Prozessfehler"](troubleshoot/_static/process-failure-page.png)

**500 Interner Serverfehler**  
Die app gestartet wurde, aber ein Fehler wird verhindert, dass es sich bei den Server die Anforderung.

Dieser Fehler tritt auf, in der app-Code während des Starts oder beim Erstellen einer Antwort. Enthält die Antwort möglicherweise keinen Inhalt, oder die Antwort möglicherweise angezeigt, wie eine *500 Internal Server Error* im Browser. Ereignisprotokoll der Anwendung gibt normalerweise an, dass die Anwendung normal gestartet. Aus Sicht des Servers ist richtig. Der Anwendungsstart, aber es kann keine gültige Antwort generiert. [Führen Sie die app in der Konsole Kudu](#run-the-app-in-the-kudu-console) oder [aktivieren Sie das ASP.NET Core-Modul "stdout" Protokoll](#aspnet-core-module-stdout-log) um das Problem zu beheben.

**Zurücksetzen der Verbindung**

Wenn ein Fehler auftritt, nachdem die Header gesendet werden, ist es zu spät für den Server zum Senden einer **500 Internal Server Error** Wenn ein Fehler auftritt. Dies erfolgt häufig auf, wenn ein Fehler, während der Serialisierung von komplexen Objekten auf eine Antwort auftritt. Diese Art von Fehler angezeigt wird, als ein *Zurücksetzen der Verbindung* Fehler auf dem Client. [Anwendungsprotokollierung](xref:fundamentals/logging/index) hilft diese Fehlertypen zu beheben.

## <a name="default-startup-limits"></a>Starten von standardeinschränkungen

ASP.NET Core-Modul konfiguriert ist, hat den Standardwert *StartupTimeLimit* von 120 Sekunden. Wenn auf den Standardwert belassen, kann eine app dauern, bis zu zwei Minuten starten, bevor das Modul einen Prozessfehler protokolliert. Informationen zum Konfigurieren des Moduls finden Sie unter [Attribute des-Elements AspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Fehlerbehebung bei der app-Start

### <a name="application-event-log"></a>Anwendungsereignisprotokoll

Verwenden Sie das Ereignisprotokoll der Anwendung für den Zugriff auf die **Diagnose und Lösung von Problemen** Blatt im Azure-Portal:

1. Öffnen Sie im Azure-Portal, die app Blatt in der **Anwendungsdienste** Blatt.
1. Wählen Sie die **Diagnose und Lösung von Problemen** Blatt.
1. Klicken Sie unter **PROBLEMKATEGORIE wählen**, wählen die **Web-App nach unten** Schaltfläche.
1. Klicken Sie unter **vorgeschlagen Projektmappen**, öffnen Sie den Bereich für **Anwendungsereignisprotokolle öffnen**. Wählen Sie die **öffnen Anwendungsereignisprotokolle** Schaltfläche.
1. Überprüfen Sie den letzten Fehler gebotenen der *IIS AspNetCoreModule* in der **Quelle** Spalte.

Eine Alternative zur Verwendung der **Diagnose und Lösung von Problemen** Blatt ist, überprüfen Sie das Anwendungsereignisprotokoll-Datei, die direkt mit [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Wählen Sie die **erweiterte Tools** Blatt in der **ENTWICKLUNGSTOOLS** Bereich. Wählen Sie die **Go&rarr;**  Schaltfläche. Kudu-Konsole wird in einer neuen Browserregisterkarte oder Fenster geöffnet.
1. Öffnen Sie die Navigationsleiste am oberen Rand der Seite mit **Debug-Konsole** , und wählen Sie **CMD**.
1. Öffnen der **LogFiles** Ordner.
1. Wählen Sie den Bleistift neben dem *eventlog.xml* Datei.
1. Untersuchen Sie das Protokoll an. Bildlauf zum Ende des Protokolls auf der aktuellsten Ereignisse finden Sie unter.

### <a name="run-the-app-in-the-kudu-console"></a>Führen Sie die app in der Konsole Kudu

Viele Startfehlern erzeugen keine nützlichen Informationen in das Anwendungsereignisprotokoll. Sie können die app ausführen, der [Kudu](https://github.com/projectkudu/kudu/wiki) Ausführung Remotekonsole, um den Fehler zu ermitteln:

1. Wählen Sie die **erweiterte Tools** Blatt in der **ENTWICKLUNGSTOOLS** Bereich. Wählen Sie die **Go&rarr;**  Schaltfläche. Kudu-Konsole wird in einer neuen Browserregisterkarte oder Fenster geöffnet.
1. Öffnen Sie die Navigationsleiste am oberen Rand der Seite mit **Debug-Konsole** , und wählen Sie **CMD**.
1. Öffnen Sie die Ordner aus, den Pfad **Website** > **"Wwwroot"**.
1. Führen Sie die app durch Ausführen der app-Assembly, in der Konsole.
   * Wenn die app ist eine [Framework abhängiges Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd), führen Sie die app-Assembly mit *dotnet.exe*. Ersetzen Sie in den folgenden Befehl ein, den Namen der app-Assembly für `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Wenn die app ist eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd)führen die app der ausführbaren Datei. Ersetzen Sie in den folgenden Befehl ein, den Namen der app-Assembly für `<assembly_name>`: `<assembly_name>.exe`
1. Die Konsolenausgabe aus der app, Anzeigen von Fehlern ist an die Kudu-Konsole weitergeleitet.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core-Modul "stdout" Protokoll

Die ASP.NET Core-Modul "stdout" Protokolldatensätze häufig nützlich, Fehlermeldungen im Ereignisprotokoll der Anwendung nicht gefunden. Zum Aktivieren und Anzeigen von "stdout" protokolliert:

1. Navigieren Sie zu der **Diagnose und Lösung von Problemen** Blatt im Azure-Portal.
1. Klicken Sie unter **PROBLEMKATEGORIE wählen**, wählen die **Web-App nach unten** Schaltfläche.
1. Klicken Sie unter **vorgeschlagen Projektmappen** > **"stdout" Log-Umleitung aktivieren**, wählen Sie die Schaltfläche, um **geöffnete Kudu-Konsole so bearbeiten Sie die Datei "Web.config"**.
1. In der Kudu **Diagnostics-Konsole**, öffnen Sie die Ordner aus, den Pfad **Website** > **"Wwwroot"**. Bildlauf nach unten zur Anzeigen der *"Web.config"* Datei am unteren Rand der Liste.
1. Klicken Sie auf den Bleistift neben dem *"Web.config"* Datei.
1. Legen Sie **StdoutLogEnabled** zu `true` , und ändern Sie die **"stdoutlogfile"** Pfad zur: `\\?\%home%\LogFiles\stdout`.
1. Wählen Sie **speichern** , speichern Sie die aktualisierte *"Web.config"* Datei.
1. Stellen Sie eine Anforderung an die app an.
1. Zurück zum Azure-Portal. Wählen Sie die **erweiterte Tools** Blatt in der **ENTWICKLUNGSTOOLS** Bereich. Wählen Sie die **Go&rarr;**  Schaltfläche. Kudu-Konsole wird in einer neuen Browserregisterkarte oder Fenster geöffnet.
1. Öffnen Sie die Navigationsleiste am oberen Rand der Seite mit **Debug-Konsole** , und wählen Sie **CMD**.
1. Wählen Sie die **LogFiles** Ordner.
1. Überprüfen Sie die **"geändert"** Spalte, und wählen Sie das Stiftsymbol so bearbeiten Sie die "stdout" Melden Sie sich mit dem Datum der letzten Änderung.
1. Wenn die Protokolldatei geöffnet wird, wird der Fehler angezeigt.

**Wichtig** Deaktivieren Sie "stdout" protokollieren, wenn die Problembehandlung abgeschlossen ist.

1. In der Kudu **Diagnostics-Konsole**, kehren Sie zurück auf den Pfad **Website** > **"Wwwroot"** offengelegt der *"Web.config"* Datei. Öffnen der **"Web.config"** Datei erneut, indem Sie das Stiftsymbol auswählen.
1. Legen Sie **StdoutLogEnabled** auf `false`.
1. Wählen Sie **speichern** zum Speichern der Datei.

> [!WARNING]
> Fehler beim Deaktivieren des Protokolls "stdout" kann zur app oder Serverausfall führen. Für die Protokollgröße oder die Anzahl von erstellten Protokolldateien ist kein Grenzwert festgelegt. Verwenden Sie nur die Protokollierung, um die app Startproblemen "stdout".
>
> Verwenden Sie für die allgemeine Protokollierung in einer ASP.NET Core-app nach dem Start, eine Protokollierung-Bibliothek, die Protokolldateigröße beschränkt und die Protokolle dreht. Weitere Informationen finden Sie unter [eines Drittanbieters Protokollanbieter](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Gemeinsame Startfehlern 

Finden Sie unter der [ASP.NET Core allgemeine Fehler Begriff](xref:host-and-deploy/azure-iis-errors-reference). Die meisten der am häufigsten Probleme, die app-Start zu verhindern, werden in der Referenz im Abschnitt behandelt.

## <a name="slow-or-hanging-app"></a>Langsame oder hängenden-app

Wenn eine Anwendung langsam reagiert, oder auf eine Anforderung hängt, finden Sie unter [Leistungsprobleme der Behandlung von Problemen bei langsamen Web app in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) zum Debuggen von Anweisungen.

## <a name="remote-debugging"></a>Remotedebuggen

Informationen hierzu finden Sie in den folgenden Themen:

* [Web-apps-Bereich der Behandlung von Problemen bei einer Web-app in Azure App Service mithilfe von Visual Studio für das Remotedebuggen](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure-Dokumentation)
* [Remote Debug ASP.NET Core unter IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio-Dokumentation)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) stellt Telemetriedaten von apps in Azure App Service, einschließlich Fehler protokollieren und den Berichterstattungsfunktionen gehostet. Application Insights können nur bei Fehlern melden, die auftreten, nachdem die app gestartet wird, wenn die app Protokollierungsfunktionen verfügbar sind. Weitere Informationen finden Sie unter [Application Insights für ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Überwachen der Blätter

Überwachung Blades bieten eine Alternative zur Problembehandlung Erfahrung, um die weiter oben in diesem Thema beschriebenen Methoden. Diese blättern können verwendet werden, um-Serie 500-Fehler zu diagnostizieren.

Vergewissern Sie sich, dass ASP.NET Core-Erweiterungen installiert sind. Wenn die Erweiterungen installiert sind, installieren Sie diese manuell:

1. In der **ENTWICKLUNGSTOOLS** Blade-Abschnitt der **Erweiterungen** Blatt.
1. Die **Core ASP.NET-Erweiterungen** sollte in der Liste angezeigt werden.
1. Wenn die Erweiterungen installiert sind, wählen Sie die **hinzufügen** Schaltfläche.
1. Wählen Sie die **Core ASP.NET-Erweiterungen** aus der Liste.
1. Wählen Sie **OK** die rechtlichen Bedingungen zu akzeptieren.
1. Wählen Sie **OK** auf die **Erweiterung hinzufügen** Blatt.
1. Eine Popup informationsmeldung gibt an, wenn die Erweiterungen erfolgreich installiert sind.

Wenn "stdout" Protokollierung nicht aktiviert ist, gehen Sie folgendermaßen vor:

1. Wählen Sie im Azure-Portal die **erweiterte Tools** Blatt in der **ENTWICKLUNGSTOOLS** Bereich. Wählen Sie die **Go&rarr;**  Schaltfläche. Kudu-Konsole wird in einer neuen Browserregisterkarte oder Fenster geöffnet.
1. Öffnen Sie die Navigationsleiste am oberen Rand der Seite mit **Debug-Konsole** , und wählen Sie **CMD**.
1. Öffnen Sie die Ordner aus, den Pfad **Website** > **"Wwwroot"** und Bildlauf nach unten zur Anzeigen der *"Web.config"* Datei am unteren Rand der Liste.
1. Klicken Sie auf den Bleistift neben dem *"Web.config"* Datei.
1. Legen Sie **StdoutLogEnabled** zu `true` , und ändern Sie die **"stdoutlogfile"** Pfad zur: `\\?\%home%\LogFiles\stdout`.
1. Wählen Sie **speichern** , speichern Sie die aktualisierte *"Web.config"* Datei.

Fahren Sie mit der diagnoseprotokollierung zu aktivieren:

1. Wählen Sie im Azure-Portal die **Diagnoseprotokolle** Blatt.
1. Wählen Sie die **auf** wechseln für **Anwendungsprotokollierung (Dateisystem)** und **detaillierte Fehlermeldungen**. Wählen Sie die **speichern** Schaltfläche am oberen Rand der Blatt.
1. Wählen Sie für Anforderungsfehler, auch bekannt als Protokollierung der Fehler bei Anforderung Ereignis Pufferung (FREB) enthalten die **auf** wechseln für **für Anforderungsfehler**. 
1. Wählen Sie die **Protokolldatenstrom** Blatt, die sofort, unter angezeigt wird der **Diagnoseprotokolle** Blatt im Portal.
1. Stellen Sie eine Anforderung an die app an.
1. In den Protokolldaten Stream wird die Ursache des Fehlers angegeben.

**Wichtig** Achten Sie darauf, deaktivieren Sie die Protokollierung "stdout", wenn die Problembehandlung abgeschlossen ist. Lesen Sie die Anweisungen in der [ASP.NET Core-Modul "stdout" Log](#aspnet-core-module-stdout-log) Abschnitt.

So zeigen Sie die Ablaufverfolgungsprotokolle für Anforderungsfehler (FREB-Protokolle) an:

1. Navigieren Sie zu der **Diagnose und Lösung von Problemen** Blatt im Azure-Portal.
1. Wählen Sie **fehlerhafte Tracing Anforderungsprotokolle** aus der **SUPPORTTOOLS** Clientbereich der Randleiste.

Finden Sie unter [Fehler Anforderung verfolgt die aktivieren diagnoseprotokollierung für Web-apps in Azure App Service-Thema im Abschnitt](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) und [Anwendungsleistung häufig gestellte Fragen für Web Apps in Azure: Wie aktiviere ich auf Anforderungsfehler?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Weitere Informationen.

Weitere Informationen finden Sie unter [Aktivieren der diagnoseprotokollierung für webapps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Fehler beim Deaktivieren des Protokolls "stdout" kann zur app oder Serverausfall führen. Für die Protokollgröße oder die Anzahl von erstellten Protokolldateien ist kein Grenzwert festgelegt.
>
> Verwenden Sie für die routinemäßige Protokollierung in einer ASP.NET Core-app, eine Protokollierung-Bibliothek, die Protokolldateigröße beschränkt und die Protokolle dreht. Weitere Informationen finden Sie unter [eines Drittanbieters Protokollanbieter](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Einführung in die Fehlerbehandlung in ASP.NET Core](xref:fundamentals/error-handling)
* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Problembehandlung bei HTTP-Fehler "502 Ungültiger Gateway" und "503 Dienst nicht verfügbar" in Ihren Azure-Web-apps](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Problembehandlung bei langsamen Web-app-Leistungsprobleme in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Leistung der Anwendung häufig gestellte Fragen für Web Apps in Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web App Sandbox (App Service Runtime Ausführung Einschränkungen)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (Azure Friday: Azure App Service-Diagnose und -Fehlerbehebung; zwölfminütiges Video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
