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
ms.openlocfilehash: 9ec635b596520e3d19f4040758dd59c1cbca97f4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Verzeichnisstruktur der veröffentlichten ASP.NET Core-apps

Durch [Luke Latham](https://github.com/GuardRex)

In ASP.NET Core, das Anwendungsverzeichnis *veröffentlichen*, Anwendungsdateien, Konfigurationsdateien, statische Bestand, Pakete und die Common Language Runtime (für eigenständige apps) besteht. Dies ist die gleiche Verzeichnisstruktur wie in früheren Versionen von ASP.NET verwenden, in dem die gesamte Anwendung in das Stammverzeichnis der Webserver aktiv ist.

| App-Typ | Verzeichnisstruktur |
| --- | --- |
| Framework-abhängige-Bereitstellung | <ul><li>Veröffentlichen\*<ul><li>Protokolle\* (falls in PublishOptions eingeschlossen)</li><li>Refs\*</li><li>Laufzeiten\*</li><li>Sichten\* (falls in PublishOptions eingeschlossen)</li><li>"Wwwroot"\* (falls in PublishOptions eingeschlossen)</li><li>DLL-Datei</li><li>MyApp.deps.JSON</li><li>MyApp.dll</li><li>MyApp.PDB</li><li>"MyApp". PrecompiledViews.dll (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>"MyApp". PrecompiledViews.pdb (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>MyApp.runtimeconfig.JSON</li><li>"Web.config" (falls in PublishOptions eingeschlossen)</li></ul></li></ul> |
| Eigenständige Bereitstellung | <ul><li>Veröffentlichen\*<ul><li>Protokolle\* (falls in PublishOptions eingeschlossen)</li><li>Refs\*</li><li>Sichten\* (falls in PublishOptions eingeschlossen)</li><li>"Wwwroot"\* (falls in PublishOptions eingeschlossen)</li><li>DLL-Datei</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.PDB</li><li>"MyApp". PrecompiledViews.dll (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>"MyApp". PrecompiledViews.pdb (wenn es sich um eine Razor-Ansichten Vorkompilieren von)</li><li>MyApp.runtimeconfig.JSON</li><li>"Web.config" (falls in PublishOptions eingeschlossen)</li></ul></li></ul> |
\*Gibt ein Verzeichnis an

Den Inhalt der *veröffentlichen* Verzeichnis darstellt der *Content Stammpfad*auch Namens der *Anwendungsbasispfads*, der Bereitstellung. Auf einen beliebigen Namen gewährt wird die *veröffentlichen* Verzeichnis in der Bereitstellung am Speicherort mit dem physischen Pfad des Servers für die gehostete Anwendung dient. Die *"Wwwroot"* Verzeichnis, falls vorhanden, enthält nur statische Objekte. Die *Protokolle* Verzeichnis in der Bereitstellung enthalten sein kann, indem es in das Projekt erstellen und Hinzufügen der `<Target>` folgenden Element Ihre *csproj* Datei oder beim Erstellen des Verzeichnisses physisch auf den Server.

```xml
<Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
  <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
  <MakeDir Directories="$(PublishUrl)Logs" Condition="!Exists('$(PublishUrl)Logs')" />
</Target>
```

Die erste `<MakeDir>` -Element, das verwendet die `PublishDir` -Eigenschaft wird von der .NET Core-CLI verwendet, um zu bestimmen, das der Zielort für den Veröffentlichungsvorgang. Die zweite `<MakeDir>` -Element, das verwendet die `PublishUrl` -Eigenschaft wird von Visual Studio verwendet, um den Zielspeicherort zu bestimmen. Visual Studio verwendet die `PublishUrl` Eigenschaft für die Kompatibilität mit nicht - .NET Core-Projekte.

Das Bereitstellungsverzeichnis erfordert Lese-/Execute-Berechtigungen, während die *Protokolle* Directory erfordert Lese-/Schreibberechtigungen. Weitere Verzeichnisse, in denen Objekte geschrieben wird, erfordert Lese-/Schreibberechtigungen.
