---
title: Problembehandlung bei ASP.NET Core unter IIS
author: guardrex
description: Informationen Sie zur diagnose von Problemen mit IIS-Bereitstellungen von ASP.NET Core-apps.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2e42d5904e2be8c031c5c6d4bbc104a251b699f2
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Problembehandlung bei ASP.NET Core unter IIS

Von [Luke Latham](https://github.com/guardrex)

Diagnose von Problemen mit IIS-Bereitstellungen:

* Untersuchen Sie die Browserausgabe.
* Überprüfen Sie das **Anwendungsprotokoll** des Systems über die **Ereignisanzeige**.
* Aktivieren Sie die `stdout`-Protokollierung. Das Protokoll des **ASP.NET Core-Moduls** befindet sich im vom Attribut *stdoutLogFile* des Elements `<aspNetCore>` in *web.config* angegebenen Pfad. Alle Ordner im Pfad, der im Attributwert angegeben ist, müssen in der Bereitstellung vorhanden sein. Legen Sie *StdoutLogEnabled* auf `true`. Apps, die die der `Microsoft.NET.Sdk.Web` SDK zum Erstellen der *"Web.config"* Datei standardmäßig die *StdoutLogEnabled* auf `false`manuell Bereitstellen der *"Web.config"* -Datei ein, oder ändern Sie die Datei, damit möglichen `stdout` Protokollierung.

Verwenden Sie die Informationen aus diesen drei Quellen mit der [häufige Fehler verweisen Thema](xref:host-and-deploy/azure-iis-errors-reference) um das Problem zu ermitteln. Führen Sie die Hinweise zur Fehlerbehebung bereitgestellt, um das Problem zu beheben.

Einige der häufigen Fehler werden erst im Browser, Anwendungsprotokoll und ASP.NET Core-Modulprotokoll angezeigt, wenn die Werte für *startupTimeLimit* (Standardwert: 120 Sekunden) und *startupRetryCount* (Standardwert: 2) für das Modul überschritten wurden. Warten Sie daher sechs Minuten, bevor Sie davon ausgehen, dass das Modul keinen Prozess für die App gestartet hat.

Eine schnelle Möglichkeit, um festzustellen, ob die App ordnungsgemäß funktioniert, ist die direkte Ausführung der App auf Kestrel. Wenn die app als veröffentlicht wurde eine [Framework abhängiges Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd), führen Sie `dotnet <assembly_name>.dll` im Bereitstellungsordner, also der physische Pfad der IIS für die app. Wenn die app als veröffentlicht wurde eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd)führen die app des ausführbaren direkt über eine Eingabeaufforderung `<assembly_name>.exe`, in den Bereitstellungsordner. Wenn der Standardport 5000 Kestrel gelauscht wird, sollte die app am verfügbar sein `http://localhost:5000/`. Wenn die app normalerweise an die Endpunktadresse Kestrel antwortet, wird das Problem wahrscheinlich Zusammenhang mit der reverse-Proxy-Konfiguration und weniger wahrscheinlich innerhalb der app.

Eine Möglichkeit, um festzustellen, ob die reverse-Proxy ordnungsgemäß funktioniert ist zum Ausführen einer einfachen statische Datei-Anforderung für ein Stylesheet, Skript oder Image aus der app statische Dateien in *"Wwwroot"* mit [Middleware für statische Dateien](xref:fundamentals/static-files). Wenn die app statische Dateien dienen kann jedoch MVC-Ansichten und andere Endpunkte, schlagen fehl, ist das Problem wahrscheinlich weniger verwandt der reverse-Proxy-Konfiguration und wahrscheinlicher innerhalb der app (z. B. MVC-routing oder 500 Internal Server Error).

Wenn Kestrel normalerweise hinter IIS beginnt, aber die app wird nicht nach der Ausführung erfolgreich lokal auf dem System ausgeführt, eine Umgebungsvariable kann vorübergehend hinzugefügt *"Web.config"* festzulegende der `ASPNETCORE_ENVIRONMENT` auf `Development`. Solange überschreiben die Umgebung in app-Starts ist nicht die Umgebungsvariable festlegen kann die [Developer Ausnahmeseite](xref:fundamentals/error-handling) angezeigt werden Wenn die app ausgeführt wird. Festlegen der Umgebungsvariablen `ASPNETCORE_ENVIRONMENT` auf diese Weise wird nur empfohlen für Staging/Testzwecke Servern, die verfügbar gemacht, sind nicht mit dem Internet. Achten Sie darauf, dass Sie so entfernen Sie die Umgebungsvariable aus der *"Web.config"* nach Abschluss der Datei. Informationen zum Festlegen von Umgebungsvariablen über *"Web.config"*, finden Sie unter [EnvironmentVariables untergeordnetes Element des AspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

In den meisten Fällen hilft das Aktivieren der Anwendungsprotokollierung bei der Behandlung von Problemen mit der App oder dem Reverseproxy. Weitere Informationen finden Sie unter [Protokollierung](xref:fundamentals/logging/index).

Die letzte Tipps zur Problembehandlung beziehen sich auf apps, die nicht ausgeführt werden, nach dem Upgrade entweder .NET Core SDK in den Development-Versionen für Computer oder ein Paket innerhalb der app. In einigen Fällen können inkohärente Pakete eine App beschädigen, wenn größere Upgrades durchgeführt werden. Die meisten dieser Probleme können durch behoben werden:

* Durch das Löschen der `bin` und `obj` Ordner im Projekt.
* Clearing Paket am zwischenspeichert `%UserProfile%\.nuget\packages\` und `%LocalAppData%\Nuget\v3-cache`.
* Wiederherstellen und das Projekt neu zu erstellen.
* Bestätigen, dass die vorherige Bereitstellung auf dem Server vor dem erneuten Bereitstellen der app vollständig gelöscht wurde.

> [!TIP]
> Eine einfache Möglichkeit zum leeren von Caches des Pakets wird auszuführende `dotnet nuget locals all --clear` über eine Eingabeaufforderung.
> 
> Löschen von Zwischenspeichern von Paket kann auch erfolgen mithilfe der [nuget.exe](https://www.nuget.org/downloads) Tool und die Ausführung des Befehls `nuget locals all -clear`. *NuGet.exe* , ist eine gebündelte Installation mit Windows 10 und muss separat von der NuGet-Website abgerufen werden.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
