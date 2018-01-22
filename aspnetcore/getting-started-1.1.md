---
title: Erste Schritte mit ASP.NET Core 1.1
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core 1.1 erstellt und ausgeführt wird."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: 7f178618508e1a1e9c49d8ace619b9f942998dae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-11"></a>Erste Schritte mit ASP.NET Core 1.1

> [!NOTE]
> Diese Anweisungen gelten für ASP.NET Core 1.1. Suchen Sie die neueste Version? Siehe [die aktuelle Version dieses Tutorials](xref:getting-started).

1. Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der [Downloadseite für .NET Core 1.0.5 und 1.1.2 SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Erstellen Sie einen Ordner für das neue .NET Core-Projekt.

   Öffnen Sie unter macOS und Linux ein Terminalfenster. Öffnen Sie unter Windows eine Eingabeaufforderung.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Erstellen Sie ein neues .NET Core-Projekt.

   ```terminal
   dotnet new web
   ```
   
3.  Stellen Sie die Pakete wieder her.

    ```terminal
    dotnet restore
    ```

4. Führen Sie die App aus.

   Mit dem Befehl `dotnet run` wird die App bei Bedarf zunächst erstellt.

   ```terminal
   dotnet run
   ```

5. Wechseln Sie zu `http://localhost:5000`.

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Nächste Schritte

Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).

Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).

Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden. Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
