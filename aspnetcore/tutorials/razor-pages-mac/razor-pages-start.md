---
title: Erste Schritte mit Razor-Seiten mit ASP.NET Core unter Mac
author: rick-anderson
description: "Erster Schritte bei Razor-Seiten in ASP.NET Core mithilfe von Visual Studio für Mac"
keywords: "ASP.NET Core, Razor-Seiten, Gerüstbau, Entity Framework Core, EF, EF Core, Datenbank, Mac, macOS, Visual Studio für Mac"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: caadc3fcb3bb71abe0773aed4f6ff60a043e3a02
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Web-App mit Razor-Seiten. Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor-Seiten)](xref:mvc/razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen. Razor-Seiten sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* [.NET Core 2.0.0 SDK](https://dot.net/core) oder höher
* [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

Führen Sie über ein Terminal die folgenden Befehle aus:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Bei diesen Befehlen wird die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) verwendet, um Projekte mit Razor-Seiten zu erstellen und auszuführen. Navigieren Sie in einem Browser zu „http://localhost:5000“, um sich die Anwendung anzeigen zu lassen.

![Seite „Home“ oder „Index“](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Öffnen des Projekts

Drücken Sie STRG+C, um die Anwendung zu beenden.

Klicken Sie in Visual Studio auf **Datei > Öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.

### <a name="launch-the-app"></a>Starten der App

Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten. Visual Studio startet dann [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) und einen Browser und navigiert zu `http://localhost:5000`.

Im nächsten Tutorial wird dem Projekt ein Modell hinzugefügt.

>[!div class="step-by-step"]
[Weiter: Hinzufügen eines Modells](xref:tutorials/razor-pages-mac/model)
