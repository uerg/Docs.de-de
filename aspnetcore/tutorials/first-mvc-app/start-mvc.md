---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 6e233a0114967405a67b05365e0125620441efb4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>Erste Schritte mit ASP.NET Core MVC und Visual Studio

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio 2017](https://www.visualstudio.com/). [!INCLUDE[consider RP](../../includes/razor.md)]

Es gibt drei Versionen von diesem Tutorial:

* macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

Die Version dieses Tutorials für Visual Studio 2015 finden Sie unter [Visual Studio 2015-Version der ASP.NET Core-Dokumentation im PDF-Format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).

## <a name="install-visual-studio-and-net-core"></a>Installieren von Visual Studio und .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installieren Sie Visual Studio Community 2017. Wählen Sie den Download „Community“ aus. Überspringen Sie diesen Schritt, wenn Sie Visual Studio 2017 bereits installiert haben.

* [Visual Studio 2017-Installationsprogramm auf der Homepage](https://www.visualstudio.com/)

Führen Sie das Installationsprogramm aus, und wählen Sie die folgenden Workloads aus:

* **ASP.NET- und Webentwicklung** (unter **Web & Cloud**)
* **Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)

![**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)](start-mvc/_static/web_workload.png)

![**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

Schließen Sie das Dialogfeld **Neues Projekt**ab:

* Tippen Sie im linken Bereich auf **.NET Core**.
* Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.
* Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.
* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:

* Klicken Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 2.–**.
* Klicken Sie auf **Webanwendung (Model View Controller)**.
* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:

* Tippen Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 1.1**.
* Tippen Sie auf **Webanwendung**.
* Behalten Sie den Standardwert **Keine Authentifizierung** bei.
* Tippen Sie auf **OK**.

![Neue ASP.NET Core-Web-App](start-mvc/_static/p3.png)

---

Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben. Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits. Dies ist ein einfaches Startprojekt und ein guter Einstieg.

Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* Visual Studio startet [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus. Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt. Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt. Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet. In der obigen Abbildung ist die Portnummer 5000. Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.
* Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen. Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.
* Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.

![IIS Express](start-mvc/_static/iis_express.png)

Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**. Die obenstehende Browserabbildung zeigt diese Links nicht an. Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.

Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.

>[!div class="step-by-step"]
[Nächste](adding-controller.md)  
