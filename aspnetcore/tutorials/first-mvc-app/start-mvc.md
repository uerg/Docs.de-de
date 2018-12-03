---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC und Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9d50607899058c887597a3d73198552d3ef5b020
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710087"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Erste Schritte mit ASP.NET Core MVC und Visual Studio

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Es gibt drei Versionen dieses Tutorials:

* macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

> [!NOTE]
> Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.  Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).

## <a name="install-visual-studio-and-net-core"></a>Installieren von Visual Studio und .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

Schließen Sie das Dialogfeld **Neues Projekt**ab:

* Tippen Sie im linken Bereich auf **.NET Core**.
* Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.
* Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.
* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web ](start-mvc/_static/new_project2-21.png)

Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:

* Wählen Sie im Dropdownfeld der Versionsauswahl den Eintrag **ASP.NET Core 2.1.** aus
* Wählen Sie **Webanwendung (Model-View-Controller)** aus.
* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web ](start-mvc/_static/new_project22-21.png)

Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben. Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits. Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.

Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus. Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt. Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt. Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet. In der obigen Abbildung ist die Portnummer 5000. Die URL im Browser zeigt `localhost:5000` an. Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.
* Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen. Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.
* Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.

![IIS Express](start-mvc/_static/iis_express.png)

Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**. Die obenstehende Browserabbildung zeigt diese Links nicht an. Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.

Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

Schließen Sie das Dialogfeld **Neues Projekt**ab:

* Tippen Sie im linken Bereich auf **.NET Core**.
* Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.
* Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.
* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web ](start-mvc/_static/new_project2.png)

Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:

* Klicken Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 2.–**.
* Klicken Sie auf **Webanwendung (Model View Controller)**.
* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web ](start-mvc/_static/new_project22.png)

Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben. Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits. Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.

Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus. Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt. Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt. Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet. In der obigen Abbildung ist die Portnummer 5000. Die URL im Browser zeigt `localhost:5000` an. Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.
* Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen. Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.
* Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.

![IIS Express](start-mvc/_static/iis_express.png)

Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**. Die obenstehende Browserabbildung zeigt diese Links nicht an. Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.

Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.

::: moniker-end

> [!div class="step-by-step"]
> [Nächste](adding-controller.md)  
