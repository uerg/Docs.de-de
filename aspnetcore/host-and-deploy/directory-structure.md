---
title: ASP.NET Core-Verzeichnisstruktur
author: guardrex
description: Erfahren Sie, bis die Verzeichnisstruktur der veröffentlichten ASP.NET Core-apps.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core-Verzeichnisstruktur

Von [Luke Latham](https://github.com/guardrex)

In ASP.NET Core, im Verzeichnis der veröffentlichten Anwendung *veröffentlichen*, besteht der Anwendungsdateien, Konfigurationsdateien, statische Bestand, Pakete und die Common Language Runtime (für [eigenständige Bereitstellungen](/dotnet/core/deploying/#self-contained-deployments-scd)).


| App-Typ | Verzeichnisstruktur |
| -------- | ------------------- |
| [Framework-abhängige-Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Veröffentlichen&dagger;<ul><li>Protokolle&dagger; (Dies ist optional, es sei denn, die zum Empfangen von Protokollen für "stdout" erforderlich)</li><li>Sichten&dagger; (MVC-apps; Wenn Ansichten vorkompiliert werden nicht)</li><li>Seiten&dagger; (Razor-Seiten oder MVC-apps; Wenn Seiten vorkompiliert werden nicht)</li><li>"Wwwroot"&dagger;</li><li>*\.DLL-Dateien</li><li>\<Assembly-Name >. deps.json</li><li>\<Assembly-Name > .dll</li><li>\<Assembly-Name > PDB-Datei</li><li>\<Assembly-Name >. PrecompiledViews.dll</li><li>\<Assembly-Name >. PrecompiledViews.pdb</li><li>\<Assembly-Name >. runtimeconfig.json</li><li>"Web.config" (IIS-Bereitstellungen)</li></ul></li></ul> |
| [Eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Veröffentlichen&dagger;<ul><li>Protokolle&dagger; (Dies ist optional, es sei denn, die zum Empfangen von Protokollen für "stdout" erforderlich)</li><li>Refs&dagger;</li><li>Sichten&dagger; (MVC-apps; Wenn Ansichten vorkompiliert werden nicht)</li><li>Seiten&dagger; (Razor-Seiten oder MVC-apps; Wenn Seiten vorkompiliert werden nicht)</li><li>"Wwwroot"&dagger;</li><li>\*DLL-Dateien</li><li>\<Assembly-Name >. deps.json</li><li>\<Assembly-Name > .exe</li><li>\<Assembly-Name > PDB-Datei</li><li>\<Assembly-Name >. PrecompiledViews.dll</li><li>\<Assembly-Name >. PrecompiledViews.pdb</li><li>\<Assembly-Name >. runtimeconfig.json</li><li>"Web.config" (IIS-Bereitstellungen)</li></ul></li></ul> |

&dagger;Gibt ein Verzeichnis an

Die *veröffentlichen* Verzeichnis darstellt der *Content Stammpfad*auch Namens der *Anwendungsbasispfads*, der Bereitstellung. Auf einen beliebigen Namen gewährt wird die *veröffentlichen* Verzeichnis die bereitgestellte app auf dem Server, dessen Speicherort dient, mit dem physischen Pfad des Servers für die gehostete app.

Die *"Wwwroot"* Verzeichnis, falls vorhanden, enthält nur statische Objekte.

Die "stdout" *Protokolle* Verzeichnis kann die Bereitstellung mithilfe einer der beiden folgenden Methoden erstellt werden:

* Fügen Sie die folgenden `<Target>` Element an der Projektdatei:

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

   Die `<MakeDir>` Element erstellt ein leeres *Protokolle* Ordner in der veröffentlichten Ausgabe. Das Element verwendet die `PublishDir` -Eigenschaft können Sie den Zielspeicherort für die Ordner erstellen, zu bestimmen. Mehrere Bereitstellungsmethoden zur Verfügung, z. B. Webbereitstellung, überspringen Sie leere Ordner während der Bereitstellung. Die `<WriteLinesToFile>` Element erzeugt eine Datei in die *Protokolle* Ordner, in dem Bereitstellung des Ordners mit dem Server gewährleistet. Beachten Sie, dass Ordner erstellen noch fehlschlagen, wenn der Arbeitsprozess Schreibzugriff in den Zielordner keine.

* Erstellen Sie physisch die *Protokolle* -Verzeichnisses auf dem Server in der Bereitstellung.

Das Bereitstellungsverzeichnis erfordert Lese-/Execute-Berechtigungen. Die *Protokolle* Directory erfordert Lese-/Schreibberechtigungen. Weitere Verzeichnisse, in Dateien geschrieben werden, erfordern Lese-/Schreibberechtigungen.
