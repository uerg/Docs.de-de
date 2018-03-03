---
title: ASP.NET Core-Modul Konfigurationsverweis
author: guardrex
description: Erfahren Sie, wie die ASP.NET Core-Modul zum Hosten von ASP.NET Core apps konfigurieren.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 5aac5cf2b8fd4bc53ba7201645b9bb02a5d1ecae
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core-Modul Konfigurationsverweis

Durch [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), und [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Dieses Dokument enthält Anweisungen zum Konfigurieren der ASP.NET Core-Modul zum Hosten von ASP.NET Core-apps. Eine Einführung in die ASP.NET Core-Modul und installationsanweisungen, finden Sie unter der [ASP.NET Core Modulübersicht](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Mit der Datei "Web.config"

ASP.NET Core-Modul ist konfiguriert, mit der `aspNetCore` Teil der `system.webServer` Knoten am Standort *"Web.config"* Datei.

Die folgenden *"Web.config"* Datei veröffentlicht wird, für eine [Framework abhängiges Bereitstellung](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) und konfiguriert den ASP.NET Core-Modul um Website-Anforderungen zu verarbeiten:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Die folgenden *"Web.config"* netzwerkschutzentwurf für eine [eigenständige Bereitstellung](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Wenn eine app bereitgestellt wird, um [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` -Pfad folgendermaßen festgelegt `\\?\%home%\LogFiles\stdout`. Der Pfad speichert "stdout" die Protokolle, um die *LogFiles* Ordner, einen Speicherort automatisch vom Dienst erstellt.

Finden Sie unter [untergeordnete Anwendung Konfiguration](xref:host-and-deploy/iis/index#sub-application-configuration) für ein wichtiger Hinweis bezieht sich auf die Konfiguration des *"Web.config"* Dateien im Unterordner apps.

### <a name="attributes-of-the-aspnetcore-element"></a>Attribute des-Elements aspNetCore

| Attribut | Beschreibung | Standard |
| --------- | ----------- | :-----: |
| `arguments` | <p>Optionales Zeichenfolgeattribut.</p><p>Argumente für die ausführbare Datei im angegebenen **ProcessPath**.</p>| |
| `disableStartUpErrorPage` | True oder False.</p><p>Bei "true", die **502.5 - Prozessfehler** Seite unterdrückt wird, und die Status 502-Codepage konfiguriert wird, der *"Web.config"* hat Vorrang vor.</p> | `false` |
| `forwardWindowsAuthToken` | True oder False.</p><p>Bei "true", wird das Token an den untergeordneten Prozess gelauscht. "% ASPNETCORE_PORT" als Header "MS-ASPNETCORE-WINAUTHTOKEN" pro Anforderung weitergeleitet. Es liegt die Verantwortung des entsprechenden Prozesses CloseHandle dieses Token pro Anforderung aufgerufen werden.</p> | `true` |
| `processPath` | <p>Erforderliches Zeichenfolgenattribut.</p><p>Pfad zur ausführbaren Datei, die einen HTTP-Anforderungen Lauschen Prozess startet. Relative Pfade werden unterstützt. Wenn der Pfad mit beginnt `.`, gilt der Pfad relativ zum Stammverzeichnis Website sein.</p> | |
| `rapidFailsPerMinute` | <p>Optionales Ganzzahlattribut.</p><p>Gibt die Anzahl der Wiederholungsversuche, die vom Prozess angegeben wird, in **ProcessPath** zum Absturz (Crash) pro Minute zulässig ist. Wenn dieses Limit überschritten wird, beendet das Modul mit dem Starten des Prozesses für den Rest der Minute.</p> | `10` |
| `requestTimeout` | <p>Optionales Timespan-Attribut.</p><p>Gibt die Dauer für die ASP.NET Core-Modul auf eine Antwort vom Prozess "% ASPNETCORE_PORT" lauscht wartet.</p><p>Die `requestTimeout` muss angegeben werden in ganzen Minuten nur, andernfalls wird standardmäßig auf 2 Minuten.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Dauer in Sekunden, die das Modul für die ausführbare Datei ordnungsgemäß schließen wartet, wenn die *app_offline.htm* Datei erkannt wird.</p> | `10` |
| `startupTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Dauer in Sekunden an, denen das Modul wartet, bis die ausführbare Datei Starten eines Prozesses, der Port überwacht. Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess an. Das Modul versucht, den Prozess neu starten, wenn eine neue Anforderung erhält und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu starten, es sei denn, die app nicht gestartet **RapidFailsPerMinute** -Mal in den letzten Paralleles Minute an.</p> | `120` |
| `stdoutLogEnabled` | <p>Optionales boolesches Attribut.</p><p>Bei "true", **"stdout"** und **"stderr"** für den Prozess, der im angegebenen **ProcessPath** an die angegebene Datei umgeleitet werden **"stdoutlogfile"**.</p> | `false` |
| `stdoutLogFile` | <p>Optionales Zeichenfolgeattribut.</p><p>Gibt an, der relative oder absolute Pfad für die **"stdout"** und **"stderr"** aus dem Prozess, der im angegebenen **ProcessPath** werden protokolliert. Relative Pfade sind relativ zum Stamm des Standorts. Jeder Pfad beginnend mit `.` sind relativ zur Website Stamm und alle anderen Pfade als absolute Pfade behandelt werden. Alle Ordner im Pfad angegeben, müssen in der Reihenfolge für das Modul zum Erstellen der Protokolldatei vorhanden sein. Mithilfe des Unterstrich Trennzeichen, Timestamp, Prozess-ID, und die Erweiterung (*.log*) hinzukommen bis zum letzten Segment der **"stdoutlogfile"** Pfad. Wenn `.\logs\stdout` angegeben wird als ein Wert wird als ein Beispiel für "stdout" Protokoll gespeichert *stdout_20180205194132_1934.log* in der *Protokolle* Ordner, wenn auf 2/5/2018 um 19:41:32 mit 1934 Prozess-ID gespeichert.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Festlegen von Umgebungsvariablen

Umgebungsvariablen können angegeben werden, für den Prozess in den `processPath` Attribut. Geben Sie eine Umgebungsvariable mit dem `environmentVariable` untergeordnetes Element von einer `environmentVariables` Elements der Auflistung. In diesem Abschnitt festgelegten Umgebungsvariablen haben Vorrang gegenüber System Umgebungsvariablen.

Im folgenden Beispiel wird zwei Umgebungsvariablen. `ASPNETCORE_ENVIRONMENT` konfiguriert die app-Umgebung zu `Development`. Ein Entwickler möglicherweise vorübergehend legen Sie diesen Wert der *"Web.config"* Datei, um zu erzwingen der [Developer Ausnahmeseite](xref:fundamentals/error-handling) laden, wenn eine Ausnahme für die app zu debuggen. `CONFIG_DIR` ist ein Beispiel für eine benutzerdefinierte Umgebungsvariable, in dem der Entwickler Code, der den Wert beim Start geschrieben hat, um zu bilden, einen Pfad für das Laden der app-Konfigurationsdatei liest.

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

> [!WARNING]
> Legen Sie nur die `ASPNETCORE_ENVIRONMENT` Envirnonment-Variable, die `Development` für staging und Testen von Servern, die für nicht vertrauenswürdige Netzwerke wie z. B. das Internet zugänglich sind.

## <a name="appofflinehtm"></a>app_offline.htm

Wenn eine Datei mit dem Namen *app_offline.htm* wird erkannt, im Stammverzeichnis einer App ASP.NET Core-Modul versucht, ordnungsgemäß heruntergefahren, die app und die Verarbeitung von eingehender Anforderungen beenden. Wenn nach der Anzahl von Sekunden, die in definierten Anwendung ausgeführter `shutdownTimeLimit`, die ASP.NET Core-Modul beendet den laufenden Prozess.

Während der *app_offline.htm* Datei ist vorhanden, die ASP.NET Core-Modul-reagiert auf Anforderungen durch Zurücksenden der Inhalt des der *app_offline.htm* Datei. Wenn die *app_offline.htm* Datei wird entfernt, die nächste Anforderung wird die Anwendung gestartet.

## <a name="start-up-error-page"></a>Start-Fehler (Seite)

Wenn ASP.NET Core-Modul ein Fehler auftritt, starten Sie den Back-End-Prozess oder dem Back-End-Prozess gestartet wird, aber ein Fehler auftritt, an den konfigurierten Port Lauschen eine *502.5 Prozessfehler* Code Statusseite wird angezeigt. Verwenden Sie zum Unterdrücken der auf dieser Seite, und stellen Sie die Standardcodepage für IIS 502 Status wieder her, die `disableStartUpErrorPage` Attribut. Weitere Informationen zum Konfigurieren von benutzerdefinierten Fehlermeldungen finden Sie unter [HTTP-Fehler `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 Codepage Status "Fehler"-Prozess](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Umleitung und protokollerstellung

ASP.NET Core-Modul leitet `stdout` und `stderr` Protokolle, wenn auf den Datenträger der `stdoutLogEnabled` und `stdoutLogFile` Attribute der `aspNetCore` Element festgelegt werden. Alle Ordner in der `stdoutLogFile` Pfad muss vorhanden sein, in der Reihenfolge für das Modul zum Erstellen der Protokolldatei. Sinnvolles Zeitstempel und der Datei werden automatisch hinzugefügt, wenn die Protokolldatei erstellt wird. Protokolle werden nicht gedreht werden, es sei denn, der Prozess wiederverwendet/Neustart stattfindet. Es ist die Zuständigkeit für den Hoster den Speicherplatz zu begrenzen, die, den die Protokolle zu nutzen. Mithilfe der `stdout` Protokoll wird nur für die Fehlerbehebung bei Startproblemen app empfohlen. Verwenden Sie das Protokoll "stdout" nicht zur Protokollierung allgemeiner app. Verwenden Sie für die routinemäßige Protokollierung in einer ASP.NET Core-app, eine Protokollierung-Bibliothek, die Protokolldateigröße beschränkt und die Protokolle dreht. Weitere Informationen finden Sie unter [eines Drittanbieters Protokollanbieter](xref:fundamentals/logging/index#third-party-logging-providers).

Besteht aus der Protokolldateinamen durch Anhängen der Zeitstempel, die Prozess-ID und die Dateierweiterung (*.log*) auf das letzte Segment des der `stdoutLogFile` Pfad (in der Regel *"stdout"*) getrennt durch Unterstriche enthalten. Wenn die `stdoutLogFile` Pfad endet mit *"stdout"*, ein Protokoll für eine app mit einem PID des 1934 auf 2/5/2018 erstellt werden, um 19:42:32 wurde der Dateiname *stdout_20180205194132_1934.log*.

Im folgenden Beispiel `aspNetCore` Element konfiguriert `stdout` Protokollierung für eine app in Azure App Service gehostet. Ein lokaler Pfad oder Netzwerkfreigabepfad ist akzeptabel, für die lokale Anmeldung. Vergewissern Sie sich, dass die Identität des AppPool-Benutzers über die Berechtigung zum Schreiben in den angegebenen Pfad verfügt.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Finden Sie unter [mit der Datei "Web.config"](#configuration-with-webconfig) ein Beispiel für die `aspNetCore` Element in der *"Web.config"* Datei.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Die Proxykonfiguration verwendet das HTTP-Protokoll und ein Paarbildungstoken

Der Proxy zwischen dem ASP.NET Core-Modul und Kestrel erstellten verwendet das HTTP-Protokoll. Mithilfe von HTTP ist zur Optimierung der Leistung, abgewickelt der Datenverkehr zwischen dem Modul und Kestrel für einen Loopback-Adresse aus der Netzwerkschnittstelle. Es ist kein Risiko, dass Lauschangriffe des Datenverkehrs zwischen dem Modul und Kestrel von einem anderen Speicherort aus dem Server.

Ein Paarbildungstoken wird verwendet, um sicherzustellen, dass die von Kestrel empfangenen Anfragen von IIS über einen Proxy gesendet wurden und nicht von einer anderen Quelle stammen. Ereignispaarbildung Token erstellt und in einer Umgebungsvariablen festgelegt (`ASPNETCORE_TOKEN`) durch das Modul. Das Paarbildungstoken ist auch bei jeder Proxyanforderung in einem Header (`MSAspNetCoreToken`) festgelegt. IIS-Middleware überprüft jede erhaltene Anforderung, um sicherzustellen, dass der Headerwert des Paarbildungstokens dem Wert der Umgebungsvariablen entspricht. Wenn die Tokenwerte nicht übereinstimmen, wird die Anforderung protokolliert und abgelehnt. Die ereignispaarbildung-token-Umgebungsvariable und den Datenverkehr zwischen dem Modul und Kestrel sind nicht von einem anderen Speicherort aus dem Server zugegriffen werden kann. Wenn ein Angreifer den Wert des Paarbildungstokens nicht kennt, kann er keine Anforderungen einreichen, die die IIS-Middleware-Prüfung umgehen.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>ASP.NET Core-Modul mit einer IIS freigegebene Konfiguration

Das Installationsprogramm von ASP.NET Core ausgeführt wird, mit den Berechtigungen des der **SYSTEM** Konto. Da das lokale Systemkonto keine Berechtigung zum Ändern für den Pfad der Bibliotheksfreigabe, in der IIS-Freigabekonfiguration, trifft der Installer "Zugriff verweigert" Fehler beim Konfigurieren der moduleinstellungen in *"applicationHost.config"* auf die Freigabe. Wenn Sie eine IIS-Freigabekonfiguration verwenden zu können, gehen Sie folgendermaßen vor:

1. Deaktivieren Sie die IIS-Freigabekonfiguration.
1. Führen Sie den Installer aus.
1. Exportieren Sie die aktualisierte *"applicationHost.config"* Datei auf die Freigabe.
1. Reaktivieren Sie den IIS-Freigabekonfiguration.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Version des Moduls und Hosten von Paket-Installer-Protokolle

Die Version des installierten ASP.NET Core Moduls zu ermitteln:

1. Navigieren Sie auf dem Hostsystem zu *%windir%\System32\inetsrv*.
1. Suchen Sie die *aspnetcore.dll* Datei.
1. Mit der rechten Maustaste in der das, und wählen Sie **Eigenschaften** aus dem Kontextmenü.
1. Wählen Sie die **Details** Registerkarte. Die **Dateiversion** und **Produktversion** die installierte Version des Moduls darstellen.

Das Hosting mit Windows Server Paket-Installer-Protokolle für das Modul finden Sie unter *"c:"\\Benutzer\\%UserName%\\AppData\\lokale\\Temp*. Die Datei heißt *Dd_DotNetCoreWinSvrHosting__\<Zeitstempel > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Dateispeicherorte für Module, Schema und Konfiguration

### <a name="module"></a>Modul

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfiguration

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Die Basisdateien befinden durch Suchen nach *aspnetcore.dll* in der *"applicationHost.config"* Datei. Für IIS Express die *"applicationHost.config"* Datei wird nicht standardmäßig vorhanden. Die Datei wird erstellt am  *\<Application_root >\\VS\\Config* beim Starten alle Web-app-Projekt in Visual Studio-Projektmappe.
