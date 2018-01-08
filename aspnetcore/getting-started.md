---
title: Erste Schritte mit ASP.NET Core 2.0
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird."
keywords: ASP.NET Core, Tutorial, erste Schritte
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5b8c9f770e749c13bc562f157b4ebfd25a88a4e0
ms.sourcegitcommit: 019e5a0342fd49a94056d14fc7a1a1d0f81d2a39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-aspnet-core"></a>Erste Schritte mit ASP.NET Core

> [!NOTE]
> Diese Anweisungen gelten für die neueste Version von ASP.NET Core. Suchen Sie zum Einstieg eine frühere Version? Siehe die [Version 1.1 dieses Tutorials](xref:getting-started-1.1).

1. Installieren Sie [.NET Core](https://www.microsoft.com/net/core/).

2. Erstellen Sie ein neues .NET Core-Projekt.

   Öffnen Sie unter macOS und Linux ein Terminalfenster. Öffnen Sie unter Windows eine Eingabeaufforderung. Geben Sie folgenden Befehl ein:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Führen Sie die App aus.

    Verwenden Sie die folgenden Befehle, um die App auszuführen:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Wechseln Sie dann zu [http://localhost:5000](http://localhost:5000).

6. Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt. Die Zeit auf dem Server beträgt @DateTime.Now ":

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Navigieren Sie zu [http://localhost:5000/About](http://localhost:5000/About), und überprüfen Sie die Änderungen.

### <a name="next-steps"></a>Nächste Schritte

Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).

Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).

Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden. Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
