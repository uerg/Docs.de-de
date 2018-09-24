---
title: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erfahren Sie Grundlegendes über das Erstellen einer Razor-Seiten-Web-App in ASP-NET Core mit Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: b7f6ca377a892fce912dc0ee9d4b7378f40fbf24
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/21/2018
ms.locfileid: "46522926"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages. Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor Pages)](xref:razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen. Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

Führen Sie über ein Terminal die folgenden Befehle aus:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor Pages zu erstellen und auszuführen. Öffnen Sie http://localhost:5000 in einem Browser, um sich die Anwendung anzeigen zu lassen.

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Öffnen des Projekts

Drücken Sie STRG+C, um die Anwendung zu beenden.

Klicken Sie in Visual Studio Code (VS Code) auf **Datei > Ordner öffnen**, und wählen Sie dann den Ordner *RazorPagesMovie* aus.

- Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'RazorPagesMovie' nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird.
- Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.

### <a name="launch-the-app"></a>Starten der App

Drücken Sie STRG+F5, um die App ohne Debugging zu starten. Alternativ können Sie auch über das Menü **Debuggen** die Option **Starten ohne Debugging** auswählen.

Im nächsten Tutorial fügen wir dem Projekt ein Modell hinzu. 

> [!div class="step-by-step"]
> [Weiter: Hinzufügen eines Modells](xref:tutorials/razor-pages-vsc/model)  
