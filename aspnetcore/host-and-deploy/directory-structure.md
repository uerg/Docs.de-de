---
title: Verzeichnisstruktur für ASP.NET Core
author: guardrex
description: Erfahren Sie mehr über die Verzeichnisstruktur veröffentlichter ASP.NET Core-Apps.
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 06d3f097cd93ceb2a23b9f6516a9b7a1f3ca3089
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273672"
---
# <a name="aspnet-core-directory-structure"></a>Verzeichnisstruktur für ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

In ASP.NET Core besteht das veröffentlichte Anwendungsverzeichnis, *publish*, aus Anwendungsdateien, Konfigurationsdateien, statischen Objekten, Paketen und der Runtime (bei [eigenständigen Bereitstellungen](/dotnet/core/deploying/#self-contained-deployments-scd)).


| App-Typ | Verzeichnisstruktur |
| -------- | ------------------- |
| [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (optional, es sei denn, es müssen stdout-Protokolle empfangen werden)</li><li>Ansichten&dagger; (MVC-Apps, wenn Ansichten nicht vorkompiliert sind)</li><li>Seiten&dagger; (MVC- oder Razor Pages-Apps, wenn Seiten nicht vorkompiliert sind)</li><li>wwwroot&dagger;</li><li>*\.DLL-Dateien</li><li>\<assembly-name>.deps.json</li><li>\<assembly-name>.dll</li><li>\<assembly-name>.pdb</li><li>\<assembly-name>.PrecompiledViews.dll</li><li>\<assembly-name>.PrecompiledViews.pdb</li><li>\<assembly-name>.runtimeconfig.json</li><li>web.config (IIS-Bereitstellungen)</li></ul></li></ul> |
| [Eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (optional, es sei denn, es müssen stdout-Protokolle empfangen werden)</li><li>refs&dagger;</li><li>Ansichten&dagger; (MVC-Apps, wenn Ansichten nicht vorkompiliert sind)</li><li>Seiten&dagger; (MVC- oder Razor Pages-Apps, wenn Seiten nicht vorkompiliert sind)</li><li>wwwroot&dagger;</li><li>\*DLL-Dateien</li><li>\<assembly-name>.deps.json</li><li>\<assembly-name>.exe</li><li>\<assembly-name>.pdb</li><li>\<assembly-name>.PrecompiledViews.dll</li><li>\<assembly-name>.PrecompiledViews.pdb</li><li>\<assembly-name>.runtimeconfig.json</li><li>web.config (IIS-Bereitstellungen)</li></ul></li></ul> |

&dagger;Gibt ein Verzeichnis an

Das Verzeichnis *publish* stellt den *Pfad des Inhaltsstammverzeichnisses* (auch als *Pfad der Anwendungsbasis* bekannt) der Bereitstellung dar. Unabhängig vom Namen des Verzeichnisses *publish* der auf dem Server bereitgestellten App dient der Speicherort des Verzeichnisses als physischer Pfad des Servers für die gehostete App.

Das Verzeichnis *wwwroot* enthält, sofern vorhanden, nur statische Objekte.

Das stdout-Verzeichnis *logs* kann mit einem der folgenden beiden Ansätze für die Bereitstellung erstellt werden:

* Fügen Sie das folgende `<Target>`-Element zur Projektdatei hinzu:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   Mit dem `<MakeDir>`-Element wird in der veröffentlichten Ausgabe ein leerer *Logs*-Ordner erstellt. Das Element bestimmt mithilfe der Eigenschaft `PublishDir` den Zielspeicherort für die Erstellung des Ordners. Mehrere Bereitstellungsmethoden, wie z.B. Web Deploy, überspringen leere Ordner während der Bereitstellung. Das Element `<WriteLinesToFile>` generiert im Ordner *Logs* eine Datei, die eine Bereitstellung des Ordners auf dem Server garantiert. Beachten Sie, dass die Ordnererstellung weiterhin fehlschlagen kann, wenn der Workerprozess keinen Schreibzugriff auf den Zielordner hat.

* Erstellen Sie das Verzeichnis *Logs* physisch auf dem Server in der Bereitstellung.

Für das Bereitstellungsverzeichnis sind Lese-/Ausführungsberechtigungen erforderlich. Für das Verzeichnis *Logs* sind Lese-/Schreibberechtigungen erforderlich. Für weitere Verzeichnisse, in die Dateien geschrieben werden, sind Lese-/Schreibberechtigungen erforderlich.
