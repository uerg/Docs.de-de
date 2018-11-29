---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 5b5384b0bfa933f40f82513b02f7a14367fbef76
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549088"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutorial: Erste Schritte mit ASP.NET Core

Dieses Tutorial zeigt, wie die .NET Core-Befehlszeilenschnittstelle verwendet wird, um eine ASP.NET Core-Web-App zu erstellen. Sie lernen, die folgende Aufgaben auszuführen:

> [!div class="checklist"]
> * Erstellen Sie ein Web-App-Projekt.
> * Aktivieren Sie lokales HTTPS.
> * Führen Sie die App aus.
> * Bearbeiten Sie eine Razor-Seite.

Am Schluss werden Sie eine funktionierende Web-App auf Ihrem lokalen Computer besitzen.

![Web-App-Startseite](_static/home-page.png)


## <a name="prerequisites"></a>Erforderliche Komponenten

* Installieren Sie [!INCLUDE [](~/includes/2.1-SDK.md)].

## <a name="create-a-web-app-project"></a>Erstellen Sie ein Web-App-Projekt.

* Öffnen Sie eine Befehlsshell, und geben Sie den folgenden Befehl ein:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a>Aktivieren Sie lokales HTTPS.

* Vertrauen Sie dem HTTPS-Entwicklungszertifikat:

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

  *Trusting the HTTPS development certificate was requested. Wenn das Zertifikat nicht bereits vertrauenswürdig, ist führen wir den folgenden Befehl aus:*  `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
  *Dieser Befehl fordert Sie möglicherweise zur Eingabe Ihres Kennworts auf, um das Zertifikat für die Systemkeychain zu installieren.
  
  Kennwort:*

  Geben Sie Ihr Kennwort ein, wenn Sie die Vertrauenswürdigkeit des Entwicklungszertifikats bestätigen möchten.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  Weitere Informationen zum Bestätigen der Vertrauenswürdigkeit eines HTTPS-Entwicklungszertifikats finden Sie in der Dokumentation zu Ihrer Linux-Distribution.
   
---

## <a name="run-the-app"></a>Ausführen der App

* Führen Sie die folgenden Befehle aus:

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* Wechseln Sie zu [https://localhost:5001](https://localhost:5001). Klicken Sie auf **Accept** (Akzeptieren), um die Datenschutz- und Cookierichtlinie zu akzeptieren. Diese App bewahrt keine personenbezogenen Informationen auf.

## <a name="edit-a-razor-page"></a>Bearbeiten einer Razor-Seite

* Öffnen Sie *Pages/About.cshtml*, und modifizieren Sie die Seite mit dem folgenden hervorgehobenen Markup:

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* Navigieren Sie zu [https://localhost:5001/About](https://localhost:5001/About), und überprüfen Sie, ob die Änderungen angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen Sie ein Web-App-Projekt.
> * Aktivieren Sie lokales HTTPS.
> * Führen Sie das Projekt aus.
> * Führen Sie eine Änderung durch.

Um mehr über ASP.NET Core zu lernen, lesen Sie die Einleitung:

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.  Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).