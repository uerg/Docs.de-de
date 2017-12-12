---
title: ASP.NET Core-Verzeichnisstruktur
author: guardrex
description: "Die Verzeichnisstruktur veröffentlichter ASP.NET Core-Anwendungen."
keywords: ASP.NET Core, Verzeichnisstruktur
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Verzeichnisstruktur der veröffentlichten ASP.NET Core-apps

Von [Luke Latham](https://github.com/guardrex)

In ASP.NET Core, das Anwendungsverzeichnis *veröffentlichen*, Anwendungsdateien, Konfigurationsdateien, statische Bestand, Pakete und die Common Language Runtime (für eigenständige apps) besteht. Dies ist die gleiche Verzeichnisstruktur wie in früheren Versionen von ASP.NET verwenden, in dem die gesamte Anwendung in das Stammverzeichnis der Webserver aktiv ist.

| App-Typ | Verzeichnisstruktur |
| --- | --- |
| Framework-abhängige-Bereitstellung | <ul><li>Veröffentlichen\*<ul><li>Protokolle\* (falls in PublishOptions eingeschlossen)</li><li>Refs\*</li><li>Laufzeiten\*</li><li>Sichten\* (falls in PublishOptions eingeschlossen)</li><li>"Wwwroot"\* (falls in PublishOptions eingeschlossen)</li><li>DLL-Datei</li><li>MyApp.deps.JSON</li><li>MyApp.dll</li><li>MyApp.PDB</li><li>"MyApp". PrecompiledViews.dll (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>"MyApp". PrecompiledViews.pdb (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>MyApp.runtimeconfig.JSON</li><li>"Web.config" (falls in PublishOptions eingeschlossen)</li></ul></li></ul> |
| Eigenständige Bereitstellung | <ul><li>Veröffentlichen\*<ul><li>Protokolle\* (falls in PublishOptions eingeschlossen)</li><li>Refs\*</li><li>Sichten\* (falls in PublishOptions eingeschlossen)</li><li>"Wwwroot"\* (falls in PublishOptions eingeschlossen)</li><li>DLL-Datei</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.PDB</li><li>"MyApp". PrecompiledViews.dll (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>"MyApp". PrecompiledViews.pdb (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>MyApp.runtimeconfig.JSON</li><li>"Web.config" (falls in PublishOptions eingeschlossen)</li></ul></li></ul> |
\*Gibt ein Verzeichnis an

Den Inhalt der *veröffentlichen* Verzeichnis darstellt der *Content Stammpfad*auch Namens der *Anwendungsbasispfads*, der Bereitstellung. Auf einen beliebigen Namen gewährt wird die *veröffentlichen* Verzeichnis in der Bereitstellung am Speicherort mit dem physischen Pfad des Servers für die gehostete Anwendung dient. Die *"Wwwroot"* Verzeichnis, falls vorhanden, enthält nur statische Objekte. Die *Protokolle* Verzeichnis in der Bereitstellung enthalten sein kann, indem es in das Projekt erstellen und Hinzufügen der `<Target>` folgenden Element Ihre *csproj* Datei oder beim Erstellen des Verzeichnisses physisch auf den Server.

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

Das Bereitstellungsverzeichnis erfordert Lese-/Execute-Berechtigungen, während die *Protokolle* Directory erfordert Lese-/Schreibberechtigungen. Weitere Verzeichnisse, in denen Objekte geschrieben wird, erfordert Lese-/Schreibberechtigungen.
