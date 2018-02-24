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
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="e823c-103">ASP.NET Core-Modul Konfigurationsverweis</span><span class="sxs-lookup"><span data-stu-id="e823c-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="e823c-104">Durch [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), und [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="e823c-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="e823c-105">Dieses Dokument enthält Anweisungen zum Konfigurieren der ASP.NET Core-Modul zum Hosten von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="e823c-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="e823c-106">Eine Einführung in die ASP.NET Core-Modul und installationsanweisungen, finden Sie unter der [ASP.NET Core Modulübersicht](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e823c-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="e823c-107">Mit der Datei "Web.config"</span><span class="sxs-lookup"><span data-stu-id="e823c-107">Configuration with web.config</span></span>

<span data-ttu-id="e823c-108">ASP.NET Core-Modul ist konfiguriert, mit der `aspNetCore` Teil der `system.webServer` Knoten am Standort *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e823c-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="e823c-109">Die folgenden *"Web.config"* Datei veröffentlicht wird, für eine [Framework abhängiges Bereitstellung](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) und konfiguriert den ASP.NET Core-Modul um Website-Anforderungen zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="e823c-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="e823c-110">Die folgenden *"Web.config"* netzwerkschutzentwurf für eine [eigenständige Bereitstellung](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="e823c-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="e823c-111">Wenn eine app bereitgestellt wird, um [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` -Pfad folgendermaßen festgelegt `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="e823c-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="e823c-112">Der Pfad speichert "stdout" die Protokolle, um die *LogFiles* Ordner, einen Speicherort automatisch vom Dienst erstellt.</span><span class="sxs-lookup"><span data-stu-id="e823c-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="e823c-113">Finden Sie unter [untergeordnete Anwendung Konfiguration](xref:host-and-deploy/iis/index#sub-application-configuration) für ein wichtiger Hinweis bezieht sich auf die Konfiguration des *"Web.config"* Dateien im Unterordner apps.</span><span class="sxs-lookup"><span data-stu-id="e823c-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="e823c-114">Attribute des-Elements aspNetCore</span><span class="sxs-lookup"><span data-stu-id="e823c-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="e823c-115">Attribut</span><span class="sxs-lookup"><span data-stu-id="e823c-115">Attribute</span></span> | <span data-ttu-id="e823c-116">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e823c-116">Description</span></span> | <span data-ttu-id="e823c-117">Standard</span><span class="sxs-lookup"><span data-stu-id="e823c-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="e823c-118">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-118">Optional string attribute.</span></span></p><p><span data-ttu-id="e823c-119">Argumente für die ausführbare Datei im angegebenen **ProcessPath**.</span><span class="sxs-lookup"><span data-stu-id="e823c-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="e823c-120">True oder False.</span><span class="sxs-lookup"><span data-stu-id="e823c-120">true or false.</span></span></p><p><span data-ttu-id="e823c-121">Bei "true", die **502.5 - Prozessfehler** Seite unterdrückt wird, und die Status 502-Codepage konfiguriert wird, der *"Web.config"* hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="e823c-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="e823c-122">True oder False.</span><span class="sxs-lookup"><span data-stu-id="e823c-122">true or false.</span></span></p><p><span data-ttu-id="e823c-123">Bei "true", wird das Token an den untergeordneten Prozess gelauscht. "% ASPNETCORE_PORT" als Header "MS-ASPNETCORE-WINAUTHTOKEN" pro Anforderung weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="e823c-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="e823c-124">Es liegt die Verantwortung des entsprechenden Prozesses CloseHandle dieses Token pro Anforderung aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e823c-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="e823c-125">Erforderliches Zeichenfolgenattribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-125">Required string attribute.</span></span></p><p><span data-ttu-id="e823c-126">Pfad zur ausführbaren Datei, die einen HTTP-Anforderungen Lauschen Prozess startet.</span><span class="sxs-lookup"><span data-stu-id="e823c-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="e823c-127">Relative Pfade werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e823c-127">Relative paths are supported.</span></span> <span data-ttu-id="e823c-128">Wenn der Pfad mit beginnt `.`, gilt der Pfad relativ zum Stammverzeichnis Website sein.</span><span class="sxs-lookup"><span data-stu-id="e823c-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="e823c-129">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="e823c-130">Gibt die Anzahl der Wiederholungsversuche, die vom Prozess angegeben wird, in **ProcessPath** zum Absturz (Crash) pro Minute zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="e823c-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="e823c-131">Wenn dieses Limit überschritten wird, beendet das Modul mit dem Starten des Prozesses für den Rest der Minute.</span><span class="sxs-lookup"><span data-stu-id="e823c-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="e823c-132">Optionales Timespan-Attribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="e823c-133">Gibt die Dauer für die ASP.NET Core-Modul auf eine Antwort vom Prozess "% ASPNETCORE_PORT" lauscht wartet.</span><span class="sxs-lookup"><span data-stu-id="e823c-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="e823c-134">Die `requestTimeout` muss angegeben werden in ganzen Minuten nur, andernfalls wird standardmäßig auf 2 Minuten.</span><span class="sxs-lookup"><span data-stu-id="e823c-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="e823c-135">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="e823c-136">Dauer in Sekunden, die das Modul für die ausführbare Datei ordnungsgemäß schließen wartet, wenn die *app_offline.htm* Datei erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="e823c-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="e823c-137">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="e823c-138">Dauer in Sekunden an, denen das Modul wartet, bis die ausführbare Datei Starten eines Prozesses, der Port überwacht.</span><span class="sxs-lookup"><span data-stu-id="e823c-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="e823c-139">Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess an.</span><span class="sxs-lookup"><span data-stu-id="e823c-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="e823c-140">Das Modul versucht, den Prozess neu starten, wenn eine neue Anforderung erhält und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu starten, es sei denn, die app nicht gestartet **RapidFailsPerMinute** -Mal in den letzten Paralleles Minute an.</span><span class="sxs-lookup"><span data-stu-id="e823c-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="e823c-141">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e823c-142">Bei "true", **"stdout"** und **"stderr"** für den Prozess, der im angegebenen **ProcessPath** an die angegebene Datei umgeleitet werden **"stdoutlogfile"**.</span><span class="sxs-lookup"><span data-stu-id="e823c-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="e823c-143">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-143">Optional string attribute.</span></span></p><p><span data-ttu-id="e823c-144">Gibt an, der relative oder absolute Pfad für die **"stdout"** und **"stderr"** aus dem Prozess, der im angegebenen **ProcessPath** werden protokolliert.</span><span class="sxs-lookup"><span data-stu-id="e823c-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="e823c-145">Relative Pfade sind relativ zum Stamm des Standorts.</span><span class="sxs-lookup"><span data-stu-id="e823c-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="e823c-146">Jeder Pfad beginnend mit `.` sind relativ zur Website Stamm und alle anderen Pfade als absolute Pfade behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="e823c-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="e823c-147">Alle Ordner im Pfad angegeben, müssen in der Reihenfolge für das Modul zum Erstellen der Protokolldatei vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="e823c-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="e823c-148">Mithilfe des Unterstrich Trennzeichen, Timestamp, Prozess-ID, und die Erweiterung (*.log*) hinzukommen bis zum letzten Segment der **"stdoutlogfile"** Pfad.</span><span class="sxs-lookup"><span data-stu-id="e823c-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="e823c-149">Wenn `.\logs\stdout` angegeben wird als ein Wert wird als ein Beispiel für "stdout" Protokoll gespeichert *stdout_20180205194132_1934.log* in der *Protokolle* Ordner, wenn auf 2/5/2018 um 19:41:32 mit 1934 Prozess-ID gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e823c-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="e823c-150">Festlegen von Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="e823c-150">Setting environment variables</span></span>

<span data-ttu-id="e823c-151">Umgebungsvariablen können angegeben werden, für den Prozess in den `processPath` Attribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="e823c-152">Geben Sie eine Umgebungsvariable mit dem `environmentVariable` untergeordnetes Element von einer `environmentVariables` Elements der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="e823c-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="e823c-153">In diesem Abschnitt festgelegten Umgebungsvariablen haben Vorrang gegenüber System Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="e823c-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="e823c-154">Im folgenden Beispiel wird zwei Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="e823c-154">The following example sets two environment variables.</span></span> <span data-ttu-id="e823c-155">`ASPNETCORE_ENVIRONMENT` konfiguriert die app-Umgebung zu `Development`.</span><span class="sxs-lookup"><span data-stu-id="e823c-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="e823c-156">Ein Entwickler möglicherweise vorübergehend legen Sie diesen Wert der *"Web.config"* Datei, um zu erzwingen der [Developer Ausnahmeseite](xref:fundamentals/error-handling) laden, wenn eine Ausnahme für die app zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="e823c-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="e823c-157">`CONFIG_DIR` ist ein Beispiel für eine benutzerdefinierte Umgebungsvariable, in dem der Entwickler Code, der den Wert beim Start geschrieben hat, um zu bilden, einen Pfad für das Laden der app-Konfigurationsdatei liest.</span><span class="sxs-lookup"><span data-stu-id="e823c-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="e823c-158">Legen Sie nur die `ASPNETCORE_ENVIRONMENT` Envirnonment-Variable, die `Development` für staging und Testen von Servern, die für nicht vertrauenswürdige Netzwerke wie z. B. das Internet zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="e823c-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="e823c-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="e823c-159">app_offline.htm</span></span>

<span data-ttu-id="e823c-160">Wenn eine Datei mit dem Namen *app_offline.htm* wird erkannt, im Stammverzeichnis einer App ASP.NET Core-Modul versucht, ordnungsgemäß heruntergefahren, die app und die Verarbeitung von eingehender Anforderungen beenden.</span><span class="sxs-lookup"><span data-stu-id="e823c-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="e823c-161">Wenn nach der Anzahl von Sekunden, die in definierten Anwendung ausgeführter `shutdownTimeLimit`, die ASP.NET Core-Modul beendet den laufenden Prozess.</span><span class="sxs-lookup"><span data-stu-id="e823c-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="e823c-162">Während der *app_offline.htm* Datei ist vorhanden, die ASP.NET Core-Modul-reagiert auf Anforderungen durch Zurücksenden der Inhalt des der *app_offline.htm* Datei.</span><span class="sxs-lookup"><span data-stu-id="e823c-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="e823c-163">Wenn die *app_offline.htm* Datei wird entfernt, die nächste Anforderung wird die Anwendung gestartet.</span><span class="sxs-lookup"><span data-stu-id="e823c-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="e823c-164">Start-Fehler (Seite)</span><span class="sxs-lookup"><span data-stu-id="e823c-164">Start-up error page</span></span>

<span data-ttu-id="e823c-165">Wenn ASP.NET Core-Modul ein Fehler auftritt, starten Sie den Back-End-Prozess oder dem Back-End-Prozess gestartet wird, aber ein Fehler auftritt, an den konfigurierten Port Lauschen eine *502.5 Prozessfehler* Code Statusseite wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e823c-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="e823c-166">Verwenden Sie zum Unterdrücken der auf dieser Seite, und stellen Sie die Standardcodepage für IIS 502 Status wieder her, die `disableStartUpErrorPage` Attribut.</span><span class="sxs-lookup"><span data-stu-id="e823c-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="e823c-167">Weitere Informationen zum Konfigurieren von benutzerdefinierten Fehlermeldungen finden Sie unter [HTTP-Fehler `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="e823c-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 Codepage Status "Fehler"-Prozess](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="e823c-169">Umleitung und protokollerstellung</span><span class="sxs-lookup"><span data-stu-id="e823c-169">Log creation and redirection</span></span>

<span data-ttu-id="e823c-170">ASP.NET Core-Modul leitet `stdout` und `stderr` Protokolle, wenn auf den Datenträger der `stdoutLogEnabled` und `stdoutLogFile` Attribute der `aspNetCore` Element festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e823c-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="e823c-171">Alle Ordner in der `stdoutLogFile` Pfad muss vorhanden sein, in der Reihenfolge für das Modul zum Erstellen der Protokolldatei.</span><span class="sxs-lookup"><span data-stu-id="e823c-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="e823c-172">Sinnvolles Zeitstempel und der Datei werden automatisch hinzugefügt, wenn die Protokolldatei erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="e823c-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="e823c-173">Protokolle werden nicht gedreht werden, es sei denn, der Prozess wiederverwendet/Neustart stattfindet.</span><span class="sxs-lookup"><span data-stu-id="e823c-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="e823c-174">Es ist die Zuständigkeit für den Hoster den Speicherplatz zu begrenzen, die, den die Protokolle zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="e823c-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="e823c-175">Mithilfe der `stdout` Protokoll wird nur für die Fehlerbehebung bei Startproblemen app empfohlen.</span><span class="sxs-lookup"><span data-stu-id="e823c-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="e823c-176">Verwenden Sie das Protokoll "stdout" nicht zur Protokollierung allgemeiner app.</span><span class="sxs-lookup"><span data-stu-id="e823c-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="e823c-177">Verwenden Sie für die routinemäßige Protokollierung in einer ASP.NET Core-app, eine Protokollierung-Bibliothek, die Protokolldateigröße beschränkt und die Protokolle dreht.</span><span class="sxs-lookup"><span data-stu-id="e823c-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e823c-178">Weitere Informationen finden Sie unter [eines Drittanbieters Protokollanbieter](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="e823c-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="e823c-179">Besteht aus der Protokolldateinamen durch Anhängen der Zeitstempel, die Prozess-ID und die Dateierweiterung (*.log*) auf das letzte Segment des der `stdoutLogFile` Pfad (in der Regel *"stdout"*) getrennt durch Unterstriche enthalten.</span><span class="sxs-lookup"><span data-stu-id="e823c-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="e823c-180">Wenn die `stdoutLogFile` Pfad endet mit *"stdout"*, ein Protokoll für eine app mit einem PID des 1934 auf 2/5/2018 erstellt werden, um 19:42:32 wurde der Dateiname *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="e823c-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="e823c-181">Im folgenden Beispiel `aspNetCore` Element konfiguriert `stdout` Protokollierung für eine app in Azure App Service gehostet.</span><span class="sxs-lookup"><span data-stu-id="e823c-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="e823c-182">Ein lokaler Pfad oder Netzwerkfreigabepfad ist akzeptabel, für die lokale Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="e823c-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="e823c-183">Vergewissern Sie sich, dass die Identität des AppPool-Benutzers über die Berechtigung zum Schreiben in den angegebenen Pfad verfügt.</span><span class="sxs-lookup"><span data-stu-id="e823c-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="e823c-184">Finden Sie unter [mit der Datei "Web.config"](#configuration-with-webconfig) ein Beispiel für die `aspNetCore` Element in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e823c-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="e823c-185">ASP.NET Core-Modul mit einer IIS freigegebene Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e823c-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="e823c-186">Das Installationsprogramm von ASP.NET Core ausgeführt wird, mit den Berechtigungen des der **SYSTEM** Konto.</span><span class="sxs-lookup"><span data-stu-id="e823c-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="e823c-187">Da das lokale Systemkonto keine Berechtigung zum Ändern für den Pfad der Bibliotheksfreigabe, in der IIS-Freigabekonfiguration, trifft der Installer "Zugriff verweigert" Fehler beim Konfigurieren der moduleinstellungen in *"applicationHost.config"* auf die Freigabe.</span><span class="sxs-lookup"><span data-stu-id="e823c-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="e823c-188">Wenn Sie eine IIS-Freigabekonfiguration verwenden zu können, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="e823c-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="e823c-189">Deaktivieren Sie die IIS-Freigabekonfiguration.</span><span class="sxs-lookup"><span data-stu-id="e823c-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="e823c-190">Führen Sie den Installer aus.</span><span class="sxs-lookup"><span data-stu-id="e823c-190">Run the installer.</span></span>
1. <span data-ttu-id="e823c-191">Exportieren Sie die aktualisierte *"applicationHost.config"* Datei auf die Freigabe.</span><span class="sxs-lookup"><span data-stu-id="e823c-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="e823c-192">Reaktivieren Sie den IIS-Freigabekonfiguration.</span><span class="sxs-lookup"><span data-stu-id="e823c-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="e823c-193">Version des Moduls und Hosten von Paket-Installer-Protokolle</span><span class="sxs-lookup"><span data-stu-id="e823c-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="e823c-194">Die Version des installierten ASP.NET Core Moduls zu ermitteln:</span><span class="sxs-lookup"><span data-stu-id="e823c-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="e823c-195">Navigieren Sie auf dem Hostsystem zu *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="e823c-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="e823c-196">Suchen Sie die *aspnetcore.dll* Datei.</span><span class="sxs-lookup"><span data-stu-id="e823c-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="e823c-197">Mit der rechten Maustaste in der das, und wählen Sie **Eigenschaften** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="e823c-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="e823c-198">Wählen Sie die **Details** Registerkarte. Die **Dateiversion** und **Produktversion** die installierte Version des Moduls darstellen.</span><span class="sxs-lookup"><span data-stu-id="e823c-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="e823c-199">Das Hosting mit Windows Server Paket-Installer-Protokolle für das Modul finden Sie unter *"c:"\\Benutzer\\%UserName%\\AppData\\lokale\\Temp*. Die Datei heißt *Dd_DotNetCoreWinSvrHosting__\<Zeitstempel > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="e823c-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="e823c-200">Dateispeicherorte für Module, Schema und Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e823c-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="e823c-201">Modul</span><span class="sxs-lookup"><span data-stu-id="e823c-201">Module</span></span>

<span data-ttu-id="e823c-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="e823c-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="e823c-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e823c-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="e823c-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e823c-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="e823c-205">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="e823c-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="e823c-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e823c-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="e823c-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e823c-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="e823c-208">Schema</span><span class="sxs-lookup"><span data-stu-id="e823c-208">Schema</span></span>

<span data-ttu-id="e823c-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e823c-209">**IIS**</span></span>

   * <span data-ttu-id="e823c-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e823c-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="e823c-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e823c-211">**IIS Express**</span></span>

   * <span data-ttu-id="e823c-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e823c-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="e823c-213">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e823c-213">Configuration</span></span>

<span data-ttu-id="e823c-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e823c-214">**IIS**</span></span>

   * <span data-ttu-id="e823c-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e823c-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="e823c-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e823c-216">**IIS Express**</span></span>

   * <span data-ttu-id="e823c-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e823c-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="e823c-218">Die Basisdateien befinden durch Suchen nach *aspnetcore.dll* in der *"applicationHost.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="e823c-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="e823c-219">Für IIS Express die *"applicationHost.config"* Datei wird nicht standardmäßig vorhanden.</span><span class="sxs-lookup"><span data-stu-id="e823c-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="e823c-220">Die Datei wird erstellt am  *\<Application_root >\\VS\\Config* beim Starten alle Web-app-Projekt in Visual Studio-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="e823c-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
