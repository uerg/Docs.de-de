---
title: ASP.NET Core-Modul Konfigurationsverweis
author: guardrex
description: Wie Sie die ASP.NET Core-Modul zum Hosten von ASP.NET Core-Anwendungen zu konfigurieren.
keywords: ASP.NET Core, Ancm, Core-Modul, Iis, "stdout" Protokollierung, Umgebungsvariablen, Var Env, Subapplication, Subapp, Appoffline, App_offline, 502, Schema
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: 277e63a5663aca622e8252d6c6be1671e57cbf68
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core-Modul Konfigurationsverweis

Durch [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), und [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Dieses Dokument enthält ausführliche Informationen zum Konfigurieren der ASP.NET Core-Modul zum Hosten von ASP.NET Core-Anwendungen. Eine Einführung in die ASP.NET Core-Modul und installationsanweisungen, finden Sie unter der [ASP.NET Core Modulübersicht](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Konfiguration haben Sie über die Datei "Web.config"

ASP.NET Core-Modul wird über eine Site oder Anwendung konfiguriert *"Web.config"* Datei und verfügt über eine eigene `aspNetCore` Konfigurationsabschnitt innerhalb `system.webServer`. Hier ist ein Beispiel *"Web.config"* -Datei mit der `Microsoft.NET.Sdk.Web` SDK werden bereitstellen, sobald der Veröffentlichung des Projekts für eine [Framework abhängiges Bereitstellung](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) mit Platzhaltern für die `processPath` und `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Die *"Web.config"* unten gezeigte Beispiel ist für eine [eigenständige Bereitstellung](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) auf die [Azure App Service](https://azure.microsoft.com/services/app-service/). Weitere Informationen finden Sie unter [in IIS veröffentlichen](xref:publishing/iis). Finden Sie unter [Konfiguration des untergeordneten Anwendungen](xref:publishing/iis#configuration-of-sub-applications) für ein wichtiger Hinweis bezieht sich auf die Konfiguration des *"Web.config"* Dateien im Unterordner Anwendungen.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Attribute des-Elements aspNetCore

| Attribut | Beschreibung |
| --- | --- |
| processPath | <p>Erforderliches Zeichenfolgenattribut.</p><p>Pfad zur ausführbaren Datei, die einen HTTP-Anforderungen Lauschen Prozess gestartet wird. Relative Pfade werden unterstützt. Wenn der Pfad mit beginnt '.', gilt der Pfad relativ zum Stammverzeichnis Website sein.</p><p>Es ist kein Standardwert vorhanden.</p> |
| Argumente | <p>Optionales Zeichenfolgeattribut.</p><p>Argumente für die ausführbare Datei im angegebenen **ProcessPath**.</p><p>Der Standardwert ist eine leere Zeichenfolge.</p> |
| startupTimeLimit | <p>Optionales Ganzzahlattribut.</p><p>Dauer in Sekunden an, denen das Modul für die ausführbare Datei Starten eines Prozesses, der Port überwacht gewartet wird. Wenn dieses Zeitlimit überschritten wird, wird das Modul den Prozess zu beenden. Das Modul versucht, den Prozess erneut zu starten, wenn er eine neue Anforderung empfangen und versucht weiterhin wird, den Prozess auf nachfolgende eingehende Anforderungen neu zu starten, wenn die Anwendung kann nicht gestartet **RapidFailsPerMinute** Anzahl innerhalb der letzten parallelen Minute wiederholt.</p><p>Der Standardwert ist 120.</p> |
| shutdownTimeLimit | <p>Optionales Ganzzahlattribut.</p><p>Die Dauer in Sekunden, die auf die das Modul für die ausführbare Datei ordnungsgemäß schließen wartet bei der *app_offline.htm* Datei erkannt wird.</p><p>Der Standardwert ist 10.</p> |
| rapidFailsPerMinute | <p>Optionales Ganzzahlattribut.</p><p>Gibt die Anzahl der Wiederholungsversuche, die vom Prozess angegeben wird, in **ProcessPath** zum Absturz (Crash) pro Minute zulässig ist. Wenn dieses Limit überschritten wird, hält das Modul die Starten des Prozesses für den Rest der Minute an.</p><p>Der Standardwert ist 10.</p> |
| requestTimeout | <p>Optionales Timespan-Attribut.</p><p>Gibt die Dauer für die ASP.NET Core-Modul für eine Antwort des Prozesses "% ASPNETCORE_PORT" lauscht gewartet wird.</p><p>Der Standardwert ist "00:02:00".</p><p>Die `requestTimeout` muss angegeben werden in ganzen Minuten nur, andernfalls wird standardmäßig auf 2 Minuten.</p> |
| stdoutLogEnabled | <p>Optionales boolesches Attribut.</p><p>Bei "true", **"stdout"** und **"stderr"** für den Prozess, der im angegebenen **ProcessPath** werden an die angegebene Datei umgeleitet **"stdoutlogfile"**.</p><p>Der Standardwert ist false.</p> |
| "stdoutlogfile" | <p>Optionales Zeichenfolgeattribut.</p><p>Gibt an, der relative oder absolute Pfad für die **"stdout"** und **"stderr"** aus dem Prozess, der im angegebenen **ProcessPath** protokolliert werden. Relative Pfade sind relativ zum Stamm des Standorts. Jeder Pfad beginnend mit '.' relativ zum Stammverzeichnis Website, und alle anderen Pfade als absolute Pfade behandelt werden. Alle Ordner im Pfad angegeben, müssen in der Reihenfolge für das Modul zum Erstellen der Protokolldatei vorhanden sein. Die Prozess-ID, Zeitstempel (*YyyyMdhms*), und die Dateierweiterung (*.log*) mit einem Unterstrich Trennzeichen werden hinzugefügt, bis zum letzten Segment der **"stdoutlogfile"** bereitgestellt.</p><p>Der Standardwert ist `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | True oder False.</p><p>Bei "true", wird das Token an den untergeordneten Prozess gelauscht. "% ASPNETCORE_PORT" als Header "MS-ASPNETCORE-WINAUTHTOKEN" pro Anforderung weitergeleitet werden. Es liegt die Verantwortung des entsprechenden Prozesses CloseHandle dieses Token pro Anforderung aufgerufen werden.</p><p>Der Standardwert ist true.</p> |
| disableStartUpErrorPage | True oder False.</p><p>Bei "true", die **502.5 - Prozessfehler** Seite wird unterdrückt werden, und die Status 502-Codepage in konfiguriert Ihre *"Web.config"* Vorrang.</p><p>Der Standardwert ist false.</p> |

### <a name="setting-environment-variables"></a>Festlegen von Umgebungsvariablen

ASP.NET Core-Modul können Sie angeben, dass die Umgebungsvariablen für den Prozess, der im angegebenen der `processPath` Attribut durch Angabe in einer oder mehreren `environmentVariable` untergeordneten Elemente des ein `environmentVariables` Auflistungselement unter der `aspNetCore` Element. In diesem Abschnitt festgelegten Umgebungsvariablen haben Vorrang gegenüber System Umgebungsvariablen für den Prozess.

Im folgenden Beispiel werden zwei Umgebungsvariablen festgelegt. `ASPNETCORE_ENVIRONMENT`Konfigurieren der Umgebung der Anwendung zu `Development`. Ein Entwickler möglicherweise vorübergehend legen Sie diesen Wert der *"Web.config"* Datei, um zu erzwingen der [Developer Ausnahmeseite](xref:fundamentals/error-handling) laden, wenn eine Ausnahme für die app zu debuggen. `CONFIG_DIR`ist ein Beispiel für eine benutzerdefinierte Umgebungsvariable, in denen der Entwickler Code geschrieben hat, die den Wert beim Start zu einen Pfad kombiniert, um die app-Konfigurationsdatei zu laden gelesen wird.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

## <a name="appofflinehtm"></a>App_offline.htm

Wenn Sie eine Datei mit dem Namen einfügen *app_offline.htm* im Stammverzeichnis eines Verzeichnisses Web Application ASP.NET Core-Modul versuchen zum ordnungsgemäßen Herunterfahren die app, und beendet die Verarbeitung eingehender Anforderungen. Wenn die Anwendung weiterhin, nach dem ausgeführt wird `shutdownTimeLimit` Anzahl von Sekunden, wird ASP.NET Core-Modul brechen Sie den laufenden Prozess.

Während der *app_offline.htm* Datei vorhanden ist, das ASP.NET Core-Modul wird auf Anforderungen reagiert durch Senden von den Inhalt der *app_offline.htm* Datei. Sobald die *app_offline.htm* Datei wird entfernt, die nächste Anforderung lädt die Anwendung, die Sie dann auf Anforderungen reagiert.

## <a name="start-up-error-page"></a>Start-Fehler (Seite)

Wenn ASP.NET Core-Modul ein Fehler auftritt, starten Sie den Back-End-Prozess oder dem Back-End-Prozess gestartet wird, aber ein Fehler auftritt, am konfigurierten Port lauschen, sehen Sie eine 502.5 http-Status-Codepage. Verwenden Sie zum Unterdrücken der auf dieser Seite, und stellen Sie die Standardcodepage für IIS 502 Status wieder her, die `disableStartUpErrorPage` Attribut. Weitere Informationen zum Konfigurieren von benutzerdefinierten Fehlermeldungen finden Sie unter [HTTP-Fehler `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![502-Statusseite](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Umleitung und protokollerstellung

ASP.NET Core-Modul leitet `stdout` und `stderr` Protokolle auf den Datenträger, wenn Sie festlegen, die `stdoutLogEnabled` und `stdoutLogFile` Attribute der `aspNetCore` Element. Alle Ordner in der `stdoutLogFile` Pfad muss vorhanden sein, in der Reihenfolge für das Modul zum Erstellen der Protokolldatei. Eine Timestamp-Werte und Datei-Erweiterung wird automatisch hinzugefügt werden, wenn die Protokolldatei erstellt wird. Protokolle werden nicht gedreht, es sei denn, der Prozess wiederverwendet/Neustart stattfindet. Es ist die Zuständigkeit für den Hoster den Speicherplatz zu begrenzen, die, den die Protokolle zu nutzen. Mithilfe der `stdout` Protokoll wird nur empfohlen, für die Fehlerbehebung bei Startproblemen Anwendung und nicht für allgemeine Anwendung Protokollieren von Zwecken.

Name der Protokolldatei besteht durch die Prozess-ID (PID), Zeitstempel anfügen (*YyyyMdhms*), und die Dateierweiterung (*.log*) auf das letzte Segment des der `stdoutLogFile` Pfad (in der Regel *"stdout"* ) getrennt durch Unterstriche enthalten. Z. B. wenn die `stdoutLogFile` Pfad endet mit *"stdout"*, ein Protokolls für eine Anwendung mit einem PID des 10652 am 8/10/2017 um 12:05:02 erstellt wurde, wurde der Dateiname *stdout_10652_20178101252.log*.

Hier ist ein Beispiel für `aspNetCore` Element, konfiguriert `stdout` Protokollierung. Die `stdoutLogFile` Pfad angezeigt, die im Beispiel eignet sich für die Azure App Service. Ein lokaler Pfad oder Netzwerkfreigabepfad ist akzeptabel, für die lokale Anmeldung. Vergewissern Sie sich, dass die Identität des AppPool-Benutzers über die Berechtigung zum Schreiben in den angegebenen Pfad verfügt.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
Finden Sie unter [Konfiguration haben Sie über die Datei "Web.config"](#configuration-via-webconfig) ein Beispiel für die `aspNetCore` Element in der *"Web.config"* Datei.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>ASP.NET Core-Modul mit einer IIS freigegebene Konfiguration

Das Installationsprogramm von ASP.NET Core ausgeführt wird, mit den Berechtigungen des der **SYSTEM** Konto. Da das lokale Systemkonto haben ändert nicht die Berechtigung für den Pfad der Bibliotheksfreigabe der von der IIS-Freigabekonfiguration verwendet wird, wird das Installationsprogramm "Zugriff verweigert" Fehler beim Konfigurieren der moduleinstellungen in erreicht  *Datei "applicationHost.config"* auf die Freigabe.

Nicht unterstützte behelfslösung ist das IIS-Freigabekonfiguration deaktivieren, führen Sie das Installationsprogramm, exportieren Sie die aktualisierte *"applicationHost.config"* auf die Freigabe der Datei und die IIS-Freigabekonfiguration erneut zu aktivieren.

## <a name="module-schema-and-configuration-file-locations"></a>Dateispeicherorte für Module, Schema und Konfiguration

### <a name="module"></a>Modul

**IIS (X86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (X86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * % ProgramFiles% (x86) %\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.Xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfiguration

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Sie können nach suchen *aspnetcore.dll* in der *"applicationHost.config"* Datei. Für IIS Express die *"applicationHost.config"* Datei wird nicht standardmäßig vorhanden. Die Datei wird erstellt am *{Anwendungsstamm}\.Vs\config* beim Starten alle Webanwendungsprojekt in Visual Studio-Projektmappe.
