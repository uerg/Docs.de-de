---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>Erste Schritte mit ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Installieren Sie [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Erstellen Sie ein neues .NET Core-Projekt.

   Öffnen Sie unter macOS und Linux ein Terminalfenster. Öffnen Sie unter Windows eine Eingabeaufforderung. Geben Sie folgenden Befehl ein:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Führen Sie die App mit den folgenden Befehlen aus:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Wechseln Sie zu [http://localhost:5000](http://localhost:5000).

5. Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt. Die Zeit auf dem Server ist @DateTime.Now" :

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Wechseln Sie zu [http://localhost:5000/About](http://localhost:5000/About), und bestätigen Sie die Änderungen.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der .NET Core-Seite [Alle Downloads](https://www.microsoft.com/net/download/all).

2. Erstellen Sie einen Ordner für das neue .NET Core-Projekt.

   Öffnen Sie unter macOS und Linux ein Terminalfenster. Öffnen Sie unter Windows eine Eingabeaufforderung.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Erstellen Sie ein neues .NET Core-Projekt.

   ```terminal
   dotnet new web
   ```

5. Stellen Sie die Pakete wieder her.

    ```terminal
    dotnet restore
    ```

6. Führen Sie die App aus.

   ```terminal
   dotnet run
   ```

   Mit dem Befehl [dotnet run](/dotnet/core/tools/dotnet-run) wird die App bei Bedarf zunächst erstellt.

7. Wechseln Sie zu `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end