---
title: "Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac"
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio
keywords: "ASP.NET Core, MVC, Visual Studio für Mac, Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE[consider RP](../../includes/razor.md)]

Es gibt drei Versionen dieses Tutorials:

* macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Tutorial erfordert das [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher.

Installieren Sie Folgendes:

- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher
- [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Klicken Sie in Visual Studio auf **Datei > Neue Projektmappe**.

![Neue Projektmappe in macOS](../first-web-api-mac/_static/sln.png)

Klicken Sie auf **.NET Core-App > ASP.NET Core > Web-App > Weiter**.

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/1.png)

Nennen Sie das Projekt **MvcMovie**, und wählen Sie dann **Erstellen** aus.

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/2.png)

### <a name="launch-the-app"></a>Starten der App

Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten. Visual Studio startet [Kestrel](xref:fundamentals/servers/index#kestrel) und einen Browser und navigiert zu `http://localhost:port`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.

![Browser mit einem neuen Projekt](start-mvc/b1.png)

* Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`. Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt. Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet. Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.
* Sie können die App über das Menü **Ausführen** im Debugmodus oder Nicht-Debugmodus starten.

Die Standardvorlage bietet Ihnen Links für **Startseite, Info** und **Kontakt**. Die obenstehende Browserabbildung zeigt diese Links nicht an. Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.

![Browser mit einem neuen Projekt](start-mvc/b2.png)

Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.

>[!div class="step-by-step"]
[Nächste](adding-controller.md)  
