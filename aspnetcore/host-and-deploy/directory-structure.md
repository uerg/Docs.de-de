---
title: ASP.NET Core-Verzeichnisstruktur
author: guardrex
description: "Finden Sie die Verzeichnisstruktur veröffentlichter ASP.NET Core-Anwendungen."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 55e1e0dac32609446243098dbb4a4373f06b4212
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Verzeichnisstruktur der veröffentlichten ASP.NET Core-apps

Von [Luke Latham](https://github.com/guardrex)

In ASP.NET Core, das Anwendungsverzeichnis *veröffentlichen*, Anwendungsdateien, Konfigurationsdateien, statische Bestand, Pakete und die Common Language Runtime (für eigenständige apps) besteht.

| App-Typ                       | Verzeichnisstruktur |
| ------------------------------ | ------------------- |
| Framework-abhängige-Bereitstellung | <ul><li>Veröffentlichen\*<ul><li>Protokolle\* (falls in PublishOptions eingeschlossen)</li><li>refs\*</li><li>Laufzeiten\*</li><li>Sichten\* (falls in PublishOptions eingeschlossen)</li><li>"Wwwroot"\* (falls in PublishOptions eingeschlossen)</li><li>DLL-Datei</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>"MyApp". PrecompiledViews.dll (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>myapp.PrecompiledViews.pdb (if precompiling Razor Views)</li><li>myapp.runtimeconfig.json</li><li>"Web.config" (falls in PublishOptions eingeschlossen)</li></ul></li></ul> |
| Eigenständige Bereitstellung      | <ul><li>Veröffentlichen\*<ul><li>Protokolle\* (falls in PublishOptions eingeschlossen)</li><li>refs\*</li><li>Sichten\* (falls in PublishOptions eingeschlossen)</li><li>"Wwwroot"\* (falls in PublishOptions eingeschlossen)</li><li>DLL-Datei</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>"MyApp". PrecompiledViews.dll (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>myapp.PrecompiledViews.pdb (if precompiling Razor Views)</li><li>myapp.runtimeconfig.json</li><li>"Web.config" (falls in PublishOptions eingeschlossen)</li></ul></li></ul> |
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
