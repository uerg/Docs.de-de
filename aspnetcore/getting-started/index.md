---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228581"
---
# <a name="get-started-with-aspnet-core"></a>Erste Schritte mit ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Installieren Sie [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Erstellen Sie ein ASP.NET Core-Projekt. Öffnen Sie eine Befehlsshell, und geben Sie den folgenden Befehl ein:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. Vertrauen Sie dem HTTPS-Entwicklungszertifikat:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   Über den vorherigen Befehl wird der folgende Dialog angezeigt:

   ![Dialogfeld „Sicherheitswarnung“](_static/cert.png)

   Klicken Sie auf **Ja**, wenn Sie zustimmen möchten, dass das Entwicklungszertifikat vertrauenswürdig ist.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   Über den vorherigen Befehl wird die folgende Meldung angezeigt:

   *Trusting the HTTPS development certificate was requested. Sie müssen bestätigen, dass Sie dem HTTPS-Entwicklungszertifikat vertrauen. Wenn Sie dies noch nicht bestätigt haben, wird der folgende Befehl ausgeführt:*  `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Sie werden dann möglicherweise aufgefordert, Ihr Kennwort einzugeben, um das Zertifikat auf der Keychain des Systems zu installieren    Kennwort:*

   Geben Sie Ihr Kennwort ein, wenn Sie die Vertrauenswürdigkeit des Entwicklungszertifikats bestätigen möchten.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a>Weitere Informationen zum Bestätigen der Vertrauenswürdigkeit eines HTTPS-Entwicklungszertifikats finden Sie in der Dokumentation zu Ihrer Linux-Distribution.
---

4. Führen Sie die App aus:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Wechseln Sie zu [http://localhost:5001](http://localhost:5001).  Klicken Sie auf **Accept** (Akzeptieren), um die Datenschutz- und Cookierichtlinie zu akzeptieren. Diese App bewahrt keine personenbezogenen Informationen auf.

6. Öffnen Sie *Pages/About.cshtml*, und modifizieren Sie die Seite mit dem folgenden hervorgehobenen Markup:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Navigieren Sie zu [http://localhost:5001/About](http://localhost:5001/About), und überprüfen Sie, ob die Änderungen angezeigt werden.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Installieren Sie [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Erstellen Sie ein neues ASP.NET Core-Projekt.

   Öffnen Sie eine Befehlsshell. Geben Sie folgenden Befehl ein:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Führen Sie die App mit den folgenden Befehlen aus:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Wechseln Sie zu [http://localhost:5000](http://localhost:5000).

5. Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt. Die Zeit auf dem Server ist @DateTime.Now" :

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Wechseln Sie zu [http://localhost:5000/About](http://localhost:5000/About), und bestätigen Sie die Änderungen.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der .NET Core-Seite [Alle Downloads](https://www.microsoft.com/net/download/all).

2. Erstellen Sie einen Ordner für das neue ASP.NET Core-Projekt.

   Öffnen Sie eine Befehlsshell. Geben Sie die folgenden Befehle ein:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Erstellen Sie ein neues ASP.NET Core-Projekt.

   ```console
   dotnet new web
   ```

5. Stellen Sie die Pakete wieder her.

    ```console
    dotnet restore
    ```

6. Führen Sie die App aus.

   ```console
   dotnet run
   ```

   Mit dem Befehl [dotnet run](/dotnet/core/tools/dotnet-run) wird die App bei Bedarf zunächst erstellt.

7. Wechseln Sie zu `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
