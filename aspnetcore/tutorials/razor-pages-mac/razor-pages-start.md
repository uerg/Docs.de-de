---
title: Erste Schritte mit Razor-Seiten mit ASP.NET Core unter Mac
author: rick-anderson
description: "Erster Schritte bei Razor-Seiten in ASP.NET Core mithilfe von Visual Studio für Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: d4d8c7c543273aa38d1599b51d00a348df7182de
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor-Seiten. Es wird empfohlen, zuerst das Tutorial [Einführung in Razor-Seiten](xref:mvc/razor-pages/index) zu lesen, bevor Sie mit diesem Tutorial beginnen. Razor-Seiten sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie Folgendes:

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher
* [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

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

Klicken Sie in Visual Studio auf **Datei > Öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.

### <a name="launch-the-app"></a>Starten der App

Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten. Visual Studio startet dann [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) und einen Browser und navigiert zu `http://localhost:5000`.

Im nächsten Tutorial wird dem Projekt ein Modell hinzugefügt.

>[!div class="step-by-step"]
[Weiter: Hinzufügen eines Modells](xref:tutorials/razor-pages-mac/model)
