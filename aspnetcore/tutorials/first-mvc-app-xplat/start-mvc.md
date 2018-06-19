---
title: Einführung in ASP.NET Core MVC unter macOS, Linux und Windows
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC und Visual Studio Code unter macOS, Linux und Windows.
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893559"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Einführung in ASP.NET Core MVC unter macOS, Linux und Windows

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio Code](https://code.visualstudio.com) (VS Code). Das Tutorial setzt voraus, dass Sie mit VS Code vertraut sind. Weitere Informationen finden Sie unter [Erste Schritte mit VS Code](https://code.visualstudio.com/docs) und [Hilfe zu Visual Studio Code](#visual-studio-code-help). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Es gibt drei Versionen dieses Tutorials:

* macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Erstellen einer Web-App mit dotnet

Führen Sie über ein Terminal die folgenden Befehle aus:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Öffnen den Ordner *MvcMovie* in Visual Studio Code (VS Code), und wählen Sie die Datei *Startup.cs* aus.

- Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird.
- Klicken Sie in der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden. Sollen Sie hinzugefügt werden?“) „Nicht mehr fragen“, „Nicht jetzt“, „Ja“ und auch die Information über nicht aufgelöste Abhängigkeiten – Wiederherstellen – Schließen](../web-api-vsc/_static/vsc_restore.png)

Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.

![Ausgeführte App](../first-mvc-app/start-mvc/_static/1.png)

VS Code startet den [Kestrel](xref:fundamentals/servers/kestrel)-Webserver und führt Ihre App aus. Beachten Sie, dass die Adressleiste `localhost:5000` und nicht etwas wie `example.com` anzeigt. Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.

Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**. Die obenstehende Browserabbildung zeigt diese Links nicht an. Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.

![Navigationssymbol oben rechts](../first-mvc-app/start-mvc/_static/2.png)

Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.

## <a name="visual-studio-code-help"></a>Hilfe zu Visual Studio Code

- [Erste Schritte](https://code.visualstudio.com/docs)
- [Debuggen](https://code.visualstudio.com/docs/editor/debugging)
- [Integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Tastenkombinationen](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [macOS-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Nächstes Tutorial: Hinzufügen eines Controllers](adding-controller.md)
