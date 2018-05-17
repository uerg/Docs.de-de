---
title: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac unter macOS
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in Razor-Seiten in ASP.NET Core mit Visual Studio für Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 2da9b34f797758c02132d5cf6cc2f2fb2fe6f05a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac unter macOS

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages. Es wird empfohlen, zuerst das Tutorial [Einführung in Razor Pages](xref:mvc/razor-pages/index) zu lesen, bevor Sie mit diesem Tutorial beginnen. Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

Führen Sie über ein Terminal die folgenden Befehle aus:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor Pages zu erstellen und auszuführen. Öffnen Sie http://localhost:5000 in einem Browser, um sich die Anwendung anzeigen zu lassen.

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Öffnen des Projekts

Drücken Sie STRG+C, um die Anwendung zu beenden.

Klicken Sie in Visual Studio auf **Datei > Öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.

### <a name="launch-the-app"></a>Starten der App

Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten. Visual Studio startet dann [Kestrel](xref:fundamentals/servers/kestrel) und einen Browser und navigiert zu `http://localhost:5000`.

Im nächsten Tutorial wird dem Projekt ein Modell hinzugefügt.

> [!div class="step-by-step"]
> [Weiter: Hinzufügen eines Modells](xref:tutorials/razor-pages-mac/model)
