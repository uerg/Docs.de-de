---
title: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
keywords: "ASP.NET Core, Razor-Seiten, Gerüstbau, Entity Framework Core, EF, EF Core, Datenbank, Mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: aa39de71addb2499af6d322db6da0ec635c54970
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor-Seiten. Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor-Seiten)](xref:mvc/razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen. Razor-Seiten stellen die empfohlene Methode für die Erstellung der Benutzeroberfläche für Webanwendungen in ASP.NET Core dar.

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* [.NET Core 2.0.0 SDK](https://dot.net/core) oder höher
* [Visual Studio Code](https://code.visualstudio.com)
* Visual Studio Code [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

Führen Sie über ein Terminal die folgenden Befehle aus:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor-Seiten zu erstellen und auszuführen. Öffnen Sie in einem Browser „http://localhost:5000“, um sich die Anwendung anzeigen zu lassen.

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Öffnen des Projekts

Drücken Sie STRG+C, um die Anwendung zu beenden.

Klicken Sie in Visual Studio Code (VS Code) auf **Datei > Ordner öffnen**, und wählen Sie dann den Ordner *RazorPagesMovie* aus.

- Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'RazorPagesMovie' nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird.
- Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.

### <a name="launch-the-app"></a>Starten der App

Drücken Sie STRG+F5, um die App ohne Debugging zu starten. Alternativ können Sie auch über das Menü **Debuggen** die Option **Starten ohne Debugging** auswählen.

Im nächsten Tutorial fügen wir dem Projekt ein Modell hinzu. 

>[!div class="step-by-step"]
[Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac)](xref:tutorials/razor-pages-vsc/model)  
