---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Erste Schritte mit ASP.NET Core

> [!NOTE]
> Diese Anweisungen gelten für die neueste Version von ASP.NET Core. Die Version 1.1. dieses Dokuments finden Sie unter [Erste Schritte mit ASP.NET Core 1.1](xref:getting-started-1.1).

1. Installieren Sie [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Erstellen Sie ein neues .NET Core-Projekt.

   Öffnen Sie unter macOS und Linux ein Terminalfenster. Öffnen Sie unter Windows eine Eingabeaufforderung. Geben Sie folgenden Befehl ein:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Führen Sie die App aus.

    Verwenden Sie die folgenden Befehle, um die App auszuführen:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Wechseln Sie zu [http://localhost:5000](http://localhost:5000).

5. Öffnen Sie <em>Pages/About.cshtml</em>, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt. Die Zeit auf dem Server beträgt @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Wechseln Sie zu [http://localhost:5000/About](http://localhost:5000/About), und bestätigen Sie die Änderungen.

### <a name="next-steps"></a>Nächste Schritte

Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).

Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).

Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden. Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
